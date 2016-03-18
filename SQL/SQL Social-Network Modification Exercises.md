Q1  (1/1 point)
It's time for the seniors to graduate. Remove all 12th graders from Highschooler. 

delete from highschooler
where grade=12;

Q2  (1/1 point)
If two students A and B are friends, and A likes B but not vice-versa, remove the Likes tuple. 

delete from likes
where id1 in (select likes.id1 from friend,likes where
    friend.id1=likes.id1 and friend.id2=likes.id2
    and (likes.id2 in 
    (select l2.id1 from likes l2
    where l2.id1=likes.id2 and l2.id2<>likes.id1)
    or likes.id2 not in 
    (select l3.id1 from likes l3))
    )
;

Q3  (1/1 point)
For all cases where A is friends with B, and B is friends with C, add a new friendship for the pair A and C. Do not add duplicate friendships, friendships that already exist, or friendships with oneself. (This one is a bit challenging; congratulations if you get it right.) 

insert into friend
select distinct f1.id1,f2.id2
    from friend f1,friend f2
    where f2.id1=f1.id2 and f2.id2<>f1.id1
except
select * from friend
;