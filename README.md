# league_of_legends_analysis

## Introduction
In this analysis, we wanted to analyze the best of the best: Competitive Games, and we decided to use the most recent completed season, the 2022 season, to show this. In our analysis, we will analyze one specific question that we believe is very important to how winning games, the primary objective of League, is decided:

**Does a winning top lane affect the probability of a team winning the game?**

Top lane, one of the five positions in League of Legends, is typically observed as a solo lane, meaning they often don’t receive too much help from other players on their team and especially the jungle, which typically (at least recently) prioritizes objectives like Dragons, Heralds, and Barons instead of helping near top.

Therefore, we wanted to ask this question, because it is especially relevant—we observed (with our eyes) that top lanes often didn’t really help a team unless they were doing exceptionally well and could carry the team, meaning they observed a very large gold lead. Therefore, we wanted to see how often they actually made teams better, and if they did, was it significant to winning in pro-play. In doing so, we looked through 149400 rows (or 12450 games) worth of data. In filtering our data, we realized that of the 123 columns that were provided, we only needed these 7 columns to help us answer our question: ['gameid', 'datacompleteness','side', 'position', 'result', 'xpat15', 'xpdiffat15'].

For these columns, ‘gameid’ allowed us to differentiate different games, ‘datacompleteness’ allowed us to verify if the data was complete (and therefore usable), ‘position’ allowed us to get the position we wanted (top). The two xp columns allowed us to decide how well a top lane player was doing relative to their opponent and also allowed us to create our test statistic for the hypothesis test.

Description of columns:

`gameid`: The unique identifiers for the games that occurred. Used to differentiate each game.
`datacompleteness`: Indicator of whether the data for this row is complete or missing certain information. Used to check if the row may be missing data in other relevant columns.
`side`: Differentiates the players in the game as 'blue' or 'red' based on the side they were playing as.
`position`: Describes which position the player played as.
`result`: Describes whether the team of the player won in the end or not. True when the team won, False when the team lost.
`xpat15`: The amount of xp the player had by the 15-minute mark in the game. Using the 15-minute mark because before 15 minutes, players usually stay in their own lane, which allows us to decide which top laner was better in the laning phase.
`xpdiffat15`: The difference in xp between a player and their opponent in the same position. The sign of the value is positive if the player was in the lead. The sign is negative if the opponent was in the lead.


In 2024, League of Legends remains a leading esports game. Players increasingly agree that, in competitive play, the top position has relatively less impact compared to mid, bot, jungle, and support roles. This is due to the top lane being considered solo, receiving less attention. Performance in the laning phase is often overlooked if competent in team fights. While top laners can carry games, it occurs less frequently than with other roles. This player perspective led us to investigate:

**Does the performance of the top lane player during the laning phase affect the team's winning chances?** 
(`<br>`)
## Data Cleaning and Exploratory Data Analysis
### Cleaning
Initially, we needed to source our data. Oracle provided a csv file containing all competitive matches from 2022, which we downloaded and loaded into a pandas DataFrame.

Next, we focused on shaping a DataFrame that could effectively isolate the necessary data for our analysis. We converted several non-boolean fields to boolean values, namely True or False, to simplify future data filtering. Specifically, we changed the datacompleteness and result columns to boolean formats. Moreover, we identified essential columns for our analysis: [`gameid`, `datacompleteness`, `side`, `position`, `result`, `xpat15`, `xpdiffat15`].

Finally, we applied filters to hone in on fully complete data entries (datacompleteness == True) and entries specifically related to the top lane position (position == top). This step was crucial to streamline access to the relevant data later on.
**Here is the head of the cleaned dataframe**


|    | gameid                | datacompleteness   | playoffs   | side   | position   | champion   |   pick1 |   pick2 |   pick3 |   pick4 |   pick5 |   gamelength | result   |   kills |   deaths |   assists |   teamkills |   teamdeaths |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   opp_towers |   firstmidtower |   firsttothreetowers |   turretplates |   opp_turretplates |   wardsplaced |    wpm |   wardskilled |   wcpm |   controlwardsbought |   visionscore |   vspm |   totalgold |   earnedgold |   goldspent |   gspd |   gpr |   total cs |   minionkills |   monsterkills |   monsterkillsownjungle |   monsterkillsenemyjungle |   cspm |   goldat10 |   xpat10 |   csat10 |   opp_goldat10 |   opp_xpat10 |   opp_csat10 |   golddiffat10 |   xpdiffat10 |   csdiffat10 |   killsat10 |   assistsat10 |   deathsat10 |   opp_killsat10 |   opp_assistsat10 |   opp_deathsat10 |   goldat15 |   xpat15 |   csat15 |   opp_goldat15 |   opp_xpat15 |   opp_csat15 |   golddiffat15 |   xpdiffat15 |   csdiffat15 |   killsat15 |   assistsat15 |   deathsat15 |   opp_killsat15 |   opp_assistsat15 |   opp_deathsat15 |
|---:|:----------------------|:-------------------|:-----------|:-------|:-----------|:-----------|--------:|--------:|--------:|--------:|--------:|-------------:|:---------|--------:|---------:|----------:|------------:|-------------:|--------------:|--------------:|--------------:|-------------:|-------------:|-------------:|----------------:|---------------------:|---------------:|-------------------:|--------------:|-------:|--------------:|-------:|---------------------:|--------------:|-------:|------------:|-------------:|------------:|-------:|------:|-----------:|--------------:|---------------:|------------------------:|--------------------------:|-------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|-----------:|---------:|---------:|---------------:|-------------:|-------------:|---------------:|-------------:|-------------:|------------:|--------------:|-------------:|----------------:|------------------:|-----------------:|
|  0 | ESPORTSTMNT01_2690210 | True               | False      | Blue   | top        | Renekton   |     nan |     nan |     nan |     nan |     nan |         1713 | False    |       2 |        3 |         2 |           9 |           19 |             0 |             0 |             0 |            0 |            0 |          nan |             nan |                  nan |            nan |                nan |             8 | 0.2802 |             6 | 0.2102 |                    5 |            26 | 0.9107 |       10934 |         7164 |       10275 |    nan |   nan |        231 |           220 |             11 |                     nan |                       nan | 8.0911 |       3228 |     4909 |       89 |           3176 |         4953 |           81 |             52 |          -44 |            8 |           0 |             0 |            0 |               0 |                 0 |                0 |       5025 |     7560 |      135 |           4634 |         7215 |          121 |            391 |          345 |           14 |           0 |             1 |            0 |               0 |                 1 |                0 |
|  5 | ESPORTSTMNT01_2690210 | True               | False      | Red    | top        | Gragas     |     nan |     nan |     nan |     nan |     nan |         1713 | True     |       1 |        1 |        12 |          19 |            9 |             0 |             0 |             0 |            0 |            0 |          nan |             nan |                  nan |            nan |                nan |            15 | 0.5254 |             2 | 0.0701 |                    8 |            30 | 1.0508 |       10001 |         6231 |        9750 |    nan |   nan |        229 |           221 |              8 |                     nan |                       nan | 8.021  |       3176 |     4953 |       81 |           3228 |         4909 |           89 |            -52 |           44 |           -8 |           0 |             0 |            0 |               0 |                 0 |                0 |       4634 |     7215 |      121 |           5025 |         7560 |          135 |           -391 |         -345 |          -14 |           0 |             1 |            0 |               0 |                 1 |                0 |
| 12 | ESPORTSTMNT01_2690219 | True               | False      | Blue   | top        | Gragas     |     nan |     nan |     nan |     nan |     nan |         2114 | False    |       0 |        5 |         2 |           3 |           16 |             0 |             0 |             0 |            0 |            0 |          nan |             nan |                  nan |            nan |                nan |             9 | 0.2554 |             7 | 0.1987 |                    4 |            28 | 0.7947 |       11076 |         6488 |       10900 |    nan |   nan |        245 |           241 |              4 |                     nan |                       nan | 6.9536 |       2880 |     3995 |       57 |           3774 |         5124 |           87 |           -894 |        -1129 |          -30 |           0 |             1 |            1 |               1 |                 0 |                1 |       4673 |     7020 |      110 |           6157 |         7672 |          140 |          -1484 |         -652 |          -30 |           0 |             1 |            1 |               1 |                 0 |                1 |
| 17 | ESPORTSTMNT01_2690219 | True               | False      | Red    | top        | Gangplank  |     nan |     nan |     nan |     nan |     nan |         2114 | True     |       2 |        2 |         6 |          16 |            3 |             0 |             0 |             0 |            0 |            0 |          nan |             nan |                  nan |            nan |                nan |            13 | 0.369  |             6 | 0.1703 |                    8 |            30 | 0.8515 |       17877 |        13289 |       16650 |    nan |   nan |        341 |           328 |             13 |                     nan |                       nan | 9.6783 |       3774 |     5124 |       87 |           2880 |         3995 |           57 |            894 |         1129 |           30 |           1 |             0 |            1 |               0 |                 1 |                1 |       6157 |     7672 |      140 |           4673 |         7020 |          110 |           1484 |          652 |           30 |           1 |             0 |            1 |               0 |                 1 |                1 |
| 36 | ESPORTSTMNT01_2690227 | True               | False      | Blue   | top        | Renekton   |     nan |     nan |     nan |     nan |     nan |         1972 | True     |       3 |        2 |         7 |          14 |            5 |             0 |             0 |             0 |            0 |            0 |          nan |             nan |                  nan |            nan |                nan |            18 | 0.5477 |             6 | 0.1826 |                   15 |            38 | 1.1562 |       14070 |         9771 |       12950 |    nan |   nan |        304 |           284 |             20 |                     nan |                       nan | 9.2495 |       3412 |     5065 |       96 |           3359 |         5053 |           92 |             53 |           12 |            4 |           0 |             0 |            0 |               0 |                 0 |                0 |       5856 |     7759 |      146 |           4952 |         7674 |          142 |            904 |           85 |            4 |           2 |             0 |            0 |               0 |                 0 |                0 |

### Univariate Analysis
One of the most important stats regarding the performance of players in a game is 'kills', which reveals the 
Below is the histogram representing the distribution of the 'xpat15' column and some relevant descriptive stats:


<iframe
  src="assets/kill_dist2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Descriptive statistics for the 'kills' column:

* mean   : 2.796550743864304
  
* median : 2.0
  
* mode   : [1]
  
* max    : 25
  
* min    : 0
  
* std    : 2.4190900419068813

This graph shows that the distribution of  `kills` is rightly-skewed, which means it has a ‘tail’ to the right, indicating that large extreme values are more likely than large values. This is consistent with our expectation because it's easier to get a few kills in a game, rather than a lot. Only the top players who are highly skilled can get a higher kills rate. This observation on `kills` could be useful for our future analysis when we look into what factors could predict a team's performance.

### Bivariate Analysis
Our research question is concerned with the stats of the top player that affect the chances of the team winning overall. So, we can now investigate the relationship between the team winning or not and the top player’s xp level.

Below is our bar chart comparing the difference of means for player’s gold earned between winning teams and losing teams as well as some relevant stats:
<iframe
  src="assets/bi_earngold"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Statistics for 'earnedgold' feature:

Absolute difference     : 2019.6618085416794

Proportional difference : 0.12071365886583755

As we expected, the winning team has a higher amount of gold earned on average. However, we thought the proportional difference would have been more significant than 12%. This information will become very valuable later as it supports our prediction at the beginning that the amount of difference in gold earned between the winning and losing teams have a significant impact on the team's outcome.

### Interesting Aggregates
(Will fill in later)

## Assessment of Missingness
### NMAR Argument
For data classified as NMAR (Not Missing at Random), the absence of data is inherently related to its own characteristics. If certain data is missing, it's likely due to intrinsic reasons tied to its value.

In our analysis of the dataset, we found several columns with frequent missing values, such as 'dragons', 'opp_dragons', and their respective types ('infernals', 'mountains', 'clouds', 'oceans', and 'chemtechs'). We suspect these columns follow the NMAR pattern. When these entries are NaN, it implies the absence of dragons or a specific type of dragon in that game, indicating potential non-input rather than a zero value. Additionally, another factor potentially influencing missingness could be the total number of dragons in the game, which also contains numerous NaN values. Intuitively, as the dragon count increases in a game, the probability of encountering a wider variety of dragons also rises.

### Missingness Dependency
In investigating missing data, our aim was to understand why certain entries were labeled complete while others were marked as partial. Upon examination, we observed a notable trend of partial data particularly within the entries from the 'LPL' league, representing China. This trend was especially evident in columns situated towards the end of the DataFrame, which were crucial for our analysis. We sought to determine if this pattern extended beyond mere coincidence, as our initial observations suggested a prevalence of incomplete data originating from Chinese leagues like 'LPL' and 'LDL'.

Consequently, we conducted an analysis on our primary DataFrame, df, to identify columns consistently experiencing missing data. Notably, we discovered that 'opp_csat15', one of our key columns, exhibited frequent absence alongside other columns located towards the end of the DataFrame.

To further investigate, we selected variables we believed might exhibit a correlation with the presence or absence of 'opp_csat15' data. Initially, we determined our test parameters using our standard dataset. Specifically, we isolated rows where 'golddiffat15' was NaN and extracted the columns ['league', 'split', 'playoffs', 'patch', and 'position'].

By using the groupby() method, we were able to test out our columns and see the missingness of certain leagues. Jumping to our test statistic, we noticed that the Missingness Distribution of ‘league’ compared to the others that we tested seemed a lot more lopsided.

<iframe
  src="assets/missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Upon reviewing the graph, it's evident that there's a notable amount of missing data within leagues like LPL and LDL, which aligns with our earlier observations during dataset analysis. Particularly, the LDL league demonstrates a Missingness Proportion of 0.517867, indicating that over 50% of the instances of missing values in golddiffat15 are attributed to the LDL league.

Although this observation could be explained by the LDL league accounting for over 50% of all competitive games, we seek to comprehensively assess the dataset by conducting permutations on the columns under examination. This involves shuffling the league, split, playoffs, patch, and position columns.

Following this permutation process and regrouping by league, we randomized the leagues once again and reexamined the occurrence of null values to determine if any league could exceed a 50% rate of missingness.
<iframe
  src="assets/pmissing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
As you can see, even our league with the largest share of missing values when permutated cannot even hit 0.1 of the total missingness on column golddiffat15. This implies that the missingness of this column is likely Missing at Random, dependent on the league column. To test this, let’s use a pair of hypotheses and a significance level of 0.01 with 1000 trial runs.

**Null Hypothesis**: Data that is missing comes from the same league distribution as all other data.

**Alternative Hypothesis**: Data that is missing is significantly more likely to be one league than others.

We simulated this test 1000 times and returned a p-value of 0.0.

Here, our p-value suggests that we can reject our null hypothesis, meaning that it is likely that data missing from golddiffat15 is significantly more likely to be in at least one league than others - this in turn means that golddiffat15 has data that is Missing At Random.

Now, let’s find a column that golddiffat15 does not depend on. Intuitively, golddiffat15 shouldn’t depend on columns that are aggregated throughout our data extremely evenly; that is, columns that have no relation and are evenly spread out should not have any correlation with golddiffat15. One of these columns is position, which will always be even regardless of the matches, since all games will have 2 of each position.

Here, let’s test what the position missingness of golddiffat15 looks like when we groupby() on the position column.
<iframe
  src="assets/position.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

As you can see, this happened very often, because at the end of the day, the missingness of ‘golddiffat15’ never relied on ‘position’.
## Hypothesis Testing
We revisited our initial inquiry posed at the onset of this project: whether teams with a winning top lane (defined here as the player with higher gold gained) would enjoy an increased likelihood of winning.

**Null hypothesis**: Teams with a winning top lane **DO NOT** demonstrate a significantly higher probability of winning the game overall.

**Alternative hypothesis**: Teams with a winning top lane exhibit a significantly elevated probability of winning the game overall.

To explore this, we're introducing a new column into our dataset to indicate whether the top lane player held the lead at a specific juncture in the game. To determine this, we're examining the `golddiffat15` values, which reflect the gold difference between each team 15 minutes into the game. A positive value suggests that the top laner was ahead in gold compared to their opponent, denoted as True. Conversely, a negative value indicates the top laner was trailing, labeled as False. In cases where the value is zero, signifying both players had an equal amount of gold, we'll assign np.NaN, as such instances do not contribute to our analysis. This methodology enables us to comprehend the early game dynamics and their potential influence on the match's outcome.
Below is what the DataFrame prepared for permutations test looks like:

| result   | winninglane   |
|:---------|:--------------|
| False    | True          |
| True     | False         |
| False    | False         |
| True     | True          |
| True     | True          |

Finally, we perform the permutation test at a significance level of 0.05 using 1000 simulations of the test statistic.

Below is the histogram of the distribution of simulated test statistics:

<iframe
  src="assets/permutation_distribution_test_stat.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The permutation test returns a p-value of 0.0. Under the null hypothesis, we rarely see differences in probability as large as the observed statistic (0.0 < 0.05).

Thus, we can reject the null hypothesis and have sufficient evidence to support the alternate hypothesis: suggesting having a winning top lane, defined as the player with more gold, significantly increases the chance of the team winning overall.

## Framing a Prediction Problem
The key issue that really gets to the heart of League of Legends is **predicting which team will win**?

Every game has a clear outcome, either winning or losing, but understanding why is trickier than it seems. Many different elements influence each game, leading us to examine a dataset of competitive matches from 2022. This dataset broke down each game into 12 rows - 10 for the individual players (5 per team) and 2 summarizing team data. Essentially, every row corresponded to either a player or a team in a specific match, with columns providing various statistics for each participant, like Kills, Deaths, Assists, and other more intricate details that gain complexity throughout the game.

Considering that winning a game is represented by a binary outcome, either True or False (equivalent to 1s and 0s), we identified our task as a binary classification problem. Our goal is to forecast the 'result' variable, which is pivotal for determining success in League of Legends, classifying it into True or False outcomes.

We chose accuracy as our evaluation metric for the model because our primary objective is to assess the model's ability to correctly predict the winning or losing status of a team. We opted for accuracy over metrics like the F1-score, as the impact of false positives or false negatives didn't hold more weight than accurately identifying wins or losses. Given that each game results in a win or a loss, assigning equal importance to false positives and false negatives aligns with our rationale, justifying the use of accuracy for our predictions.

Additionally, in terms of "time of prediction", our prediction phase was constrained to the dataset provided, without any external data or biases, ensuring that our analyses are based solely on the available information. This approach guarantees that our predictive insights are not influenced by data from past or future seasons, maintaining the dataset's integrity for isolated prediction purposes.

## Baseline Model
For our initial model, we aimed to develop a predictive tool capable of determining wins solely based on key individual player statistics commonly available in League of Legends matches.

**Quantitative Features**: Kills, Deaths, and Assists

**Response Variable**: Result (Win (1) / Lose (0))

Since these variables were already discrete, they could be easily integrated into a Pipeline and our model for training and prediction.

Performance Baseline
For our model selection, we opted for a DecisionTreeClassifier with a maximum depth of 4 for this straightforward prediction task. We reasoned that a simpler model would yield more straightforward results.

Based on our analysis, we achieved an accuracy of approximately **~84%** for both training and testing data. The similarity in training and testing scores suggests that our model hasn't overfit the training data and can generalize well to unseen data. We view this as a promising baseline model, as it logically aligns with the notion that team success is often tied to individual player performance, particularly in aspects like Kills and Assists (which contribute to gold and time advantages), while also minimizing Deaths (as they grant the enemy team resources and opportunities). Overall, we are satisfied with the performance of this baseline model, considering its effectiveness despite limited training data.

## Final Model
We decided to add `Towers`, `Double Kills`, `Dragons`, `Barons`, `Triple Kills`, `VSPM`, `Inhibitors`, `Earned GPM`, `Position`, `golddiffat15`, `SPM`, `Heralds`, `Teamname`, `xpdiffat15`, `DPM`, First Blood, and `Turret Plates` as new features.

Here are the reasons why we believe the following variables are important to our analysis:
**Position and Teamname:**
As esports enthusiasts, we recognize that teams like T1 and DRX, finalists in Worlds, are likely to win matches due to their overall skill. Additionally, different positions prioritize varied objectives, such as supports focusing on assists and objectives, while tops prioritize kills due to their isolated early game role.

**Objectives:**
In League of Legends, objectives like **Dragons, Barons, and Towers** provide gold and buffs, aiding in teamfights and objective control. These boosts empower champions, enhancing their scaling and potential for victory in subsequent engagements.

**Kills, Gold, and XP:**
Features like First Blood and earned gold per minute (GPM) underscore the importance of both gold and kills in securing advantages. Gold enables item purchases and empowerment, while XP allows for level and ability progression, enhancing both individual and team strength.

**Damage, CS, and Vision Score:**
Damage output, minion kills (CS), and Vision Score are indicators of team performance. Damage correlates with winning, CS contributes to gold generation, and Vision Score aids in map awareness, facilitating strategic decision-making and objective control.

### Hyperparameters
**Model Description** 
In the end, we opted to develop a model based on either the DecisionTreeClassifier or LogisticRegression. Initially, we manually tested various LogisticRegression and DecisionTreeClassifier models. The most accurate LogisticRegression model achieved around 91.5% accuracy, while the best DecisionTreeClassifier model reached approximately 90.5% accuracy. Based on this evaluation, we selected LogisticRegression as our final model. Employing GridSearchCV with cross-validation, we determined the optimal max_iter and penalty parameters to be 300 and 'l2' respectively. Upon training the model with the entire dataset, we achieved a final accuracy of approximately 91.5%. Overall, we were content with this level of accuracy, especially after incorporating what we considered to be effective features into our model. The optimization of hyperparameters and features enabled us to develop a model capable of predicting match outcomes with a high degree of accuracy.

**Quantitative Variables**: Our discrete variables included `kills`, `deaths`, `assists`, `dragons`, `barons`, `heralds`, `firstblood`, `doublekills`, `triplekills`, `dpm`, `cspm`, `vspm`, `earned gpm`, `turretplates`, `towers`, and `inhibitors`. To account for fluctuations and differences in these variables, particularly in explaining player/team performance relative to others, we standardized `dpm`, `cspm`, `vspm`, `golddiffat15`, and `xpdiffat15`.

**Qualitative Variables**: For our qualitative variables, namely position and teamname, we utilized the OneHotEncoder to convert them into quantitative columns, given their ordinal data types.


## Fairness Analysis
