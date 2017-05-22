---
layout: post
title: Measuring the Competitive Balance across US Professional Sports Leagues Using R
subtitle: A look at Comebacks, Close Games, Blowouts, and Salary Cap Disparity
published: false
---

This past February, the sports world witnessed one of the most improbable comebacks in sports history and it came during the sports world's biggest stage. The New England Patriots rallied from a <a href="http://www.nfl.com/videos/nfl-super-bowl/0ap3000000783876/Patriots-wild-comeback-in-114-seconds" target="_blank">28-3 deficit</a> to beat Atlanta Falcons and win the National Football League (NFL) Super Bowl. The National Hockey League's (NHL) 2017 Stanley Cup Playoffs set a record with <a href="https://www.nhl.com/news/2017-stanley-cup-playoffs-sets-overtime-record/c-289053508" target="_blank">18 overtime games</a>. In Game 2 of the National Basketball Association (NBA) Eastern Conference Finals, the Cleveland Cavaliers set a playoff record with a <a href="https://streamable.com/kddo0" target="_blank">41 point halftime</a> lead against the Boston Celtics. Postseason drama in the form of improbable comebacks, close games, and the lack of drama with blowout games have brought about a question I was itching to answer, which professional sports league is the most competitive? 

To answer this question, I needed to expand on the following:

-  Which leagues are most likely to produce comebacks? 
-  Which leagues are most likely to produce close games? 
-  Which leagues are most likely to produce blowouts? 
-  Which leagues have the most linear relationships between wins per season and team salary?
-  Which league is the most predictable? 

I came across an expansive <a href="http://developers.stattleship.com/" target="_blank">sports data API</a> powered by Stattleship. This API most importantly includes a team game logs endpoint which provides historical game logs with scores by quarter. With this granular box score data I was able to find the score deficits at each breakpoint for all regular season and playoff games since 2015. 

** The R code used for data wrangling and analysis can be found here for the <a href="" target="_blank">NBA</a>, <a href="" target="_blank">NHL</a>, <a href="" target="_blank">NFL</a>, and <a href="" target="_blank">MLB</a>. **

I will first cover the approach I took to define a comeback, close game, and blow out across all four leagues. 

**Comeback**

Case 1: Team A is down by X points going into the final quarter and wins the game. 

Case 2: Team A is down by X points going into the third quarter and wins the game. 

_X is the average deficit for losing teams going into the final quarter._

![alt text][logo]

![alt text][logo2]


**Close Game**

Case 1: The game is within Y points through each quarter and final score is within Y points (or game goes to OT). 

Case 2: The game is within Y points going into the final quarter and final score is within Y points (or game goes to OT). 

Case 3: The final score is within Y points (or game goes to OT).

_Y points is the maximum scorable one one possession per sport._ 

![alt text][logo3]

![alt text][logo4]

![alt text][logo5]


**Blow Out**

Case 1: The game score deficit is greater than Z points through each quarter and final score deficit is greater than Z points. 

Case 2: The game score deficit is greater than Z points going into the final quarter and final score deficit is greater than Z points. 

Case 3: The final score deficit is greater than Z points.

_Z points is 3x the maximum scorable one one possession per sport._ 


![alt text][logo6]

![alt text][logo7]

![alt text][logo8]


**Salary Cap**

Salary caps have been introduced over the last three decades in three of the four leagues: NBA, NHL, and NFL. Major League Baseball is the only major sports league in the United States that does not have a salary cap; however, the MLB does have a luxury tax. 

![alt text][logo9]

![alt text][logo10]

![alt text][logo11]

![alt text][logo12]

![alt text][logo13]


**Predictive Modeling**

![alt text][logo14]

![alt text][logo15]

![alt text][logo16]

![alt text][logo17]

![alt text][logo18]

![alt text][logo19]

![alt text][logo20]

![alt text][logo21]









<strong>Update:</strong>

<strong>May 31st, 2017: This post has been updated with the latest NBA and NHL playoff outcomes.</strong>


[logo]: link "Comebacks - Case 1"
[logo2]: link "Comebacks - Case 2"
[logo3]: link "Close Game - Case 1"
[logo4]: link "Close Game - Case 2"
[logo5]: link "Close Game - Case 3"
[logo6]: link "Blowout - Case 1"
[logo7]: link "Blowout - Case 2"
[logo8]: link "Blowout - Case 3"
[logo9]: link "Salary Cap by League"
[logo10]: link "NBA Salary v Wins"
[logo11]: link "NHL Salary v Wins"
[logo12]: link "NFL Salary v Wins"
[logo13]: link "MLB Salary v Wins"
[logo14]: link "NBA Decision Tree"
[logo15]: link "NBA ROC Curve"
[logo16]: link "NHL Decision Tree"
[logo17]: link "NHL ROC Curve"
[logo18]: link "NFL Decision Tree"
[logo19]: link "NFL ROC Curve"
[logo20]: link "MLB Decision Tree"
[logo21]: link "MLB ROC Curve"









