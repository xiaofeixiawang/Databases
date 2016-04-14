Q1  (1/1 point)
For every situation where student A likes student B, but student B likes a different student C, return the names and grades of A, B, and C. 
```
select h1.name,h1.grade,h2.name,h2.grade,h3.name,h3.grade
from likes l1,likes l2,highschooler h1,highschooler h2,highschooler h3
where l1.id2=l2.id1 and not l1.id1=l2.id2 and h1.id=l1.id1 and h2.id=l1.id2 and h3.id=l2.id2
```
Q2  (1/1 point)
Find those students for whom all of their friends are in different grades from themselves. Return the students' names and grades. 
```
select h3.name,h3.grade
from friend f2,highschooler h3
where f2.id1=h3.id
except
select h1.name,h1.grade
from friend f1,highschooler h1,highschooler h2
where f1.id1=h1.id and f1.id2=h2.id and h2.grade=h1.grade;
```
Q3  (1/1 point)
What is the average number of friends per student? (Your result should be just one number.) 
```
select avg(c)
from(select count(f.id2) c
from friend f
group by f.id1) g;
```
Q4  (1/1 point)
Find the number of students who are either friends with Cassandra or are friends of friends of Cassandra. Do not count Cassandra, even though technically she is a friend of a friend. 
```
select count(distinct f1.id2)+count(distinct f2.id2)
from friend f1,friend f2,highschooler h
where f1.id1=h.id and f1.id2=f2.id1 and not f2.id2=f1.id1 and h.name='Cassandra'
```
Q5  (1/1 point)
Find the name and grade of the student(s) with the greatest number of friends. 
```
select h1.name,h1.grade
from (select max(g.c) m
from (select f1.id1,count(f1.id2) c
from friend f1
group by f1.id1) g) g2,
(select f1.id1,count(f1.id2) c
from friend f1
group by f1.id1) g, highschooler h1
where g.c=g2.m and g.id1=h1.id
;
```