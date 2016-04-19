Q1  (1/1 point)
Write an instead-of trigger that enables updates to the title attribute of view LateRating. 
```
create trigger UpdateLateRating
instead of update of title on LateRating
for each row
when new.mid=old.mid
begin
    update Movie
    set title=new.title
    where mid=old.mid;
end;
```
Q2  (1/1 point)
Write an instead-of trigger that enables updates to the stars attribute of view LateRating. 
```
create trigger UpdateLateRating
instead of update of stars on LateRating
for each row
when new.mid=old.mid and old.ratingdate=new.ratingdate
begin
    update Rating
    set stars=new.stars
    where mid=old.mid and ratingdate=old.ratingdate;
end;
```
Q3  (1/1 point)
Write an instead-of trigger that enables updates to the mID attribute of view LateRating. 
```
create trigger updatelaterating
instead of update of mid on laterating
for each row
begin
    update Movie
    set mid=new.mid
    where mid=old.mid;
    update Rating
    set mid=new.mid
    where mid=old.mid;
end;
```
Q4  (1/1 point)
Finally, write a single instead-of trigger that combines all three of the previous triggers to enable simultaneous updates to attributes mID, title, and/or stars in view LateRating. Combine the view-update policies of the three previous problems, with the exception that mID may now be updated. Make sure the ratingDate attribute of view LateRating has not also been updated -- if it has been updated, don't make any changes.
```
create trigger updatelaterating
instead of update of mid,title,stars on laterating
for each row 
when new.ratingdate=old.ratingdate
begin
    update movie set title=new.title where mid=old.mid;
    update rating set stars=new.stars where mid=old.mid and ratingdate=old.ratingdate;
    update movie set mid=new.mid where mid=old.mid;
    update rating set mid=new.mid where mid=old.mid;
end;
```
Q5  (1/1 point)
Write an instead-of trigger that enables deletions from view HighlyRated. 
```
create trigger delhighlyrated
instead of delete on highlyrated
for each row
begin
    delete from rating
    where mid=old.mid and stars>3;
end;
```
Q6  (1/1 point)
Write an instead-of trigger that enables deletions from view HighlyRated. 
```
create trigger delhighlyrated
instead of delete on highlyrated
for each row
begin
    update rating set stars=3 where mid=old.mid and stars>3;
end;
```
Q7  (1/1 point)
Write an instead-of trigger that enables insertions into view HighlyRated. 
```
create trigger inshighlyrated
instead of insert on highlyrated
for each row
when new.mid in (select mid from movie where title=new.title and mid=new.mid)
begin
    insert into rating values(201,new.mid,5,null);
end;
```
Q8  (1/1 point)
Write an instead-of trigger that enables insertions into view NoRating. 
```
create trigger insnorating
instead of insert on norating
for each row
when new.mid in (select mid from movie where mid=new.mid and title=new.title)
begin
    delete from rating where mid=new.mid;
end;
```
Q9  (1/1 point)
Write an instead-of trigger that enables deletions from view NoRating. 
```
create trigger delnorating
instead of delete on norating
for each row
begin
    delete from movie where mid=old.mid;
end;
```
Q10  (1/1 point)
Write an instead-of trigger that enables deletions from view NoRating. 
```
create trigger delnorating
instead of delete on norating
for each row
begin
    insert into rating values(201,old.mid,1,NULL);
end;
```