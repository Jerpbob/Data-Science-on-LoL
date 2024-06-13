# An Analysis on the Importance of Kills Versus Objectives in League of Legends
Jeru Paul Perena Balares

## Introduction
League of Legends (LoL) is a multiplayer online battle arena (MOBA) game which was developed by Riot Games. The game consists of two five-player teams, each consisting of characters called "champions", where the objective is to destroy the enemy team's base, or the "nexus" as called by the game. Additional traits of the game, such as player-killing and objectives, are inlcuded as aspects of the game that contribute to beating the enemy team.

Upon learning more about the game, a question is raised concerning the fact that newer players focus on kills rather than on objectives. Whereas, a lot of the more experienced playerbase claim objectives is how one wins games more consistently, as it also consistently leads to more kills. Thus, I would like to conduct a data analysis on how important objectives are, and predict if having won more objectives dictates the difference in kills.

The data used for this analysis was taken from https://oracleselixir.com/tools/downloads using match data from the year 2022.

### Dataset Introduction
The dataset consists of 148992 rows and 131 columns, showcasing the game metrics and details. For sake of simplicity, the columns showcased will only be relevant columns used in the data analysis.

`date`: the date and time the game took place

`participantid`: the id of the player's position in the game **(id's with 100/200 describe the team: 100 is team blue, 200 is team red)**

`gamelength`: the length of a game in seconds

`result`: the result of the game **(0 is a loss, 1 is a win)**

`kills`: the number of kills contributed by the participantid at the end of the game

`firstblood`: shows which team got firstblood **(if team got firstblood, then value is 1)**

`firstdragon`: shows which team got the first dragon **(if team got first dragon, then value is 1)**

`dragons`: number of dragons each team killed

`elders`: number of elders each team killed

`firstbaron`: shows which team got the first baron **(if team got first baron, then value is 1)**

`barons`: number of barons each team killed

`firsttower`: shows which team got the first tower **(if team got first tower, then value is 1)**

`towers`: number of towers each team destroyed

`inhibitors`: number of inhibitors each team destroyed

`killsat10`: the number of kills contributed by the participantid at the 10 minute mark

`killsat15`: the number of kills contributed by the participantid at the 15 minute mark

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The dataset provided conveniently has a row for each game consisting of the cumulated data of all members for each team such as total kills, total towers destroyed, total kills at 10 minutes, etc. So, we'll just filter out the rows that are statistics about the individuals, rather than the team as a whole.

Some games also seem to finish earlier as showcased by the columns in `killsat10` and `killsat15` having null values, thus I'll make a column that showcases whether a game finished before the 15 miinute mark. 

Amongst the relevant "objective" columns (towers, inhibitors, firstblood, etc.), the columns `inhibitors` and `firstblood` have NaN values. I probabilistically imputed missing data from a the distribution of non-null values to clean the two columns. 

The following is a sample of the cleaned dataset: 

| date                |   participantid |   gamelength |   result |   firstblood |   firstdragon |   dragons |   elders |   firstbaron |   barons |   firsttower |   towers |   inhibitors |   killsat10 |   killsat15 |   kill_diff |   kills |   finished_early |
|:--------------------|----------------:|-------------:|---------:|-------------:|--------------:|----------:|---------:|-------------:|---------:|-------------:|---------:|-------------:|------------:|------------:|------------:|--------:|-----------------:|
| 2022-01-10 07:44:08 |             100 |         1713 |        0 |            1 |             0 |         1 |        0 |            0 |        0 |            1 |        3 |            0 |           3 |           5 |          10 |       9 |                0 |
| 2022-01-10 07:44:08 |             200 |         1713 |        1 |            0 |             1 |         3 |        0 |            0 |        0 |            0 |        6 |            1 |           0 |           6 |          10 |      19 |                0 |
| 2022-01-10 08:38:24 |             100 |         2114 |        0 |            0 |             0 |         1 |        0 |            0 |        0 |            0 |        3 |            0 |           1 |           1 |          13 |       3 |                0 |
| 2022-01-10 08:38:24 |             200 |         2114 |        1 |            1 |             1 |         4 |        0 |            1 |        2 |            1 |       11 |            2 |           3 |           3 |          13 |      16 |                0 |
| 2022-01-10 09:24:26 |             100 |         1365 |        1 |            0 |           nan |         2 |      nan |          nan |        1 |          nan |        8 |            1 |         nan |         nan |           7 |      13 |                1 |

### Exploratory Data Analysis

The distribution of total kills seems to showcase that the majority of games are in the 5 to 20 range of total kills. 
<iframe
  src="assets/total_kill.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The following bivariate distribution showcases that winning teams tend to have a higher average of dragons taken than losing teams.

<iframe
  src="assets/dragon_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The following bivariate distribution showcases that winning teams tend to have a higher average of towers taken than losing teams.

<iframe
  src="assets/tower_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>