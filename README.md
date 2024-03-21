# league_of_legends_analysis

## Introduction
In this analysis, we wanted to analyze the best of the best: Competitive Games, and we decided to use the most recent completed season, the 2022 season, to show this. In our analysis, we will analyze one specific question that we believe is very important to how winning games, the primary objective of League, is decided:

**Does a winning top lane affect the probability of a team winning the game?**

Top lane, one of the five positions in League of Legends, is typically observed as a solo lane, meaning they often don’t receive too much help from other players on their team and especially the jungle, which typically (at least recently) prioritizes objectives like Dragons, Heralds, and Barons instead of helping near top.

Therefore, we wanted to ask this question, because it is especially relevant—we observed (with our eyes) that top lanes often didn’t really help a team unless they were doing exceptionally well and could carry the team, meaning they observed a very large gold lead. Therefore, we wanted to see how often they actually made teams better, and if they did, was it significant to winning in pro-play. In doing so, we looked through 149400 rows (or 12450 games) worth of data. In filtering our data, we realized that of the 123 columns that were provided, we only needed these 7 columns to help us answer our question: ['gameid', 'datacompleteness','side', 'position', 'result', 'xpat15', 'xpdiffat15'].

For these columns, ‘gameid’ allowed us to differentiate different games, ‘datacompleteness’ allowed us to verify if the data was complete (and therefore usable), ‘position’ allowed us to get the position we wanted (top). The two xp columns allowed us to decide how well a top lane player was doing relative to their opponent and also allowed us to create our test statistic for the hypothesis test.

Description of columns:

'gameid': The unique identifiers for the games that occurred. Used to differentiate each game.
'datacompleteness': Indicator of whether the data for this row is complete or missing certain information. Used to check if the row may be missing data in other relevant columns.
'side': Differentiates the players in the game as 'blue' or 'red' based on the side they were playing as.
'position': Describes which position the player played as.
'result: Describes whether the team of the player won in the end or not. True when the team won, False when the team lost.
'xpat15: The amount of xp the player had by the 15-minute mark in the game. Using the 15-minute mark because before 15 minutes, players usually stay in their own lane, which allows us to decide which top laner was better in the laning phase.
xpdiffat15: The difference in xp between a player and their opponent in the same position. The sign of the value is positive if the player was in the lead. The sign is negative if the opponent was in the lead.


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




## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
