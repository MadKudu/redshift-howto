# WLM

## See number of queries by WLM

```SQL
SELECT * FROM STV_WLM_SERVICE_CLASS_STATE;
```

## See what queue is what

```SQL
SELECT * FROM STV_WLM_SERVICE_CLASS_CONFIG
```

## See how long a given query has been queued

```SQL
SELECT * FROM STL_WLM_QUERY
```

## See queries that spend a lot of time waiting

```SQL
select trim(database) as DB , w.query,
substring(q.querytxt, 1, 100) as querytxt,  w.queue_start_time,
w.service_class as class, w.slot_count as slots,
w.total_queue_time/1000000 as queue_seconds,
w.total_exec_time/1000000 exec_seconds, (w.total_queue_time+w.total_Exec_time)/1000000 as total_seconds
from stl_wlm_query w
left join stl_query q on q.query = w.query and q.userid = w.userid
where w.queue_start_Time >= dateadd(day, -7, current_Date)
and w.total_queue_Time > 0  and w.userid >1
and q.starttime >= dateadd(day, -7, current_Date)
order by w.total_queue_time desc, w.queue_start_time desc limit 35;
```
https://github.com/awslabs/amazon-redshift-utils/blob/master/src/AdminScripts/queuing_queries.sql

## See average wait time per queue

```SQL
select
service_class,
final_state,
count(*),
avg(total_exec_time) / 1000000 AS avg_exec_time_s,
avg(total_queue_time) / 1000000 AS avg_queue_time_s,
percentile_cont(0.9) within group (order by total_queue_time) / 1000000 AS ninetieth_queue_time_s
from stl_wlm_query
where userid >= 100
-- and queue_start_time >= dateadd(hour, -1, current_date)
and queue_start_time >= dateadd(day, -1, current_date)
group by 1,2 order by 1,2;
```

## See max concurrency for a queue

https://github.com/awslabs/amazon-redshift-utils/blob/master/src/AdminScripts/wlm_apex.sql

# More resources

https://docs.aws.amazon.com/redshift/latest/dg/r_STL_WLM_QUERY.html
