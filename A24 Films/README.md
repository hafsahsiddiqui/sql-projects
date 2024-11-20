# A24 Films Analysis

I wanted to find a way to quickly explore, analyze, and gain insights about the filmography of A24 films such as ratings, directors, box office performance, and more

## Background

### Tools:
- SQLite3

### Data Set
When I was starting this project, I found an old public dataset from Kraggle. However, it was a little outdated so I went ahead and manually added all the films and their details from 2023-2025. I also deleted some columns that I wasn't interested in analyzing before using it to write SQL queries. 

1. Kraggle Dataset Link: 

2. My updated dataset:

## SQL Queries + Answers

### What movies have the highest imdb ratings?

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


