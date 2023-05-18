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

| gameid                | position   | playername   | playerid                                  | teamid                                  | champion   | result   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   golddiffat15 |   KillsTeamContribution |   DeathsTeamContribution |
|:----------------------|:-----------|:-------------|:------------------------------------------|:----------------------------------------|:-----------|:---------|--------:|---------:|----------:|------------:|-------------:|---------------:|------------------------:|-------------------------:|
| ESPORTSTMNT01_2690264 | mid        | Roamer       | oe:player:ea58943d300b14d684bb05b3975fe15 | oe:team:2a5e5fceaa40b335ebc6b49359abf62 | Sylas      | Lose     |       1 |        3 |         2 |           8 |           13 |           -901 |                0.175    |                 0.230769 |
| NA1_4170815896        | mid        | Doxa         | oe:player:afcd3cdcf44e5da63f9e2a3dcb5ba50 | oe:team:5e1b75330088d864dcd0c2cef261890 | Sylas      | Lose     |       5 |        3 |         2 |           8 |           15 |            -60 |                0.675    |                 0.2      |
| NA1_4173580606        | jng        | CptShrimps   | oe:player:df4f91120caa080682bf595c5f7f2d3 | oe:team:ea04c2731d4cce306a17d765544e7e9 | Sylas      | Lose     |       3 |        3 |         5 |          11 |           19 |           -319 |                0.363636 |                 0.157895 |
| NA1_4173570661        | mid        | Diomarr      | oe:player:39f3a788b5909c17ee05ed45d8343b2 | oe:team:bedeb2d8da4a388b246a256c9da7957 | Sylas      | Lose     |       4 |        4 |         4 |          10 |           20 |           -759 |                0.48     |                 0.2      |
| NA1_4173481965        | mid        | Radar        | oe:player:bbaf1416538fd783e597b64ef99043e | oe:team:d7ec1f968a609382e6e6d1bc2f6e4c3 | Sylas      | Lose     |      10 |        5 |        13 |          33 |           24 |            769 |                0.381818 |                 0.208333 |

#### Univariate Analysis

First lets see the distribution of kills contribution of Sylas in all matches.

<iframe src="assets/Univariate Analysis-1.html" width=800 height=600 frameBorder=0></iframe>

This histogram shows the distribution of KillsTeamContribution column from the dataset. The trend from this histogram shows that the shape of Sylas kills contribution is likely a normal distribution, and most data concentrated in the range 0.3 to 0.5. Since a game consists only 5 players, averagely each players contributes 0.2, thus Sylas player mostly takes large proportion of kills in team. And also this plot has some other infos, like outliers(with 100%)contribution, since we count assists as 0.2 kills, thus 1 implies all kills are concentrated in Sylas player only in that match. 

Does that means that match sylas plays really well and carries the team? Apparently not, Since mostly winning games ends with each player having at least some kills and contributions, that implies the team advantage is large enough where everyone has a chance to get a kill. But for the 100% contribution kills in this case, mostly it implies that the team has only 1 to 2 kills in total, and all kills are done by Sylas, thus it doesn't give any useful infos to represents Sylas carries the team, but saying Sylas at least kill someone in that match.


Then lets see the distribution of deaths contribution of Sylas in all matches. Since  we all know that death and kill are opposite, what we pursue are more kills and fewer deaths, so in order to be consistent with the above, I changed the numbers of death from positive to negative in the distribution.

<iframe src="assets/Univariate Analysis-2.html" width=800 height=600 frameBorder=0></iframe>

This histogram shows the distribution of DeathsTeamContribution column from the dataset. The trend from this histogram shows that Sylas are mostly takes 0-0.3 proportions of deaths in whole team, and most of them never dead once in a game. Since a game consists only 5 players, averagely each players contributes 0.2, thus Sylas player mostly takes less proportion of deaths in team.


Finally, lets look at the distribution of position of Sylas player will choose.

<iframe src="assets/Univariate Analysis-3.html" width=800 height=600 frameBorder=0></iframe>

This plot shows that mostly Sylas will play mid role, and sometimes it will be chosed as top, but seldom on other lanes, thus my analysis will mainly focus on analyzing data from mid players only, since they represents the most common Sylas performance in general. 

#### Bivariate Analysis

Since we are looking for the best Sylas player, and League of Legends is a competitive game, and the result is determined by the team's victory or defeat, so the winning rate of sylas will always reflect some part of his performance. 

Firstly, lets look at the relationship between result and KillsTeamContribution.

<iframe src="assets/Bivariate Analysis-1.html" width=800 height=600 frameBorder=0></iframe>

The conditional distribution above shows that the shape of both win and lose looks similar, but the distribution of lose is more flatter than that of win. This implies that when winning the game, sylas player's contribution in teamkills are more concentrated in the range of 0.2 to 0.5, which means their contribution are more stablized when winning. Meanwhile when losing the game, sylas player's contribution in teamkills are quite randomly distributed. This implies me that maybe KillsTeamContribution cannot be the only variables to reflect their performance.


Then, lets look at the relationship between KillsTeamContribution and DeathsTeamContribution

<iframe src="assets/Bivariate Analysis-2.html" width=800 height=600 frameBorder=0></iframe>

This scatterplot shows a uniform arrangment of dots in the plot. There is no clear best fit line shown in the plot, and since dots are arrange uniformly, we can say the relationship between KillsTeamContribution and DeathsTeamContribution are weak.


Finally, lets see the relationship between golddiffat15 and KillsTeamContribution

<iframe src="assets/Bivariate Analysis-3.html" width=800 height=600 frameBorder=0></iframe>

This scatterplot shows a weak positive relationship between KillsTeamContribution and golddiffat15. Even we the dots in the plot are still separated and not concentrated on one exact line, but the trend is much more clear than preivous scatter plot between kill and death, thus we can start evaluating the gold difference at 15mins as one of performance that correlates to kills contribution.

#### Interesting Aggregates
