```
SELECT DISTINCT
      dependent_ns.nspname as dependent_schema,
      dependent_view.relname as dependent_view,
      source_ns.nspname as source_schema,
      source_table.relname as source_table
FROM pg_depend
JOIN pg_rewrite ON pg_depend.objid = pg_rewrite.oid
JOIN pg_class as dependent_view ON pg_rewrite.ev_class = dependent_view.oid
JOIN pg_class as source_table ON pg_depend.refobjid = source_table.oid
JOIN pg_attribute ON pg_depend.refobjid = pg_attribute.attrelid
    AND pg_depend.refobjsubid = pg_attribute.attnum
JOIN pg_namespace dependent_ns ON dependent_ns.oid = dependent_view.relnamespace
JOIN pg_namespace source_ns ON source_ns.oid = source_table.relnamespace
WHERE
source_ns.nspname = 'mk_3313_1'
AND source_table.relname = 'contacts_all'
AND pg_attribute.attnum > 0
ORDER BY 1,2;
```
