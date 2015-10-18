title: Turning on php error log
id: 51
categories:
  - Uncategorized
date: 2015-01-05 09:44:23
tags:
---

### Three things to do:

1.  _set error reporting level_
2.  _set error log path_
3.  _create log file manually and grant privileges._

## Set the error reporting level

In the file <span class="lang:default decode:true  crayon-inline">/etc/php5/apache2/php.ini</span> , find entry <span class="lang:default decode:true  crayon-inline ">error_reporting</span> , set it to <span class="lang:default decode:true  crayon-inline ">E_ALL</span>

## Set error log path

in php.ini, find <span class="lang:default decode:true  crayon-inline ">error_log</span>  entry, set it to <span class="lang:default decode:true  crayon-inline ">/var/log/php_errors.log</span>

## Create the file manually and grant privileges

<pre class="lang:default decode:true " title="manually create the log file and grant privileges">touch /var/log/php_errors.log
chown www-data: /var/log/php_errors.log
chmod +rw /var/log/php_errors.log</pre>
&nbsp;