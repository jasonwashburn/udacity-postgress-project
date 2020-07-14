# Project: Data Modeling with Postgres

## Description ##
The purpose of this project is to create a postgres database of song and user 
activity for Sparkify's new music streaming app.  This will allow them to 
analyze their data which currently resides in a directory of JSON logs and 
JSON metadata.

JSON log and song data files are parsed and added to tables in a postgres 
database to allow simple SQL queries for data analysis. 

## Usage ##

### create_tables.py ###   
This script must be run first. Connects to sparkifydb and drops any existing 
tables if they exist, then creates `songplay`, `artists`, `songs`, `time`, 
and `user` tables.

### etl.py ###
Running this script parses all log and song JSON files in data/.. then inserts 
the data into appropriate tables in the sparkifydb database.

### sql_queries.py ###
Contains SQL queries used by etl.py to create, drop, and insert data to 
sparkifydb. Does not need to be run by user.

## Database Design ##
The database design schema consists of the following tables:

    songplays - contains a consolidated list of song play activity for analysis
        serial (serial, PRIMARY KEY)
        start_time (datetime bigint)
        user_id (int)
        level (text)
        song_id (text)
        artist_id (text)
        session_id (int)
        location (text)
        user_agent (text)

    users - contains data on sparkify users derived from log files in ./data/log_data
        user_id (int, PRIMARY KEY)
        first_name (text)
        last_name (text)
        gender (char)
        level (text)

    songs - contains details on songs from song files in ./data/song_data
        song_id (text, PRIMARY KEY)
        title (text)
        artist_id (text)
        year (int)
        duration (float)

    artists - constains details on artists from song files in ./data/song_data
        artist_id (text, PRIMARY KEY)
        name (text)
        location (text)
        latittude (float)
        longitude (float)

    time - contains a non-duplicate list of timestamps and converted time data
        start_time (timestamp, PRIMARY KEY)
        hour (int)
        day (int)
        week (int)
        month (int)
        year (int)
        weekday (int)
