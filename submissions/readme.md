# *Onramp x Vanguard Spotify Project*

<img src="https://github.com/rjpost20/vanguard_de_project_Ryan_Posternak/blob/main/images/pexels-vishnu-r-nair-1105666.jpg?raw=true">
Image by <a href="https://www.pexels.com/@vishnurnair/" >Vishnu R Nair</a> on <a href="https://www.pexels.com/photo/people-at-concert-1105666/" >Pexels.com</a>

<br>
<br>

## *By Ryan Posternak*

<br>

## Overview

This project consists of four steps — ingestion, transformation, storage, and analytics of Spotify data. With the help of the Spotipy library, albums, tracks, and track features from 20 artists in total were obtained and analyzed.

The database for this project consists of four tables, the schema for which is presented below.

<img width="964" alt="Screen Shot 2022-09-29 at 11 57 37 AM" src="https://user-images.githubusercontent.com/105675055/193080528-e3dc6fa7-272f-40a3-b564-e411199eec70.png">

### Table of Contents:

**Notebook 1: Data Ingestion**
* Step 1: Data Ingestion
  * Establish Connection to Spotify's API
  * Part 1: Artists Dataframe
  * Part 2: Albums Dataframe
  * Part 3: Tracks Dataframe
  * Part 4: Track Features Dataframe

**Notebook 2: Data Transformation and Storage**
* Step 2: Data Transformation
  * Part 1: Handling Null/Missing Values
  * Part 2: Deduplication
    * 2.1: Remove duplicate albums
    * 2.2: Remove duplicate songs
* Step 3: Storage
  * Part 1: artist Table
  * Part 2: album Table
  * Part 3: track Table
  * Part 4: track_feature Table

**Notebook 3: Data Analytics and Visualizations**
* Step 4: Analytics / Visualizations
  * Part 1: SQL Query Analytics
    * 1.1: Top songs by artist in terms of `duration_ms`
    * 1.2: Top artists in the database by number of `followers`
    * 1.3: Top songs by artist in terms of `tempo`
    * 1.4: Average `track_feature` values by artist
    * 1.5: Max and min average `track_feature` and `duration_ms` values by album
    * 1.6: Proportion of `explicit` tracks by artist
    * 1.7: Median `track_feature` values by artist
  * Part 2: Data Visualizations
    * 2.1: Mean `track_feature` values by `genre`
    * 2.2: Distribution of `track_feature` and `duration_ms` values
    * 2.3: Evolution of artists, visualized
    * 2.4: Evolution of genres, visualized
    * 2.5: Correlations between `track_feature` + `duration_ms` + `followers` values

<br>

## Notebook 1: Data Ingestion

I begin the project by accessing my API.txt file, which is a locally-stored file that contains my API credentials to access the Spotify API. Next, I define my 20 favorite artists, loop through each name in the list of favorite artists with the Spotipy `.search()` function, and create nine separate lists (one for each column in the `artist` table) containing the compiled information for each feature. Lastly, I organize the lists into a dictionary, and create a pandas table by passing in the dictionary.

This process is then repeated for the `album` table, where I obtain the albums of each artist by looping through each `artist_id` using the `.artist_albums()` function. This part is a bit more complicated than the last, as I needed to consider that duplicate albums may be pulled in the process. I implemented a variety of measures to prevent this:
1. I record the unique album names of each artist (I use a RegEx search to remove any comments in parentheses or square brackets, e.g. "(Remastered)" or "\[Deluxe Edition]". I then check during each loop if the next album is in the duplicate albums list, and skip it if so.
2. To avoid live-recorded albums (which would result in duplicate songs being added) I skip any album with the word "live" in it (after setting all words in the title to `.lower()` format.
3. To avoid re-release albums, I skip any albums released after a band's lifecycle when applicable (only for older band's that are no longer producing music, e.g. The Beatles).

Next, I obtain data for the `track` table by looping through each `album_id`. The only measure I used here to prevent duplicate tracks being added is another duplicate tracks list that is checked during each loop through an album.

Lastly, I obtain data for the `track_features` table for each of the tracks obtained from the last step using each `track_id`, and save the pandas tables as `.pkl` files for use in the next notebook.




