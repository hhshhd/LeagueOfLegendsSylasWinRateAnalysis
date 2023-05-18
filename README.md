# DSC80 Project 3: Data Science behind LOL
**Name**: Housheng Hai
this is a project analyzing hero "Sylas"'s winning rate in the game League of Legends for DSC 80 at UCSD

### Introduction

The data set I am going to study is the game data of professional players in various League of Legends matches. The data set contains match data from the LCS, LEC, LCK, LPL, PCS, CBLoL, and many more leagues.Since I am a player of League of Legends, and also a spectator of League of Legends events, so I think I should have enough background knowledge of analyzing this data. In addition, since I am a pro player of Sylas(I believe so), and Sylas is one of popular heroes who is often banned/picked in the professional matches, I am curious about is there any difference between best sylas players and weaker players in 2022 matches. Also, hopefully this will help some teams and players to use this analysis to ban pick when facing the best Sylas player after analysis. 

So my question is: **Does best Sylas players and remained Sylas players have similar distribution of difference in Kills contributions and Deaths Contributions?**

The raw dataset has 149400 rows in total, I will choose ['gameid', 'position', 'playername', 'playerid', 'teamid', 'champion', 'result', 'kills', 'deaths', 'assists',  'teamkills', 'teamdeaths', 'golddiffat15'] as my columns relevant to my question.

'gameid': the id of each game/match. Each 'gameid' corresponds to up to 12 rows – one for each of the 5 players on both teams and 2 containing summary data for the two teams.

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
