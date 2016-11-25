# Managing locks

## See current locks

```SQL

SELECT
      t.schema,
      t.table AS table_locked,
      l.lock_owner_start_ts,
      l.lock_owner_end_ts,
      l.lock_status,
      r.user_name AS lock_owner,
      l.lock_owner_pid,
      r.query AS locking_query
FROM
      STV_LOCKS AS l
INNER JOIN
      SVV_TABLE_INFO AS t
      ON l.table_id = t.table_id
INNER JOIN
      STV_RECENTS AS r
      ON l.lock_owner_pid = r.pid
ORDER BY
      t.schema,
      t.table
;
```
