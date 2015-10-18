title: set up a reverse proxy on ihome
id: 62
categories:
  - Uncategorized
tags:
---

The reverse proxy will look like a mirror site

however since we only have access to .htaccess we can only use a rewrite rule, instead of ProxyPass and ProxyPassReverse (this one is able to handle redirect headers).

&nbsp;