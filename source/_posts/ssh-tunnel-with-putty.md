title: SSH Tunnel with Putty
id: 46
categories:
  - 'Coding & Geek Stuff'
date: 2014-12-28 18:51:56
tags:
---

Using Putty we are able to create SSH Tunnel

There are three types of SSH Tunnels: Local (Forward), Remote (Backward), and Dynamic (mimicking a socks proxy).

First I tried to set up a local type SSH Tunnel, and used _nc _to test out the connection:

[![Local SSH Tunnel](http://wordpress.jowos.moe/wp-content/uploads/2014/12/屏幕截图14-300x168.png)](http://wordpress.jowos.moe/wp-content/uploads/2014/12/屏幕截图14.png)

&nbsp;

What a local tunnel does, basically, is opening up a listening port on your _Local _machine and bind it with a _remote_ _port_ on the remote machine so that everything going to the _local port_ will appear on the other side of the tunnel right from the _remote port._

The mechanism is similar with remote Tunnel. However, this time it's the remote machine that opens up a listening port, effectively creating a "Trojan" to allow anyone that could access the remote port communicate with your local machine even if it may be hidden inside NATs.

Then with the dynamic tunnel. It's pretty much like a proxy, frankly saying. Everything packet sent from a certain _local port _to the _proxy port_ will appear from the exactly same port on the remote machine. Good proxy, easy to use.

However, it's said that large data packets are not friendly with SSH Tunnels. Indeed, when I tried to download an SSH Tunnel apk from the internet, the Putty session broke off immediately. So far it's pretty enough for email and web browsing. Next thing I'm going to look at shadowsocks.