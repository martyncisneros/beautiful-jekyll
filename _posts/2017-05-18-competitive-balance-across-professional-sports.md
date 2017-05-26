---
layout: post
title: Measuring the Competitive Balance across U.S. Professional Sports Using R
subtitle: A look at Comebacks, Close Games, Blowouts, Payroll Disparity, and Predictability
bigimg: 
- "/img/competitive-analysis/sports_header.png" : "© 2013 Stadium Management Company, LLC"
published: false
---

This past February, the sports world witnessed one of the most improbable comebacks in sports history and it came during the biggest stage. The New England Patriots rallied from a <a href="http://www.nfl.com/videos/nfl-super-bowl/0ap3000000783876/Patriots-wild-comeback-in-114-seconds" target="_blank">28-3 deficit</a> to beat Atlanta Falcons and win the National Football League (NFL) Super Bowl. Right now hockey fans are being treated to one of the most entertaining playoffs ever. The National Hockey League's (NHL) 2017 Stanley Cup Playoffs set a record with <a href="https://www.nhl.com/news/2017-stanley-cup-playoffs-sets-overtime-record/c-289053508" target="_blank">18 overtime games</a>. On the other hand, the National Basketball Association (NBA) playoffs have been a <a href="https://streamable.com/kddo0" target="_blank">record breaking lopsided affair</a> leading to the inevitable <a href="https://fivethirtyeight.com/features/the-cavs-and-warriors-might-be-doing-this-finals-thing-for-a-long-time/" target="_blank">third straight</a> Cavaliers and Golden State Warriors Finals matchup. 

These recent events triggered a question primed for a data-based answer, **which league (NBA, NHL, NFL, or MLB) is the most competitive?** 

I will measure competitiveness by looking at: 

-  Which league is most likely to produce a comeback game? 
-  Which league is most likely to produce a close game? 
-  Which league is most likely to produce a blowout game? 
-  Which league has the most linear relationship between wins and payroll?
-  Which league is the most predictable? 

In this analysis, I utilized an expansive <a href="http://developers.stattleship.com/" target="_blank">sports data API</a>, powered by <a href="https://www.stattleship.com/" target="_blank">Stattleship</a>, to get historical game data and game scores by quarter. I also scraped <a href="http://www.sports-reference.com/" target="_blank">Sports Reference</a> for team payroll data. 

** The R code used for data wrangling and analysis can be found here for the <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NBA.R" target="_blank">NBA</a>, <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NHL.R" target="_blank">NHL</a>, <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NFL.R" target="_blank">NFL</a>, and <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_MLB.R" target="_blank">MLB</a>. **

First, in order to measure the likelyhood of comebacks, close games, and blowouts, I calculated score differentials at each quarter/inning/intermission point for all regular season and playoff games since 2015. I then used these score differentials by quarter to determine if a game fit one of the categories. 


## **Comebacks**

I define comebacks by two cases: 

Case 1: Team is down by X points going into the final quarter and ends up wining the game. 

![](http://latex.codecogs.com/gif.latex?IsComebackCase1%20%3D%20%28IF%20%5C%2C%20DeltaScoreQ3%20%5Cleq%20XPts%20%5C%2C%20AND%20%5C%2C%20DeltaScoreFinal%20%3E%201%5C%2C%20THEN%20%5C%2C%20Yes%20%5C%2C%20ELSE%20%5C%2C%20No%29)

Case 2: Team is down by X points going into halftime and ends up winning the game. 

![](http://latex.codecogs.com/gif.latex?IsComebackCase2%20%3D%20%28IF%20%5C%2C%20DeltaScoreQ2%20%5Cleq%20XPts%20%5C%2C%20AND%20%5C%2C%20DeltaScoreFinal%20%3E%201%5C%2C%20THEN%20%5C%2C%20Yes%20%5C%2C%20ELSE%20%5C%2C%20No%29)

To determine an appropiate X point threshold to qualify a 'comeback', I chose the average deficit going into the final quarter for a team who lost.

| League | X Threshold |
|--------|-------------|
| NBA    | 9 points    |
| NHL    | 1 goal      |
| NFL    | 9 points    |
| MLB    | 2 runs      |

Example Comeback Game: 

| Game          | Team                 | Q1 | Q2 | Q3 | Q4 | OT | Final |
|---------------|----------------------|----|----|----|----|----|-------|
| Super Bowl 51 | New England Patriots | 0  | 3  | 6  | 19 | 6  | 34    |
| Super Bowl 51 | Atlanta Falcons      | 0  | 21 | 7  | 0  | 0  | 28    |

The game log data I used has two entries per game, 1 per team. I calculated the running differentials by quarter. 

| Game          | Team                 | DeltaQ1 | DeltaQ2 | DeltaQ3 | DeltaQ4 | DeltaFinal |
|---------------|----------------------|---------|---------|---------|---------|------------|
| Super Bowl 51 | New England Patriots | 0       | -18     | -19     | 0       | 6          |
| Super Bowl 51 | Atlanta Falcons      | 0       | 18      | 19      | 0       | -6         |

This game would qualify as both case 1 and case 2 comebacks since the Patriots were down by 18 points at halftime and 19 points going into the 4th but still won the game. Now let's look at the likelyhood of a comeback game across the 4 leagues. 

_Case 1: Down by Xpts going into final quarter and win_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase1.png" alt="alt text" width="640" height="427">

_Case 2: Down by Xpts going into halftime and win_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase2.png" alt="alt text" width="640" height="427">

Overall, the likelyhood of a comeback game is small across the leagues. There aren't any clear signals. Let's now calculate a Global Comeback Rate, to contribute to the final competitiveIndex calculation, for each league by averaging the comeback game rates for regular season and playoffs for both cases. 

| League | Global Comeback Rate |
|--------|----------------------|
| NFL    | 8%                   |
| NBA    | 7.4%                 |
| MLB    | 7.1%                 |
| NHL    | 5.6%                 |


## **Close Games**

I define close games by three cases: 

Case 1: The game is within Y points through each quarter and final score is within Y points (or game goes to OT). 

![](http://latex.codecogs.com/gif.latex?IsCloseGameCase1%20%3D%20%28IF%5C%2C%20ABS%28DeltaScoreQ1%29%5C%2C%5Cleq%5C%2C%20YPts%5C%2CAND%5C%2C%20ABS%28DeltaScoreQ2%29%5C%2C%5Cleq%5C%2C%20YPts%5C%2CAND%5C%2C%20ABS%28DeltaScoreQ3%29%5C%2C%5Cleq%5C%2C%20YPts%5C%2CAND%5C%2C%20%5BABS%28DeltaFinalScore%29%5C%2C%5Cleq%5C%2C%20YPts%5C%2COR%5C%2C%20Overtime%5D%5C%2C%20THEN%5C%2CYes%5C%2CELSE%5C%2CNo%29)

Case 2: The game is within Y points going into the final quarter and final score is within Y points (or game goes to OT).

![](http://latex.codecogs.com/gif.latex?IsCloseGameCase2%20%3D%20%28IF%5C%2C%20ABS%28DeltaScoreQ3%29%5C%2C%5Cleq%5C%2C%20YPts%5C%2CAND%5C%2C%20%5BABS%28DeltaFinalScore%29%5C%2C%5Cleq%5C%2C%20YPts%5C%2COR%5C%2C%20Overtime%5D%5C%2C%20THEN%5C%2CYes%5C%2CELSE%5C%2CNo%29)

Case 3: The final score is within Y points (or game goes to OT).

![](http://latex.codecogs.com/gif.latex?IsCloseGameCase3%20%3D%20%28IF%5C%2C%20ABS%28DeltaFinalScore%29%5C%2C%5Cleq%5C%2C%20YPts%5C%2COR%5C%2C%20Overtime%5C%2C%20THEN%5C%2CYes%5C%2CELSE%5C%2CNo%29)

To determine an appropiate Y point threshold to qualify a 'close game', I chose the maximum scorable points on one possession. 

| League | Y Threshold |
|--------|-------------|
| NBA    | 4 points    |
| NHL    | 1 goal      |
| NFL    | 8 points    |
| MLB    | 1 run       |

Example Close Game Game: 

For our close game example, I chose the <a href="https://streamable.com/t7ac" target="_blank">epic back-n-forth NBA Finals Game 7</a> (aka the undercard to Game of Thrones' <a href="http://www.imdb.com/title/tt4283088/" target="_blank">Battle of the Bastards</a> which aired right after). 

| Game                   | Team                  | Q1 | Q2 | Q3 | Q4 | Final |
|------------------------|-----------------------|----|----|----|----|-------|
| 2016 NBA Finals Game 7 | Golden State Warriors | 22 | 27 | 27 | 13 | 89    |
| 2016 NBA Finals Game 7 | Cleveland Cavaliers   | 23 | 19 | 33 | 18 | 93    |

The game log data I used has two entries per game, 1 per team. I calculated the running differentials by quarter. 

| Game                   | Team                  | DeltaQ1 | DeltaQ2 | DeltaQ3 | DeltaQ4 | DeltaFinal |
|------------------------|-----------------------|---------|---------|---------|---------|------------|
| 2016 NBA Finals Game 7 | Golden State Warriors | -1      | 7       | 1       | -4      | -4         |
| 2016 NBA Finals Game 7 | Cleveland Cavaliers   | 1       | -7      | -1      | 4       | 4          |

This game would qualify as both case 2 and case 3 close games since the game score differential going into the final quarter and final score were within 4 points. However, because the halftime score differential was 7 points, I am not qualifying this game as a case 1 close game. Now let's look at the likelyhood of a close game across the 4 leagues. 

_Case 1: Game is within Y points through each quarter and final score (or OT)_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase1.png" alt="alt text" width="640" height="427">

_Case 2: Game is within Y points going into final quarter and final score (or OT)_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase2.png" alt="alt text" width="640" height="427">

_Case 3: Game's final score is within Y points (or OT)_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase3.png" alt="alt text" width="640" height="427">

The NHL and NFL stand out as most likely to produce close games. Now let's calculate a Global Close Game Rate, to contribute to the final competitiveIndex calculation, for each league by averaging the close game rates for regular season and playoffs for both cases. 


| League | Global Close Game Rate |
|--------|------------------------|
| NHL    | 43.7%                  |
| NFL    | 35.6%                  |
| MLB    | 19.4%                  |
| NBA    | 12.5%                  |


## **Blow Outs**

I define blowouts by three cases: 

Case 1: The game score deficit is greater than Z points through each quarter and final score deficit is greater than Z points. 

![](http://latex.codecogs.com/gif.latex?IsBlowoutCase1%20%3D%20%28IF%5C%2C%20ABS%28DeltaScoreQ1%29%5C%2C%3E%20%5C%2CZPts%5C%2CAND%5C%2C%20ABS%28DeltaScoreQ2%29%5C%2C%3E%20%5C%2CZPts%5C%2CAND%5C%2C%20ABS%28DeltaScoreQ3%29%5C%2C%3E%20%5C%2CZPts%5C%2CAND%5C%2C%20ABS%28DeltaFinalScore%29%5C%2C%3E%20%5C%2CZPts%5C%2CTHEN%5C%2CYes%5C%2CELSE%5C%2CNo%29)

Case 2: The game score deficit is greater than Z points going into the final quarter and final score deficit is greater than Z points. 

![](http://latex.codecogs.com/gif.latex?IsBlowoutCase2%20%3D%20%28IF%5C%2C%20ABS%28DeltaScoreQ3%29%5C%2C%3E%20%5C%2CZPts%5C%2CAND%5C%2C%20ABS%28DeltaFinalScore%29%5C%2C%3E%20%5C%2CZPts%5C%2CTHEN%5C%2CYes%5C%2CELSE%5C%2CNo%29)

Case 3: The final score deficit is greater than Z points. 

![](http://latex.codecogs.com/gif.latex?IsBlowoutCase3%20%3D%20%28IF%5C%2C%20ABS%28DeltaFinalScore%29%5C%2C%3E%20%5C%2C%20ZPts%5C%2C%20THEN%5C%2CYes%5C%2CELSE%5C%2CNo%29)

To determine an appropiate Z point threshold to qualify a 'blowout game', I chose a threshold of three times the maximum scorable points on one possession. 

| League | Z Threshold |
|--------|-------------|
| NBA    | 12 points   |
| NHL    | 3 goals     |
| NFL    | 24 points   |
| MLB    | 3 runs      |

Example Blowout Game: 

| Game              | Team                | Q1 | Q2 | Q3 | Q4 | Final |
|-------------------|---------------------|----|----|----|----|-------|
| 2017 NBA Playoffs | Boston Celtics      | 18 | 13 | 26 | 29 | 86    |
| 2017 NBA Playoffs | Cleveland Cavaliers | 32 | 40 | 31 | 27 | 130   |

The game log data I used has two entries per game, 1 per team. I calculated the running differentials by quarter. 

| Game              | Team                | DeltaQ1 | DeltaQ2 | DeltaQ3 | DeltaQ4 | DeltaFinal |
|-------------------|---------------------|---------|---------|---------|---------|------------|
| 2017 NBA Playoffs | Boston Celtics      | -14     | -41     | -46     | -44     | -44        |
| 2017 NBA Playoffs | Cleveland Cavaliers | 14      | 41      | 46      | 44      | 44         |

This game would qualify as case 1, case 2, and case 3 blowouts since the game differentials are all greater than 12 points..by a mile. Now let's look at the likelyhood of a blowout game across the 4 leagues. 

_Case 1: Game score differential is greater than Z points through each quarter and final score_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase1.png" alt="alt text" width="640" height="427">

_Case 2: Game score differential is greater than Z points going into the final quarter and final score_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase2.png" alt="alt text" width="640" height="427">

_Case 3: Game final score differential is greater than Z points_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase3.png" alt="alt text" width="640" height="427">

The NBA and MLB stand out as most likely to produce blowout games. Let's now calculate a Global Blowout Game Rate, to contribute to the final competitiveIndex calculation, for each league by averaging the blowout rates for regular season and playoffs for both cases. 

Blowout Average

| League | Global Blowout Rate |
|--------|---------------------|
| NBA    | 24.2%               |
| MLB    | 21.8%               |
| NFL    | 5.4%                |
| NHL    | 4.9%                |


## **Salary Cap**

Salary caps have been introduced over the last three decades in three of the four leagues: NBA, NHL, and NFL. Major League Baseball is the only major sports league in the United States that does not have a salary cap; however, it does have a luxury tax in place of a salary cap in order to level the spending an individual team can spend on their roster. In Major League Baseball, their “luxury tax” allows teams to go over the threshold, but at a premium.

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Salarycapbyleague.png" alt="alt text" width="640" height="427">

Another variable that we will consider in our competitiveIndex will be the correlation between total wins in a season and a team's payroll for that season. Let's look at the relationship between wins and salary across the leagues. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHLpayrollandwins.png" alt="alt text" width="640" height="427">

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBApayrollandwins.png" alt="alt text" width="640" height="427">

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLBpayrollandwins.png" alt="alt text" width="640" height="427">

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFLpayrollandwins.png" alt="alt text" width="640" height="427">

Using R's correlation function, I calculated the relationship between wins and salary. 

- Score of 1 means perfect linear relationship
- Score of say 0.7 means strong linear relationship
- Score of 0.5 means a moderate linear relationship 
- Score of 0.3 means a a weak liner relationship
- Score of 0 means no relationship


| League | SalaryWinCorrelation |
|--------|----------------------|
| MLB    | 0.5943053            |
| NBA    | 0.4561342            |
| NFL    | 0.4083673            |
| NHL    | 0.3987338            |


## **Predictive Modeling**



### CART (Classification & Regression Trees) 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBA_blowouts_tree.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLB_blowouts_tree.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHL_closegames_tree.png" alt="alt text" width="640" height="427">
..commentary> 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFL_closegames_tree.png" alt="alt text" width="640" height="427">
..commentary> 


### Random Forest



### Logistic Regression



Model Accuracy? 

| League | Decision Tree | Random Forest | Logistic Regression |
|--------|---------------|---------------|---------------------|
| NBA    |               |               |                     |
| NHL    |               |               |                     |
| NFL    |               |               |                     |
| MLB    |               |               |                     |




## **Conclusion**

To determine the most and least competitive leagues, I calculated a 'Competitive Index' using the results from the analysis. This index is essentially an average of all values. 

![](http://latex.codecogs.com/gif.latex?competitiveIndex%20%3D%20%5Cfrac%7BComebackRate%5C%2C%20&plus;%5C%2C%20CloseGameRate%5C%2C%20&plus;%5C%2C%281%5C%2C%20-%5C%2C%20BlowoutRate%29%5C%2C%20&plus;%5C%2C%20%281%5C%2C%20-%5C%2C%20SalaryWinCorrelation%29%5C%2C%20&plus;%5C%2C%20%281%5C%2C%20-%5C%2C%20PredictiveAccuracy%29%20%7D%7B5%7D)

competitiveIndex is a scale of 0 to 1. 1 being most competitive and 0 being least competitive. 

| League | competitiveIndex |
|--------|------------------|
| NHL    | 0.51             |
| NFL    | 0.49             |
| NBA    | 0.38             |
| MLB    | 0.36             |

And there we have it, the NHL is the most competitive league as its competitiveIndex of 0.51 confirms <a href="http://wpmedia.o.canada.com/2013/12/flames_rangers_hockey_213564240.jpg" target="_blank">hurdles</a> the other leagues. 


<strong>Update:</strong>

<strong>May 31st, 2017: This post has been updated with the latest NBA and NHL playoff outcomes.</strong>

