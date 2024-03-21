
# League of Legends Match Predictions
This is a DSC80 final project that analyzes the relationship between position and first blood kills, and tries to use these two features to predict the outcome of a professional League of Legends match.

<b> Names: </b> Arnav Saxena, Leena Khattat

## Introduction

We are analyzing a League of Legends dataset that contains data from professional competitive League of Legends matches. This dataset contains around 130 features ranging from creep score to dragon data to first blood participation - essentially any metric that can be measured during a League of Legends profesional match. Additionally, the dataset contains over 12,000 rows, with each unique match representing 12 rows: 10 for the players and 2 for the teams.

Using this data, we will analyze two subsets of the data: teams that obtained a first blood, and players that obtained a first blood. We will use the teams subset to analyze whether or not there is a relationship between obtaining a first blood and winning the match. To dig deeper into our data, we will then try to see if there is a relationship between the position that got the first blood and the result of the match using a subset of the data that only includes players. We are interested in these two features specifically because we are curious on whether getting the first blood may change the outcome of the game. Additionally, we want to observe the potential impact of each position getting the first blood: for example, does it have more of an impact on the game if the jungler gets the first blood?

The specific columns we are most interested in for our players analysis are: "firstbloodkill", "position", and "result". These columns represent which position obtained the first blood, the side they played on, and the overall result of the game. This will allow us to analyze how impactful each position's first blood is to winning the game. For our teams analysis, we will analyze the same columns, except we will use "firstblood" rather than "firstbloodkill".

Once cleaned, our players subset had 104,910 rows and 3 columns to analyze. Our teams subset had 20984 rows and 2 columns.

<b> Descriptions of Columns: </b>

<ul>
    <li> "firstbloodkill": 1 if the player obtained the first blood kill, 0 otherwise </li>
    <li> "position": Represents the position each player was playing. Possible values include top, mid, jng, bot, and sup, which represent the 5 roles in the game. </li>
    <li> "result": 1 if the team won, 0 otherwise </li>
</ul>

For our teams analysis, we will use the same columns as above, except we will use "firstblood" instead of "firstbloodkill". This column represents whether or not the team got the first blood, using a 1 if they did and 0 if not.

Using this data, we will answer the question: 

<b> Does getting the first blood increase a team's chance of winning, and do different positions getting the first blood have varying
imapcts on the overall game? </b>

## Data Cleaning and Exploratory Data Analysis

First, we dropped all unnecessary columns in both our team and player subset of the datasets.

Then, we created 2 subsets of our data: players and teams. We will use the teams subset to establish a relationship between first bloods and game result. If we observe a relationship, we can use the players subset to analyze each position's first blood impact on the overall result of the game.

Lastly, we dropped all null values. The very last match had null values for all the first blood values, so we were unable to use that match for our analysis.

The resulting datasets are shown below:

Teams Dataset:                   

|    |   result |   firstblood |                 
|---:|---------:|-------------:| 
| 10 |        1 |            0 |
| 11 |        0 |            1 |
| 22 |        0 |            0 |
| 23 |        1 |            1 |
| 34 |        1 |            0 | 


Players Dataset: 
|    |   result |   firstbloodkill | position   |
|---:|---------:|-----------------:|:-----------|
|  0 |        1 |                0 | top        |
|  1 |        1 |                0 | jng        |
|  2 |        1 |                0 | mid        |
|  3 |        1 |                0 | bot        |
|  4 |        1 |                0 | sup        |

Using the cleaned datasets, we looked at relationships between variables.

## Interesting Aggregations

Using our players data, we created a pivot table that shows us the relationship between position, result, and number of first bloods. To do this, we used position as the index, result as the columns, and first blood kills as the values to aggregate. The table shows us that there are more first bloods overall in winning games, which is consistent with our univariate analysis shown below, and also shows us which role has the most first bloods overall for both winning and losing games.

| position   |    0 |    1 |
|:-----------|-----:|-----:|
| bot        | 1001 | 1718 |
| jng        | 1218 | 1852 |
| mid        |  738 | 1179 |
| sup        |  433 |  661 |
| top        |  656 | 1028 |
### Univariate Analysis

Using the teams dataset, we were able to plot the relationship between getting the first blood and winning the game. This plot shows us that winning games have a higher first blood count overall.

### Bivariate Analysis

We also plotted the distribution of first blood kills across both winning and losing games, and across all positions using our pivot table described above. This plot shows the same general trend across all roles for both winning and losing games, and also shows that ADCs and junglers have the highest first bloods overall.

## Assesssment of Missingness

## Framing a Prediction Model

<b> Question: </b> Does getting the first blood increase a team's chances of winning?

<b> Null Hypothesis:</b> Getting the first blood will not increase your chances of winning.

<b> Alternative Hypothesis:</b> Getting the first blood will increase your chances of winning.

To answer this question, we conducted a permutation test by randomly shuffling the first blood column for the teams dataset 10000 times. This allows us to see whether the difference in group means for winning vs losing games is purely due to chance, or if there is a significant relationship between first bloods and winning the game. We chose an alpha level of 0.05 for our test.

<iframe
  src="assets/empirical_dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After conducting our test, we obtained a p-value of <b> 0.0 </b>. This is much below the threshold of 0.05, indicating there may be a significant relationship between obtaining the first blood and winning the game. From our empirical distribution shown below, we can see that our observed value is much greater than any of the values obtained in our permutation test.


Now that we know that there may be a relationship between getting a first blood and winning the game, we can dive deeper into our analysis to observe the potential impact of each position on the overall outcome of the game.
## Baseline Model

## Final Model

## Fairness Analysis