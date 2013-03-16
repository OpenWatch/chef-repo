# Vagrant

What you need to know:

1. The `Vagrantfile` has all of the configuration you could need.
2. `vagrant up` - this makes a fresh box from your vagrant config

The vagrant box (precise64) we are using has an outdated version of Chef-client so we need to upgrade it first.

http://stackoverflow.com/questions/11325479/how-to-control-the-version-of-chef-that-vagrant-uses-to-provision-vms

This upgrades chef-client to the latest version on the server:

`curl -L https://www.opscode.com/chef/install.sh | sudo bash`

I should package this in a box we use.

## Rebuild box from scratch

Currently vagrant creates a lot of client and node entries on the Chef Server that we need to delete manually.

    $ vagrant destroy
    $ knife client delete #{fqdn} --yes 
    $ knife node delete #{fqdn} --yes
    
https://gist.github.com/cassianoleal/4223941 might be a better solution


## Failure

Why does this fail? :(

	[2013-03-15T22:08:49+00:00] INFO: Processing package[nodejs] action install (nodejs::install_from_package line 49)
	[2013-03-15T22:09:13+00:00] INFO: Processing package[npm] action install (nodejs::install_from_package line 49)
	
	
	================================================================================
	
	Error executing action `install` on resource 'package[npm]'
	
	================================================================================
	
	
	
	
	Chef::Exceptions::Exec
	
	----------------------
	
	apt-get -q -y install npm=1.1.4~dfsg-1 returned 100, expected 0
	
	
	
	
	Resource Declaration:
	
	---------------------
	
	# In /srv/chef/file_store/cookbooks/nodejs/recipes/install_from_package.rb
	
	 49:       package pkg
	 50:     end
	
	
	
	
	
	Compiled Resource:
	
	------------------
	
	# Declared in /srv/chef/file_store/cookbooks/nodejs/recipes/install_from_package.rb:49:in `block in from_file'
	
	
	package("npm") do
	  action :install
	  retries 0
	  retry_delay 2
	  package_name "npm"
	  version "1.1.4~dfsg-1"
	  cookbook_name "nodejs"
	  recipe_name "install_from_package"
	end
	
	
	
	
	
	[2013-03-15T22:09:14+00:00] INFO: Running queued delayed notifications before re-raising exception
	[2013-03-15T22:09:14+00:00] INFO: template[/etc/ssh/sshd_config] sending restart action to service[ssh] (delayed)
	[2013-03-15T22:09:14+00:00] INFO: Processing service[ssh] action restart (openssh::default line 33)
	[2013-03-15T22:09:14+00:00] INFO: service[ssh] restarted
	[2013-03-15T22:09:14+00:00] INFO: template[nginx.conf] sending reload action to service[nginx] (delayed)
	[2013-03-15T22:09:14+00:00] INFO: Processing service[nginx] action reload (nginx::default line 43)
	[2013-03-15T22:09:15+00:00] INFO: service[nginx] reloaded
	[2013-03-15T22:09:15+00:00] ERROR: Running exception handlers
	[2013-03-15T22:09:15+00:00] FATAL: Saving node information to /srv/chef/file_store/failed-run-data.json
	[2013-03-15T22:09:15+00:00] ERROR: Exception handlers complete
	[2013-03-15T22:09:15+00:00] FATAL: Stacktrace dumped to /srv/chef/file_store/chef-stacktrace.out
	[2013-03-15T22:09:15+00:00] FATAL: Chef::Exceptions::Exec: package[npm] (nodejs::install_from_package line 49) had an error: Chef::Exceptions::Exec: apt-get -q -y install npm=1.1.4~dfsg-1 returned 100, expected 0
	The following SSH command responded with a non-zero exit status.
	Vagrant assumes that this means the command failed!
	
	chef-client -c /tmp/vagrant-chef-1/client.rb -j /tmp/vagrant-chef-1/dna.json