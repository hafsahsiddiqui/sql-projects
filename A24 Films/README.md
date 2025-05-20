# 🎞️  A24 Films Analysis

## Project Overview

I have always enjoyed watching movies, and after joining Letterboxd, I started watching A24 films and wanted to know more about how A24 is doing. Therefore, I decided to put together a project that allows me to explore, analyze, and gain insights about the various aspects of the A24 films to better understand the studio's strategies. 

**Goal:** To explore the performance, trends, and key success factors behind A24 films by analyzing their financial returns, critical reception, and production patterns using SQL, Microsoft Excel, and Tableau.

**Tools Used:**
1. Microsoft Excel for data cleaning
2. SQL for querying and analysis
4. Tableau for visualization

## Background

When I first started this project, I discovered a public [dataset](https://www.kaggle.com/datasets/sebastiansuliborski/a24-studio-movies-dataset) on A24 films on Kaggle. The dataset was scraped from [Wikipedia](https://en.wikipedia.org/wiki/List_of_A24_films) and then modified and cleaned. While it served as a helpful foundation, it was outdated. To improve the accuracy and depth of my analysis, I manually updated the dataset by adding all A24 film releases from 2023 to 2025, including key details such as release dates, directors, genres, and box office figures. Additionally, I performed data cleaning by removing duplicates, standardizing formats, and dropping irrelevant or inconsistent columns to ensure a cleaner, more analysis-ready dataset.

**Key Data Cleaning and Prep:**
1. Renamed columns (example: changed "movie_title" to just "title")
2. Removed duplicates based on the title column to avoid repeated entries and insure data integrity
3. Manually added 2023-2025 films and data, including release date, directors, box office data, and more
4. Expanded the dataset to include new columns such as original_or_adaptation, target_age_group, roi, audience_critics_gap, star_attached, director_experience, and more
5. Deleted columns that I didn't need such as “Music by”, “Screenplay by”, “Based on”, “Edited By”, “Cinematography” and “Written by”
6. Standardized column formats and corrected inconsistencies (example: spacing, casing, null values)
7. I made sure all the columns I added are written SQL friendly 

[View Updated Dataset](https://docs.google.com/spreadsheets/d/19XDTzVPtD-lCmuN4hJW1MTJ2pu0ymlAF54sYuPQ7vqo/edit?usp=sharing)

## SQL Queries + Description on what it does

### 💵 1. Box Office Analysis

a. Which movies had the biggest financial success relative to their budget?  (Top 10 ROI Return) 

```
SELECT title, budget, box_office, roi * 100 AS roi_percentage
FROM a24_movies
ORDER BY roi_percentage DESC
LIMIT 10;
```

Description: Identifies the top 10 movies that generated the highest return on investment (ROI) relative to their budget. The result helps identify which films achieved the highest profitability.

b. Average ROI by Budget Group
```
SELECT
  CASE 
    WHEN budget < 1000000 THEN 'Under $1M'
    WHEN budget < 5000000 THEN '$1M–$5M'
    WHEN budget < 10000000 THEN '$5M–$10M'
    ELSE 'Over $10M'
  END AS budget_group,
  AVG(roi) AS avg_roi
FROM a24_movies
WHERE budget > 0
GROUP BY budget_group;
```

Description: Reveals which budget levels give A24 the best returns on investment. This informs whether to lean into micro-budget indies or gradually scale up to mid-range features.


### ⭐ 2. Star & Director Impact

a. Average ROI for Star-Attached vs Not

```
SELECT star_attached, AVG(roi) AS avg_roi, COUNT(*) AS film_count
FROM a24_movies
GROUP BY star_attached;
```
Description: Helps determine whether having a well-known actor actually pays off financially.

b. Performance by Director Experience

```
SELECT director_experience, AVG(roi) AS avg_roi, AVG(IMDb) AS avg_imdb, COUNT(*) AS film_count
FROM a24_movies
GROUP BY director_experience;
```
Description: Analyzes whether new or veteran directors perform better both commercially and critically.


### 🎭 3. Genre & Story Type

a. IMDb by Genre
```
SELECT genres, AVG(IMDb) AS avg_imdb, COUNT(*) AS film_count
FROM a24_movies
GROUP BY genres
ORDER BY avg_imdb DESC;
```

Description: Reveals which genres consistently perform best with audiences

b. ROI: Original vs Adaptation
```
SELECT original_or_adaptation, AVG(roi) AS avg_roi, COUNT(*) AS film_count
FROM a24_movies
GROUP BY original_or_adaptation;
```

Description: This query compares creative risks (originals) to familiar stories (adaptations) in terms of profitability


 c. Movies by Running Time

```
SELECT title, running_time, budget, box_office, roi
FROM a24_movies
ORDER BY running_time DESC
LIMIT 10;
```

Description: This query retrieves the longest films based on their running time and their profitability

d. Film Count by Genre and Setting
```
SELECT genres, setting, COUNT(*) AS film_count
FROM a24_movies
GROUP BY genres, setting
ORDER BY count DESC;
```
Description: This query groups films by both genre and setting, providing a count of how many films fall into each combination. This gives insight into which genre-settings are most popular.

e. Success by Target Group
```
SELECT target_age_group, AVG(roi) AS avg_roi
FROM a24_movies
GROUP BY target_age_group;
```
Description: This helps A24 tailor content and marketing—for example, focusing more on Gen Z stories if that audience drives the highest ROI.

### 🧠 4. Critical Reception 
a. Top Films by Critics-Audience Gap
```
SELECT title, audience_critics_gap, RTC, RTA
FROM a24_movies
ORDER BY ABS(audience_critics_gap) DESC
LIMIT 10;
```

Description: Spotlights polarizing films with a big disconnect between critics and audience

b. Correlation Proxy: Critics vs ROI

```
SELECT title, RTC, RTA, metacritic, roi
FROM a24_movies
ORDER BY roi DESC;
```

Description: Helps explore potential patterns between critical acclaim and financial success

c. What movies have the highest imdb ratings?

```
SELECT title, imdb
FROM a24_movies
WHERE imdb IS NOT NULL
ORDER BY imdb DESC
LIMIT 10;
```

Description: Ranks the highest-rated A24 films according to IMDb (a form of critical acclaim)

### 🗓️ 5. Long-Term Trends

a. How has the ROI has changed over the year?

```
SELECT year, AVG(roi) AS avg_roi
FROM a24_movies
GROUP BY year
ORDER BY year;
```

Description: Tracks whether A24’s financial performance is improving, plateauing, or declining over time

b. How has the average box office revenue changed over the years?
```
SELECT year, AVG(box_office) AS avg_box_office
FROM a24_movies
WHERE box_office IS NOT NULL
GROUP BY YEAR(release_date)
ORDER BY year DESC;
```

Description: Measures changes in average revenue year-over-year—can reflect marketing, trends, or shifts in quality.

c. Number of Films by Genre Over Time

```
SELECT year, genres, COUNT(*) AS film_count
FROM a24_movies
GROUP BY year, genres
ORDER BY year, num_films DESC;
```
Description: Highlights genre production trends—e.g., if horror has increased over time

a. High Budget, High ROI
```
SELECT title, budget, roi, genres
FROM a24_movies
WHERE budget > 5000000 AND roi > 3
ORDER BY roi DESC;
```
Description: Shows which high-investment films paid off the most

b. Low Budget, Low ROI

```
SELECT title, budget, roi, genres
FROM a24_movies
WHERE budget < 1000000 AND roi < 1
ORDER BY roi ASC;
```
Description: Identifies low-budget projects that underperformed—useful for avoiding similar pitfalls.

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
1. **NULL Values**: One thing I would do differently is not manually add information to each film because normally there is data missing for one of the important features and I want to see how I would navigate that.

What are some problems I faced:
1. **Calculating Profit**: It was hard to calculate the profit because there are multiple things to consider such as production budget, marketing and distribution costs, theaters' share of box office revenue, internaional vs domestic revenue, and more.
2. **Multiple Output**: There are films with multiple genres and films or there would be a film where there are two directors working on it and one of them is newbie and the other is experienced.
