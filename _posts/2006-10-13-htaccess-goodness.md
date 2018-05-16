---
title: .htaccess goodness
layout: post
---
I just found a nice bit of .htaccess goodness:

```apache
RewriteEngine On
RewriteCond %{HTTP_HOST} ^.*\.todd\.durham\.nc\.us
RewriteRule (.*) http://127.0.0.1:8888/$1 [P]
```

Essentially, all wildcard names get proxied from Apache over to lighttpd. Fun for little one-off rails apps. (Note, this doesn't handle reverse-proxying, so absolute redirects will expose lighttpd's port number.)
