# Project 3 - Web Scraping and NLP Classification


## Executive Summary 
ABC Travel aims to help travellers easily gather information during the COVID-19 pandemic. Rules and regulations are constantly changing, and cost of travel has also gone up due to the various restrictions.

To build our information portal, we have decided that the best "hacks" and tips would be from people who have had first hand experience. For phase 1 of our information portal, we will be web-scraping data from 2 subreddits: r/shoestring for budget travel, and r/solotravel for solo travellers.

Following which, we will leverage NLP models for classification of the posts into 2 tags on our portal - solo and budgetbarbies. We will then compare a couple of models to see which gives us the best results.


## Background
The [recovery forecast](https://www.bain.com/insights/air-travel-forecast-when-will-airlines-recover-from-covid-19-interactive/) for air travel looks promising . COVID-19 has drastically changed the travel landscape, and a lot more research has to be done now when planning a trip. 


## Problem Statement 
There are too many information sources when it comes to trip planning, making it hard to discern which are actually true or up to date.

Our company ABC Travel has decided to build a one-stop information portal built from prior experiences to make trip planning hassle-free in the time of COVID-19.

---

## Contents

* [Webscraping](#link1)
* [Data Cleaning](01_cleaning_eda.ipynb#link2)
* [EDA](01_cleaning_eda.ipynb#link3)
* [Model Preparation](02_modelling.ipynb#link6)
* [Modelling](02_modelling.ipynb#link4)
* [Recommendations](02_modelling.ipynb#link5)

---

### Data Sets
* [`shoestring.csv`](data/shoestring.csv) -- original data from r/shoestring
* [`solotravel.csv`](data/solotravel.csv) -- original data from r/solotravel
* [`data_vec.csv`](data/data_vec.csv) -- cleaned data used for EDA and Modelling


### Data Dictionary (for data_vec)
* 'titletext' -- column created for joining of both title, and text in post
* 'lemmatized' -- column for text lemmatized using Spacy
* 'stemmed' -- column for text stemmed using SnowballStemmer

--- 

## Model Summaries

| Model | Vectorizer | Train Score | Test Score | Misclassification Rate |
| --- | --- | --- | --- | --- |
| Logistic Regression | TFIDF | 82.8% |  72.2%  |   27.8% |  
| Naive-Bayes | Count | 81.9% |  72.7%  |  27.3%  | 
| Random Forest | TFIDF | 84.2% |  73.9%  |  26.1%  |  
| KNN | TFIDF | 71.2% |  72.0%  |  28.0%  | 
| SVM | Count | 89.7% |  72.7%  |  27.3%  |   

--- 

## Model Analysis

ROC-AUC was chosen to score our model, as the split between the 2 classes are pretty balanced, and misclassification would not have too much of a negative impact.

The SVM model severly overfits and will not be chosen to be our production model. 
Logistic regression produced the best roc-auc score, however it is also severely overfit. 

We have chosen to use a KNN as our production model, as it is the least overfit, and has a decent ROC-AUC score that does not differ too much from the other models. 


When looking at misclassified text, we see a mix of: 
* truly misclassified posts 
* posts that could belong in both subreddits 
* posts that were posted in the wrong subreddit
* posts that do not belong in either subreddit

--- 

## Recommendations

To summarize, in the first notebook we scraped data from 2 subreddits: r/solotravel and r/shoestring. After which we created a lemmatized text column and ran 5 models on these columns to examine if we are able to classify them into their original subreddits.

We first ran a gridsearch to find the best parameters for both CountVectorizer and TFIDFVectorizer, before remodelling with these parameters.

The model that performed well with respect to our problem statement is: KNN with a TFIDFVectorizer. It returned an accuracy score of 72.0% on our test data.

Although our accuracy scores for our model wasn't particularly high, we have still gained valuable insight.

   1) Perhaps the 2 subreddits are not at all that different from each other, and could be combined into 1 subreddit for solo travel. The distinction between the 2 subreddits can also be seen when we look at misclassified text, there were a couple of instances where posts could belong to either subreddit.

   2) To explore grouping the portal tags by 'travel type' instead - for example by solo or group travel.

       - Could there be a correlation between solo travel and budget travel? 

--- 
## Future Improvements

- Experimenting with different train and test splits to deal with model overfitting
- Try modelling with XGBoost to see if it returns better results (first model took 7H to run)
- Explore relationship between different types of travellers (solo or group) and what their considerations are
- Sub-categorizing posts into regions (e.g. Europe, South East Asia) to help in faster, more targeted information search