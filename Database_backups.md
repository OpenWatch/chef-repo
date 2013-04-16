# Dumping

		sudo -u postgres pg_dump -Fc database_name > file
		

# Restoring

## 1. Decrypt

Assuming you've set up gpg with the appropriate private key.

		gpg -o output_file -d input_file

	
## 2. Decompress

		xz -d input_file
		

## 3. Restore
If restoring to a fresh database:
	
		sudo -u postgres pg_restore --create --no-owner -d database_name file

If restoring to a database with existing relations, use the --data-only flag:

		sudo -u postgres pg_restore --data-only -d database_name file