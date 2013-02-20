## Duck Soup Chef
An evolving simplest possible explanation of how Chef works for us.

### Roles

We specify **roles** for our server with json files in the `./roles` directory of our chef repo.

 A role consists only of **run lists** and **attributes**: 

+ **Run lists** are arrays of **recipes**. **Recipes** are defined in **cookbooks** which reside in the `./cookbooks` directory. We generally pull cookbooks from the community, and define our run lists with a selection of their recipes.

		# Example run_list for our node role
		...
		"run_list": [
    		"recipe[nodejs]",
    		"recipe[nodejs::npm]"
  		]
  		
  	`recipe[nodejs]` directs Chef to `/cookbooks/nodejs/recipes/default.rb`

+ **Attributes** are key-value data that describe the system state. Recipes supply **default attributes**, which can be overridden by role or node **override attributes**. Finally, **[automatic attributes](http://wiki.opscode.com/display/chef/Automatic+Attributes)** represent the state of the target machine during a chef run.

	In order of increasing precedence: 
	
	(lowest) default -> normal -> override -> automatic (highest)
	
	In our node role, we override the version of node to install (The cookbook's defaults are available in /cookbooks/name/attributes/default.rb):
	
		# Example override attributes for our node role
		...
		"override_attributes": {
		    "nodejs": {
		      "version": "0.8.20",
		      "npm": "1.2.11"
		    }
		  }

#### Example Node.js Role

		# /roles/nodejs.json
		# Example role which specifies installing node and npm
		{
		  "name": "nodejs",
		  "description": "Node and npm",
		  "json_class": "Chef::Role",
		  "default_attributes": {
		  },
		  "override_attributes": {
		    "nodejs": {
		      "version": "0.8.20",
		      "npm": "1.2.11"
		    }
		  },
		  "chef_type": "role",
		  "run_list": [
		    "recipe[nodejs]",
		    "recipe[nodejs::npm]"
		  ]
		}
