# Performance

https://docs.aws.amazon.com/redshift/latest/dg/diagnostic-queries-for-query-tuning.html

## Compression and encoding

## Check encoding / keys of a table

```SQL
SELECT
      "column",
      type,
      encoding,
      distkey,
      sortkey
FROM
      pg_table_def
WHERE
      tablename = 'xxx';
```
