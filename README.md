nodebox
============

Vagrantfile for Node.js applications. Briefly:

* Ubuntu 13.10 64-bit
* forever support
* no nginx/Apache (nginx will be added)

The machine will expect your app at `/usr/srv/www/nodejs/app.js`.

Installation
------------

Install Vagrant (v1.5.2+) and ansible (v.1.5.4+). Then, simply:

> `vagrant up`

Enjoy.

Provider support
---------

### lxc 
The vagrant-lxc plugin has been flaky with vagrant 1.5+, so it's not currently supported.

### AWS EC2
Needs much more testing

To Do
=====
* add nginx reverse proxy
* AWS support
  * SSH access
  * read EC2 ENV vars
  * forward ports
  * make security group setting recommendations?

