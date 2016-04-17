Q1  (1/1 point)
Write a trigger that makes new students named 'Friendly' automatically like everyone else in their grade. That is, after the trigger runs, we should have ('Friendly', A) in the Likes table for every other Highschooler A in the same grade as 'Friendly'.
```
create trigger Test
after insert on Highschooler
for each row
when (new.name='Friendly')
begin
    insert into Likes select new.id, id from Highschooler where new.grade=grade and new.id<>id;
end; 
```
Q2  (1/1 point)
Write one or more triggers to manage the grade attribute of new Highschoolers. If the inserted tuple has a value less than 9 or greater than 12, change the value to NULL. On the other hand, if the inserted tuple has a null value for grade, change it to 9. 
```
create trigger GradeChange
after insert on Highschooler
for each row
when (new.grade<9 or new.grade>12)
begin
update Highschooler set grade=NULL where id=new.id;
end;
|
create trigger GradeChange2
after insert on Highschooler
for each row
when (new.grade is NULL)
begin
update Highschooler set grade=9 where id=new.id;
end;
```
Q3  (1/1 point)
Write one or more triggers to maintain symmetry in friend relationships. Specifically, if (A,B) is deleted from Friend, then (B,A) should be deleted too. If (A,B) is inserted into Friend then (B,A) should be inserted too. Don't worry about updates to the Friend table. 
```
create trigger t1
after insert on Friend
for each row
when not exists(select id1,id2 from Friend where id1=new.id2 and id2=new.id1)
begin
    insert into Friend values(new.id2,new.id1);
end; 
|
create trigger t2
after delete on Friend
for each row
when exists (select id1,id2 from Friend where id1=old.id2 and id2=old.id1)
begin
    delete from Friend where id1=old.id2 and id2=old.id1;
end; 
```
Q4  (1/1 point)
Write a trigger that automatically deletes students when they graduate, i.e., when their grade is updated to exceed 12. 
```
create trigger t1
after update of grade on Highschooler
for each row
when new.grade>12
begin
    delete from Highschooler where id=new.id;
end; 
```
Q5  (1/1 point)
Write a trigger that automatically deletes students when they graduate, i.e., when their grade is updated to exceed 12 (same as Question 4). In addition, write a trigger so when a student is moved ahead one grade, then so are all of his or her friends. 
```
create trigger t1
after update of grade on Highschooler
for each row
when new.grade>12
begin
    delete from Highschooler where id=new.id;
end;
|
create trigger t2
after update of grade on Highschooler
for each row
when new.grade=old.grade+1
begin
    update Highschooler set grade=grade+1 where id in (select id1 from Friend where id2=new.id);
end;
```
Q6  (1/1 point)
Write a trigger to enforce the following behavior: If A liked B but is updated to A liking C instead, and B and C were friends, make B and C no longer friends. Don't forget to delete the friendship in both directions, and make sure the trigger only runs when the "liked" (ID2) person is changed but the "liking" (ID1) person is not changed. 
```
create trigger t1
after update of id2 on Likes
for each row
when old.id1=new.id1
begin
    delete from Friend where (id1=old.id2 and id2=new.id2) or (id2=old.id2 and id1=new.id2);
end; 
```