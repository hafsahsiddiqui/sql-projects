# 📚 Books Tracker

I tried making a SQL database inspired by **Goodreads** where users can search books, read and write reviews, rate books, creating reading lists, track progress, and more

## Background

### Tools
- MySQL

### Data Set


### Tableau
I made a Tableau dashboard after writing the SQL queries to showcase the results using visuals

[Link to Tableau]


## SQL Queries 

-- Users Table
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Username VARCHAR(50) NOT NULL UNIQUE,
    MyEmail VARCHAR(100) NOT NULL UNIQUE,
    MyPassword VARCHAR(255) NOT NULL,
    DateJoined TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    Bio TEXT,
    ProfilePictureURL VARCHAR(255)
);
