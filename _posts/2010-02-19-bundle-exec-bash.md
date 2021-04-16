---
title: bundle exec bash
layout: post
---

The <a href="http://github.com/carlhuda/bundler#readme">bundler README</a>
makes passing mention of <code>bundle exec bash</code>&mdash;when you bundle
everything, you want to avoid typing <code>bundle exec rake</code> all the
time.

Here are 3 tips to help you enjoy the experience:

<h2>Sort your <code>.bashrc</code> from your <code>.profile</code></h2>

The first time I ran <code>bundle exec bash</code>, I lost the git branch from my prompt:

<div class="highlight"><pre>
~/Code/captain <span class="color-magenta">master*</span> bundle exec bash
bash: __git_ps1: command not found
~/Code/captain
</pre></div>

Poring through the documentation for the <a
href="http://www.gnu.org/software/bash/manual/bashref.html#Bash-Startup-Files">bash
startup files</a>, here's what I pieced together:

* <code>Terminal.app</code> launches a &ldquo;login&rdquo; shell, which reads <code>~/.profile</code>.

* The shell launched by <code>(bundle exec) bash</code> is <em>not</em> a login shell, so it reads <code>~/.bashrc</code> instead. (Moreover, it only inherits the <em>environment variables</em> of its parent shell, none of the functions, aliases, or completions.)

Connecting the dots, you'll want to:

1. Move all your aliases, functions and completions out of <code>~/.profile</code> and into <code>~/.bashrc</code>. (<a href="http://github.com/matthewtodd/dotfiles/commit/84d57288548c484d59f0f1e1d43ab3b0abb1b263">example</a>)

2. Source your <code>~/.bashrc</code> at the bottom of your <code>~/.profile</code>:

```bash
if [ -f ~/.bashrc ]; then
  source ~/.bashrc
fi
```

And then you should have <code>__git_ps1</code> and friends again:

<div class="highlight"><pre>
~/Code/captain <span class="color-magenta">master*</span> bundle exec bash
~/Code/captain <span class="color-magenta">master*</span>
</pre></div>

<h2>Beware the <code>RUBYOPT</code></h2>

What you can't see in the prompt above is the next problem I ran into. I should
have written something more like this:

<div class="highlight"><pre>
~/Code/captain <span class="color-magenta">master*</span> bundle exec bash
... THREE SECOND DELAY ...
~/Code/captain <span class="color-magenta">master*</span>
</pre></div>

Here's what's going on:

1. <code>__git_ps1</code> calls <code>git</code> <a href="http://github.com/git/git/blob/v1.6.5.7/contrib/completion/git-completion.bash#L87-178">5 or 6 times.</a>

2. I had mixed (the <a href="http://gist.github.com/284823">non-rubygems version</a> of) <a href="http://github.com/defunkt/hub"><code>hub</code></a> into <code>git</code> with `alias git=hub`.

3. <code>bundle exec</code> <a href="http://github.com/carlhuda/bundler/blob/0.9.7/lib/bundler/cli.rb#L119-123">adds <code>-rbundler/setup</code> to your <code>RUBYOPT</code></a>.

Boom! I was resolving the dependencies in my <code>Gemfile</code> 5 or 6 times
just to generate a prompt!

The simple workaround is to clear <code>RUBYOPT</code> when calling <code>hub</code>:

```bash
alias git='RUBYOPT= hub'
```

<h2>Show the bundle in your prompt</h2>

It gets hard to remember if you've already run <code>bundle exec bash</code>,
or, if you tend to <code>cd</code> wildly all over the place, <em>which</em>
project you ran it in.

With this function in your <code>~/.bashrc</code>:

```bash
function __bundler_ps1 {
  if [ -n "${BUNDLE_GEMFILE-}" ]; then
    project_path="${BUNDLE_GEMFILE%/Gemfile}"
    project_name="${project_path##**/}"

    if [ -n "${1-}" ]; then
      printf "$1" "${project_name}"
    else
      printf " (%s)" "${project_name}"
    fi
  fi
}
```

And a <code>PS1</code> setting in your <code>~/.profile</code> that calls it:

```bash
export PS1='\[\e[36m\]$(__bundler_ps1 "[%s] ")\[\e[0m\]\w\[\e[35m\]$(__git_ps1 " %s")\[\e[0m\] '
```

You'll see the name of the active bundle in your prompt:

<div class="highlight"><pre>
~/Code/captain <span class="color-magenta">master*</span> bundle exec bash
<span class="color-cyan">[captain]</span> ~/Code/captain <span class="color-magenta">master*</span>
</pre></div>

<h2>Conclusion</h2>

It seems like the main thing I'm learning is that it's worth spending time
tinkering with your <code>$SHELL</code>.

Along the way, I've found it immensely helpful to version my <a
href="http://github.com/matthewtodd/dotfiles">dotfiles</a>. If you haven't done
that yet, make it your weekend project&mdash;you'll love the freedom it gives
you to experiment! I found <a
href="http://github.com/jferris/config_files/blob/master/install.sh">Joe
Ferris' <code>install.sh</code></a> quite helpful as I got started.
