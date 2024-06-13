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

It seems that getting first tower yields a slightly larger kill difference than getting first dragon. And teams who win usually have much more towers destroyed, than dragons killed.

|            |        0 |        1 |
|:-----------|---------:|---------:|
| (0.0, 0.0) | 11.3297  |  9.27691 |
| (0.0, 1.0) |  9.47458 | 10.6462  |
| (1.0, 0.0) | 10.6315  |  9.48226 |
| (1.0, 1.0) |  9.27691 | 11.3233  |

## Assessment of Missingness

### NMAR Analysis
The columns `killsat15` and `killsat10` having missing values that are NMAR as the values can be explained to be missing because of the length of the game not reaching 15 or 10 minutes, respectively. 

### Missingness Dependency

The column `firstdragon` is MAR, dependent on the column of `kills`. This is supported by the permutation tests where the observed test statistic of 0.7436895564590529 (the mean difference) is greater than 100% of all data in the permutation tests (as showed in the following plot where the observed test statistic is not even visible as it is so out of bounds!)

<iframe
  src="assets/missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing
### Hypothesis Pair
Null hypothesis: The distribution of kills with first tower destroyed and first dragon is equal to the distribution of kills with no 'first' objectives

Alternative hypothesis: The distribution of kills with first tower and first dragon is greater than the distribution of kills with no 'first' objectives

### Conlusion
The difference between the means of the distributions will be used as a test statistic. The observed test statistic is 6.070766940270982. The significance level of 5% is used as a good moderate value to compare if the test statistic is too much of an extreme or not. Upon conducting hypothesis testing, the calculated p-value is 0.499. With a 5% significance level, we fail to reject the null hypothesis because there is not enough statistical evidence to reject it, leading us to conclude that getting both the first tower and the first dragon does not seem to give as much of an advantage as first thought.

## Prediction Problem
We will predict if a team will win or lose. This will be a binary classification problem. The response variable is the result of the game (1 for win, 0 for loss). The metric will be precision as getting a false positive is more unwanted as we want to be sure that a game is a win or a loss.

Since the time of prediction is before the end of the game, we cannot use the `gamelength`, `kills`, `kill_diff`, or `finished_early`. We will just be using columns that showcase the objectives taken to predict.

## Baseline Model
The baseline model is a `DecisionTreeClassifier()` with the `barons` and `dragons` columns transformed using the `StandardScaler()` transformer. There are 4 ordinal features used in this model `barons`, `dragons`, `towers`, `inhibitors` and 1 nominal feature `firstblood`. The model scored 0.8741945876288659 on the testing set; however, some hyperparameters could be tuned for a better performance.

## Final Model
The final model is a `RandomForestClassifier()` with the addition of the `towers` and `inhibitors` columns being transformed using the `QuantileTransformer()` transformer. This should help to make the model more accurate as the number of towers and inhibitors destroyed fall into a higher range as by logic of higher objectives taken should lead to a won game. 

The hyperparamters I planned to tune are the criterion (as its a function to measure the quality of the split), the max depth (so to not overfit the data where it gets a good score on the training set, but a worse score on the test set), and then the minimum number of samples to split (so the tree does not overfit with many branches with very small samples). I simply used a `GridSearchCV` to find the best hyperparameters and fitted the training set.

The final model score 0.9727770618556701, a 10% increase from the previous model!

## Fairness Analysis
The groups to be compared are the games that finished before the 15 minute mark and games that finished afterward.

### Pair of Hypothesis
Null Hypothesis: Our model is fair. Its precision for games with more than 15 minutes and games that finish before the 15 minutes are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: Our Model is not fair. Its precision for games that finish before the 15 minutes are lower than its precision for longer games.

### Conclusion

The difference between the precision of the models will be used as a test statistic. The observed test statistic is 0.00608768864996212. The significance level of 5% is used as a good moderate value to compare if the test statistic is too much of an extreme or not. Upon conducting hypothesis testing, the calculated p-value is 0.47. With a 5% significance level, we fail to reject the null hypothesis, there is not enough statistical evidence to conclude that are model is not fair.



