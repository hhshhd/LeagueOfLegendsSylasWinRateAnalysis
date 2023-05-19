#Data Science behind LOL
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

From previous Bivariate Analysis we can see that use win and lose as categorical variables is hard to find relationship with other potential variables. So we will start grouping rows from each game to each player's stats, with winning rates as new elements representing result.

Since we need to calculate the winning rate, we will use 0,1 to represent result instead of lose/win.

And as only winning rate might be biased, so I merge the number of games for each player in the new column "NumOfGames".

|   WinningRate |   KillsTeamContribution |   DeathsTeamContribution |   golddiffat15 |   NumOfGames |
|--------------:|------------------------:|-------------------------:|---------------:|-------------:|
|      0.916667 |                0.33354  |                 0.167231 |        319.917 |           12 |
|      0        |                0.282222 |                 0.289216 |       -178.5   |            2 |
|      1        |                0.2      |                 0.181818 |       -682     |            1 |
|      1        |                0.481818 |                 0.2      |       2339     |            1 |
|      1        |                0.4      |                 0.222222 |       -112     |            1 |

In this case we are using playerid instead of playername as index is that playerid is always unique, but player might change their playername and two playername might be same, so to use the unique key as index, I will use playerid here. At the end finding the best player, I'll use his/her id to track back their current name as result.

This group table consists each players winning rate, num of games, and other stats with average value among all games. This will help us in future to dig in which player has the best performance on average, instead of using previous table that consists all each games stats, cannot shows how players performances if they have more than one games among 2022 matches.

### Assessment of Missingness
#### NMAR Analysis

In general, There should not be any NMAR in this data set, because each data is the content of game settlement statistics in each battle, and all data will not involve any player's privacy and personal tendencies, and should be fully transparent. However, because different leagues (different regions) have different data statistics preferences and tactical biases, there may be situations where some leagues will prefer to count some detailed statistics and some leagues will not. For example, "monsterkillsownjungle" and "monsterkillsenemyjungle" columns are collected only by several leagues, including LPL, LEC, LDL, etc. This is because these leagues has their own system of analyzing the data. Therefore, they will collect more data that is more biased towards the style of its competition area in terms of data processing and collection, just like the difference in the number of jungle monsters here is the same since they pays more attention to the tactical arrangement of the jungle area and the jungle position. Thus these leagues, like LPL and LEC, are good at gaining an advantage in the jungle, they will be more inclined to collect relevant information and display it to show their strength. And this also leads to potential NMAR, where other leagues will not collect these data because their inconfidence to the tactical arrangement of the jungle position, thus Resulting in potential NMARs appearing in the data collection. Similar includes things like baron resource scrambles and others.

#### Missingness Dependency

In this part, I choose 'monsterkillsownjungle' to analyze, and perform permutation tests to analyze the dependency of the missingness of this column on other columns. I choose 'league' and 'side' as columns to analyze the dependency of missingness with "monsterkillsownjungle".

|   monsterkillsownjungle | league   | side   |
|------------------------:|:---------|:-------|
|                     nan | LCK CL   | Blue   |
|                     nan | LCK CL   | Blue   |
|                     nan | LCK CL   | Blue   |
|                     nan | LCK CL   | Blue   |
|                     nan | LCK CL   | Blue   |

Verifying that monsterkillsownjungle in lol2022_missingness

Each row of lol2022_missingness belongs to one of two groups:

Group 1: 'monsterkillsownjungle' is missing.

Group 2: 'monsterkillsownjungle' is not missing.

|   monsterkillsownjungle | league   | side   | monsterkillsownjungle_missing   |
|------------------------:|:---------|:-------|:--------------------------------|
|                     nan | LCK CL   | Blue   | True                            |
|                     nan | LCK CL   | Blue   | True                            |
|                     nan | LCK CL   | Blue   | True                            |
|                     nan | LCK CL   | Blue   | True                            |
|                     nan | LCK CL   | Blue   | True                            |

Comparing null and non-null 'monsterkillsownjungle' distributions for 'side'

|   monsterkillsownjungle_missing = False |   monsterkillsownjungle_missing = True |
|----------------------------------------:|---------------------------------------:|
|                                     0.5 |                                    0.5 |
|                                     0.5 |                                    0.5 |

The two columns look exactly same, which is evidence that 'monsterkillsownjungle_missing''s missingness does not depend on 'side', But i will run a permutation test later to prove that again.

<iframe src="assets/Missingness-1.html" width=800 height=600 frameBorder=0></iframe>

To measure the "distance" between two categorical distributions, we use the total variation distance as test statistic. By applying permutation testing to run 500 shuffled samples, empirical distribution of the test statistic along with the observed statistic is shown below.

<iframe src="assets/Missingness-2.html" width=800 height=600 frameBorder=0></iframe>

`np.mean(np.array(tvds) >= observed_tvd)`

By calculating we get the p value as 1.0 . We fail to reject the null.

Recall, the null stated that the distribution of 'side' when 'monsterkillsownjungle_missing' is missing is the same as the distribution of 'side' when 'monsterkillsownjungle_missing' is not missing.

**Hence, we conclude that the missingness in the 'monsterkillsownjungle_missing' column is not dependent on 'side'.**

Then lets compare null and non-null 'monsterkillsownjungle' distributions for 'league'

|   monsterkillsownjungle_missing = False |   monsterkillsownjungle_missing = True |
|----------------------------------------:|---------------------------------------:|
|                             nan         |                             0.0229505  |
|                             nan         |                             0.0204005  |
|                             nan         |                             0.0068946  |
|                             nan         |                             0.00245561 |
|                               0.0413534 |                           nan          |


<iframe src="assets/Missingness-3.html" width=800 height=600 frameBorder=0></iframe>

Among these columns, we can obviously findWe can see from the plot that a few leagues are very sensitive to the missingness of junglekill. that only limited numbers of leagues are mainly

To measure the "distance" between several categorical distributions, we use the total variation distance. By applying permutation testing to run 500 shuffled samples, empirical distribution of the test statistic along with the observed statistic is shown below.

<iframe src="assets/Missingness-4.html" width=800 height=600 frameBorder=0></iframe>

`observed_tvd = league_dist.diff(axis=1).iloc[:, -1].abs().sum() / 2`

By calculating we get the p value as 0.2711422797102855. We reject the null.

Recall, the null stated that the distribution of 'league' when 'monsterkillsownjungle_missing' is missing is the same as the distribution of 'league' when 'monsterkillsownjungle_missing' is not missing.

**Hence, we conclude that the missingness in the 'monsterkillsownjungle_missing' column is dependent on 'league'.**

### Hypothesis Testing

Now we are back to our question, who is the best sylas player in 2022 matches? Since for this questions, winning rate can easily become the numerical representation of player's performance, so we can say that the top 25% winning rate would become best sylas player. 

Then We create two new columns, 'Best' represents boolean value of whether the player is the best, 'Diff' represents the difference in Kills Team Contribution and Deaths Team Contribution.

| Best   |        Diff |
|:-------|------------:|
| True   |  0.16631    |
| False  | -0.00699346 |
| True   |  0.0181818  |
| True   |  0.281818   |
| True   |  0.177778   |

Then lets look at the exploratory analysis of the data and its visualization.

|     mean |   count |
|---------:|--------:|
| 0.128805 |     433 |
| 0.115274 |     144 |

<iframe src="assets/Hypothesis-1.html" width=800 height=600 frameBorder=0></iframe>

Since our question is that: Does best Sylas players and remained Sylas players have similar distribution of difference in Kills contributions and Deaths Contributions? We have a two samples and are consdier whether they are drawn from the same population. Thus we should apply permutation testing in this case. 

Null Hypothesis: In 2022 matches, the distribution of difference in Kills and Deaths Team Contribution among those who are top 25% is the same as among those who are NOT Top 25% pro players.

Alternative Hypothesis: In 2022 matches, the distribution of these two groups in Kills and Deaths Team Contribution are different.

For the choice of test statistic, We need one that can measure how different two numerical distributions are, so I choose difference in group means as our test statistic.

Thus we got our observed statistic below.

`observed_difference = -0.013531175277358004`

For significane level, I'll choose one of common used one, 5%, as the the probability, under the null hypothesis, that the test statistic is equal to the value that was observed in the data or is even further in the direction of the alternative.

Lets run our permutation test.

After 500 shuffled samples, we get a list of 500 differences. here is randomly selected 10 of them as shown below.

`[0.005012848948314455,
 0.0007250083385850481,
 -0.004606296968549722,
 -0.01254582544763072,
 -0.004475722833128942,
 0.0006263420906117145,
 -0.02060701466381462,
 0.018798667174356007,
 0.016003001868274802,
 0.006432248174125327]`
 
 Empirical distribution of the test statistic along with the observed statistic is shown below.
 
 <iframe src="assets/Hypothesis-2.html" width=800 height=600 frameBorder=0></iframe>
 
 By looking at the graph and two lines(red represents observed stats and purple represents significance level of 5%, or by looking at the p_value = 0.838, thus we reject that the null hypothesis that the two groups come from the same distribution.
 
 After all analysis, we can give our conclusion that, from the dataset was given, There is no exactly same distribution of difference in Kills contributions and Deaths Contributions between best Sylas players and remained Sylas players under 5% significance level.
