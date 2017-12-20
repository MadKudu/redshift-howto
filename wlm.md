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

## See average wait time per queue

```SQL
select service_class as svc_class, count(*),
avg(datediff(seconds, queue_start_time, queue_end_time)) as avg_queue_time,
max(datediff(seconds, queue_start_time, queue_end_time)) as max_queue_time,
sum(datediff(seconds, queue_start_time, queue_end_time)) as sum_queue_time,
avg(datediff(seconds, exec_start_time, exec_end_time )) as avg_exec_time
from stl_wlm_query
where service_class > 4
and queue_start_Time >= dateadd(day, -7, current_Date)
group by service_class
order by service_class;
```

# More resources

https://docs.aws.amazon.com/redshift/latest/dg/r_STL_WLM_QUERY.html
