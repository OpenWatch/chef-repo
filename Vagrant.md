# Vagrant

What you need to know:

1. The `Vagrantfile` has all of the configuration you could need.
2. `vagrant up` - this makes a fresh box from your vagrant config

The vagrant box (precise64) we are using has an outdated version of Chef-client so we need to upgrade it first.

http://stackoverflow.com/questions/11325479/how-to-control-the-version-of-chef-that-vagrant-uses-to-provision-vms

I should package this in a box we use.

## Rebuild box from scratch

Currently vagrant creates a lot of client and node entries on the Chef Server that we need to delete manually.

    $ vagrant destroy
    $ knife client delete #{fqdn} --yes 
    $ knife node delete #{fqdn} --yes
    
https://gist.github.com/cassianoleal/4223941 might be a better solution