---
layout: post
title: Measuring Competitive Balance across US Professional Sports Leagues Using R
subtitle: A look at Comebacks, Close Games, Blowouts, and Salary Cap Disparity
published: false
---

This past February, the sports world witnessed one of the most improbable comebacks in sports history and it came during the sports world's biggest stage. The New England Patriots rallied from a 28-3 deficit to beat Atlanta Falcons and win the National Football League (NFL) Super Bowl. The National Hockey League's (NHL) 2017 Stanley Cup Playoffs set a record with 18 overtime games. The National Basketball Association (NBA) playoffs have been underwhelming. In Game 2 of the Eastern Conference Finals, the Cleveland Cavaliers set a NBA playoff record with a <a href="https://streamable.com/kddo0" target="_blank">41-point halftime</a> lead against the Boston Celtics. The postseason excitement and disappointments have brought about a question I was itching to answer, which professional sports league is the most competitive? 

To answer this question, I needed to expand on the following:

-  Which leagues are most likely to produce comebacks? 
-  Which leagues are most likely to produce close games? 
-  Which leagues are most likely to produce blowouts? 
-  Which leagues have the most linear relationships between wins per season and team salary?
-  Which league is the most predictable? 

I came across an expansive <a href="http://developers.stattleship.com/" target="_blank">sports data API</a> powered by Stattleship. This API most importantly includes a team game logs endpoint which provides historical game logs with scores by quarter. With this granular box score data I was able to find the score deficits at each breakpoint for all regular season and playoff games since 2015. 

**The exhaustive R code used for data wrangling and analysis can be found here for the <a href="" target="_blank">NBA</a>, <a href="" target="_blank">NHL</a>, <a href="" target="_blank">NFL</a>, and <a href="" target="_blank">MLB</a>.**

I will first cover the approach I took to define a comeback, close game, and blow out across all four leagues. 

**Comeback**

Case 1: Team A is down by X points going into the final quarter and wins the game. 

Case 2: Team A is down by X points going into the third quarter and wins the game. 

_X points _

**Close Game**

Case 1: The game is within Y points through each quarter and final score is within Y points (or game goes to OT). 

Case 2: The game is within Y points going into the final quarter and final score is within Y points (or game goes to OT). 

Case 3: The final score is within Y points (or game goes to OT).

_Y points is the maximum scorable one one possession per sport._ 

**Blow Out**

Case 1: The game score deficit is greater than Z points through each quarter and final score deficit is greater than Z points. 

Case 2: The game score deficit is greater than Z points going into the final quarter and final score deficit is greater than Z points. 

Case 3: The final score deficit is greater than Z points.

_Z points is 3x the maximum scorable one one possession per sport._ 







Salary caps have been introduced over the last three decades in three of the four major sports leagues in the United States: the National Basketball Association (NBA), the National Hockey League (NHL), and the National Football League (NFL). Major League Baseball is the only major sports league in the United States that does not have a salary cap; however, the MLB does have a luxury tax which is ...... 









