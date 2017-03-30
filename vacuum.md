# Vacuum

## Find tables that need Vacuum

```SQL
SELECT
    sti.schema AS schema_name,
    sti.table AS table_name,
    sti.size AS mb,
    sti.unsorted AS unsorted
FROM
    svv_table_info AS sti
WHERE
    sti.schema = 'schema'
    AND sti.size >= 100
    AND (sti.unsorted >= 50.0 OR sti.unsorted IS NULL)
ORDER BY
    sti.size DESC,
    sti.unsorted DESC,
    sti.tbl_rows DESC
;
```

(note: tune the values above)
