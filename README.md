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
<iframe
  src="assets/kill_dist2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness

## Hypothesis Testing

## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis
