Deploying
=========

When you're ready to push your changes to the Chef Server.


1. Ensure you've created `/data-bags/user-passwords/passwords.json`
2. Create data bags if they don't yet exist on the Chef Server
	
		$ knife data bag create users
		$ knife data bag create user-passwords
		
3. Sync data bags with the Chef Server. This only needs to be done once per each data bag change.

		$ knife data bag from file users username.json
		    # repeat for each user
		$ knife data bag from file user-passwords passwords.json --secret-file /path/to/encrypted-data-bag-secret

4. Upload all repo cookbooks to the Chef Server:

		knife cookbook upload --all

5. TO BE CONTINUED...