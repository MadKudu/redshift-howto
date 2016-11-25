# Managing queries

## List longest running queries

```SQL
SELECT
      duration / (60 * 1000 * 1000) as duration_minutes,
      *
FROM
      STV_RECENTS
WHERE
      status = 'Running'
ORDER BY
      starttime ASC;
```

## Kill a query by PID

```SQL
SELECT PG_CANCEL_BACKEND([pid)
```
