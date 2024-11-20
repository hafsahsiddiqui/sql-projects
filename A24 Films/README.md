# 🎞️  A24 Films Analysis

I enjoy watching movies. I started watching A24 films more recently such as Civil War, Hereditary, and Dream Scenario and wanted to know more about how A24 is doing. Therefore, I decided to use a dataset that allows me to quickly explore, analyze, and gain insights about the filmography of A24 films such as ratings, directors, box office performance, and more using SQL.

## Background

### Tools
- SQLite3
- Kraggle

### Data Set
When I was starting this project, I found an old public [dataset](https://www.kaggle.com/datasets/sebastiansuliborski/a24-studio-movies-dataset) from Kraggle. However, it was a little outdated so I went ahead and manually added all the films and their details from 2023-2025. I also deleted some columns that I wasn't interested in analyzing before using it to write SQL queries. 


[My Updated Dataset](https://docs.google.com/spreadsheets/d/19XDTzVPtD-lCmuN4hJW1MTJ2pu0ymlAF54sYuPQ7vqo/edit?usp=sharing)

### Tableau
I also made a Tableau dashboard after writing the SQL queries to showcase the results using visuals

[Link to Tableau]


## SQL Queries + Answers

### 1. How has the average box office revenue changed over the years?


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


### 2. Which directors have the highest average box office earnings?

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

<img width="431" alt="Screenshot 2024-11-20 at 2 38 53 PM" src="https://github.com/user-attachments/assets/d65522a0-bd20-419f-9339-42e19cb4091a">


### 5. What movies have the highest imdb ratings?

```
SELECT title, imdb
FROM movies
WHERE imdb IS NOT NULL
ORDER BY imdb DESC
LIMIT 10;
```


Answer: 

<img width="340" alt="Screenshot 2024-11-19 at 11 56 04 PM" src="https://github.com/user-attachments/assets/ac062397-46b9-48b2-b3f0-28fc628aea6e">

### Box Office vs. Budget Comparison

```
SELECT title, budget, box_office, 
       (box_office - budget) AS profit
FROM movies
WHERE box_office IS NOT NULL AND budget IS NOT NULL
ORDER BY profit DESC
LIMIT 10;
```

Answer:

<img width="549" alt="Screenshot 2024-11-19 at 11 58 53 PM" src="https://github.com/user-attachments/assets/103cf613-bcbd-4a37-bc40-9c24416f6201">

### Movies by Running Time

```
SELECT title, running_time
FROM movies
ORDER BY running_time DESC
LIMIT 10;
```
Answer:

<img width="393" alt="Screenshot 2024-11-20 at 12 02 44 AM" src="https://github.com/user-attachments/assets/79c05be8-ce81-463b-acc5-595a96a22119">


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





