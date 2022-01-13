# Aurora
* supported #database engines
	* #database/postgres
		* Aurora Postgres is 3x faster over regular Postgres
	* #database/mysql
		* Aurora MySQL is 5x faster over regular MySQL
* fully-managed service
	* high availability
	* fault tolerant
	* scales automatically
	* automatic backups

# DB snapshots
* AWS RDS allows sharing manual DB snapshots in the following ways
	* sharing a manual DB snapshot, whether encrypted or unencrypted, enables authorized AWS accounts to copy the snapshots
	* sharing an unencrypted manual DB snapshot enables authorized AWS accounts to directly restore a DB instance from the snbapshot instead of taking a copy of it and restoring from that
	* restoring DB instances from a DB snapshot that is both shared and encrypted does not work
		* make a copy of the DB snapshot
		* restore the DB instance from the copy

## Copying a DB snapshot
* copy manual or automated DB snapshots
	* after copying an automated snapshot, the copy is a manual copy
	* copy snapshot across AWS Regions
	* copy shared snapshots
* deleting a soure snapshot before the target snapshot becomes available, copying might fail
