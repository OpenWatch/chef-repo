# Installing in Mac OS X

## Checkin' out
To easily clone the repo with all submodules:

		git clone git@github.com:OpenWatch/chef-repo.git --recursive
		
To update all submodules after the initial clone.

		git submodule update --init --recursive

## Preparing Ruby Environment

We're using [JewelryBox](http://jewelrybox.unfiniti.com/) to manage our ruby installation due to potential conflicts between ruby packages and the ruby build bundled with OSX.

1. Download [JewelryBox](http://jewelrybox.unfiniti.com/)

1.  Within JewelryBox, install [rvm](https://rvm.io/)
This is a virtual environment manager for ruby

1. Under the 'Add Ruby' tab, select the most recent 'pxxx' release. Click 'Install'. See Notes if your build fails.

1. Check that the following has been added to your `~/.bash_profile`:

		source $HOME/.rvm/scripts/rvm

1. In JewelryBox, select the 'Manage Rubies' tab, and then the build you just installed. The first 'Options' pane should include a 'Set As Default' Button. Click it.

	+ Check that this worked with `ruby --version`

## Install Chef

1. `gem install chef`
2. `cd /chef-repo`

## Bundle Project Secrets

1. `mkdir .chef`
2. Copy the project secrets into `/chef-repo/.chef`:

	+ Chef User private key (username.pem)
	+ Knife configuration file (knife.rb)
	+ Chef Organization private key (organization.pem)

## Setup Librarian

Librarian helps tracks the cookbooks and dependencies via a `Cheffile`. Read more about Librarian [here](https://github.com/applicationsonline/librarian).

1. If you don't have it already: `$ gem install librarian`
2. Ensure the cookbooks directory is empty
2. From your Chef repo's directory, run `librarian-chef install --clean --verbose`
    + **Note**: This will delete the contents of `/cookbooks`
3. Add the following to your knife.rb file
		
		require 'librarian/chef/integration/knife'
			cookbook_path Librarian::Chef.install_path,
			              "/path/to/chef-repo/site-cookbooks"

	+**Note**: Librarian recommends in-development cookbooks be stored in `/site-cookbooks` rather than being managed by Librarian.

5. Upload cookbooks to Chef Server

		$ knife cookbook upload --all

## Installing FoodCritic
[FoodCritic](http://acrmp.github.com/foodcritic/) is a cookbook linter that requires Ruby 1.9.2+

		$ gem install foodcritic
		
To criticize a cookbook invoke:

		$ foodcritic /path/to/cookbook
		

## Notes

#### Developing Cookbooks

When developing cookbooks, be sure to pass a path command to knife to ensure they don't end up in `/cookbooks` to be overwritten by Librarian.

		$ knife cookbook create new_cookbook -o site-cookbooks/

#### Adding a cookbook or LWRP to Librarian

1. Add your new resource to the `Cheffile`

		# Cheffile
		...
		cookbook "chef-ssh-wrapper",
  			:git => "https://github.com/OpenWatch/chef-git_ssh_wrapper.git"

2. Call the librarian

		librarian-chef install

#### If Ruby fails to build
  **Note**: If building halts with an error like that below, you need to install apple-gcc-4.2 with Homebrew.

		regparse.c:582:15: error: implicit conversion loses integer precision: 'st_index_t' (aka 'unsigned long') to 'int' [-Werror,-Wshorten-64-to-32]
		return t->num_entries;

**Remedy**:
	
    	$ brew tap homebrew/dupes
		$ brew install apple-gcc42
