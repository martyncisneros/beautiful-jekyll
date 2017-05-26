---
layout: post
title: Measuring the Competitive Balance across U.S. Professional Sports Using R
subtitle: A look at Comebacks, Close Games, Blowouts, Payroll Disparity, and Predictability
bigimg: 
- "/img/competitive-analysis/sports_header.png" : "© 2013 Stadium Management Company, LLC"
published: false
---

This past February, the sports world witnessed one of the most improbable comebacks in sports history. The New England Patriots rallied from a <a href="http://www.nfl.com/videos/nfl-super-bowl/0ap3000000783876/Patriots-wild-comeback-in-114-seconds" target="_blank">28-3 deficit</a> to beat Atlanta Falcons and win the NFL Super Bowl. Right now hockey fans are being treated to one of the most entertaining playoffs ever. The NHL 2017 Stanley Cup Playoffs set a record with <a href="https://www.nhl.com/news/2017-stanley-cup-playoffs-sets-overtime-record/c-289053508" target="_blank">18 overtime games</a>. On the other hand, the NBA Playoffs have been a <a href="https://streamable.com/kddo0" target="_blank">record breaking lopsided affair</a> leading to the inevitable <a href="https://fivethirtyeight.com/features/the-cavs-and-warriors-might-be-doing-this-finals-thing-for-a-long-time/" target="_blank">third straight</a> Cavaliers vs. Golden State Warriors Finals matchup. 

These recent events triggered a question primed for a data-based answer, **which league (NBA, NHL, NFL, or MLB) is the most competitive?** 

I will measure competitiveness by looking at: 

-  Which league is most likely to produce a comeback game? 
-  Which league is most likely to produce a close game? 
-  Which league is most likely to produce a blowout game? 
-  Which league has the most linear relationship between wins and payroll?
-  Which league is the most predictable? 

In this analysis, I utilized an expansive <a href="http://developers.stattleship.com/" target="_blank">sports data API</a>, powered by <a href="https://www.stattleship.com/" target="_blank">Stattleship</a>, to get historical game data and game scores by quarter. I also scraped <a href="http://www.sports-reference.com/" target="_blank">Sports Reference</a> for team payroll data. 

#### **_The R code used for data wrangling and analysis can be found here for the <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NBA.R" target="_blank">NBA</a>, <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NHL.R" target="_blank">NHL</a>, <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NFL.R" target="_blank">NFL</a>, and <a href="https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_MLB.R" target="_blank">MLB</a>._**

First, in order to measure the likelyhood of comebacks, close games, and blowouts, I used the granular box score data to calculate game score differentials at each quarter/inning/intermission break for all regular season and playoff games since 2015. I then used these score differentials to determine if a game fit one of the categories. 


## **Comebacks**

I define comebacks by two cases: 

> Case 1: Team is down by X points going into the final quarter and ends up wining the game. 

> Case 2: Team is down by X points going into halftime and ends up winning the game. 


To determine an appropiate X point threshold to qualify a 'comeback', I looked at the the average deficit going into the final quarter for a team who ultimately lost the game. 

| League | X Threshold |
|--------|-------------|
| NBA    | 9 points    |
| NHL    | 1 goal      |
| NFL    | 9 points    |
| MLB    | 2 runs      |

Example Comeback Game: 

Box Score

| Game          | Team                 | Q1 | Q2 | Q3 | Q4 | OT | Final |
|---------------|----------------------|----|----|----|----|----|-------|
| Super Bowl 51 | New England Patriots | 0  | 3  | 6  | 19 | 6  | 34    |
| Super Bowl 51 | Atlanta Falcons      | 0  | 21 | 7  | 0  | 0  | 28    |

Calculated Game Score Differentials

| Game          | Team                 | DeltaQ1 | DeltaQ2 | DeltaQ3 | DeltaQ4 | DeltaFinal |
|---------------|----------------------|---------|---------|---------|---------|------------|
| Super Bowl 51 | New England Patriots | 0       | -18     | -19     | 0       | 6          |
| Super Bowl 51 | Atlanta Falcons      | 0       | 18      | 19      | 0       | -6         |

This game would qualify as both case 1 and case 2 comebacks since the Patriots were down by 18 points at halftime and 19 points going into the 4th but still won the game. Now let's look at the likelyhood of a comeback game across the 4 leagues. 

_Case 1: Down by Xpts going into final quarter and win_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase1.png" alt="alt text" width="640" height="427">

_Case 2: Down by Xpts going into halftime and win_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase2.png" alt="alt text" width="640" height="427">

Overall, the likelyhood of a comeback game is small across the leagues. There aren't any clear signals. I average the comeback game rates for regular season, playoffs, and both cases to arrive at a Global Comeback Rate. We'll use this rate later in the final competitiveIndex calculation. 

| League | Global Comeback Rate |
|--------|----------------------|
| NFL    | 8%                   |
| NBA    | 7.4%                 |
| MLB    | 7.1%                 |
| NHL    | 5.6%                 |


## **Close Games**

I define close games by three cases: 

> Case 1: The game is within Y points through each quarter and final score is within Y points (or game goes to OT). 

> Case 2: The game is within Y points going into the final quarter and final score is within Y points (or game goes to OT).

> Case 3: The final score is within Y points (or game goes to OT).


For the Y point threshold to qualify a 'close game', I chose the maximum scorable points on one possession. 

| League | Y Threshold |
|--------|-------------|
| NBA    | 4 points    |
| NHL    | 1 goal      |
| NFL    | 8 points    |
| MLB    | 1 run       |

Example Close Game Game: 

For our close game example, I chose the <a href="https://streamable.com/t7ac" target="_blank">epic back-n-forth NBA Finals Game 7</a> (aka the undercard to Game of Thrones' <a href="http://www.imdb.com/title/tt4283088/" target="_blank">Battle of the Bastards</a> which aired right after). 

Box Score

| Game                   | Team                  | Q1 | Q2 | Q3 | Q4 | Final |
|------------------------|-----------------------|----|----|----|----|-------|
| 2016 NBA Finals Game 7 | Golden State Warriors | 22 | 27 | 27 | 13 | 89    |
| 2016 NBA Finals Game 7 | Cleveland Cavaliers   | 23 | 19 | 33 | 18 | 93    |

Calculated Game Score Differentials

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

The NHL and NFL stand out as most likely to produce close games. I average the close game rates for regular season, playoffs, and all three cases to arrive at a Global Close Game Rate. We'll use this rate later in the final competitiveIndex calculation. 


| League | Global Close Game Rate |
|--------|------------------------|
| NHL    | 43.7%                  |
| NFL    | 35.6%                  |
| MLB    | 19.4%                  |
| NBA    | 12.5%                  |


## **Blow Outs**

I define blowouts by three cases: 

> Case 1: The game score deficit is greater than Z points through each quarter and final score deficit is greater than Z points. 

> Case 2: The game score deficit is greater than Z points going into the final quarter and final score deficit is greater than Z points. 

> Case 3: The final score deficit is greater than Z points. 


For the Z point threshold to qualify a 'blowout game', I chose three times the maximum scorable points on one possession. 

| League | Z Threshold |
|--------|-------------|
| NBA    | 12 points   |
| NHL    | 3 goals     |
| NFL    | 24 points   |
| MLB    | 3 runs      |

Example Blowout Game: 

Box Score

| Game              | Team                | Q1 | Q2 | Q3 | Q4 | Final |
|-------------------|---------------------|----|----|----|----|-------|
| 2017 NBA Playoffs | Boston Celtics      | 18 | 13 | 26 | 29 | 86    |
| 2017 NBA Playoffs | Cleveland Cavaliers | 32 | 40 | 31 | 27 | 130   |

Calculated Game Score Differentials

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

The NBA and MLB stand out as most likely to produce blowout games. I average the blowout rates for regular season, playoffs, and all three cases to arrive at a Global Blowout Game Rate. We'll use this rate later in the final competitiveIndex calculation.


| League | Global Blowout Rate |
|--------|---------------------|
| NBA    | 24.2%               |
| MLB    | 21.8%               |
| NFL    | 5.4%                |
| NHL    | 4.9%                |


## **Salary Cap**

Salary caps were introduced to the NBA, NHL, and NFL over the last three decades with the goal to reduce the competitive imbalance between bigger spending, large market clubs and the lower revenue, smaller market clubs. The NHL and NFL's caps are considered "hard", meaning that they offer relatively few (if any) circumstances under which teams can exceed the salary cap. The NBA feautures a "soft" cap, meaning that there are several significant exceptions that allow teams to exceed the salary cap to sign players. In place of a salary cap, the MLB implemented a luxury tax which allow teams to spend as much as they want on salary, but it penalizes them a percentage of the amount by which they exceed a threshold. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Salarycapbyleague.png" alt="alt text" width="640" height="427">

I scraped the latest payroll numbers for every team across the four leagues, to determine the correlation between total wins in a season and a team's payroll for that season. We will use these correlations in our own competitiveIndex calculation of parity in sports. 

Using R's correlation function, I calculated the relationship between wins and salary. 

> Score of 1 means perfect linear relationship

> Score of say 0.7 means strong linear relationship

> Score of 0.5 means a moderate linear relationship 

> Score of 0.3 means a a weak liner relationship

> Score of 0 means no relationship

_SalaryWinCorrelation = 0.5943053_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLBpayrollandwins.png" alt="alt text" width="640" height="427">

_SalaryWinCorrelation = 0.4561342_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBApayrollandwins.png" alt="alt text" width="640" height="427">

_SalaryWinCorrelation = 0.4083673_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFLpayrollandwins.png" alt="alt text" width="640" height="427">

_SalaryWinCorrelation = 0.3987338_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHLpayrollandwins.png" alt="alt text" width="640" height="427">

Hard cap leagues do have a weaker correlation between salary and wins, good sign for parity in the NFL and NHL. We'll use this correlation later in the final competitiveIndex calculation. 

| League | SalaryWinCorrelation |
|--------|----------------------|
| MLB    | 0.5943053            |
| NBA    | 0.4561342            |
| NFL    | 0.4083673            |
| NHL    | 0.3987338            |


## **Predictive Modeling**

The final variable in our competitiveIndex calculation will be a measure of how predictable the four leagues are. In this part of the analysis, I look to predict Case 3 blow outs for the NBA and MLB and Case 3 close games for the NFL and NHL. These were the most likely cases for each league and will provide the largest sample size of game data. 

For the NBA and MLB, our dependent variable is whether or not a game was a blowout. This is a binary variable taking value 1 if the game met the case 3 criteria, and taking value 0 if a game did not meet the blowout game criteria. Our independent variables are calculated absolute value differentials of various game statistics. These vary by league. For the NFL and NHL, I looked at an dependent variable of whether or not a game was close. 

### Decision Tree Model 

The first method I use is called classification and regression trees, or CART. This method builds what is called a tree by splitting on the values of the independent variables. To predict the outcome for a new observation or case, you can follow the splits in the tree and at the end, you predict the most frequent outcome in the training set that followed the same path.

Some advantages of CART are that it does not assume a linear model, like logistic regression or linear regression, and it's a very interpretable model.



<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBA_blowouts_tree.png" alt="alt text" width="640" height="427">


<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLB_blowouts_tree.png" alt="alt text" width="640" height="427">


<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHL_closegames_tree.png" alt="alt text" width="640" height="427">


<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFL_closegames_tree.png" alt="alt text" width="640" height="427">



### Random Forest Model 






### Logistic Regression Model 








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

competitiveIndex is a measure of competitiveness on a scale of 0 to 1 

> 1 being most competitive 

> 0 being least competitive 


| League | competitiveIndex |
|--------|------------------|
| NHL    | 0.51             |
| NFL    | 0.49             |
| NBA    | 0.38             |
| MLB    | 0.36             |

And there we have it, the NHL is the most competitive league with competitiveIndex score of 0.51. The NFL is close behind with a score of 0.49. Anecdotally, these results align with my own ranking of live sporting events to attend. 

Thanks for reading, send me a message with your thoughts. 


<strong>Update:</strong>

<strong>May 31st, 2017: This post has been updated with the latest NBA and NHL playoff outcomes.</strong>

