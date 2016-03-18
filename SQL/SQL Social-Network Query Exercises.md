Q1  (1/1 point)
Find the names of all students who are friends with someone named Gabriel. 

select name
from highschooler,friend
where highschooler.id=friend.id1 and id2 in (select id from highschooler where
name='Gabriel');

Q2  (1/1 point)
For every student who likes someone 2 or more grades younger than themselves, return that student's name and grade, and the name and grade of the student they like. 

select h1.name,h1.grade,h2.name,h2.grade
from highschooler h1,highschooler h2,likes
where h1.id=likes.id1 and h2.id=likes.id2 and 
    (h1.grade-h2.grade)>=2;

Q3  (1/1 point)
For every pair of students who both like each other, return the name and grade of both students. Include each pair only once, with the two names in alphabetical order. 

select h1.name,h1.grade,h2.name,h2.grade
from (select l1.*
    from likes l1,likes l2
    where l1.id1=l2.id2 and l1.id2=l2.id1) g1,
    highschooler h1,highschooler h2
where g1.id1=h1.id and g1.id2=h2.id and h1.name<h2.name
order by h1.name;

Q4  (1/1 point)
Find all students who do not appear in the Likes table (as a student who likes or is liked) and return their names and grades. Sort by grade, then by name within each grade. 

select name,grade
from highschooler
where id not in (select id1
from likes
union
select id2
from likes)
order by grade,name;

Q5  (1/1 point)
For every situation where student A likes student B, but we have no information about whom B likes (that is, B does not appear as an ID1 in the Likes table), return A and B's names and grades. 

select h2.name,h2.grade,h1.name,h1.grade
from likes l2,highschooler h1,highschooler h2
where l2.id2 not in (select l1.id1 from likes l1) and l2.id2=h1.id
    and l2.id1=h2.id;

Q6  (1/1 point)
Find names and grades of students who only have friends in the same grade. Return the result sorted by grade, then by name within each grade. 

select h3.name,h3.grade
from highschooler h3
where h3.id not in (select distinct h1.id
       from friend f1,highschooler h1,highschooler h2
       where f1.id1=h1.id and f1.id2=h2.id and h1.grade<>h2.grade)
order by h3.grade,h3.name;

Q7  (1/1 point)
For each student A who likes a student B where the two are not friends, find if they have a friend C in common (who can introduce them!). For all such trios, return the name and grade of A, B, and C. 

select h1.name,h1.grade,h2.name,h2.grade,h3.name,h3.grade
from friend f1,friend f2,highschooler h1,highschooler h2,highschooler h3,
    (select id1,id2
    from likes
    except
    select id1,id2
    from friend) g
where g.id1=f1.id1 and g.id2=f2.id1 and f1.id2=f2.id2
        and h1.id=g.id1 and h2.id=g.id2 and h3.id=f1.id2;

Q8  (1/1 point)
Find the difference between the number of students in the school and the number of different first names. 

select count(name)-count(distinct name)
from highschooler;

Q9  (1/1 point)
Find the name and grade of all students who are liked by more than one other student. 

select name,grade
from (select count(id1) c,id2
from likes
group by id2) g,highschooler h1
where c>1 and id2=h1.id;            