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


