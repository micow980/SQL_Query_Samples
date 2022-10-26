# SQL_Query_Samples
This is an ongoing file where I will be adding SQL Queries I have written for specific Data Science questions involving tech aggregation. I will try to encapsolate the main SQL Emphasis above the question to show the thought process behind the query.

# Conditional Formatting then Group By 
## Find departments with less than 5 employees. Output the department along with the corresponding number of workers.
select 
    department,
    count(worker_id) as employee
from worker
where 2 <5
group by 1

# Utilizing Unions to stack data
## Find employees in the HR department and output the result with one duplicate. Output the first name and the department of employees.

select 
    first_name
    department
from worker
where department = "HR"
UNION ALL
select 
    first_name
    department
from worker
where department = "HR";

# CTE/Temp Table to Dense Rank
## Find artists with the highest number of top 10 ranked songs over the years. Output the artist along with the corresponding number of top 10 rankings.

with temp as (select
artist,
dense_rank() over (order by count(distinct trackname)) as top_10_songs
from (spotify_worldwide_daily_song_ranking)
where position <= 10
group by 1
order by 2 DESC)

select
artist, 
top_10_songs
from temp
where top_10_songs = (select max(top_10_songs) from temp)

# Dinstinct Variable
## Find all the songs that were top-ranked (at first position) at least once in the past 20 years.

select 
distinct(song_name)
from billboard_top_100_year_end
where year >= 2002 and year_rank = 1

# Joining two datasets 
## You have a table with US rankings of tracks and another table with worldwide rankings of tracks.
## Find the number of days a US track has stayed in the 1st position for both the US and worldwide rankings. Output the track name and the number of days in the 1st position. Order your output alphabetically by track name.

SELECT a.trackname, count(a.date) as num_days1
FROM spotify_worldwide_daily_song_ranking as a 
LEFT JOIN spotify_daily_rankings_2017_us as b
ON a.trackname = b.trackname AND a.date = b.date
WHERE a.position = 1 and b.position = 1
GROUP by a.trackname
ORDER by num_days1

