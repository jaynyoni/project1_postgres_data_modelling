# Nanodegree project 1: Data Modeling with Postgres

Topics in this Readme:

[Summary](#Summary)

[Installing](#Installing)

[How to Run The Script](#Running)

[Database Schema Design](#DatabaseSchemaDesign)

[File Descriptions](#FileDescriptions)




## Summary 
This project is part of the Udacity Data Engineering Nanodegree. The project is an ETL that collects songs and 
user activity information for a streaming service called Sparkify. This data resides in the app in json logs for both
the songs and user activity. This project transforms this data and loads it into a Postgres database. Once the data is
in the Postgres database it is further transformed into a final table called songplays which is easier to query and get 
analytics from.

## Installing
1. Create a Python virtual environment and activate it.
    1. If you do not have virtualenv install it

    ```pip3 install virtualenv```

    1. Initialise your virtual environment

    ```bash

    virtualenv -p python3 venv
    source venv/bin/activate

    ```

  1. Install the package and all of its requirements.

      ```
      pip3 install requirements.txt
      ```

## Running

1. In your virtualenv run run below code to create the database, table and scripts needed
    ```
      python create_tables.py
     ```
2. Once the tables and database are created you can load the .json files with below command
    ```
   python etl.py
    ```
3. Once the above has successfully completed processing the files you can then use the `test.ipynb` jupyter notebook to
   query the data.

## DatabaseSchemaDesign

### Purpose 

The purpose of the project is to be able to show what songs users are listening to, this is achieved in the `songplays` table
This fact table makes it easy to analyze the data through basic SQL statements. The fact table is in a star-schema model 
with several other dimensional tables feeding into it. Updates in the dimensions will also be easily captured in the fact table.

### Schema

   **Fact Table**

   songplays
   ```
         CREATE TABLE songplays (
         songplay_id serial,
         start_time timestamp references time(start_time),
         user_id int references users(user_id),
         level text,
         song_id text references songs(song_id),
         artist_id text references artists(artist_id),
         session_id int,
         location text,
         user_agent text,
         PRIMARY KEY (songplay_id));
   ```

   **Dimension Tables**

  users
  ```
         CREATE TABLE users (
         user_id int,
         first_name text,
         last_name text,
         gender text,
         level text,
         PRIMARY KEY (user_id));
  ```

 songs
 ```
         CREATE TABLE songs (
         song_id text,
         title text,
         artist_id text,
         year int,
         duration double precision,
         PRIMARY KEY (song_id));
 ```

 artists
```
         CREATE TABLE artists (
         artist_id text,
         name text,
         location text,
         latitude double precision,
         longitude double precision,
         PRIMARY KEY (artist_id));
```
 time
```
         CREATE TABLE time (
         start_time timestamp,
         hour int,
         day int,
         week_of_year int,
         month int,
         year int,
         weekday int,
         PRIMARY KEY (start_time));
``` 

## FileDescriptions
This project requires python version 3.9x to work. The explanation of the files in the project

**`create_tables.py`**
Python script that uses `sql_queries.py` queries to drop any existing database tables. Create new tables and sparkifydb.

**`etl.py`**
This is the main ETL script for the project. It reads the data from the data/ folder and inserts it into the sparkifydb.

**`sql_queries.py`**
This is the package that contains the SQL queries that are used for creating, inserting, updating and selecting tables in
   the sparkifydb.
   
**`etl.ipynb`**
This is a jupyter notebook used as a guide/test before creating the `etl.py` script. Can also be used to check modifications.

**`test.ipynb`**
This is a jupyter notebook used to query the tables that are loaded by `etl.py` or `etl.ipynb` during development.

**`data/`**
The folder with song information and user log information, all in .json format.

**`requirements.txt`**
This file contains the packages required to run the scripts in the python virtualenv.