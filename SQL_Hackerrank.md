## SQL Coding Challenges (HackerRank)

- [Advanced Select - The PADS (Medium)](https://www.hackerrank.com/challenges/the-pads/problem)

Code (MySQL):

```
select
  concat(Name, "(", left(Occupation,1), ")") as col
from
  OCCUPATIONS
order by
  col asc
;

select
  concat("There are a total of ", name, " ", occu, "s.")
from
(select
  count(Name) as name,
  lower(Occupation) as occu
from
  OCCUPATIONS
group by
  occu
order by
  name asc) as t
;
```
