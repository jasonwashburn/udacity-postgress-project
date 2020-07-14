# Project: Data Modeling with Postgres

The purpose of this project is create a postgres database of song and user 
activity for Sparkify's new music streaming app.  This will allow them to 
analyze their data which currently resides in a directory of JSON logs and 
JSON metadata.

JSON log and song data files are parsed and added to tables in a postgres 
database to allow simple SQL queries for data analysis. 

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