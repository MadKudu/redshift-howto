# Misc

## Convert Unix Epoch to DateTime in Redshift

```SQL
SELECT (TIMESTAMP 'epoch' +  epoch_ms_column:: BIGINT / 1000 * INTERVAL '1 Second ')  
```

## Convert timezone in Redshift

```SQL
SELECT listtime, convert_timezone('US/Pacific', listtime) FROM listing LIMIT 1
```

## Reloading data in a table with an identity column

```SQL
UNLOAD ('SELECT * FROM table_name')
TO 's3://bucket_name/file_key'
CREDENTIALS 'aws_access_key_id=xxx;aws_secret_access_key=xxx'
DELIMITER '~'
GZIP
ADDQUOTES;

COPY table_name_new ([columns])
FROM 's3://bucket_name/file_key'
CREDENTIALS 'aws_access_key_id=xxxx;aws_secret_access_key=xxx'
GZIP
REMOVEQUOTES
DELIMITER '~'
EMPTYASNULL
BLANKSASNULL
EXPLICIT_IDS;

DROP TABLE table_name;

ALTER TABLE table_name_new RENAME TO table_name;
```
