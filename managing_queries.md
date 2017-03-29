# Managing queries

## List longest running queries

```sql
SELECT
      DATEDIFF(minutes, starttime, GETDATE()) AS duration_minutes,
      *,
      'SELECT PG_CANCEL_BACKEND(' || pid || ');'
FROM
      STV_RECENTS
WHERE
      status = 'Running'
ORDER BY
      starttime ASC
;
```

## Kill a query by PID

```sql
SELECT PG_CANCEL_BACKEND(pid)
```

## Get pids of running queries for a given user

```sql
SELECT DISTINCT
      pid
FROM
      STV_RECENTS
WHERE
      status = 'Running'
      AND user_name = 'xxx'
;
