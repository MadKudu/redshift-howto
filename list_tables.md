# List tables and columns

## Get list table + columns

### Option 1:

```SQL
SELECT   
      *
FROM
      information_schema.tables as tabl
INNER JOIN
      information_schema.columns as cols
      ON tabl.table_schema = cols.table_schema
      AND tabl.table_name = cols.table_name
WHERE
      tabl.table_name = 'contacts_stage'
      AND cols.column_name = 'domain';
```

### Option 2:

```SQL
SELECT
			pg_namespace.nspname AS schema_name,
			pg_class.relname AS table_name,
			pg_attribute.attname AS column_name,
			pg_type.typname AS type,
			pg_attribute.atttypmod AS column_len
FROM
			pg_class
INNER JOIN
			pg_namespace
			ON pg_class.relnamespace = pg_namespace.oid
INNER JOIN
			pg_attribute
			ON pg_class.oid = pg_attribute.attrelid
INNER JOIN
			pg_type
			ON pg_attribute.atttypid = pg_type.oid
WHERE
			pg_namespace.nspname = '{{schema_name}}'
			AND pg_class.relname = '{{table_name}}'
			AND pg_type.typname NOT IN ('oid','xid','tid','cid')
			AND pg_attribute.attnum >= 0
ORDER BY
			pg_attribute.attnum ASC
;
```

### Option 3:

```SQL
SELECT
      *
FROM
      pg_table_def
WHERE
      tablename = 'prototype_salesforce_lead_changes'
      AND schemaname = 'mk_3311_1';
```
