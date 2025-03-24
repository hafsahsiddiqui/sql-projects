# 📚 Books Tracker

I tried making a SQL database inspired by **Goodreads** where users can search books, read and write reviews, rate books, creating reading lists, track progress, and more

## Background

### Tools
- MySQL

Unique Features to Improve Upon Goodreads:

Personalized Recommendations – Use AI-based suggestions based on reading habits.

Book Clubs & Discussion Rooms – Users can create/join clubs with real-time discussions.

Custom Tags & Categories – Let users tag books for better organization.

Enhanced Progress Tracking – Users can track time spent reading, notes per chapter, and mood tracking.

Challenges & Achievements – Gamify reading with badges, streaks, and challenges.

Audiobook Integration – Allow users to track progress for audiobooks.

Book Trades & Borrowing – Connect readers for book swaps and lending.


### Build a Data Set

Users table
```
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    Bio TEXT,
    ProfilePictureURL VARCHAR(255)
);
```
Authors table
```
CREATE TABLE authors (
    author_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    bio TEXT,
    birth_date DATE,
    death_date DATE
);
```
-- Books table
```
CREATE TABLE books (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    author_id INT,
    publication_year YEAR,
    genre VARCHAR(100),
    description TEXT,
    cover_image_url VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (author_id) REFERENCES authors(author_id) ON DELETE SET NULL
);
```
-- Reviews table
```
CREATE TABLE reviews (
    review_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    book_id INT,
    review_text TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE
);
```
-- Ratings table
```
CREATE TABLE ratings (
    rating_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    book_id INT,
    rating INT CHECK (rating BETWEEN 1 AND 5),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE
);
```

-- Reading lists table
```
CREATE TABLE reading_lists (
    list_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE
);
```
-- Reading list items
```
CREATE TABLE reading_list_items (
    list_item_id INT AUTO_INCREMENT PRIMARY KEY,
    list_id INT,
    book_id INT,
    added_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (list_id) REFERENCES reading_lists(list_id) ON DELETE CASCADE,
    FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE
);
```
-- Progress tracking
```
CREATE TABLE reading_progress (
    progress_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT,
    book_id INT,
    current_page INT,
    total_pages INT,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (book_id) REFERENCES books(book_id) ON DELETE CASCADE
);
```

### Analysis with SQL Queries 
