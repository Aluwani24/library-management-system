# library-management-system

Step 1. 
- Login into Postgres pgAdmin
- Right click on Databases, and select create database
- Database name: LibraryDB

CREATE TABLES
- Under databases, click on the DB you've created and select schemas
- Right click on tables, and open query tool
- Write the following queries to create table Books, Authors, and patrons

CREATE TABLE Books (
id SERIAL PRIMARY KEY NOT NULL,
title VARCHAR (255) NOT NULL,
author_id INT,
genre VARCHAR (100) NOT NULL,
published_year INT,
available BOOLEAN
);

CREATE TABLE Books (
id SERIAL PRIMARY KEY NOT NULL,
title VARCHAR (255) NOT NULL,
author_id INT,
genre VARCHAR (100) NOT NULL,
published_year INT,
available BOOLEAN
);

CREATE TABLE Patrons (
id SERIAL PRIMARY KEY NOT NULL,
name VARCHAR (100) NOT NULL,
email VARCHAR (100) NOT NULL,
borrowed_books INT[] DEFAULT ARRAY[]::INT[]
);



