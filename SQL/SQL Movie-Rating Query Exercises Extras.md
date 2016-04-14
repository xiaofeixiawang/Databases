Q1  (1/1 point)
Find the names of all reviewers who rated Gone with the Wind. 
```
select distinct name
from (reviewer join rating using(rid)) join movie using(mid)
where title='Gone with the Wind'
```
Q2  (1/1 point)
For any rating where the reviewer is the same as the director of the movie, return the reviewer name, movie title, and number of stars. 
```
select name,title,stars
from (reviewer join rating using(rid)) join movie using(mid)
where name=director;
```
Q3  (1/1 point)
Return all reviewer names and movie names together in a single list, alphabetized. (Sorting by the first name of the reviewer and first word in the title is fine; no need for special processing on last names or removing "The".) 
```
select name
from reviewer
union
select title
from movie
```
Q4  (1/1 point)
Find the titles of all movies not reviewed by Chris Jackson. 
```
select title
from movie
except
select distinct title
from (rating join reviewer using(rid)) join movie using(mid)
where name = 'Chris Jackson';
```
Q5  (1/1 point)
For all pairs of reviewers such that both reviewers gave a rating to the same movie, return the names of both reviewers. Eliminate duplicates, don't pair reviewers with themselves, and include each pair only once. For each pair, return the names in the pair in alphabetical order. 
```
select distinct G2.name,G1.name
from (select * from reviewer join rating using(rid)) G1,
    (select * from reviewer join rating using(rid)) G2
where G1.mid=G2.mid and G1.name>G2.name;
```
Q6  (1/1 point)
For each rating that is the lowest (fewest stars) currently in the database, return the reviewer name, movie title, and number of stars. 
```
select name,title,stars
from (rating R1 join reviewer using(rid)) join movie using(mid)
where R1.stars not in (select R1.stars from rating R2 where R2.stars<R1.stars);
```
Q7  (1/1 point)
List movie titles and average ratings, from highest-rated to lowest-rated. If two or more movies have the same average rating, list them in alphabetical order. 
```
select title,avg(stars) av
from movie join rating using(mid)
group by mid
order by av desc,title;
```
Q8  (1/1 point)
Find the names of all reviewers who have contributed three or more ratings. (As an extra challenge, try writing the query without HAVING or without COUNT.) 
```
select name
from (select name,count(*) C
from reviewer join rating using(rid)
group by rid) G
where C>=3;
```
Q9  (1/1 point)
Some directors directed more than one movie. For all such directors, return the titles of all movies directed by them, along with the director name. Sort by director name, then movie title. (As an extra challenge, try writing the query both with and without COUNT.) 
```
select title,director
from (select director,count(mid) C
from movie
group by director) G join movie using(director)
where C>1;
```
Q10  (1/1 point)
Find the movie(s) with the highest average rating. Return the movie title(s) and average rating. (Hint: This query is more difficult to write in SQLite than other systems; you might think of it as finding the highest average rating and then choosing the movie(s) with that average rating.) 
```
select title, avgRating
from (select mID, avg(stars) as avgRating
from Rating
group by mID) join Movie using(mID)
where not exists (select avgRating from (select avg(stars) as avgTest from Rating group by mID) where avgRating < avgTest)
```
Q11  (1/1 point)
Find the movie(s) with the lowest average rating. Return the movie title(s) and average rating. (Hint: This query may be more difficult to write in SQLite than other systems; you might think of it as finding the lowest average rating and then choosing the movie(s) with that average rating.) 
```
select title,A1
from (select mid,avg(stars) A1
    from rating
    group by mid) join movie using(mid)
where not exists (select A1
                    from (select avg(stars) A2
                         from rating
                         group by mid)
                    where A2<A1);
```
Q12  (1/1 point)
For each director, return the director's name together with the title(s) of the movie(s) they directed that received the highest rating among all of their movies, and the value of that rating. Ignore movies whose director is NULL. 
```
select director,title,G.stars
from(select *
from rating R1
where not exists (select R1.stars from rating R2 where R1.mid=R2.mid and R2.stars>R1.stars)
group by R1.mid) G,movie
where G.mid=movie.mid and director<>'NULL'
group by director;     
```               