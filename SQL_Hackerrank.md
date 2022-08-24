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

- [Advanced Select - Occupations (Medium)](https://www.hackerrank.com/challenges/occupations/problem?h_r=next-challenge&h_v=legacy&isFullScreen=true)

Code (MySQL):

```
-- 1.filter records by occupation, add row number col to each table ranking records asc
-- 2.pull all four tables and see how many records are in each in order to decide which joins to use (so that all records are included)

select
  dname,
  pname,
  sname,
  aname
from
(select
  Name as dname, row_number() over(order by Name asc) as rnk
from
  OCCUPATIONS
where
  Occupation='Doctor') as d
  right join
(select
  Name as pname, row_number() over(order by Name asc) as rnk
from
  OCCUPATIONS
where
  Occupation='Professor') as p
  on d.rnk=p.rnk
  left join
(select
  Name as sname, row_number() over(order by Name asc) as rnk
from
  OCCUPATIONS
where
  Occupation='Singer') as s
  on p.rnk=s.rnk
  left join
(select
  Name as aname, row_number() over(order by Name asc) as rnk
from
  OCCUPATIONS
where
  Occupation='Actor') as a
  on s.rnk=a.rnk
;
```

- [Advanced Select - Binery Tree Nodes (Medium)](https://www.hackerrank.com/challenges/binary-search-tree-1/problem?isFullScreen=true)

Code (MySQL):

```
/*
Root: when a node has no parent - when N = x, P is null
Inner: when a node has both parent and children - x appears in both col N and P
Leaf: when a node has no children - x does not appear in col P
*/


select
  N,
  case when P is null then 'Root'  
       when N in (select P from BST) then "Inner" 
       else 'Leaf' end as T
from 
  BST
order by
  N asc
;
```

