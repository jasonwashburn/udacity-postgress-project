# Project: Data Modeling with Postgres

## Description
The purpose of this project is to create a postgres database of song and user 
activity for Sparkify's new music streaming app.  This will allow them to 
analyze their data which currently resides in a directory of JSON logs and 
JSON metadata.

JSON log and song data files are parsed and added to tables in a postgres 
database to allow simple SQL queries for data analysis. 

## Usage

### create_tables.py
This script must be run first. Connects to sparkifydb and drops any existing 
tables if they exist, then creates `songplay`, `artists`, `songs`, `time`, 
and `user` tables.

### etl.py
Running this script parses all log and song JSON files in data/.. then inserts 
the data into appropriate tables in the sparkifydb database.

### sql_queries.py
Contains SQL queries used by etl.py to create, drop, and insert data to 
sparkifydb. Does not need to be run by user.

## Database Design
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

    artists - contains details on artists from song files in ./data/song_data
        artist_id (text, PRIMARY KEY)
        name (text)
        location (text)
        latitude (float)
        longitude (float)

    time - contains a non-duplicate list of timestamps and converted time data
        start_time (bigint, PRIMARY KEY)
        hour (int)
        day (int)
        week (int)
        month (int)
        year (int)
        weekday (int)


## Example Queries
The following are some examples of queries that can now be easily performed for 
user and song play analysis.

### Top 5 Most Played Songs
`SELECT songplays.song_id, songs.title, COUNT(*) FROM songplays JOIN songs 
ON songplays.song_id = songs.song_id GROUP BY songplays.song_id, songs.title 
ORDER BY COUNT(*) DESC LIMIT 5`

    song_id	            title           count
    SOZCTXZ12AB0182364  Setanta matins  1
    **Note: Only 1 song is currently displayed due to limited sample size**

### Top 5 Most Used Browsers
`SELECT COUNT(*), user_agent FROM songplays GROUP BY user_agent ORDER BY 
COUNT(*) DESC LIMIT 5`

    count	user_agent
    971	    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/36.0.1985.143 Safari/537.36"
    708	    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_9_4) AppleWebKit/537.78.2 (KHTML, like Gecko) Version/7.0.6 Safari/537.78.2"
    696	    Mozilla/5.0 (Windows NT 5.1; rv:31.0) Gecko/20100101 Firefox/31.0
    577	    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/36.0.1985.125 Chrome/36.0.1985.125 Safari/537.36"
    573	    "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.94 Safari/537.36"

### Top 5 Busiest Hours of the Day by Song Plays
`SELECT time.hour, COUNT(*) FROM time JOIN songplays ON time.start_time = 
songplays.start_time GROUP BY time.hour ORDER BY COUNT(*) DESC LIMIT 5`

    hour	count
    16	    542
    18	    498
    17	    494
    15	    477
    14	    432

