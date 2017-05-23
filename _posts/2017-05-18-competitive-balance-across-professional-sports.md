---
layout: post
title: Measuring the Competitive Balance across US Professional Sports Leagues Using R
subtitle: A look at Comebacks, Close Games, Blowouts, and Salary Cap Disparity
published: false
---

This past February, the sports world witnessed one of the most improbable comebacks in sports history and it came during the biggest stage. The New England Patriots rallied from a <a href="http://www.nfl.com/videos/nfl-super-bowl/0ap3000000783876/Patriots-wild-comeback-in-114-seconds" target="_blank">28-3 deficit</a> to beat Atlanta Falcons and win the National Football League (NFL) Super Bowl. Right now hockey fans are being treated to one of the most competitive playoffs ever. The National Hockey League's (NHL) 2017 Stanley Cup Playoffs set a record with <a href="https://www.nhl.com/news/2017-stanley-cup-playoffs-sets-overtime-record/c-289053508" target="_blank">18 overtime games</a>. Sadly for basketball fans not tied to the Cleveland Cavaliers or Golden State Warriors, the NBA finals matchup has been determinde through <a href="https://fivethirtyeight.com/features/the-cavs-and-warriors-might-be-doing-this-finals-thing-for-a-long-time/" target="_blank">salary caps and unrestricted free agency</a>. To make matters worse, in Game 2 of the National Basketball Association (NBA) Eastern Conference Finals, the Cavaliers set a playoff record with a <a href="https://streamable.com/kddo0" target="_blank">41 point halftime</a> lead against the Boston Celtics. 

Postseason drama in the form of improbable comebacks, close games, and the lack of drama through blowout games triggered a question primed for a data-based answer, which professional sports league is the most competitive? 

I define competiveness through the following variables: comebacks, close games, blowouts, and salary disparity across teams. In this analysis I will utilize an expansive <a href="http://developers.stattleship.com/" target="_blank">sports data API</a>, powered by <a href="https://www.stattleship.com/" target="_blank">Stattleship</a>, which provides historical game log data and game scores by quarter to address the following questions: 

-  Which league is most likely to produce a comeback game? 
-  Which league is most likely to produce a close game? 
-  Which league is most likely to produce a blowout game? 
-  Which league has the most linear relationship between wins per season and team salary?
-  Which league is the most predictable? 

With granular box score data, I was able to calculate the score differential at each quarter/inning/intermission point for all regular season and playoff games since 2015. Using these running score differentials, I applied some logic to determine if a game falls into one of our categories. 

** The R code used for data wrangling and analysis can be found here for the <a href="" target="_blank">NBA</a>, <a href="" target="_blank">NHL</a>, <a href="" target="_blank">NFL</a>, and <a href="" target="_blank">MLB</a>. **

**Comebacks**

Comebacks are games where the winning team was down by some deficit at some point in the game. I attempt to provide structure to this definiton by categorizing a game as such if it meets one of the cases below. 

Case 1: Team A is down by X points going into the final quarter and wins the game. 

Case 2: Team A is down by X points going into the third quarter and wins the game. 

_X is the average deficit for losing teams going into the final quarter._

| League | X Threshold |
|--------|-------------|
| NBA    | 9 points    |
| NHL    | 1 goal      |
| NFL    | 9 points    |
| MLB    | 2 runs      |

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase1.png" alt="alt text" width="640" height="427">

Overall, there are very few games that fall into this category. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Comebackcase2.png" alt="alt text" width="640" height="427">

On average, about **10%** of games across the leagues fall into a comeback category. However, there aren't any sweeping statements to be made from this view. 


**Close Games**

Close games are those that are within some score differential, usually those 'competitive' games sports fans remember. I attempt to provide structure to this definiton by categorizing a game as such if it meets one of the cases below. 

Case 1: The game is within Y points through each quarter and final score is within Y points (or game goes to OT). 

Case 2: The game is within Y points going into the final quarter and final score is within Y points (or game goes to OT). 

Case 3: The final score is within Y points (or game goes to OT).

_Y points is the maximum scorable one one possession per sport._ 

| League | Y Threshold |
|--------|-------------|
| NBA    | 4 points    |
| NHL    | 1 goal      |
| NFL    | 8 points    |
| MLB    | 1 run       |

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase1.png" alt="alt text" width="640" height="427">

The NHL and NFL stand out in having **20-40%** of their games in the category where the game was within Y differential through each quarter and final score. Alternatively, the NBA produces less than **5%** of games in this category. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase2.png" alt="alt text" width="640" height="427">

Again, we see NHL and NFL stand out as more likely to produce a comeback game as defined by case 2. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Closegamecase3.png" alt="alt text" width="640" height="427">

And once again, we see NHL and NFL stand out as more likely to produce a comeback game as defined by case 3. 

**Blow Outs**

Blow out games are those that are outside of some score differential, usually those 'non-competitive' and lopsided games sports fans hope to forget. I attempt to provide structure to this definiton by categorizing a game as such if it meets one of the cases below. 

Case 1: The game score deficit is greater than Z points through each quarter and final score deficit is greater than Z points. 

Case 2: The game score deficit is greater than Z points going into the final quarter and final score deficit is greater than Z points. 

Case 3: The final score deficit is greater than Z points.

_Z points is 3x the maximum scorable one one possession per sport._ 

| League | Z Threshold |
|--------|-------------|
| NBA    | 12 points   |
| NHL    | 3 goals     |
| NFL    | 24 points   |
| MLB    | 3 runs      |


<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase1.png" alt="alt text" width="640" height="427">

NBA playoffs have produced just over **7.5%** case 1 blowouts, games over 12 points through each quarter. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase2.png" alt="alt text" width="640" height="427">

NBA and MLB produce **20-30%** case 2 blowouts while the NFL and NHL produce less than **10%** blow out games of this type. 

<img src="https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/Blowoutcase3.png" alt="alt text" width="640" height="427">

The trend continues with case 3 blow outs. NBA and MLB have produced **30-40%** blow out games while the NFL and NHL have produced an average of **10%** blow out games. 

**Salary Cap**

Salary caps have been introduced over the last three decades in three of the four leagues: NBA, NHL, and NFL. Major League Baseball is the only major sports league in the United States that does not have a salary cap; however, the MLB does have a luxury tax. 


![alt text][logo9]

![alt text][logo10]

![alt text][logo11]

![alt text][logo12]

![alt text][logo13]


**Predictive Modeling**

CART (Classification & Regression Trees) 



Random Forest


Logistic Regression



Model Accuracy? 

| League | CART | Random Forest | Logistic Regression |
|--------|------|---------------|---------------------|
| NBA    |      |               |                     |
| NHL    |      |               |                     |
| NFL    |      |               |                     |
| MLB    |      |               |                     |

![alt text][logo14]

![alt text][logo15]

![alt text][logo16]

![alt text][logo17]

![alt text][logo18]

![alt text][logo19]

![alt text][logo20]

![alt text][logo21]


**Conclusion**

NFL and NHL score as most competitive 






<strong>Update:</strong>

<strong>May 31st, 2017: This post has been updated with the latest NBA and NHL playoff outcomes.</strong>


[logo]: link "Comebacks - Case 1"
[logo2]: link "Comebacks - Case 2"
[logo3]: link "Close Game - Case 1"
[logo4]: link "Close Game - Case 2"
[logo5]: link "Close Game - Case 3"
[logo6]: link "Blowout - Case 1"
[logo7]: link "Blowout - Case 2"
[logo8]: (https://raw.githubusercontent.com/martyncisneros/martyncisneros.github.io/master/img/competitive-analysis/BlowOutCase3.png=640x427) "Blowout - Case 3"
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




