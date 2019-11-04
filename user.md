# User management

## Create a user

to be completed

## Remove a user

### Remove its ownership

The user can be owner of various objects, including namespaces, tables, views, stored procedures and databases.

It can't get removed if it still owns anything, so here is how to generate SQL scripts to alter the remaining elements and change their ownership:

```sql
SELECT
	o.cmd
FROM (
	SELECT
		'ALTER SCHEMA ' || nspname || ' OWNER TO data_workers;' AS cmd,
		pg_get_userbyid(nspowner) as owner
	FROM pg_namespace

	UNION ALL

	SELECT
		'ALTER TABLE ' || schemaname || '.' || tablename || ' OWNER TO data_workers;' AS cmd,
		tableowner as owner
	FROM pg_tables

	UNION ALL

	SELECT
		'ALTER TABLE ' || schemaname || '.' || viewname || ' OWNER TO data_workers;' AS cmd,
		viewowner as owner
	FROM pg_views

	UNION ALL

	SELECT
		'ALTER PROCEDURE ' || proname || ' OWNER TO data_workers;' AS cmd,
		pg_get_userbyid(proowner) as owner
	FROM pg_proc

	UNION ALL

	SELECT
		'ALTER DATABASE ' || datname || ' OWNER TO data_workers;' AS cmd,
		pg_get_userbyid(datdba) as owner
	FROM pg_database
) o
WHERE o.owner = '{user to check}';
```

Given that you replace `{user to check}` by the user you're looking at, that SQL command will list namespaces, tables, views, stored procedures and databases alter commands owned by the user.

All the generated commands are meant to give the object ownership to the user `data_workers`. Feel free to change it if needed.

The resulting commands can be run as is.

### Delete the user

You can now remove the user:
```sql
DROP USER IF EXISTS {user_to_check};
```

If this fails with a Redshift error, refer to the next section.

### Issues

The user may own other elements. To find out what, run this query:
```sql
SELECT
    relname,
    relnamespace
FROM pg_class 
WHERE
    pg_get_userbyid(relowner) = '{user to check}';
```
