# ICME 2017: Classifying derivative works of music

This site contains data and supplementary material for our ICME 2017 publication: "Classifying derivative works with search, text, audio and video features."

## Abstract

Users of video-sharing sites often search for derivative works of music, such as live versions, covers, and remixes. Audio and video content are both important for retrieval: "karaoke" specifies audio content (instrumental version) and video content (animated lyrics). Although YouTube’s text search is fairly reliable, many search results do not match the exact query. We introduce an algorithm to automatically classify YouTube videos by category of derivative work. Based on a standard pipeline for video-based genre classification, it combines search, text, and video features with a novel set of audio features derived from audio fingerprints. A baseline approach is outperformed by the search and text features alone, and combining these with video and audio features performs best of all, reducing the audio content error rate from 25% to 15%.

## Data collection

The data were collected in the following steps:

1. **Query selection.** We selected 10 songs from the [BillBoard Hot 100 year-end chart for 2012](http://www.billboard.com/charts/year-end/2012/hot-100-songs). We chose those with ranks 1, 11, 21, ..., 91.
2. **YouTube search.** For each song, we ran multiple keyword searches using the [YouTube API](https://developers.google.com/youtube/v3/docs/search) with the following form: `+"query_artist" +"query_title" +keyword`, once with each `keyword` in this list: choreo, concert, cover, dance, guitar, karaoke, live, lyrics, piano, remix, tutorial, and [*empty*], i.e., the artist and song name alone.
3. **Data selection.** Out of the thousands of results, we randomly selected 600 to annotate. In order to investigate the impact of search depth, we selected songs with peak search rank of roughly 1, 20, 100, or 300. (The peak search rank was the highest rank achieved in any keyword search.)
4. **Data annotation.** The first author alone annotated the audio and video content of the videos.

Notes:

* Some videos failed to download, so the final number of videos in the dataset dropped from 600 to 562.
* Some months after initially collecting the data, the search ranks changed. In this published dataset, we include the original search tier (1, 20, 100 or 300) and the updated search rank. The tier information dates from roughly February 2016; the rank dates from roughly December 2016.

## Data description

The data provided here are for 562 unique videos on YouTube. We provide video metadata (title, url, and query terms that led to the video); annotated audio and video content; and the output of our proposed classification algorithm as well as a baseline.

The CSV file contains the following fields:

Field Name | Description 
--- | ---
`video_id`					| YouTube video ID, a unique 11-character identifier for the video. Any video can be viewed by visiting www.youtube.com/watch?v=video_id. (This link is provided in the last column for convenience.)
`query_billboard_rank`		| Rank of the query song on the Billboard Top 100 for 2012. This number also serves as a unique identifier for the query.
`query_artist`				| Artist name, potentially including featured artists.
`query_title` 				| Song title.
`video_title` 				| Title of the video found on YouTube.
`search_rank`				| Highest search rank that the video achieved in any search (with rank 1, the first result, being the "highest").
`lead_search_term`			| Search term for which that search rank was obtained.
`search_tier`				| Search rank rounded to the nearest value among 1, 20, 100, or 300. (Originally, the data to be annotated were selected to have peak search ranks equal to one of these values, plus or minus 2. When the same searches were re-run at a later date, the search ranks changed. `search_tier` relates to the original search rank; `search_rank` is the updated search rank.)
`detected_names` 			| Common names found in the video title. List of names taken from the [US 1990 Census list](https://www.census.gov/topics/population/genealogy/data/1990_census/1990_census_namefiles.html); we kept the top 1000 last names, and all first names, resulting in a list of roughly 5500 names. We manually removed names that we noticed were common musical terms, like "Melody" and "Singer".
`detected_genres` 			| Genre words found in the video title. List adapted from [this source](http://www.musicgenreslist.com/).
`detected_inst` 			| Instrument words found in the video title. List adapted from [this source](http://www.enchantedlearning.com/wordlist/musicalinstruments.shtml).
`detected_place`			| Geographic terms found in the video title, including countries, states/provinces, and cities. Based on the Gazetteer Lists, obtained through the [Natural Language Toolkit (NLTK)](http://www.nltk.org/) word collections.
`detected_date`				| Dates and date framents detected in the video title. Detection was done using the [parsedatetime](https://github.com/bear/parsedatetime) Python package.
`true_audio_content`		| Annotated audio content (AC) category, from among: official, cover, instrumental, live, remix, tutorial.
`true_video_content`		| Annotated video content (VC) category, from among: official, dance, karaoke, live, lyrics, slides, still, tutorial, other.
`is_official_video`			| TRUE if the video is an official version of the song posted by the artist (or their label) to their verified YouTube channel. Otherwise FALSE.
`search_term_estimated_AC`	| Baseline prediction of AC, which uses the lead search term to predict AC with a decision tree.
`search_term_estimated_VC`	| Baseline prediction of VC.
`STV_prediction_AC_initial`	| Initial prediction of AC by the system after considering search, text and video features.
`STVA_prediction_AC`		| Refined prediction of AC after considering audio features.
`STV_prediction_VC`			| Prediction of VC based on search, text and video features.
`url`						| Full URL for convenience; equal to "www.youtube.com/watch?v=" + `video_id`. Not all links will work since some videos were taken down after the data collection step.

## Reference

> Jordan B. L. Smith, Masahiro Hamasaki, and Masataka Goto. 2017. "Classifying derivative works with search, text, audio and video features." In *Proceedings of the International Conference on Multimedia and Expo.* Hong Kong, China. 2017. 1428–33.

Links:

* [PDF](http://jblsmith.github.io/documents/smith2017-icme-classifying_derivative_works.pdf)
* [BIB](http://jblsmith.github.io/documents/smith2017-icme-classifying_derivative_works.bib)
* [Poster PDF](http://jblsmith.github.io/documents/smith2017-icme-classifying_derivative_works-poster.pdf)