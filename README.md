#Description

A RightScale Blueprint for FreeSWITCH.

#Requirements

* Chef 0.10.10 or higher (earlier versions may work with some cookbooks/recipes)
* A Linux host

#Usage

See the Quick Start below to get started.

##Cookbooks

The following core cookbooks are included:

* freeswitch
* system
* resolver
* rightscale
* collectd
* sudo
* ntp
* postfix
* python

Additional/depends cookbooks:

* apache2
* build-essential
* collectd_plugins
* cron
* yum

See the `Cheffile.lock` for details on their upstream sources.

##Chef Attributes

See the `examples/chef-solo` folder for examples on attribute usage using a `node.json`.
Here is a summary of the main Chef node attributes (analog to RightScale Inputs) that are configurable:

* authorization/sudo/groups
* authorization/sudo/include_sudoers_d
* authorization/sudo/passwordless
* authorization/sudo/users
* collectd/fqdn_lookup
* collectd/hostname
* collectd/servers
* ntp/ntpdate/disable
* ntp/peers
* ntp/restrictions
* ntp/servers
* postfix/mail_relay_networks (plus all postfix cookbook attributes)
* resolver/nameservers
* resolver/search
* system/domain_name
* system/short_hostname
* system/timezone

#Quick Start

##RightScale

TBR.

##VirtualBox with Vagrant

###Install VirtualBox

Follow the VirtualBox documentation to install VirtualBox if not already installed.

###Install Vagrant

	sudo gem install vagrant --no-rdoc --no-ri

###Clone the freeswitch RightScale Blueprint

	mkdir -p ~/src/github/rightscale-blueprints
	cd ~/src/github/rightscale-blueprints
	git clone git://github.com/rightscale-blueprints/freeswitch.git
	cd freeswitch/vagrant

###Run with Vagrant

Already up'd a freeswitch box?

	#vagrant status                   # check vm status
	#vagrant reload                   # reload the vm
	#vagrant suspend                  # suspend the vm
	#vagrant halt                     # power down the vm
	#vagrant destroy                  # destroy the vm
	#vagrant box remove freeswitch    # remove the box

Add a new box from local or remote (Ubuntu 12.04)

	vagrant box add freeswitch http://files.vagrantup.com/precise64.box
	#vagrant box add freeswitch ~/Binaries/vagrant/boxes/precise64.box

Need debug?

	VAGRANT_LOG=debug

Run the virtual machine

	# vagrant up!
	vagrant up
	
This uses the `Vagrantfile` in the `vagrant/` folder (and the `examples/chef-solo/node.json` for the Chef Solo provisioning).

##Chef Solo

Using the Vagrant/VirtualBox process above is recommended for desktop/workstations users and will provide an operational FreeSWITCH server out-of-box.
The instructions below demonstrate usage with Chef Solo standalone. Commands are relative the root of the Git checkout.

	sudo chef-solo -c examples/chef-solo/solo.rb
	
By default this uses the `examples/chef-solo/node.json`. You can easily switch the JSON attributes used with another example:

	sudo chef-solo -c examples/chef-solo/solo.rb -j examples/chef-solo/node.json.minimal
	
Its also possible to run with the cookbooks source as remote. This is handy because no Git checkout is needed:

	sudo chef-solo -r https://github.com/rightscale-blueprints/freeswitch/tarball/master
	
And with a specific tag such as `Rev1`:

	sudo chef-solo -r https://github.com/rightscale-blueprints/freeswitch/tarball/rev1

For more information on using Chef Solo, see http://wiki.opscode.com/display/chef/Chef+Solo

#Using Librarian

##Install Librarian

	sudo gem install librarian --no-rdoc --no-ri

##Updating cookbooks

Set this environment variable to strip .git from each cookbook checkout:

	export LIBRARIAN_CHEF_INSTALL__STRIP_DOT_GIT=1

To update a cookbook (example, freeswitch):

	librarian-chef update freeswitch

To refresh all the cookbooks in `cookbooks/` per the `Cheffile`, run the following:

	librarian-chef install

To (re)install the cookbooks clean and with verbose:

	librarian-chef install --clean --verbose

	
#Errata

##Directories and files in this repository

The root of the repository contains `Cheffile` and `Cheffile.lock` files from librarian. A shell script, `update-metadata.sh` is provided to re-generate particular cookbooks `metadata.json` (such as upstream cookbooks that do not include a `metadata.json`).

	cookbooks/			Librarian stored (cached) upstream cookbooks from `Cheffile`.
	examples/			Examples folder for Chef, Chef Solo.
	librarian/			Librarian folder for `Cheffile` templates.
	vagrant/			Vagrant project folder

TODO: MANIFEST file.

License and Author
==================

Author:: Chris Fordham (<chris.fordham@rightscale.com>)

Copyright 2012, RightScale Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.