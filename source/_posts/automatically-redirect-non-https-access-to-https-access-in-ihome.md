title: Automatically redirect non https access to https access in ihome
id: 60
categories:
  - Uncategorized
date: 2015-01-13 17:18:29
tags:
---

RewriteEngine On
RewriteCond %{HTTPS} !=on
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R,L]