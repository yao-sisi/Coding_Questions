## SQL_Coding_Questions

This is a compilation of the coding questions (found on various websites including Stratascratch and Leetcode) that I have solved.

- [StrataScratch - Microsoft - Premium vs Freemium (Hard)](https://platform.stratascratch.com/coding/10300-premium-vs-freemium?code_type=3)

Code (MySQL):

```
-- pull date from ms_download_facts, add col acc_status (paying/non-paying)
-- add col user_id and col download_cnt
-- from this table, select date, non-paying downloads, paying downloads

select
  date,
  sum(case when acc_status='no' then download_cnt else null end) as non_paying_downloads,
  sum(case when acc_status='yes' then download_cnt else null end) as paying_downloads
from
(select
  df.date,
  ad.paying_customer as acc_status,
  df.user_id,
  sum(df.downloads) as download_cnt
from
  ms_download_facts df
  inner join
  ms_user_dimension ud
  on
  df.user_id=ud.user_id
  inner join
  ms_acc_dimension ad
  on
  ud.acc_id=ad.acc_id
group by
  date, 
  acc_status,
  user_id) as t
group by date  
having sum(case when acc_status='no' then download_cnt else null end) > sum(case when acc_status='yes' then download_cnt else null end)
order by date asc  
;
```

- [StrataScratch - DoorDash - Workers with the Highest Salaries (Medium)](https://platform.stratascratch.com/coding/10353-workers-with-the-highest-salaries?code_type=3)

Code (MySQL):

```
select
  title.worker_title
from 
  title
  inner join
  worker
  on
  title.worker_ref_id = worker.worker_id
where
  worker.salary = (select MAX(worker.salary) from worker)
;
```

- [StrataScratch - Meta/Facebook - Users by Average Session Time (Medium)](https://platform.stratascratch.com/coding/10352-users-by-avg-session-time?code_type=3)

Code (MySQL):

```
select
  page_exit.user_id,
  avg(abs(timestampdiff(second, first_page_exit, last_page_load))) as avg
from
(select 
  user_id,
  date(timestamp) as date,
  min(timestamp) as first_page_exit
from
  facebook_web_log
where
  action = 'page_exit'
group by
  user_id,
  date(timestamp)
) as page_exit
inner join
(select 
  user_id,
  date(timestamp) as date,
  max(timestamp) as last_page_load
from
  facebook_web_log
where
  action = 'page_load'
group by
  user_id,
  date(timestamp)
) as page_load
on 
page_exit.user_id = page_load.user_id and (page_exit.date = page_load.date)
group by
page_load.user_id
;
```
