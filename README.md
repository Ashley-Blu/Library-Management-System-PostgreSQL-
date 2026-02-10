![Project Preview](https://socialify.git.ci/Ashley-Blu/Library-Management-System-PostgreSQL-/image?language=1&owner=1&name=1&stargazers=1&theme=Light)

# Library Management System (PostgreSQL)

---

## Project Overview

The Library Management System manages books, authors, and patrons.  
It allows librarians to:

- Add, view, update, and delete records.
- Track which books are available or borrowed.
- Perform advanced queries such as filtering and searching.

---

## Database Setup - open SQL Shell(psql)

### Step 1: Create the Database

```sql
CREATE DATABASE LibraryDB;
```

---

### Step 2: Connect to the Database

In pgAdmin 4:

- Open Query Tool
- Choose LibraryDB as your active database

## Table Creation

### Open query tool and run the commands:

CREATE TABLE authors (
id SERIAL PRIMARY KEY,
name VARCHAR(100),
nationality VARCHAR(50),
birth_year INT,
death_year INT
);

CREATE TABLE books (
id SERIAL PRIMARY KEY,
title VARCHAR(100),
author_id INT REFERENCES authors(id) ON DELETE CASCADE,
genres VARCHAR[],
published_year INT,
available BOOLEAN
);

CREATE TABLE patrons (
id SERIAL PRIMARY KEY,
name VARCHAR(100),
email VARCHAR(100),
borrowed_books INT[]
);

---

## Sample Data Insertion

### Insert Authors

INSERT INTO authors (id, name, nationality, birth_year, death_year) VALUES
(1, 'George Orwell', 'British', 1903, 1950),
(2, 'Harper Lee', 'American', 1926, 2016),
(3, 'F. Scott Fitzgerald', 'American', 1896, 1940),
(4, 'Aldous Huxley', 'British', 1894, 1963),
(5, 'J.D. Salinger', 'American', 1919, 2010),
(6, 'Herman Melville', 'American', 1819, 1891),
(7, 'Jane Austen', 'British', 1775, 1817),
(8, 'Leo Tolstoy', 'Russian', 1828, 1910),
(9, 'Fyodor Dostoevsky', 'Russian', 1821, 1881),
(10, 'J.R.R. Tolkien', 'British', 1892, 1973);

### Insert Books

INSERT INTO books (id, title, author_id, genres, published_year, available) VALUES
(1, '1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
(2, 'To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
(3, 'The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
(4, 'Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
(5, 'The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
(6, 'Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
(7, 'Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
(8, 'War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
(9, 'Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
(10, 'The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);

### Insert Patrons

INSERT INTO patrons (id, name, email, borrowed_books) VALUES
(1, 'Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
(2, 'Bob Smith', 'bob@example.com', ARRAY[1, 2]),
(3, 'Carol White', 'carol@example.com', ARRAY[]::INT[]),
(4, 'David Brown', 'david@example.com', ARRAY[3]),
(5, 'Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
(6, 'Frank Moore', 'frank@example.com', ARRAY[4, 5]),
(7, 'Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
(8, 'Hank Wilson', 'hank@example.com', ARRAY[6]),
(9, 'Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
(10, 'Jack Anderson', 'jack@example.com', ARRAY[7, 8]);

---

## Queries

### Get All Books

SELECT * FROM books;

### Get a Book by Title

SELECT * FROM books WHERE LOWER(title) = LOWER('1984');

### Get All Books by a Specific Author

SELECT b.title, a.name
FROM books b
JOIN authors a ON b.author_id = a.id
WHERE LOWER(a.name) = LOWER('George Orwell');

### Get All Available Books

SELECT * FROM books WHERE available = TRUE;

---

## Update Operations

### Mark a Book as Borrowed

UPDATE books
SET available = FALSE
WHERE LOWER(title) = LOWER('1984');

### Add a New Genre to an Existing Book

UPDATE books
SET genres = array_append(genres, 'Classic')
WHERE LOWER(title) = LOWER('1984');

### Add a Borrowed Book to a Patron’s Record

UPDATE patrons
SET borrowed_books = array_append(borrowed_books, 1)
WHERE LOWER(name) = LOWER('Alice Johnson');

---

## Delete Operations

### Delete a Book by Title

DELETE FROM books WHERE LOWER(title) = LOWER('The Great Gatsby');

### Delete an Author by ID

DELETE FROM authors WHERE id = 5;

---

## Advanced Queries

### Find books published after 1950

SELECT * FROM books WHERE published_year > 1950;

### Find all American authors

SELECT * FROM authors WHERE nationality = 'American';

### Set all books as available

UPDATE books SET available = TRUE;

### Find books that are available and published after 1950

SELECT * FROM books WHERE available = TRUE AND published_year > 1950;

### Find authors whose names contain 'George'

SELECT * FROM authors WHERE name ILIKE '%George%';

### Increment published year 1869 by 1

UPDATE books SET published_year = published_year + 1 WHERE published_year = 1869;

---

## Running Queries in pgAdmin 4

1. Open pgAdmin 4
2. Connect to your PostgreSQL server
3. Create or open your LibraryDB database
4. Click Query Tool (from the top menu)
5. Copy and paste the SQL commands above
6. Press Execute at the top to run the commands
7. View results in the Data Output panel at the bottom
