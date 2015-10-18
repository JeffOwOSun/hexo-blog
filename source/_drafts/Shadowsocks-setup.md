title: Shadowsocks setup
id: 49
categories:
  - Uncategorized
tags:
---

Official Guide: [http://shadowsocks.org/en/download/servers.html](http://shadowsocks.org/en/download/servers.html "Official Shadowsocks server guide")

Make sure python 2.6 or 2.7 is installed. Then install using pip

<span class="theme:github lang:default decode:true crayon-inline">$ sudo pip install shadowsocks</span>

After that, I created a folder .shadowsocks under my home dir, and created a file config.json with the following content
<pre class="theme:github lang:js decode:true">{
    "server":"127.0.0.1",
    "server_port":8388,
    "local_port":1080,
    "password":"barfoo!",
    "method": "aes-128-cfb",
    "timeout":600
}</pre>
Then run the shadowsocks in that folder with <span class="theme:github lang:default decode:true  crayon-inline ">shadowsocks-server</span> , and it will automatically read the config file. (Later I'll try to add a init script for this)

&nbsp;