# Predicting MLB Strikeout Rates

### Introduction

This project was motivated by "Predicting Major League Baseball Strikeout Rates from Differences in Velocity and Movement Among Player Pitch Types" by Eric P. Martin.  This paper was a finalist for the MIT Sloan Sports Analytics Conference top reserch papers.

The basic idea is to predict yearly strikeout rates using a pitcher's repertoire of pitch types.  Compared to other rate measures, strikeout percentage is one of the most reliable and consistent pitching statistics and the statistic least likely to be affected by chance or a team’s defensive ability.  

By "pitch types", we mean pitches classified by their speed, location and movement not the _names_ of pitches as classified by Pitch F/X or Statcast (such as "Four Seam Fastball" or "Curveball").  This classification is performed by means of a clustering algorithm (we try out several algorithms including K-Means, DBSCAN, etc.) which groups pitches according to characteristics of the pitches (speed and movement). 


### The Data

The raw data is scraped from Baseball Savant using the pybaseball Python module. Since "short relievers" tend to enter game situations where they have a favorable advantage that bias their statistics, we remove them from the dataset by keeping only pitchers that have thrown at least 1000 pitches in a season AND that average 10 batters faced per appearance.



