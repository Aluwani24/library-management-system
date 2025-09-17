# library-management-system

Step 1: Database Setup
- Login into Postgres pgAdmin
- Right click on Databases, and select create database
- Database name: LibraryDB

1.1 Create Tables
- Under databases, click on the DB you've created and select schemas
- Right click on tables, and open query tool
- Write the following queries to create table Books, Authors, and patrons

CREATE TABLE Books (
id SERIAL PRIMARY KEY NOT NULL,
title VARCHAR (255) NOT NULL,
author_id INT REFERENCES Authors(author_id),
genres TEXT[] DEFAULT ARRAY[]::TEXT[],
published_year INT CHECK (published_year > 0),
available BOOLEAN DEFAULT TRUE
);

CREATE TABLE Authors (
author_id SERIAL PRIMARY KEY NOT NULL,
name VARCHAR (100) NOT NULL,
nationality VARCHAR (100),
birth_year INT CHECK (birth_year > 0),
death_year INT CHECK (death_year > birth_year)
);

CREATE TABLE Patrons (
id SERIAL PRIMARY KEY NOT NULL,
name VARCHAR (100) NOT NULL,
email VARCHAR (100) NOT NULL,
borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);

Step 2: Insert Data
- write the following queries to insert records into tables:

INSERT INTO Books (title, author_id, genres, published_year, available) VALUES
    ('1984', 1, ARRAY['Dystopian', 'Political Fiction'], 1949, TRUE),
    ('To Kill a Mockingbird', 2, ARRAY['Southern Gothic', 'Bildungsroman'], 1960, TRUE),
    ('The Great Gatsby', 3, ARRAY['Tragedy'], 1925, TRUE),
    ('Brave New World', 4, ARRAY['Dystopian', 'Science Fiction'], 1932, TRUE),
    ('The Catcher in the Rye', 5, ARRAY['Realist Novel', 'Bildungsroman'], 1951, TRUE),
    ('Moby-Dick', 6, ARRAY['Adventure Fiction'], 1851, TRUE),
    ('Pride and Prejudice', 7, ARRAY['Romantic Novel'], 1813, TRUE),
    ('War and Peace', 8, ARRAY['Historical Novel'], 1869, TRUE),
    ('Crime and Punishment', 9, ARRAY['Philosophical Novel'], 1866, TRUE),
    ('The Hobbit', 10, ARRAY['Fantasy'], 1937, TRUE);


INSERT INTO Authors (name, nationality, birth_year, death_year) VALUES
    ('George Orwell', 'British', 1903, 1950),
    ('Harper Lee', 'American', 1926, 2016),
    ('F. Scott Fitzgerald', 'American', 1896, 1940),
    ('Aldous Huxley', 'British', 1894, 1963),
    ('J.D. Salinger', 'American', 1919, 2010),
    ('Herman Melville', 'American', 1819, 1891),
    ('Jane Austen', 'British', 1775, 1817),
    ('Leo Tolstoy', 'Russian', 1828, 1910),
    ('Fyodor Dostoevsky', 'Russian', 1821, 1881),
    ('J.R.R. Tolkien', 'British', 1892, 1973);

INSERT INTO Patrons (name, email, borrowed_books) VALUES
    ('Alice Johnson', 'alice@example.com', ARRAY[]::INT[]),
    ('Bob Smith', 'bob@example.com', ARRAY[1, 2]),
    ('Carol White', 'carol@example.com', ARRAY[]::INT[]),
    ('David Brown', 'david@example.com', ARRAY[3]),
    ('Eve Davis', 'eve@example.com', ARRAY[]::INT[]),
    ('Frank Moore', 'frank@example.com', ARRAY[4, 5]),
    ('Grace Miller', 'grace@example.com', ARRAY[]::INT[]),
    ('Hank Wilson', 'hank@example.com', ARRAY[6]),
    ('Ivy Taylor', 'ivy@example.com', ARRAY[]::INT[]),
    ('Jack Anderson', 'jack@example.com', ARRAY[7, 8]);

Step 3: Read Operations (Queries)
- Get all books:
  SELECT * FROM Books;

- Get books by title:
  SELECT title FROM Books;

- Get all books by a specific author:
  SELECT 
	b.id,
    b.title,
    b.genres,
    b.published_year,
    b.available
FROM Books b
INNER JOIN Authors a on b.author_id = a.author_id
WHERE a.name = 'Jane Austen';

- Get all available books:
  SELECT 
    id,
    title,
    author_id,
    genres,
    published_year
FROM 
    Books
WHERE 
    available = TRUE;

Step 4: Update Operations
- Mark a book as borrowed (set available = false):
  UPDATE Books
  SET available = FALSE
  WHERE title = 'War and Peace';

- Add a new genre to an existing book:
  UPDATE Books
  SET genres = genres || ARRAY['Documentary']
  WHERE title = 'War and Peace';

- Add a borrowed book to a patron’s record:
  UPDATE Patrons
  SET borrowed_books = borrowed_books || ARRAY [1]
  WHERE name = 'Alice Johnson';

Step 5: Delete Operations
- Delete a book by title:
  DELETE FROM Books
  WHERE title = 'Moby-Dick'

- Delete an author by ID: since there's a references you need to delete first the book.
  DELETE FROM Books
  WHERE author_id = 3;

  DELETE FROM Authors
  WHERE author_id = 3




