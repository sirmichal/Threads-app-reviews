# :bar_chart: Threads App Review Analysis Project
Threads App has been a quite a rockstar whith the high number of users registered early after the release. Lets explore a collection of SQL queries which are designed to unearth valuable insights from a dataset of almost 37,000 users reviews.

## ⭐: What is the average rating of the app all time?
```SQL
select 
count(rating) as Number_of_ratings, 
avg(rating) as Average_rating
from dbo.reviews
```
Answer: Average rating is 3 and we have 36943 ratings from users.


## 📂: What are the sourcers of the reviews?
```SQL
Select Distinct source
from dbo.Reviews
```
Answer: There are two sources of the reviews, App Store and Google Play.

## 🏪: What is the average rating of app per store?
For the purpose of this question, let's divide our dataset into temporary tables. 
### First the the reviews from App Store.
Let's create temporary table with App Store reviews.
```SQL
DROP TABLE IF EXISTS #temp_App_Store_reviews
CREATE TABLE #temp_App_Store_reviews(
Rating_App_Store int
)

INSERT INTO #temp_App_Store_reviews
SELECT rating FROM Reviews
WHERE source = 'App Store'
```
Now, we can query the average rating of Threads app in App Store and the number of reviews available.
```SQL
SELECT count(Rating_App_Store) as Number_of_reviews_App_Store, avg(Rating_App_Store) as Average_rating_App_Store
FROM #temp_App_Store_reviews
```
Answer: There are 2000 reviews with average rating of 2
### Reviews from Google Play
Let's create temporary table with Google Play reviews.
``SQL
ROP TABLE IF EXISTS #temp_Google_Play_reviews
CREATE TABLE #temp_Google_Play_reviews(
Rating_Google_Play int
)

INSERT INTO #temp_Google_Play_reviews
SELECT rating FROM Reviews
WHERE source = 'Google Play'
```
Now, we can query the average rating of Threads app in Google Play and the number of reviews available.
```SQL
SELECT count(Rating_Google_Play) as Number_of_reviews_Google_Play, avg(Rating_Google_Play) as Average_rating_Google_Play
FROM #temp_Google_Play_reviews
```
Answer: There are 34943 reviews in Google Play with average rating of 3.
## :chart_with_upwards_trend: Average Rating Over Time
By calculating the average rating for each date, we can observe how user perceptions of the app have evolved. 
```SQL
SELECT
    MONTH(review_date) AS month,
	DAY(review_date) AS day,
    COUNT(*) AS review_count,
    AVG(rating) AS avg_rating
FROM
    reviews
GROUP BY
    MONTH(review_date), DAY(review_date)
ORDER BY
    month, day
```
Answer: Observation which we can make is that over the time the number of reviews and rating has been in descending trend. 
## ⚖️:Number of reviews from the range of 2 and 4.
We would like to see the number of ratings from the range of 2 and 4 and additionaly compare it to number of other ratings. To see such information we need to use the below query.
```SQL
SELECT
	CASE 
		WHEN rating BETWEEN 2 AND 4 THEN 'rating range 2-4'
		ELSE 'other range'
	END AS Rating_range,
	COUNT(*) AS Number_of_ratings
FROM reviews
GROUP BY 
	CASE 
		WHEN rating BETWEEN 2 AND 4 THEN 'rating range 2-4'
		ELSE 'other range'
	END
```
Answer: There are 8661 ratings of the App from range between 2 and 4. 

## Percentage of Reviews with Comments/titles:
Calculate the percentage of reviews that have comments or text, helping you understand how many users provide additional feedback.
