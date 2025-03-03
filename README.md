# Platoon vs. Pitcher Recommender
![Pinch Hit Bote Bomb](images/bote_bomb.gif)

## Intro

In Major League Baseball, there are 26 players on an active roster, meaning they are available to be used any given game. Roughly half of those are pitchers, about being 5 starters in the rotation and the remaining being relief pitchers. The other half of players are positions players. There are 9 position player starters and the remaining position players / hitters are backups, or platoon players. Late in games, when starting pitchers are replaced by fresher relief pitching arms, platoon players will pinch hit/run become a defensive substitute for the starting position players. The reasons for a pinch hit situation can include things like pitcher handedness and/or the pitcher's pitch arsenal and pitch tendencies. Often, the player that pinch hits against a pitcher has prior success versus that pitcher. However, in the event that there is little or no precedent, a model that can provide a recommendation for choosing a batter given a certain type of pitcher could be helpful for managers to make the best decision possible. 

In this project, I will use a collaborative filtering recommender system to pair an optimal matchup in the offense's favor. In recommender systems there is a user, an item, and a rating. For a (simplified) example in the context of netflix, the user is the person, the item is the movie, and the rating is a metric that determines how much a user enjoyed an item. For this project, the user is a batter, the pitcher is the item and the rating is a result metric that can fairly measure the success a batter has against a pitcher. I selected to use wOBA as this metric. wOBA stands for weighted on-base average and scales a type of hit to be worth more or less based on the run value of that hit.

## Data Understanding

To calculate wOBA, I needed to pull the plate appearance events from [baseballsavant](https://baseballsavant.mlb.com/), a website that collects MLB data from MLB.com's Statcast database, using [Pybaseball](https://github.com/basstraining/platoon_recommender/tree/main/pybaseball), a python package that scrapes baseball data. This provided a dataset of all the pitches thrown in the 2021, 2022 and 2023 MLB seasons.

## Data Cleaning and Preprocessing

After pulling the data, I removed all pitches that were not event pitches, meaning they did not result in the end of a plate appearance. I dropped all columns other than the event column and mapped the values in the events column to the outcomes that are weighted in wOBA. I then 

The variables and weights are shown in the formula:

$$
\text{wOBA} = \frac{0.69 \times \text{NIBB} + 0.72 \times \text{HBP} + 0.88 \times \text{1B} + 1.24 \times \text{2B} + 1.56 \times \text{3B} + 1.95 \times \text{HR}}{\text{AB} + \text{BB} - \text{IBB} + \text{SF} + \text{HBP}}
$$

I grouped by batters and calculated their wOBA against all of the pitchers they had seen more than 10 times, creating my wOBA metric. 

## Modeling and Evaluation

I used singular value decomposition which uses linear algebra to decompose a matrix into three matrices. I had an acceptable RMSE but it was an indicator that the project could be improved using a different metric.


## Conclusion and Next Steps

This project will be improved by using xwOBA, or expected wOBA, so the rating is not as susceptible to variance.

## Contributing
If you would like to contribute to the project please fork the repository. You should activate your python env with `conda` or `venv` using  >= python3.8 and install the python package `pybaseball` with `pip`

Here is a link to the pybaseball package github repository: https://github.com/jldbc/pybaseball

#### On Linux/Mac/Windows
```
pip install pybaseball
```
















## Repo Structure 
```
├── data
├── images
├── notebooks
│   ├── clustering
│   ├── working
├── pybaseball
├── final.ipynb
├── LICENSE
├── README.md
```
