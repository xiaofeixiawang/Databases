Q1  (1/1 point)
Find the titles of all movies directed by Steven Spielberg. 

select title
from movie
where director = 'Steven Spielberg';

Q2  (1/1 point)
Find all years that have a movie that received a rating of 4 or 5, and sort them in increasing order. 

select distinct year
from Movie,Rating
where stars>=4 and Movie.mID = Rating.mID
order by year;

Q3  (1/1 point)
Find the titles of all movies that have no ratings. 

select title
from movie
where mID not in (select mID from rating);

Q4  (1/1 point)
Some reviewers didn't provide a date with their rating. Find the names of all reviewers who have ratings with a NULL value for the date. 

select name
from reviewer,rating
where reviewer.rid=rating.rid and ratingdate is null;

Q5  (1/1 point)
Write a query to return the ratings data in a more readable format: reviewer name, movie title, stars, and ratingDate. Also, sort the data, first by reviewer name, then by movie title, and lastly by number of stars. 

select name,title,stars,ratingDate
from reviewer,movie,rating
where reviewer.rid=rating.rid and movie.mid=rating.mid
order by name,title,stars;

Q6  (1/1 point)
For all cases where the same reviewer rated the same movie twice and gave it a higher rating the second time, return the reviewer's name and the title of the movie. 

select name,title
from reviewer,movie,rating R1
where reviewer.rid=R1.rid and movie.mid=R1.mid 
    and R1.rid in (select rid from rating R2 where R1.rid=R2.rid and R1.mid=R2.mid and R1.ratingdate<R2.ratingdate and R1.stars<R2.stars); 

Q7  (1/1 point)
For each movie that has at least one rating, find the highest number of stars that movie received. Return the movie title and number of stars. Sort by movie title. 

select title, max(stars)
from movie join rating using(mid)
group by mid
order by title

Q8  (1/1 point)
For each movie, return the title and the 'rating spread', that is, the difference between highest and lowest ratings given to that movie. Sort by rating spread from highest to lowest, then by movie title. 

select title,(max(stars)-min(stars)) as ratingspread
from movie join rating using(mid)
group by movie.mid
order by ratingspread desc,title;

Q9  (1/1 point)
Find the difference between the average rating of movies released before 1980 and the average rating of movies released after 1980. (Make sure to calculate the average rating for each movie, then the average of those averages for movies before 1980 and movies after. Don't just calculate the overall average rating before and after 1980.) 

select
(select avg(a1.av1)
from (select (avg(stars)) as av1,mid
from rating
group by mid) as a1,movie
where year<1980 and movie.mid=a1.mid)-
(select avg(a1.av1)
from (select (avg(stars)) as av1,mid
from rating
group by mid) as a1,movie
where year>1980 and movie.mid=a1.mid)    