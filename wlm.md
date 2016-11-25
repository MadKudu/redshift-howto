# WLM

## See queries by WLM

```SQL
SELECT
      *
FROM
      STL_WLM_QUERY
WHERE
      service_class >= 6
ORDER BY
      queue_start_time DESC
LIMIT 100;
```
