Project: Data Modeling with Postgres
-------------------------------------
Summary:
At present, Sparkify does not have an easy way to query songs and user activity data which they have been collecting using their new music streaming app. The collected data is being stored in a directory as JSON logs, as well as a directory with JSON metadata on the songs in their app. Sparkify would like to create a Postgres database with tables designed to optimize queries on song play analysis. The main goals are 

1) To create and define fact and dimension tables for a star schema 
2) To create an ETL pipeline that transfers data from files in two local directories into the tables in Postgres.


Database Schema Design
------------------------
Fact Table: 
    songplays - records in log data associated with song plays i.e. records with page NextSong
    (Schema: songplay_id, start_time, user_id, level, song_id, artist_id, session_id, location, user_agent)

Dimension Tables:
    1) users - users in the app
       (user_id, first_name, last_name, gender, level)
    2) songs - songs in music database
       (song_id, title, artist_id, year, duration)
    3) artists - artists in music database
       (artist_id, name, location, latitude, longitude)
    4) time - timestamps of records in songplays broken down into specific units
       (start_time, hour, day, week, month, year, weekday)

Justification: The database was designed using Star schema because Star schema simplifies queries and can be used to perform fast aggregations/analytics


 Explanations of Files and ETL Pipeline
 ----------------------------------------
 1) sql_queries.py: Contains all sql queries --> including queries to create and drop tables, queries to insert records in the tables
 2) create_tables.py: Drops and creates the tables --> Run this file to reset tables before running ETL script
 3) etl.py: Reads and processes files from song_data and log_data --> Loads data into respective tables
 
 Note: test.ipynb & etl.ipynb --> Notebooks used for development of the final scripts


Example queries (Result from Analysis)
---------------------------------------
Example 1: 
    QUERY: SELECT songs.title, songs.year, songs.duration FROM songplays JOIN songs ON songs.song_id=songplays.song_id WHERE songs.duration>200
    OUTPUT: ('Setanta matins', 0, Decimal('269.58322'))

Example 2:
    QUERY: SELECT songs.title, songs.year, songs.duration, artists.name FROM (songplays JOIN songs ON songs.song_id=songplays.song_id) JOIN artists ON songplays.artist_id=artists.artist_id
    OUTPUT: ('Setanta matins', 0, Decimal('269.58322'), 'Elena')
