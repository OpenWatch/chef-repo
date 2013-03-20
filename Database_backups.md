# Dumping

		sudo -u postgres pg_dump -Fc database_name > file
		

# Restoring
	
If restoring to a fresh database:
	
		sudo -u postgres pg_restore -d database_name file

If restoring to a database with existing relations, use the --data-only flag:

		sudo -u postgres pg_restore --data-only -d database_name file