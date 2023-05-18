# DSC80 Project 3: Data Science behind LOL
**Name**: Housheng Hai
this is a project analyzing hero "Sylas"'s winning rate in the game League of Legends for DSC 80 at UCSD

### Introduction

The data set I am going to study is the game data of professional players in various League of Legends matches. The data set contains match data from the LCS, LEC, LCK, LPL, PCS, CBLoL, and many more leagues.Since I am a player of League of Legends, and also a spectator of League of Legends events, so I think I should have enough background knowledge of analyzing this data. In addition, since I am a pro player of Sylas(I believe so), and Sylas is one of popular heroes who is often banned/picked in the professional matches, I am curious about is there any difference between best sylas players and weaker players in 2022 matches. Also, hopefully this will help some teams and players to use this analysis to ban pick when facing the best Sylas player after analysis. 

So my question is: **Does best Sylas players and remained Sylas players have similar distribution of difference in Kills contributions and Deaths Contributions?**

The raw dataset has **149400** rows in total, I will choose ['gameid', 'position', 'playername', 'playerid', 'teamid', 'champion', 'result', 'kills', 'deaths', 'assists',  'teamkills', 'teamdeaths', 'golddiffat15'] as my columns relevant to my question.

| Column_Name | Description |
| ----------- | ----------- |
| 'gameid | the id of each game/match. Each 'gameid' corresponds to up to 12 rows – one for each of the 5 players on both teams and 2 containing summary data for the two teams. |
| Paragraph | Text |
| 'position' | the role of the player in each match (top,jng, mid, bot, sup) |
| 'playername' | the name of the player |
| 'playerid' | the id of the player |
| 'teamid' | the id of the team |
| 'champion' | the hero the player selected of that match |
| 'result' | WIN/LOSE (1 represents win in raw data) |
| 'kills' | number of kills in the match |
| 'deaths' | number of deaths in the match |
| 'assists' | number of assists(contribution in kills but not the killer) in the match |
| 'teamkills' | number of total kills of the team in the match |
| 'teamdeaths' | number of total deaths of the team in the match |
| 'golddiffat15' | Average gold difference at 15 minutes |

### Cleaning and EDA
#### Data Cleaning

First we need to filter the rows with only Sylas players contained, Since rows in the raw data has two types of infos, one is for single player and another is for team stats, it seems like we need to filtered out all rows represents team stats first. However, since all rows with team stats has NaN in champion stats, we only need to filter out all rows with values in champion columns that is not 'Sylas'. And also fill all missing values with NaN first.

Then we need to create some new columns for future analysis, we need KillsTeamContribution and DeathsTeamContribution to replace current columns of kills, deaths, teamkills, teamdeaths. Since KillsTeamContribution and DeathsTeamContribution can represents the team contribution in each match, thus it will be reasonable to compare other players data in kills and deaths. Since there are matches where one team overwhelmed the other because of the gap in strength, thus the total teams kills will be really high, thus single player might had really high kills data with really poor performance.

And since some players might got no kills in a single match but lots of assists, I choose to count assists as 0.2 kills in calculations for avoiding bunches of zeros in calculations.

At the end, we will replace the value in column "result" from 0/1 to boolean, since 1/0 might be confused for reading. The cleaned data is shown below.
