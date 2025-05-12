# 🎞️  A24 Films Analysis

## Project Overview

I enjoy watching movies. I started watching A24 films more recently after downloading Letterboxd and wanted to know more about how A24 is doing. Therefore, I decided to use a dataset that allows me to quickly explore, analyze, and gain insights about the various aspects of the A24 films to better understand studio's strategies. 

**Goal:** To explore the performance, trends, and key success factors behind A24 films by analyzing their financial returns, critical reception, and production patterns using SQL, Microsoft Excel, and Tableau.

**Tools Used:**
1. SQL (SQLite/PostgreSQL/MySQL) for querying and analysis
2. Microsoft Excel for data cleaning and extended analysis
3. Tableau for visualization

## Background

When I began this project, I discovered a public [dataset](https://www.kaggle.com/datasets/sebastiansuliborski/a24-studio-movies-dataset) on A24 films on Kaggle. The dataset was scraped from [Wikipedia](https://en.wikipedia.org/wiki/List_of_A24_films) and then modified and cleaned. While it served as a helpful foundation, it was outdated. To improve the accuracy and depth of my analysis, I manually updated the dataset by adding all A24 film releases from 2023 to 2025, including key details such as release dates, directors, genres, and box office figures. Additionally, I performed data cleaning by removing duplicates, standardizing formats, and dropping irrelevant or inconsistent columns to ensure a cleaner, more analysis-ready dataset.

**Key Data Cleaning and Prep:**
1. Removed duplicates based on the movie_title column to ensure data integrity
2. I manually added 2023-2025 missing films and data, including release date, directors, box office data, and more
3. I included columns such as title, running_time, directed_by, genres, original_or_adaptation, target_age_group, budget, box_office, roi, year, RTC, RTA, audience_critics_gap, metacritic, IMDb, setting, star_attached, director_experience, and more to give a better analysis on what people tend to like more
4. I deleted columns that I didn't need such as “Music by”, “Screenplay by”, “Based on”, “Edited By”, “Cinematography” and “Written by”
5. Standardized column formats and corrected inconsistencies (e.g., spacing, casing, null values)
6. I made sure all the columns I added are written SQL friendly 

[View Updated Dataset](https://docs.google.com/spreadsheets/d/19XDTzVPtD-lCmuN4hJW1MTJ2pu0ymlAF54sYuPQ7vqo/edit?usp=sharing)

## SQL Queries + Answers

### 💵 1. Financial Analysis

a. Which movies had the biggest financial success relative to their budget?

```
SELECT title, budget, box_office, roi * 100 AS roi_percentage
FROM a24_movies
ORDER BY roi_percentage DESC
LIMIT 10;
```

Answer:

b. Avg Box Office Based on Year

```

```

Answer:


### ⭐ 2. Star & Director Impact

a. Average ROI for Star-Attached vs Not

```
SELECT star_attached, AVG(roi) AS avg_roi, COUNT(*) AS film_count
FROM a24_movies
GROUP BY star_attached;
```

b. Performance by Director Experience

```
SELECT director_experience, AVG(roi) AS avg_roi, AVG(IMDb) AS avg_imdb, COUNT(*) AS film_count
FROM a24_movies
GROUP BY director_experience;
```

c. Which directors have the highest average box office earnings?

```
SELECT directed_by, 
       AVG(box_office) AS avg_box_office
FROM movies
WHERE box_office IS NOT NULL
GROUP BY directed_by
ORDER BY avg_box_office DESC
LIMIT 10;
```
Answer:


### 🎭 3. Genre & Story Type

a. IMDb by Genre
```
SELECT genres, AVG(IMDb) AS avg_imdb, COUNT(*) AS count
FROM a24_movies
GROUP BY genres
ORDER BY avg_imdb DESC;
```
b. ROI: Original vs Adaptation
```
SELECT original_or_adaptation, AVG(roi) AS avg_roi, COUNT(*) AS count
FROM a24_movies
GROUP BY original_or_adaptation;
```

 c. Movies by Running Time

```
SELECT title, running_time
FROM movies
ORDER BY running_time DESC
LIMIT 10;
```
Answer:

<img width="393" alt="Screenshot 2024-11-20 at 12 02 44 AM" src="https://github.com/user-attachments/assets/79c05be8-ce81-463b-acc5-595a96a22119">

### 🧠 4. Audience vs Critics
a. Top Films by Critics-Audience Gap
```
SELECT title, audience_critics_gap, RTC, RTA
FROM a24_movies
ORDER BY ABS(audience_critics_gap) DESC
LIMIT 10;
```
b. Correlation Proxy: Critics vs ROI
(Note: This won’t calculate correlation in SQL, but shows comparison)

```
SELECT title, RTC, RTA, metacritic, roi
FROM a24_movies
ORDER BY roi DESC;
```
c. What movies have the highest imdb ratings?

```
SELECT title, imdb
FROM movies
WHERE imdb IS NOT NULL
ORDER BY imdb DESC
LIMIT 10;
```

Answer: 

<img width="340" alt="Screenshot 2024-11-19 at 11 56 04 PM" src="https://github.com/user-attachments/assets/ac062397-46b9-48b2-b3f0-28fc628aea6e">

### Movie Count by Rotten Tomatoes Rating

```
SELECT CASE 
           WHEN rotten_tomatoes BETWEEN 0 AND 59 THEN '0-59'
           WHEN rotten_tomatoes BETWEEN 60 AND 79 THEN '60-79'
           WHEN rotten_tomatoes BETWEEN 80 AND 100 THEN '80-100'
           ELSE 'No Rating'
       END AS rating_range,
       COUNT(title) AS movie_count
FROM movies
GROUP BY rating_range
ORDER BY movie_count DESC;
```

Answer:

<img width="237" alt="Screenshot 2024-11-20 at 12 02 22 AM" src="https://github.com/user-attachments/assets/ca0c7807-e407-4c5b-b4ff-c0d585dc766f">

### 🗓️ 5. Temporal Trends
a. Movies per Year
```
SELECT year, COUNT(*) AS films_released
FROM a24_movies
GROUP BY year
ORDER BY year;
```
b. Average ROI per Year
```
SELECT year, AVG(roi) AS avg_roi
FROM a24_movies
GROUP BY year
ORDER BY year;
```

c. How has the average box office revenue changed over the years?
```
SELECT strftime('%Y', release_date) AS year, 
       AVG(box_office) AS avg_box_office
FROM movies
WHERE box_office IS NOT NULL
GROUP BY year
ORDER BY year DESC;
```

Answer:

<img width="218" alt="Screenshot 2024-11-20 at 2 35 34 PM" src="https://github.com/user-attachments/assets/224eb70c-4cb6-48b1-a28d-603f33e0c3dc">

### 🌍 6. Genre and Setting Trends

a. Film Count by Genre and Setting
```
SELECT genres, setting, COUNT(*) AS count
FROM a24_movies
GROUP BY genres, setting
ORDER BY count DESC;
```

### 🧩 7. Simulated Cluster Segments (Proxy)
To simulate segmentation manually, you could try:

a. High Budget, High ROI
```
SELECT title, budget, roi, genres
FROM a24_movies
WHERE budget > 5000000 AND roi > 3
ORDER BY roi DESC;
```

b. Low Budget, Low ROI

```
SELECT title, budget, roi, genres
FROM a24_movies
WHERE budget < 1000000 AND roi < 1
ORDER BY roi ASC;
```


## Tableau
I also made a Tableau dashboard after writing the SQL queries to showcase the results using visuals

[Link to Tableau]


## Key Takeaways
1. A24's highest ROI films tend to be original, low-budget, and genre-specific (especially horror and psychological thrillers).
2. Directors with prior experience generally produce higher box office returns.
3. Star-attached films show a modest but consistent uplift in performance.
4. The audience-critic gap can be predictive of cult status or streaming longevity.
5. There’s a clear upward trend in A24’s critical reception and box office over time (pre-pandemic).
6. Drama dominates — Over 80 titles fall into this category, but Horror and Coming-of-Age show the highest ROI swings.
7. A24’s sweet spot: High creative risk, low financial risk (Moonlight, The Witch, Lady Bird).

## Reflection

What you'd do differently, or next steps (e.g., predictive modeling, sentiment analysis).

What are some problems I faced:
1. **Calculating Profit**: It was hard to calculate the profit because there are multiple things to consider such as production budget, marketing and distribution costs, theaters' share of box office revenue, internaional vs domestic revenue, and more.

