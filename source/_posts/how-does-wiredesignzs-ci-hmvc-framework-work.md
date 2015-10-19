title: "How does wiredesignz's CI HMVC framework work?"
id: 56
categories:
  - 'Coding & Geek Stuff'
date: 2015-01-07 20:39:18
tags:
---

This article assumes basic knowledge on [CodeIgniter MVC](http://www.codeigniter.com/user_guide/overview/mvc.html "CodeIgniter MVC").

We know that when a request comes in, CI invokes our controller's methods by parsing the request url. Then its work is delegated to our controller to actually aggregates data and render views. This workflow is pretty simple and intuitive, and is rather feasible for small websites.

But when things get more complicated, we want to separate things. For example, we want to create an online shopping page. There's gonna be a shopping cart. Instead of integrating the shopping cart codes into each controller whose views contains shopping cart view, we can separate the shopping cart codes and wraps them into a separate "module", with its own controllers, views, and models.

So far so clear. The problem is understanding how modules work together. The easiest case is that a module basically serves a standalone sub-site. It's resembling folder structures of the www directory of a server, with a different url for different directory. In this case, the wiredesignz's HMVC comes with its improved routing that caters to our needs: the controller that has the same name as the module becomes the default controller. Basically, if we have a module named <span class="lang:default decode:true  crayon-inline">module_name</span> then a url pointing to <span class="lang:default decode:true crayon-inline">xxx.com/module_name</span> will set off a search for the file at <span class="lang:default decode:true  crayon-inline">application/modules/module_name/controllers/module_name.php</span>

This guide on wiredesignz's page for HMVC will help you understand how it works:

###### Modular Extensions installation

1.  Start with a clean CI install
2.  Set $config[‘base_url’] correctly for your installation
3.  Access the URL /index.php/welcome =&gt; shows Welcome to CodeIgniter
4.  Drop Modular Extensions third_party files into the CI 2.0 application/third_party directory
5.  Drop Modular Extensions core files into application/core, the MY_Controller.php file is not required unless you wish to create your own controller extension
6.  Access the URL /index.php/welcome =&gt; shows Welcome to CodeIgniter
7.  Create module directory structure application/modules/welcome/controllers
8.  Move controller application/controllers/welcome.php to application/modules/welcome/controllers/welcome.php
9.  Access the URL /index.php/welcome =&gt; shows Welcome to CodeIgniter
10.  Create directory application/modules/welcome/views
11.  Move view application/views/welcome_message.php to application/modules/welcome/views/welcome_message.php
12.  Access the URL /index.php/welcome =&gt; shows Welcome to CodeIgniter
You should now have a running Modular Extensions installation.

###### Installation Guide Hints:

*   -Steps 1-3 tell you how to get a standard CI install working - if you have a clean/tested CI install, skip to step 4.
*   -Steps 4-5 show that normal CI still works after installing MX - it shouldn’t interfere with the normal CI setup.
*   -Steps 6-8 show MX working alongside CI - controller moved to the “welcome” module, the view file remains in the CI application/views directory - MX can find module resources in several places, including the application directory.
*   -Steps 9-11 show MX working with both controller and view in the “welcome” module - there should be no files in the application/controllers or application/views directories.
But more generally, a module may not function all the way from controller stage to  the rendered view. It may even intervene only at a certain point, do something, and pull off as if it never happened. Other modules should never know the existence of this module unless the developers of these modules agree on an interface.

How? The answer is hooks. The framework itself, has several hook spots by raising certain events. These become ideal "intervene point" if your module listen for these events. And if one is a qualified developer with rich experience in cooperation, he/she should design his/her own module with hooks in mind, allowing for others to hook into it. This way, the caller of the event handlers become the event system itself. Specific modules will no longer care which module is intervening.
