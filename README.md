# Predicting MLB Strikeout Rates

### Introduction

This project was motivated by "Predicting Major League Baseball Strikeout Rates from Differences in Velocity and Movement Among Player Pitch Types" by Eric P. Martin.  This paper was a finalist for the MIT Sloan Sports Analytics Conference top reserch papers.

The basic idea is to predict yearly strikeout rates using a pitcher's repertoire of pitch types.  Compared to other rate measures, strikeout percentage is one of the most reliable and consistent pitching statistics and the statistic least likely to be affected by chance or a team’s defensive ability.  Thus, it is a good indicator of a pitcher's skill and future success.

By "pitch types", we mean pitches classified by their speed, location and movement not the _names_ of pitches as classified by Pitch F/X or Statcast (such as "Four Seam Fastball" or "Curveball").  This classification is performed by means of a clustering algorithm (we try out several algorithms including K-Means, DBSCAN, etc.) which groups pitches according to characteristics of the pitches (speed and movement). 


### The Data

The raw data is scraped from Baseball Savant using the pybaseball Python module. Since "short relievers" tend to enter game situations where they have a favorable advantage that bias their statistics, we remove them from the dataset by keeping only pitchers that have thrown at least 1000 pitches in a season AND that average 10 batters faced per appearance.


### Clustering the Pitch Types

The features used in the clustering algorithms are: "plate_x", "plate_z" (the horizontal and vertical locations of the pitch as it crosses the plate), "pfx_x", "pfx_z" (the movement in the horizontal and vertical directions) and the release speed of the pitch.  These features are all normalized to lie beteen 0 and 1.

We try several algorithms for the clustering, but settle on K-Means for simplicity.  The optimal number of clusters is chosen using the "Gap Statistic" (see ["Estimating the Number of Clusters in a Data Set Via the Gap Statistic"](http://web.stanford.edu/~hastie/Papers/gap.pdf) by Tibshirani, Walther and Hastie for details). 


