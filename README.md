## Introduction
A startup called Sparkify wants to analyze the data they've been collecting on songs and user activity on their new music streaming app. The analytics team is particularly interested in understanding what songs users are listening to. 

They'd like a data engineer to create a Postgres database with tables designed to optimize queries on song play analysis. The goal is to create a database schema and ETL pipeline for this analysis.

## Data
- **Song datasets** are the json files under /data/song_data, which include the information of songs
for example:
```
{"num_songs": 1, "artist_id": "ARJIE2Y1187B994AB7", "artist_latitude": null, "artist_longitude": null, "artist_location": "", "artist_name": "Line Renaud", "song_id": "SOUPIRU12A6D4FA1E1", "title": "Der Kleine Dompfaff", "duration": 152.92036, "year": 0}
```

- **log datasets** are the json files under /data/log_data, which are generated by [this event simulator](https://github.com/Interana/eventsim) based on the songs in the dataset above
For example:
```
{"artist":"Slipknot","auth":"Logged In","firstName":"Aiden","gender":"M","itemInSession":0,"lastName":"Ramirez","length":192.57424,"level":"paid","location":"New York-Newark-Jersey City, NY-NJ-PA","method":"PUT","page":"NextSong","registration":1540283578796.0,"sessionId":19,"song":"Opium Of The People (Album Version)","status":200,"ts":1541639510796,"userAgent":"\"Mozilla\/5.0 (Windows NT 6.1) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/36.0.1985.143 Safari\/537.36\"","userId":"20"}
```

## Database Schema
This whole database is using Star Schema which contains one fact table called songplays and four dimension tables called users, song, artists and time.

### Fact Table
- Songplays:  records in log data associated with song plays i.e. records with page NextSong
{songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent}

### Dimension Tables
- **users** - users in the app
{user_id, first_name, last_name, gender, level}
- **songs** - songs in music database
{song_id, title, artist_id, year, duration}
- **artists** - artists in music database
{artist_id, name, location, latitude, longitude}
- **time** - timestamps of records in songplays broken down into specific units
{start_time, hour, day, week, month, year, weekday}

### Benefits of Using Star Schema
- Queries run faster in Star Schema because it has small tables and clear schemas
- Easier for the analyst to understand each table and extract the data they need
- Reduce time to load the data

## Files
- **data** folder contains all the json files for the project
- **test.ipynb** displays the first few rows of each table to let you check your database.
- **create_tables.py** drops and creates your tables. You run this file to reset your tables before each time you run your ETL scripts.
- **etl.ipynb** reads and processes a single file from song_data and log_data and loads the data into your tables. This notebook contains detailed instructions on the ETL process for each of the tables.
- **etl.py** reads and processes files from song_data and log_data and loads them into your tables. You can fill this out based on your work in the ETL notebook.
- **sql_queries.py** contains all your sql queries, and is imported into the last three files above.
- **README.md** provides discussion on my project.

## Running the ETL
- Create the PostgreSQL database structure by
```
python create_tables.py
```
- Then load the datasets to the database by
```
python etl.py
```
