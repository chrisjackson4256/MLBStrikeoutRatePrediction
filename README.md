# Predicting MLB Strikeout Rates

### Introduction

This project was motivated by "Predicting Major League Baseball Strikeout Rates from Differences in Velocity and Movement Among Player Pitch Types" by Eric P. Martin.  This paper was a finalist for the MIT Sloan Sports Analytics Conference top reserch papers.

The basic idea is to predict yearly strikeout rates using a pitcher's repertoire of pitch types.  Compared to other rate measures, strikeout percentage is one of the most reliable and consistent pitching statistics and the statistic least likely to be affected by chance or a team’s defensive ability.  Thus, it is a good indicator of a pitcher's skill and future success.

By "pitch types", we mean pitches classified by their speed, location and movement not the _names_ of pitches as classified by Pitch F/X or Statcast (such as "Four Seam Fastball" or "Curveball").  This classification is performed by means of a clustering algorithm (we try out several algorithms including K-Means, DBSCAN, etc.) which groups pitches according to characteristics of the pitches (speed and movement). 


### The Data

The raw data is scraped from Baseball Savant using the pybaseball Python module. Since "short relievers" tend to enter game situations where they have a favorable advantage that bias their statistics, we remove them from the dataset by keeping only pitchers that have thrown at least 1000 pitches in a season AND that average 10 batters faced per appearance.

We use data from 2016 and 2017 for clustering and predictive model training and 2018 for testing the predictive model.  After removing short relievers, the train data consists of 830555 data points from 241 unique pitchers, while the test data is made up of 400666 from 178 unique pitchers.  A snapshot of the train dataframe is shown below:

![alt text](https://github.com/chrisjackson4256/MLBStrikeoutRatePrediction/blob/master/cluster_dataframe.png "dataframe used for clustering")


### Clustering the Pitch Types

The features used in the clustering algorithms are: "plate_x", "plate_z" (the horizontal and vertical locations of the pitch as it crosses the plate), "pfx_x", "pfx_z" (the movement in the horizontal and vertical directions) and the release speed of the pitch.  These features are all normalized to lie beteen 0 and 1.

We try several algorithms for the clustering, but settle on K-Means for simplicity.  The optimal number of clusters is chosen using the "Gap Statistic" (see ["Estimating the Number of Clusters in a Data Set Via the Gap Statistic"](http://web.stanford.edu/~hastie/Papers/gap.pdf) by Tibshirani, Walther and Hastie for details).

Using data from 2016 and 2017 to train the clustering model, we find the optimal number of clusters is 7.  The pitches from the training and testing datasets are then each categorized using the cluster numbers.  Then, for each pitcher, we turn the counts of pitches of each cluster type into features.  In other words the data takes the shape:

![alt text](https://github.com/chrisjackson4256/MLBStrikeoutRatePrediction/blob/master/number_pitch_clusters.png "pitch cluster number dataframe")

The following bar chart shows, for each number of clusters, how many pitchers throw that number of different types of pitches.  The blue bars represent the data from the work done here, while the orange bars represent the clustering from the original paper by Martin.  It's clear that the clustering performed in this work results in smaller clusters and, thus, a higher average number of pitch types thrown.

![alt text](https://github.com/chrisjackson4256/MLBStrikeoutRatePrediction/blob/master/cluster_bar_plot.png "cluster number bar plot")

### Model Building

It has been a long-held belief that pitch speed and strike percentage are the leading predictors when it comes to predicting strikeout rate.  Therefore, we first train a baseline model using these features in a simple linear regression model.  The metric of choice to assess model performance here is _mean absolute error_ (MAE) and the simple baseline model achieves an MAE of 3.51%.

We next train models using a dataset consisting of strike percentage and the pitch cluster counts as features.  First, we train a linear regression and "off-the-shelf" (OTS) Random Forest and AdaBoost models (using Scikit-Learn) to get a feel of the performance of the models on this data.  Finally, we optimize over the hyperparameters for each of the Random Forest, AdaBoost and XGBoost models.  All results are shown below.

| Model                       | MAE   |
| -------------               |:-----:|
| Baseline LR                 | 3.51% |
| LR with Pitch Cluster Data  | 3.48% |
| OTS Random Forest           | 3.73% |
| OTS AdaBoost                | 3.39% |
| Optimized Random Forest     | 3.34% |
| Optimized AdaBoost          | 3.42% | 
| Optimized XGBoost           | 3.41% | 
