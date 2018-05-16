---
title: Using <tt>deprec</tt> behind an HTTP Proxy
layout: post
---
Yesterday I had a fair bit of difficulty because the app server I was setting up with <a href="http://www.deprec.org/"><code>deprec</code></a> lives behind an http proxy.

Normally, you'd hope to just set an <code>HTTP_PROXY</code> environment variable and be done, but it's a little tricky to figure out <em>where</em>, since (1) commands run over <code>ssh</code> receive a somewhat limited set of environment variables, and (2) <code>deprec</code> <a href="http://www.deprec.org/trac.cgi/browser/tags/1.9.0/lib/deprec/capistrano_extensions/deprec_extensions.rb#L87">runs <code>wget</code></a> inside the even more limited environment of <code>sh -c</code>.

Here's what I did:

<h2>Set Up <tt>ssh</tt> Environment</h2>

Create <code>~/.ssh/environment</code> on the server, <code>chmod 600</code>, with the line

```bash
HTTP_PROXY=http://proxy:8080
```

And in <code>/etc/ssh/sshd_config</code>, set

```apache
PermitUserEnvironment yes
```

This'll take care of <code>apt-get</code> and <code>gem</code>'s proxy needs, but then you'll still need to:

<h2>Configure <tt>wget</tt></h2>

Modify <code>/etc/wgetrc</code> so that it contains

```ini
http_proxy = http://proxy:8080
```

<h2>Done!</h2>

It seems like everything is easy, once you find the right 3 lines of code...
