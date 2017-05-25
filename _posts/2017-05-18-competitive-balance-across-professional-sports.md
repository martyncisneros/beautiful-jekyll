---
layout: post
title: Measuring the Competitive Balance across U.S. Professional Sports Using R
subtitle: A look at Comebacks, Close Games, Blowouts, and Salary Cap Disparity
bigimg: 
- "/img/competitive-analysis/sports_header.png" : "© 2013 Stadium Management Company, LLC"
published: false
---

This past February, the sports world witnessed one of the most improbable comebacks in sports history and it came during the biggest stage. The New England Patriots rallied from a <a href="http://www.nfl.com/videos/nfl-super-bowl/0ap3000000783876/Patriots-wild-comeback-in-114-seconds" target="_blank">28-3 deficit</a> to beat Atlanta Falcons and win the National Football League (NFL) Super Bowl. Right now hockey fans are being treated to one of the most competitive playoffs ever. The National Hockey League's (NHL) 2017 Stanley Cup Playoffs set a record with <a href="https://www.nhl.com/news/2017-stanley-cup-playoffs-sets-overtime-record/c-289053508" target="_blank">18 overtime games</a>. On the other hand, besides what will surely be an exciting National Basketball Association (NBA) finals between the Cleveland Cavaliers and Golden State Warriors (playing for the title for the <a href="https://fivethirtyeight.com/features/the-cavs-and-warriors-might-be-doing-this-finals-thing-for-a-long-time/" target="_blank">third straight year</a>), the competitive landscape in the league has suffered in recent years. In Game 2 of the Eastern Conference Finals, the Cavaliers set a playoff record with a <a href="https://streamable.com/kddo0" target="_blank">41 point halftime</a> lead against the Boston Celtics. 

This recent sports drama has triggered a question primed for a data-based answer, **which league (NBA, NHL, NFL, or MLB) is the most competitive?** 

I will measure competitiveness by looking at: 

-  Which league is most likely to produce a comeback game? 
-  Which league is most likely to produce a close game? 
-  Which league is most likely to produce a blowout game? 
-  Which league has the most linear relationship between wins per season and team salary?
-  Which league is the most predictable? 

In this analysis, I utilized an expansive <a href="http://developers.stattleship.com/" target="_blank">sports data API</a>, powered by <a href="https://www.stattleship.com/" target="_blank">Stattleship</a>, to get historical game data and game scores by quarter. I also scraped <a href="http://www.sports-reference.com/" target="_blank">Sports Reference</a> for team payroll data. 

** The R code used for data wrangling and analysis can be found here for the <a href="" target="_blank">NBA</a>, <a href="" target="_blank">NHL</a>, <a href="" target="_blank">NFL</a>, and <a href="" target="_blank">MLB</a>. **

First, in order to measure the likelyhood of comebacks, close games, and blowouts, I calculated score differentials at each quarter/inning/intermission point for all regular season and playoff games since 2015. I then used these score differentials by quarter to determine if a game fit one of the categories. 

**Comebacks**

I define comebacks by two cases: 

Case 1: Team A is down by X points going into the final quarter and wins the game. 

![](http://latex.codecogs.com/gif.latex?IsComebackCase1%20%3D%20%28IF%20%5C%2C%20DeltaScoreQ3%20%5Cleq%20XPts%20%5C%2C%20AND%20%5C%2C%20DeltaScoreFinal%20%3E%201%5C%2C%20THEN%20%5C%2C%20Yes%20%5C%2C%20ELSE%20%5C%2C%20No%29)

Case 2: Team A is down by X points going into the third quarter and wins the game. 

![](http://latex.codecogs.com/gif.latex?IsComebackCase2%20%3D%20%28IF%20%5C%2C%20DeltaScoreQ2%20%5Cleq%20XPts%20%5C%2C%20AND%20%5C%2C%20DeltaScoreFinal%20%3E%201%5C%2C%20THEN%20%5C%2C%20Yes%20%5C%2C%20ELSE%20%5C%2C%20No%29)

To determine an appropiate X Threshold to qualify a 'comeback', I looked at the average deficit for losing teams going into the final quarter.

| League | X Threshold |
|--------|-------------|
| NBA    | 9 points    |
| NHL    | 1 goal      |
| NFL    | 9 points    |
| MLB    | 2 runs      |

Example Game: 


| Game          | Team                 | Q1 | Q2 | Q3 | Q4 | OT | Final |
|---------------|----------------------|----|----|----|----|----|-------|
| Super Bowl 51 | New England Patriots | 0  | 3  | 6  | 19 | 6  | 34    |
| Super Bowl 51 | Atlanta Falcons      | 0  | 21 | 7  | 0  | 0  | 28    |

The game log data I used has two entries per game, 1 per team. I first calculate the differentials at the end of each quarter. 

| Game          | Team                 | DeltaQ1 | DeltaQ2 | DeltaQ3 | DeltaQ4 | DeltaFinal |
|---------------|----------------------|---------|---------|---------|---------|------------|
| Super Bowl 51 | New England Patriots | 0       | -18     | -19     | 0       | 6          |
| Super Bowl 51 | Atlanta Falcons      | 0       | 18      | 19      | 0       | -6         |

IsComebackCase1 = Yes

IsComebackCase2 = Yes

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase1.png" alt="alt text" width="640" height="427">

Overall, there are very few games that fall into this category. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase2.png" alt="alt text" width="640" height="427">

On average, about **10%** of games across the leagues fall into a comeback category. However, there aren't any sweeping statements to be made from this view. 

| League | Comeback Rate |
|--------|---------------|
| NFL    | 8%            |
| NBA    | 7.4%          |
| MLB    | 7.1%          |
| NHL    | 5.6%          |

**Close Games**

Close games are those that are within some score differential, usually those 'competitive' games sports fans remember. I attempt to provide structure to this definiton by categorizing a game as such if it meets one of the cases below. 

_Y points is the maximum scorable one one possession per sport._ 

Case 1: The game is within Y points through each quarter and final score is within Y points (or game goes to OT). 

Case 2: The game is within Y points going into the final quarter and final score is within Y points (or game goes to OT). 

Case 3: The final score is within Y points (or game goes to OT).

| League | Y Threshold |
|--------|-------------|
| NBA    | 4 points    |
| NHL    | 1 goal      |
| NFL    | 8 points    |
| MLB    | 1 run       |

Example Game: 

For our close game example, I chose the <a href="https://streamable.com/t7ac" target="_blank">epic back-n-forth NBA Finals Game 7</a> (aka the undercard to Game of Thrones' <a href="http://www.imdb.com/title/tt4283088/" target="_blank">Battle of the Bastards</a> which aired right after). 

| Game                   | Team                  | Q1 | Q2 | Q3 | Q4 | Final |
|------------------------|-----------------------|----|----|----|----|-------|
| 2016 NBA Finals Game 7 | Golden State Warriors | 22 | 27 | 27 | 13 | 89    |
| 2016 NBA Finals Game 7 | Cleveland Cavaliers   | 23 | 19 | 33 | 18 | 93    |

Delta's 

| Game                   | Team                  | DeltaQ1 | DeltaQ2 | DeltaQ3 | DeltaQ4 | DeltaFinal |
|------------------------|-----------------------|---------|---------|---------|---------|------------|
| 2016 NBA Finals Game 7 | Golden State Warriors | -1      | 7       | 1       | -4      | -4         |
| 2016 NBA Finals Game 7 | Cleveland Cavaliers   | 1       | -7      | -1      | 4       | 4          |

IsCloseGameCase1 = No

IsCloseGameCase2 = Yes

IsCloseGameCase3 = Yes


<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase1.png" alt="alt text" width="640" height="427">

The NHL and NFL stand out in having **20-40%** of their games in the category where the game was within Y differential through each quarter and final score. Alternatively, the NBA produces less than **5%** of games in this category. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase2.png" alt="alt text" width="640" height="427">

Again, we see NHL and NFL stand out as more likely to produce a comeback game as defined by case 2. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase3.png" alt="alt text" width="640" height="427">

And once again, we see NHL and NFL stand out as more likely to produce a close game as defined by case 3.

| League | Close Game Rate |
|--------|-----------------|
| NHL    | 43.7%           |
| NFL    | 35.6%           |
| MLB    | 19.4%           |
| NBA    | 12.5%           |

**Blow Outs**

Blow out games are those that are outside of some score differential, usually those 'non-competitive' and lopsided games sports fans hope to forget. I attempt to provide structure to this definiton by categorizing a game as such if it meets one of the cases below. 

_Z points is 3x the maximum scorable one one possession per sport._

Case 1: The game score deficit is greater than Z points through each quarter and final score deficit is greater than Z points. 

Case 2: The game score deficit is greater than Z points going into the final quarter and final score deficit is greater than Z points. 

Case 3: The final score deficit is greater than Z points. 

| League | Z Threshold |
|--------|-------------|
| NBA    | 12 points   |
| NHL    | 3 goals     |
| NFL    | 24 points   |
| MLB    | 3 runs      |

Example Game: 

| Game              | Team                | Q1 | Q2 | Q3 | Q4 | Final |
|-------------------|---------------------|----|----|----|----|-------|
| 2017 NBA Playoffs | Boston Celtics      | 18 | 13 | 26 | 29 | 86    |
| 2017 NBA Playoffs | Cleveland Cavaliers | 32 | 40 | 31 | 27 | 130   |

Delta's 

| Game              | Team                | DeltaQ1 | DeltaQ2 | DeltaQ3 | DeltaQ4 | DeltaFinal |
|-------------------|---------------------|---------|---------|---------|---------|------------|
| 2017 NBA Playoffs | Boston Celtics      | -14     | -41     | -46     | -44     | -44        |
| 2017 NBA Playoffs | Cleveland Cavaliers | 14      | 41      | 46      | 44      | 44         |

IsBlowoutCase1 = Yes

IsBlowoutCase2 = Yes

IsBlowoutCase3 = Yes

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase1.png" alt="alt text" width="640" height="427">

NBA playoffs have produced just over **7.5%** case 1 blowouts, games over 12 points through each quarter. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase2.png" alt="alt text" width="640" height="427">

NBA and MLB produce **20-30%** case 2 blowouts while the NFL and NHL produce less than **10%** blow out games of this type. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase3.png" alt="alt text" width="640" height="427">

The trend continues with case 3 blow outs. NBA and MLB have produced **30-40%** blow out games while the NFL and NHL have produced an average of **10%** blow out games. 

Blowout Average

| League | Blowout Rate |
|--------|--------------|
| NBA    | 24.2%        |
| MLB    | 21.8%        |
| NFL    | 5.4%         |
| NHL    | 4.9%         |


**Salary Cap**

Salary caps have been introduced over the last three decades in three of the four leagues: NBA, NHL, and NFL. Major League Baseball is the only major sports league in the United States that does not have a salary cap; however, the MLB does have a luxury tax. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Salarycapbyleague.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHLpayrollandwins.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBApayrollandwins.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLBpayrollandwins.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFLpayrollandwins.png" alt="alt text" width="640" height="427">
..commentary> 

| League | SalaryWinCorrelation |
|--------|----------------------|
| MLB    | 0.6059618            |
| NBA    | 0.6019024            |
| NHL    | 0.6015134            |
| NFL    | 0.03972376           |

**Predictive Modeling**

CART (Classification & Regression Trees) 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBA_blowouts_tree.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLB_blowouts_tree.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHL_closegames_tree.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFL_closegames_tree.png" alt="alt text" width="640" height="427">
..commentary> 


Random Forest



Logistic Regression



Model Accuracy? 

| League | Decision Tree | Random Forest | Logistic Regression |
|--------|---------------|---------------|---------------------|
| NBA    |               |               |                     |
| NHL    |               |               |                     |
| NFL    |               |               |                     |
| MLB    |               |               |                     |




**Conclusion**

To determine the most and least competitive leagues, I calculated a 'Competitive Index' using the results from the analysis. This index is essentially an average of all values. 

![](http://latex.codecogs.com/gif.latex?competitiveIndex%3D%5Cfrac%7BComebackRate&plus;CloseGameRate&plus;%281-BlowoutRate%29&plus;%281-SalaryWinCorrelation%29%7D%7B4%7D)

competitiveIndex is a scale of 0 to 1. 1 being most competitive and 0 being least competitive. 

| League | competitiveIndex |
|--------|------------------|
| NFL    | 0.59             |
| NHL    | 0.46             |
| MLB    | 0.36             |
| NBA    | 0.34             |

And there we have it, the NFL is the most competitive league while the NBA ranks last with a competitiveIndex of 0.34. 


<strong>Update:</strong>

<strong>May 31st, 2017: This post has been updated with the latest NBA and NHL playoff outcomes.</strong>

