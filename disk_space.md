# Managing disk space

# Overall cluster capacity

```SQL
SELECT
      SUM(used) / 1000 AS gb_used,
      SUM(capacity) / 1000 AS gb_capacity,
      SUM(used)::float / SUM(capacity) AS percentage_used
FROM
      stv_partitions
;
```

## Disk space by table
```SQL
SELECT
      trim(pgdb.datname) as database,
      trim(pgn.nspname) AS schema,
      trim(tr.name) AS table,
      ts.mbytes,
      ROUND(ts.mbytes::FLOAT / (SELECT count(*) AS mbytes FROM stv_blocklist)::FLOAT, 2)*100 AS pct,
      tr.rows
FROM
      (
        SELECT
              db_id, id, name, sum(rows) AS rows
        FROM
              stv_tbl_perm
        GROUP BY
              db_id, id, name
      ) AS tr
JOIN
      pg_class AS pgc
      ON pgc.oid = tr.id
JOIN
      pg_namespace AS pgn
      ON pgn.oid = pgc.relnamespace
      AND pgn.nspowner > 1
JOIN
      pg_database AS pgdb
      ON pgdb.oid = tr.db_id
JOIN
      (
      	SELECT
              tbl, count(*) AS mbytes
      	FROM
              stv_blocklist
      	GROUP BY
              tbl
      ) AS ts
      ON tr.id = ts.tbl
ORDER BY
      mbytes DESC, tr.db_id, tr.name
;
```

## Disk space by schema
```SQL
SELECT
      -- trim(pgdb.datname) as database,
      trim(pgn.nspname) AS schema,
      SUM(ts.mbytes) AS mbytes,
      SUM(tr.rows) AS rows
FROM
      (
        SELECT
              db_id, id, name, sum(rows) AS rows
        FROM
              stv_tbl_perm
        GROUP BY
              db_id, id, name
      ) AS tr
JOIN
      pg_class AS pgc
      ON pgc.oid = tr.id
JOIN
      pg_namespace AS pgn
      ON pgn.oid = pgc.relnamespace
      AND pgn.nspowner > 1
JOIN
      pg_database AS pgdb
      ON pgdb.oid = tr.db_id
JOIN
      (
      	SELECT
              tbl, count(*) AS mbytes
      	FROM
              stv_blocklist
      	GROUP BY
              tbl
      ) AS ts
      ON tr.id = ts.tbl
GROUP BY
      trim(pgn.nspname)
ORDER BY
      SUM(ts.mbytes) DESC
;
```
