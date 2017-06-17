---
layout: post
title: Measuring the Competitive Balance across US Professional Sports Using R
subtitle: A look at Comebacks, Close Games, Blowouts, Payroll Disparity, and Predictability
bigimg: 
- "/img/competitive-analysis/sports_header_2.png" : "wallpapercave.com"
published: true
---

<!--bigimg: 
- "/img/competitive-analysis/sports_header.png" : "© 2013 Stadium Management Company, LLC"-->

This past February, the sports world witnessed one of the most improbable comebacks in history. The New England Patriots rallied from a <a href="http://www.nfl.com/videos/nfl-super-bowl/0ap3000000783876/Patriots-wild-comeback-in-114-seconds" target="_blank">28-3 deficit</a> to beat the Atlanta Falcons and win the NFL Super Bowl. Right now, hockey fans are being treated to one of the most entertaining NHL postseasons ever. The 2017 playoffs have set a record with <a href="https://www.nhl.com/news/2017-stanley-cup-playoffs-sets-overtime-record/c-289053508" target="_blank">18 first round overtime games</a>. On the other hand, the 2017 NBA Playoffs have drawn some criticism as the lack of parity this year has lead to the inevitable <a href="https://fivethirtyeight.com/features/the-cavs-and-warriors-might-be-doing-this-finals-thing-for-a-long-time/" target="_blank">third straight</a> Cleveland Cavaliers vs. Golden State Warriors Finals matchup. The Warriors went 12-0 against the Western Conference with an average margin of victory of 16.3 points, <a href="https://twitter.com/ESPNStatsInfo/status/866865018637299712?ref_src=twsrc%5Etfw&ref_url=http%3A%2F%2Fwww.sacbee.com%2Fsports%2Fnba%2Farticle152077367.html" target="_blank">per ESPN Stats and Info</a>. The Cavaliers won 12 of 13 games against the Eastern Conference, with all four of their wins over the Boston Celtics coming by at least 13 points, including this <a href="https://streamable.com/kddo0" target="_blank">record breaking lopsided affair</a>.

These recent events triggered a question primed for a data-based answer, **which league (NBA, NHL, NFL, or MLB) is the most competitive?** 

In this post, I propose an empirical method for comparing competitiveness across leagues. I measured competitiveness by looking at: 

-  [Which league is most likely to produce a comeback game?](#comebacks)
-  [Which league is most likely to produce a close game?](#closegame) 
-  [Which league is most likely to produce a blowout game?](#blowout) 
-  [Which league has the most linear relationship between wins and payroll?](#payroll) 
-  [Which league is the most predictable?](#predictability) 

Click [here](#conclusion) to the jump to final competitiveIndex calculation. 

In this analysis, I utilized an expansive <a href="http://developers.stattleship.com/" target="_blank">sports data API</a>, powered by <a href="https://www.stattleship.com/" target="_blank">Stattleship</a>, to get historical game data and game scores by quarter. I also scraped <a href="http://www.sports-reference.com/" target="_blank">Sports Reference</a> for team payroll data. 

#### **_The R code used for data wrangling and analysis can be found here for the <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NBA.html" target="_blank">NBA</a>, <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NHL.html" target="_blank">NHL</a>, <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NFL.html" target="_blank">NFL</a>, and <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_MLB.html" target="_blank">MLB</a>._**

First, in order to measure the likelihood of comebacks, close games, and blowouts, I used the granular box score data to calculate game score differentials at each quarter/inning/intermission break for all regular season and playoff games since 2015. I then used these score differentials to determine if a game fit one of the categories. 


## **Comebacks** <a name="comebacks"></a>

I define comebacks by two cases: 

> Case 1: Team is down by X points going into the final quarter and ends up wining the game. 

> Case 2: Team is down by X points going into halftime and ends up winning the game. 


To determine an appropriate X point threshold to qualify a 'comeback', I looked at the average deficit going into the final quarter for a team who ultimately lost the game. 

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

This game would qualify as both case 1 and case 2 comebacks since the Patriots were down by 18 points at halftime and 19 points going into the 4th but still won the game. Now let's look at the likelihood of a comeback game across the 4 leagues. 

_Case 1: Down by Xpts going into final quarter and win_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase1.png" alt="alt text" width="640" height="427">

_Case 2: Down by Xpts going into halftime and win_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase2.png" alt="alt text" width="640" height="427">

Overall, the likelihood of a comeback game is small across the leagues. There aren't any clear signals. I average the comeback game rates for regular season, playoffs, and both cases to arrive at a Global Comeback Rate. We'll use this rate later in the final competitiveIndex calculation. 

| League | Global Comeback Rate |
|--------|----------------------|
| NFL    | 8%                   |
| NBA    | 7.4%                 |
| MLB    | 7.1%                 |
| NHL    | 5.6%                 |


## **Close Games** <a name="closegame"></a>

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

Example Close Game: 

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

This game would qualify as both case 2 and case 3 close games since the game score differential going into the final quarter and final score were within 4 points. However, because the halftime score differential was 7 points, I am not qualifying this game as a case 1 close game. Now let's look at the likelihood of a close game across the 4 leagues. 

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


## **Blow Outs** <a name="blowout"></a>

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

This game would qualify as case 1, case 2, and case 3 blowouts since the game differentials are all greater than 12 points... by a mile. Now let's look at the likelihood of a blowout game across the 4 leagues. 

_Case 1: Game score differential is greater than Z points through each quarter and final score_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase1.png" alt="alt text" width="640" height="427">

_Case 2: Game score differential is greater than Z pts into the final quarter and final score_
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


## **Salary Cap** <a name="payroll"></a>

Salary caps were introduced to the NBA, NHL, and NFL over the last three decades with the goal to reduce the competitive imbalance between bigger spending, large market clubs and the lower revenue, smaller market clubs. The NHL and NFL's caps are considered "hard", meaning that they offer relatively few (if any) circumstances under which teams can exceed the salary cap. The NBA features a "soft" cap, meaning that there are several significant exceptions that allow teams to exceed the salary cap to sign players. In place of a salary cap, the MLB implemented a luxury tax, which allow teams to spend as much as they want on salary, but it penalizes them a percentage of the amount by which they exceed a threshold. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Salarycapbyleague.png" alt="alt text" width="640" height="427">

I scraped the latest payroll numbers for every team across the four leagues, to determine the correlation between total wins in a season and a team's payroll for that season. We will use these correlations in our own competitiveIndex calculation of parity in sports. 

Using R's correlation function, I calculated the relationship between wins and salary. 

> Score of 1 means perfect linear relationship

> Score of 0.7 means strong linear relationship

> Score of 0.5 means moderate linear relationship 

> Score of 0.3 means weak linear relationship

> Score of 0 means no relationship

_SalaryWinCorrelation = 0.7262808_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBApayrollandwins.png" alt="alt text" width="640" height="427">

_SalaryWinCorrelation = 0.5943053_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLBpayrollandwins.png" alt="alt text" width="640" height="427">

_SalaryWinCorrelation = 0.4539921_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHLpayrollandwins.png" alt="alt text" width="640" height="427">

_SalaryWinCorrelation = 0.4083673_
<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFLpayrollandwins.png" alt="alt text" width="640" height="427">

Hard cap leagues do have a weaker correlation between salary and wins, good sign for parity in the NFL and NHL. We'll use this correlation later in the final competitiveIndex calculation. 

| League | SalaryWinCorrelation |
|--------|----------------------|
| NBA    | 0.7262808            |
| MLB    | 0.5943053            |
| NHL    | 0.4539921            |
| NFL    | 0.4083673            |


## **Predictive Modeling** <a name="predictability"></a>

The final variable in our competitiveIndex calculation will be a measure of predictability across the leagues. In this part of the analysis, I look to predict Case 3 blowouts for the NBA and MLB and Case 3 close games for the NFL and NHL. These were the most likely cases for each league and will provide the largest sample size of game data. 

For the NBA and MLB, our dependent variable is whether or not a game was a blowout. This is a binary variable taking a value of 1 if the game met the case 3 criteria, and taking a value of 0 if a game did not meet the blowout game criteria. For the NFL and NHL, I looked at a dependent variable of whether or not a game was close. The independent variables vary by league and are calculated absolute value differentials of various game statistics. We will create two models and see if there are any game stats that drive close or blowout game outcomes. 

#### **_Again, for those interested, the R code used for data wrangling and analysis can be found here for the <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NBA.html" target="_blank">NBA</a>, <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NHL.html" target="_blank">NHL</a>, <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_NFL.html" target="_blank">NFL</a>, and <a href="http://htmlpreview.github.io/?https://github.com/martyncisneros/sports_competitive_analysis/blob/master/Competitive_Analysis_MLB.html" target="_blank">MLB</a>._**


### Classification Decision Tree Model 

The first method I use is called classification and regression trees, or CART. This method builds what is called a tree by splitting on the values of the independent variables. To predict the outcome for a new observation or case, you can follow the splits in the tree and at the end, you predict the most frequent outcome in the training set that followed the same path.

Some advantages of CART are that it does not assume a linear model, like linear or log regression, and it's a very interpretable model.

**NBA** 

I made training and test data sets with variables like DeltaTotalAssists, DeltaFieldGoalPercentage, DeltaTurnovers, DeltaOffensiveRebounds, etc. I then created a CART model to predict blowout games by looking at the differentials from these and other basketball game stats. 

Each node (or leaf) shows:

> the predicted outcome (blowout = 1 or not a blowout = 0) 

> the predicted probability of a blowout

> the percentage of observations in the node

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBA_blowouts_tree.png" alt="alt text" width="640" height="427">

Now let's see how well our CART model did at making predictions for the test set. We can measure the accuracy of the tree model by creating a confusion matrix. 

_NBA Tree Model Accuracy_ = 0.7106798

A baseline model that always predicts not a blowout, which is the most common outcome, has an accuracy of 0.6412214. Our NBA CART model is better than baseline. Lastly, let's generate an ROC curve to evaluate our CART model. The Yhat Blog has a good <a href="http://blog.yhat.com/posts/roc-curves.html" target="_blank">post explaining ROC curves</a>. 

Basically..

This is a perfect model: 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/roc-perfect.png" alt="alt text" width="640" height="427">

This is random guessing: 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/roc-guessing.png" alt="alt text" width="640" height="427">

We can calculate an AUC (or area under curve) to quantify how good the model is. 

> AUC of 1 is good

> AUC of .5 is random guessing

_NBA ROC Curve_

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NBA_roc_curve.png" alt="alt text" width="640" height="427">

Now that we have a better idea of a CART model and how to quantify its significance, I'll quickly run through the results for other leagues..

**MLB** 

Each node shows:

> the predicted outcome (blowout = 1 or not a blowout = 0) 

> the predicted probability of a blowout

> the percentage of observations in the node

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLB_blowouts_tree.png" alt="alt text" width="640" height="427">

_MLB Tree Model Accuracy_ = 0.7455219

A baseline model that always predicts not a blowout, which is the most common outcome, has an accuracy of 0.6213712. Our MLB CART model is better than baseline. 

_MLB ROC Curve_

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/MLB_roc_curve.png" alt="alt text" width="640" height="427">

**NFL** 


Each node shows: 

> the predicted outcome (close game = 1 or not a close game = 0) 

> the predicted probability of a close game

> the percentage of observations in the node

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFL_closegames_tree.png" alt="alt text" width="640" height="427">

_NFL Tree Model Accuracy_ = 0.5875

A baseline model that always predicts not a close game, which is the most common outcome, has an accuracy of 0.4625. Our NFL CART model is better than baseline but only slightly better than random guessing. 

_NFL ROC Curve_

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NFL_roc_curve.png" alt="alt text" width="640" height="427">


**NHL** 

Each node shows: 

> the predicted outcome (close game = 1 or not a close game = 0) 

> the predicted probability of a close game

> the percentage of observations in the node

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHL_closegames_tree.png" alt="alt text" width="640" height="427">

_NHL Tree Model Accuracy_ = 0.531052

A baseline model that always predicts not a close game, which is the most common outcome, has an accuracy of 0.561052. Our NHL CART model is slightly worse than baseline and only slightly better than random guessing. 

_NHL ROC Curve_

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/NHL_roc_curve.png" alt="alt text" width="640" height="427">



| League | CART Model  Accuracy | 
|--------|----------------------|
| MLB    |         0.74         |                           
| NBA    |         0.71         |                        
| NFL    |         0.58         |                          
| NHL    |         0.53         |          


### Random Forest Model 

The second method I use is called Random Forest. This method was designed to improve the prediction accuracy of CART and works by building a large number of CART trees. To make a prediction for a new observation, each tree in the forest votes on the outcome and we pick the outcome that receives the majority of the votes. 

Using R's randomForest package, I created a model for each league using the same variables we used for CART. Random Forest model improved the accuracy over the previous CART method for each league. We'll use these accuracy results in the competitiveIndex calculation. 

| League | Random Forest Model Accuracy | 
|--------|------------------------------|
| NBA    |              0.77            |   
| MLB    |              0.75            |                                                
| NFL    |              0.60            |                          
| NHL    |              0.60            |    

                  
## **Conclusion** <a name="conclusion"></a>

To determine the most and least competitive leagues, I calculated a 'Competitive Index' using the results from the analysis.

![](http://latex.codecogs.com/gif.latex?competitiveIndex%20%3D%20%5Cfrac%7BComebackRate%5C%2C%20&plus;%5C%2C%20CloseGameRate%5C%2C%20&plus;%5C%2C%281%5C%2C%20-%5C%2C%20BlowoutRate%29%5C%2C%20&plus;%5C%2C%20%281%5C%2C%20-%5C%2C%20SalaryWinCorrelation%29%5C%2C%20&plus;%5C%2C%20%281%5C%2C%20-%5C%2C%20PredictiveAccuracy%29%20%7D%7B5%7D)

This index is essentially an average of all values and a measure of competitiveness on a scale of 0 to 1 

> 1 being most competitive 

> 0 being least competitive 


| League | competitiveIndex |
|--------|------------------|
| NHL    | 0.48             |
| NFL    | 0.47             |
| MLB    | 0.34             |
| NBA    | 0.29             |

And there we have it; the NHL is the <a href="https://youtu.be/4CMGYetKKP8?t=11s" target="_blank">most competitive league</a> with competitiveIndex score of 0.48. The NFL is close behind with a score of 0.47. Next, I will create an <a href="https://shiny.rstudio.com/gallery/" target="_blank">R Shiny</a> application that will allow us to manipulate the point thresholds for the various categories. For now, thanks for reading and be sure to send me a message with your thoughts. 

<strong>Update:</strong>

<strong>May 31st, 2017: This post has been updated with the latest NBA and NHL 2017 playoff outcomes.</strong>

<strong>June 13th, 2017: This post has been updated with the conclusion of both NBA and NHL 2017 finals.</strong>

