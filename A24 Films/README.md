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


## SQL Queries + Description

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
    WHEN budget < 1 THEN 'Under $1M'
    WHEN budget < 5 THEN '$1M–$5M'
    WHEN budget < 10 THEN '$5M–$10M'
    ELSE 'Over $10M'
  END AS budget_group,
  AVG(roi) AS avg_roi
FROM a24_movies
WHERE budget > 0
GROUP BY budget_group;
```

Description: Reveals which budget levels give A24 the best returns on investment. This informs whether to lean into micro-budget indies or gradually scale up to mid-range features.

c. High Budget, High ROI
```
SELECT title, budget, roi, genres
FROM a24_movies
WHERE budget > 5 AND roi > 3
ORDER BY roi DESC;
```
Description: Shows which high-investment films paid off the most

d. Low Budget, Low ROI

```
SELECT title, budget, roi, genres
FROM a24_movies
WHERE budget < 1 AND roi < 1
ORDER BY roi ASC;
```
Description: Identifies low-budget projects that underperformed—useful for avoiding similar pitfalls.

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

### 📈 5. Long-Term Trends

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
Description: Highlights genre production trends such as horror films increasing over time

## 📊 Tableau

After performing the SQL analysis, I imported the dataset into Tableau to create a dashboard that brings the insights to life. Instead of visualizing every query, I focused on the most impactful metrics and trends to help decision-makers quickly understand A24’s performance and strategic opportunities.

**Dashboard Highlights:**
- ROI Performance: showcases ROI across budget categories, star-attached films, and director experience levels, highlighting which projects deliver the highest profitability.
- Genre & Story Type: examines how different genres and story origins (original vs. adaptation) perform financially and critically, revealing audience preferences and creative trends.
- Star & Director Impact: analyzes the influence of star presence and director experience on both box office returns and critical reception
  
**Additional features Included in the Dashboard:**
- Films I have watched so far: A straightforward list of A24 movies I’ve personally watched, providing a personal touch to the analysis.
- A24 Background:
  - A brief description of A24’s origins and mission to set the context.
  - A short summary of A24’s Best Picture wins and Oscar nominations highlighting their critical recognition.
  - A small image offering a glimpse into the A24 film catalog.
  - A link to the official A24 website for further exploration.

🔗 [Link to the full dashboard](https://public.tableau.com/app/profile/hafsah.siddiqui/viz/A24_Films/Dashboard)

![Dashboard](https://github.com/user-attachments/assets/72d17d01-2a6e-48d7-a526-b6ba795edb28)


## Key Takeaways
1. A24’s box office revenue has generally increased over time (non-COVID years), but ROI has fluctuated.
2. Bigger budgets don’t always lead to better returns.
3. Horror films deliver consistently strong ROI for A24, but Comedies tend to be riskier with lower average ROI.
4. Original stories outperform adaptations in ROI.
5. Films without A-list actors tend to have higher ROI.
6. Top-performing films often come from the horror and drama genres.
7. Movies with budgets under $1 million yield the highest average ROI.
8. Low-budget indie films offer the best financial return.
9. Emerging directors can rival or outperform veterans in both ROI and critical reception.
10. Prioritizing originality and fresh talent pays off creatively and financially.

A24's highest ROI films tend to be original, low-budget, and genre-specific (especially horror and drama)
    
## Reflection

What I would do differently, or next steps:
1. **Handling Missing Data (NULL Values)**: one thing I would do differently is not manually adding information for each film because usually there’s missing data for important features, and there wouldn’t be enough time to update every column. I want to explore ways to handle missing data more efficiently.
2. **Expand on Analysis**: I want to add more visualizations that show deeper insights into critical reception, like highlighting movies where critics and audience opinions differ a lot or comparing critic acclaim directly with financial performance. I would also make the visualizations more interactive.
3. **Improve Data Formatting and Presentation**:I would keep financial figures like ROI, box office, and budget in their original units (e.g., millions) rather than decimals, to make the data more intuitive and accessible.

What are some problems I faced:
1. **Calculating Profit**: It was hard to calculate the profit because there are multiple things to consider such as production budget, marketing and distribution costs, theaters' share of box office revenue, internaional vs domestic revenue, and more.
2. **Multiple Output**: There are films with multiple genres and films where there are two directors working on it and one of them is newbie and the other is experienced. I struggled to figure out if I should just do the first main genre of each films or include both for my visualization even though it's the same film. I decided to go with the first one even though I feel like it's not 100% accurate on what kind of movie it's. For example, Everything Everywhere All At Once can be Action, Sci-Fi, Fantasy, Drama, and more.
3. **Data Formatting**: I converted ROI, box office, and budget values into decimal form where 1.0 represents 1 million, thinking it would make the data easier to work with. However, when creating the Tableau visualizations, I realized it would have been clearer to keep the numbers in full amounts (like 1 million) to avoid confusion for those without context.
