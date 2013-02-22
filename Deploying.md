Deploying
=========

When you're ready to push your changes to the Chef Server.

## Sync Data Bags

1. Ensure you've created `/data-bags/user-passwords/passwords.json`
2. Create data bags if they don't yet exist on the Chef Server
	
		$ knife data bag create users
		$ knife data bag create user-passwords
		
3. Sync data bags with the Chef Server. This only needs to be done once per each data bag change.

		$ knife data bag from file users username.json
		    # repeat for each user
		$ knife data bag from file user-passwords passwords.json --secret-file /path/to/encrypted-data-bag-secret

**Note**: knife sync-all doesn't work with encrypted data bags.

## Sync Cookbooks

Upload all repo cookbooks to the Chef Server:

		$ knife cookbook upload --all
or with [knife-sync-all](https://github.com/cdoughty77/knife-sync-all):

		$ knife sync-all -c

Testing
-------

#### Install Test Kitchen

More info [here](https://github.com/opscode/test-kitchen).

1. `gem install bundler`
2. `bundle install --binstubs`
3. `bundle exec kitchen init`
4. `bundle exec kitchen test`

## Sync Roles
Add roles per-template:

		$ knife role from file /path/to/role.json
		
Or if you've installed [knife-sync-all](https://github.com/cdoughty77/knife-sync-all) one command will do:

		$ knife sync-all -r
		
## Apply Roles

### Checking Nodes
Check the state of a chef `node`: 

		knife node edit <nodename> -e vim

This will produce a json `node` file describing the current machine's attributes and run_list.

## Invoke chef-client
Here's where we actually get to cookin'.

To bootstrap a freshly imaged server:

		$ knife bootstrap 192.168.1.1 -x username -P PASSWORD --sudo

To force invoke chef-client after a successful bootstrap. Normally this should be accomplished by configuring chef-client as a daemon on the node.

		$ knife ssh 'name:nodeName' 'sudo chef-client' -x username -P PASSWORD --sudo
