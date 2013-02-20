# Installing in Mac OS X
We're using [JewelryBox](http://jewelrybox.unfiniti.com/) to manage our ruby installation due to potential conflicts between ruby packages and the ruby build bundled with OSX.

## Preparing Ruby Environment

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
2. librarian-chef install --clean --verbose
3. Add the following to your knife.rb file
		
		require 'librarian/chef/integration/knife'
			cookbook_path Librarian::Chef.install_path,
			              "/path/to/chef-repo/site-cookbooks"


## Notes

#### If Ruby fails to build
  **Note**: If building halts with an error like that below, you need to install apple-gcc-4.2 with Homebrew.

		regparse.c:582:15: error: implicit conversion loses integer precision: 'st_index_t' (aka 'unsigned long') to 'int' [-Werror,-Wshorten-64-to-32]
		return t->num_entries;

**Remedy**:
	
    	$ brew tap homebrew/dupes
		$ brew install apple-gcc42
