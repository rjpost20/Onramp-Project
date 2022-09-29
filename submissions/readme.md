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

I begin the project by accessing my `API.txt` file, which is a locally-stored file that contains my API credentials to access the Spotify API. Next, I define my 20 favorite artists, loop through each name in the list of favorite artists with the Spotipy `.search()` function, and create nine separate lists (one for each column in the `artist` table) containing the compiled information for each feature. Lastly, I organize the lists into a dictionary, and create a pandas table by passing in the dictionary.

This process is then repeated for the `album` table, where I obtain the albums of each artist by looping through each `artist_id` using the `.artist_albums()` function. This part is a bit more complicated than the last, as I needed to consider that duplicate albums may be pulled in the process. I implemented a variety of measures to prevent this:
1. I record the unique album names of each artist (I use a RegEx search to remove any comments in parentheses or square brackets, e.g. "(Remastered)" or "\[Deluxe Edition]". I then check during each loop if the next album is in the duplicate albums list, and skip it if so.
2. To avoid live-recorded albums (which would result in duplicate songs being added) I skip any album with the word "live" in it (after setting all words in the title to `.lower()` format).
3. To avoid re-release albums, I skip any albums released after a band's lifecycle when applicable (for older band's that are no longer producing music, e.g. The Beatles).

Next, I obtain data for the `track` table by looping through each `album_id` using the `.album_tracks()` function. The only measure I used here to prevent duplicate tracks being added is another duplicate tracks list that is checked during each loop through an album.

Lastly, I obtain data for the `track_features` table for each of the tracks obtained from the last step using each `track_id`, and save the pandas tables as `.pkl` files for use in the next notebook. I use the `.audio_features()` Spotipy function for this step.

<br>

## Notebook 2: Data Transformation and Storage

I begin this phase of the project by uploading the `.pkl` files containing the pandas tables. I then move on to Part 1 of Step 2 which is handling null / missing values. To start, I create a function to display the `.info()` of each table, a list of the features that contain null values, and the total number of blank values in the dataframe. As it turned out, none of the tables had any null or blank values, so this part of the project was simple.

I then move on to Part 2 of Step 2 which is the deduplication phase. To begin I print each artist and a list of their album names contained in my database, and do a manual review of album names to look for duplicate/compilation/live-recorded albums. This was the only part of my project that was truly manual and non-scalable, but I didn't see any way around it as at this point I already implemented a simple duplicate name search in Step 1 and I didn't see a way to automate the deduplication process further. The difficulty in automating this process arises mainly from the fact that compilation albums often carry names that are indistinguishable from original-release studio albums.

After removing the 14 albums containing duplicate songs from the albums, tracks, and track features tables, I move on to removing duplicate songs. To implement this, I search for song names in the database that appear more than once (for any particular artist). I then display a table for each artist that contains their duplicate songs, and record the relevant `track_id`s in a list to drop. One of the benefits of displaying these tables was that they allowed me to see some albums which I missed in the previous step, so I was able to go back and add those to the list of albums to drop. As an aside, the reason I couldn't skip the previous manual duplicate album search step and go straight to looking for duplicate song names is that comments or notes are often added to the end of song names in duplicate albums (and not always in parentheses or brackets).

Lastly, for Step 3 I create SQL tables with the defined schemas as specified in the instructions, fill them with the data in each associated pandas table, and organize them under the `spotify.db` file.

<br>

## Notebook 3: Data Analytics and Visualizations

### Part 1: SQL Query Analytics

After loading the pandas `.pkl` file tables and connecting to the `spotify.db` SQLite database, I begin the SQL queries which comprise Part 1 of Step 4. The queries are:
1. Top 10 songs by artist in terms of `duration_ms`
   - Per instructions
2. Top 20 artists in the database by number of `followers`
   - Per instructions
3. Top 10 songs by artist in terms of `tempo`
   - Per instructions
4. Average `track_feature` values by artist
   - I wrote this query because I thought it would be insightful to compare average values across artists and see what trends come up, as well as see if I can notice differences based on genre.
5. Max and min average `track_feature` and `duration_ms` values by album
   - This was a more complicated query that I wrote because I wanted a more granular look at which specific albums were at the extremes for each `track_feature`.
6. Proportion of `explicit` tracks by artist (measured by percent of songs marked as explicit)
   - For this query, I was simply curious which artists had the most explicit music, and whether there was a difference between older and more contemporary artists.
7. Median `track_feature` values by artist
   - This is very similar to query 4, but querying median values instead of average values. There is no 'median' function in SQLite, so I had to do quite a few joins and window functions to make it work. The motivation for this query was actually my second visualization, which showed me that a handful of the `track_feature`s had highly skewed distributions, so computing averages for those values would not be as representative as the median.

### Part 2: Data Visualizations

I created five visualizations as part of this project. When creating visualizations for insight into data, I always keep in mind the main types of data visualizations and try to use as wide a variety of them as possible. For this project, I use four: ranking, distribution, evolution, and correlation.
1. Mean scaled `track_feature` values by genre
   - Similar to query 3, I wanted to look for differences in average track feature values across genres, but in a more visually-friendly way. There were a variety of insights I gleaned from this visualization, which are described in the notebook. For this visualization, I condensed the genres in the database down to four, as it would not be practical or particularly insightful to keep the wide-variety of different (often similar) genres that Spotify assigned them.
2. Distribution of `track_feature` and `duration_ms` values
   - This one is self-explanatory – I wanted a visualization to show the distributions across values. One of the insights from this visualization, as mentioned, was that many of the features were highly skewed, so taking the median would be a more representative metric than the average for those.
3. Evolution of artists, visualized
   - For this visualization and the next, I am vague in the title because I designed the code to allow the user to choose which artist and genre they want to visualize. This adds another dynamic layer to the visualization and increases the quantity and granularity of insights and understanding of the data which can be obtained. Not to mention, the more interactive you can make data representations, the more compelling and interesting they will often be.
4. Evolution of genres, visualized
   - As covered above, the user can choose which genre they want to visualize. Like in the second visualization I condensed the genres down to four. In the `visualization.pdf` file I chose hip hop, but the user could also choose pop, classic rock or dance/electronic.
5. Correlations between `track_feature` values + `duration_ms` + `followers`
   - I created this visualization because I was curious about both positive and negative correlations between track features, and particularly between track features and number of followers as well. I describe a number of insights in the notebook, but one cautionary note to keep in mind is that the relatively low volume of data used in the project renders the takeaways and correlations not particularly reliable. Still, many of the correlations were logical and predictable, while for others I would need to use more data before I fully trusted the numbers.
