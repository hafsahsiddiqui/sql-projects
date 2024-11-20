# 📚 Books Tracker

I tried making a SQL database inspired by **Goodreads** where users can search books, read and write reviews, rate books, creating reading lists, track progress, and more

## Background

### Tools
- MySQL

### Build a Data Set

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


### Analysis with SQL Queries 
