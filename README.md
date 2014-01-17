Description
============

This project provides a base image for running nodejs applications. The 
default provider is VirtualBox, although AWS EC2 instance support is coming soon.

Installation
============

Install Vagrant (v1.3.4 or higher) from the [project website](http://downloads.vagrantup.com/). 
Then install these Ruby gems:

> `gem install librarian-chef`
> 
> `gem install vagrant-aws`

If those commands showed a permissions error, you should set up 
a local installation path for Ruby gems. See [this how-to](http://jbowes.wordpress.com/2008/05/13/installing-ruby-gems-in-your-home-directory/).

If you receive the error "NotImplementedError: C extension support is not enabled. Pass -Xcext.enabled=true to JRuby or set JRUBY_OPTS or modify .jrubyrc to enable." while attempting to install librarian-chef, either:
+ create ~/.jrubyrc and add the line: `cext.enabled=true`
+ add: `cext.enabled=true` to your existing ~/.jrubyrc

## 
Next, you'll need to pull down the configuration scripts that will 
provision the VM. Since you now have access to Librarian for Chef, 
you can reference the project's Cheffile to grab the necessary cookbooks:

> `librarian-chef install`

Then, simply: 

> `vagrant up`

Enjoy.

To Do
=====
* AWS support
  * SSH access
  * read EC2 ENV vars
  * forward ports
  * make security group setting recommendations?
* fix mongodb provisioning to use --smallfiles on first run. 
* background nodejs process
  * use "forever" to daemonize?
  * use upstart script?
