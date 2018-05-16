---
title: "Rails tip #4: Writing Capistrano recipes to be loaded from gems"
layout: post
---
<p>Making a gem of your custom Capistrano recipes is a nice way to remove duplicated knowledge across your Rails projects.</p>

<p>On your first pass, you'll probably do what I did: gank and modify <a href="http://github.com/capistrano/capistrano/tree/72a254d4221e37dce10e2e7e56b2abe36fc53452/lib/capistrano/recipes/deploy.rb"><code>capistrano/recipes/deploy.rb</code></a>.</p>

<p>This works great, but you'll find it's a little tricky to use your new recipes from a <code>Capfile</code>. It turns out you can't just "<code>require mygem</code>" and "<code>load 'mygem/recipes/deploy'</code>" because Capistrano doesn't <code>load</code> from ruby's <code>$LOAD_PATH</code>&mdash;it keeps its own <a href="http://github.com/capistrano/capistrano/tree/72a254d4221e37dce10e2e7e56b2abe36fc53452/lib/capistrano/configuration/loading.rb#L57">minimally-initialized</a> <code>load_paths</code> setting instead.</p>

<p>So, you have to either modify the <code>load_paths</code> or use an absolute path, like this:</p>

```ruby
%w( rubygems wordpress ).each { |lib| require lib }
load Gem.required_location('wordpress', 'wordpress/recipes/deploy.rb')
load 'config/deploy'
```

<p>This is what I did for a while, but something didn't seem right. My <code>Capfile</code> didn't look as nice as I expected, and I wasn't using <a href="http://github.com/capistrano/capistrano/tree/72a254d4221e37dce10e2e7e56b2abe36fc53452/lib/capistrano/configuration/loading.rb#L104"><code>require</code></a>, whose comments clearly mention third-party recipes.</p>

<h2>Take two</h2>

<p>Staring at <code>capistrano/configuration/loading.rb</code> until it made sense, I saw <a href="http://github.com/capistrano/capistrano/tree/72a254d4221e37dce10e2e7e56b2abe36fc53452/lib/capistrano/configuration/loading.rb#L11"><code>instance</code></a>, which triggered some vague distant memory that somehow turned into a productive thought:</p>

<p>If you wrap your <code>deploy.rb</code> recipes in a load block, like this:</p>

```ruby
Capistrano::Configuration.instance(:must_exist).load do
  # previous file contents here
end
```

<p>You can simplify your <code>Capfile</code> with a shorter <code>require</code> statement:</p>

```ruby
require 'rubygems'
require 'wordpress/recipes/deploy'
load    'config/deploy'
```

<p>Nice. I'm still a little bummed by the <code>require</code> / <code>load</code> imbalance, but on the whole this feels more like things were supposed to be.</p>

<h2>5 Rails tips</h2>

<p>Each day this week, <a href="http://youtube.com/watch?v=J35CuC3ywnc">Joachim</a> and I will post something we've learned in our time programming together. It's fun to do, and we might just <a href="http://railscasts.com/contest">win something</a> as well.</p>

<p>So far, we've written:</p>

<ol>
  <li><a href="{{ site.url }}/2008/04/21/rails-tip-1-reloadable-custom-formbuilder.html">Reloadable custom FormBuilder</a></li>
  <li><a href="{{ site.url }}/2008/04/22/rails-tip-2-faking-data-in-tests.html">Faking DATA in tests</a></li>
  <li><a href="{{ site.url }}/2008/04/23/rails-tip-3-filter-blobs-from-activerecord-logging.html">Filter BLOBs from ActiveRecord logging</a></li>
  <li>Writing Capistrano recipes to be loaded from gems</li>
</ol>
