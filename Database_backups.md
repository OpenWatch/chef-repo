# Dumping

		sudo -u postgres pg_dump -Fc database_name > file
		

# Restoring
	
		sudo -u postgres pg_restore -d database_name file