## SQL_Coding_Questions

This is a compilation of the coding questions (found on various websites including Stratascratch and Leetcode) that I have solved.

- [StrataScratch - Microsoft - Premium vs Freemium](https://platform.stratascratch.com/coding/10300-premium-vs-freemium?code_type=3)

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
