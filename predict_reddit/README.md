# Using Reddit's API for Predicting Comments and Modeling

Kihoon Sohn | Data Science Immersive Student @ General Assembly | June 2018

## Executive Summary
  How to get an attention in a sea of information, correction, in a sea of Reddit posts? Since the user-generated-site ranked the 6th most visited webpage in the world, it is more and more competitive to be remarkable. People spend a lot time in this categorical playground. In the Alexa Global Topsites 500 released the fact that 15 minutes and 8 seconds, in average, is the time people spend in the site daily. This figure outranked all of the following competitor in top 3 site from the same source: google(7:16), youtube(8:32), facebook(10:46). Therefore, hundreds of posts are judging, even in this very moment, instantly as either eye-catchy or nah-boring.

  To analyze, therefore, the massive data is necessary whether which characteristics and/or features affect the most. As a data-scientist, I fetched 51,697 Reddit 'hot posts' in seven-days period and built the machine learning models to identify and find the odds. I utilized multiple data science methods, via python, such as CountVectorizer, TfidfVectorizer, RandomForest, Logistic Regression, etc. After evaluating multiple models, it shows that `title`, `age`, `subreddit` are the best features to build a model.

  It appeared the most appeared word in the title for the popular posts were: "new, one, first, now, time, day, will, made, game, today." Also, the most predictive popular subreddits that your can get attention are: r/AskReddit, r/cars, r/CFB, r/MemeEconomy, r/Drama. In conclusion, to make your post stays longer by capturing peoples attention, you will try to choose your subreddits and certain words in the title.

## Data Science Tools and Methods Applied

### NLP (Natural Language Processing)
 - Applied the following vectorizer to process text into numbers so that I can utilize it to my prediction models.
   - CountVectorizer
   - TfidfVectorizer

### ML (Machine Learning)
 - Tried several models to predict. The detailed performance of each models can be found the last part of this Readme.
   - RandomForest
   - LogisticRegression
   - KNN  

-------------------------------------
## Table of contents

1. **Notebooks** files
  - 1-data-fetch.ipynb
  - 2-data-prep.ipynb
  - 3-model.ipynb
2. **CSV** file
  - dataset/master_06-02-2018(hotposts).csv
3. **PDF** file
  - Reddit_analysis.pdf : presented to GA cohorts

# Web-scraping Process
  - Main target page: `http://www.reddit.com/hot.json`
  - Conditions
    1) {`User-agent`: `KH`} : it is essential for reddit to fetch data via `requests.get()`
    2) `limit=100` : each iteration attempt to fetch 100 posts. By increasing this number, it will reduce chances to get duplicated data since the target website designed as fast-changing leaderboard-typed hot-posts.  
    3) `after=` : this allows fetching from the last scraped post unique id, and put it into for-loop iteration.
    4) `.Timestamp.utcnow()` : to imprint the fetched time

# Cleaning Steps
  - Calculate age: theoretically it can be done by `fetched time` - `created_utc`, however it is needed to adjust datatime datatypes.
    - `.astype('datetime64[s]')`, `.astype('timedelta64[m]')` has been used in the notebook. Rather to calculate each post's hours in the list, here it is delivered by minutes in its ages.

# modeling
  - Baseline accuracy: 0.746 the majority of the number of comments which turned out Low as .746 and High as .254

  - Model 1: Tree-based - Feature: `subreddit` with RandomForest
    - Shown some improvement compared to baseline accuracy.
    - test set score is better than train set score.
    - Gridsearch changes the score slightly increased.

  - Model 2: Tree-based - Features : `subreddit`, `is_cat`, `is_funny` with RandomForest
    - compared to the Model 1, `is_cat` and `is_funny` doesn't affect much in the score changes. Therefore, it didn't show any meaningful improvement.

  - Model 3: Tree-based - Feature: `title`, `subreddit`, `age` with TfidfVectorizer & Random Forest
    - train set score and test set score difference is quite huge. It can be said that the model is overfitted.
    - However, among the previous two models, it shows the best score.

  - Model 4: Tree-based - Feature: `title` with CountVectorizer, TfidfVectorizer, Random Forest¶
    - The score between the sets are relatively small, the model seemed not to over/underfit. However, it stays as similar or same as baseline score.
    - Therefore, by using CVEC, TVEC, RF with title is not the best modeling.

  - Non-tree-based Model 1 - Feature: `title` with CountVectorizer, TfidfVectorizer, Logistic Regression, KNN
    - `title` with CVEC, TVEC with Logistic Regression, KNN doesn't show the best performance in my model.

  - Non-tree-based Model 2 - Feature: `title`, `age`, `subreddit` with TfidfVectorizer, LogisticRegression, KNN
    - Among all the other model, it shows the best score `.83` in Logistic Regression.
    - Also, odds ratio can be interpreted as every minutes increase % in crease in odds having higher number of posts. However, it didn't show dramatic number in odds ratio, because it was set as minutes in age. If it were in hours, it may have different result.
