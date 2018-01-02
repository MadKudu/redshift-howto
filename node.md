## Historical Node Performance
```sql
SELECT
  iq.day_d,
  sl.node,
  SUM(iq.elapsed_ms) AS elapsed,
  SUM(iq.bytes) AS bytes
FROM (
  SELECT
    start_time::DATE AS day_d,
    slice,
    qr.query,
    segment,
    datediff('ms', MIN(start_time), MAX(CASE WHEN end_time = '2000-01-01 00:00:00' THEN endtime ELSE end_time END)) AS elapsed_ms,
    SUM(bytes) AS bytes
  FROM
    svl_query_report qr
  LEFT JOIN
    stl_query q
    ON q.query = qr.query
  GROUP BY
    1, 2, 3, 4
  ) iq
JOIN
  stv_slices AS sl
  ON sl.slice = iq.slice
GROUP BY
  1, 2
ORDER BY
  1 DESC, 3 DESC
;
```

## Commit Queue Performance by Node
```sql
SELECT
  node,
  SUM(DATEDIFF(MS, startwork, endlocal))
FROM
  stl_commit_stats
WHERE
  startwork > sysdate - 1
GROUP BY
  1
ORDER BY 1
;
```
