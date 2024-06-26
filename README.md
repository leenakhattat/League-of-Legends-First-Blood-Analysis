
# League of Legends First Blood Analysis
This is a DSC80 final project that analyzes the relationship between position and first blood kills, and tries to use these two features to predict the outcome of a professional League of Legends match.

<strong> Names: </strong> Arnav Saxena, Leena Khattat

## Introduction

We will be analyzing a League of Legends dataset that contains data from professional competitive League of Legends matches. This dataset contains around 130 features ranging from creep score to dragon data to first blood participation - essentially any metric that can be measured during a League of Legends profesional match. Additionally, the dataset contains over 12,000 rows, with each unique match representing 12 rows: 10 for the players and 2 for the teams.

Using this data, we will analyze two subsets of the data: <strong>teams</strong> and <strong>players</strong>. We will use the teams subset to analyze whether or not there is a relationship between obtaining a first blood and winning the match. To dig deeper into our data, we will then try to see if there is a relationship between the position that got the first blood and the result of the match using a subset of the data that only includes players. We are interested in these two features specifically because we are curious on whether getting the first blood may change the outcome of the game. Additionally, we want to observe the potential impact of each position getting the first blood: for example, does it have more of an impact on the game if the jungler gets the first blood?

The specific columns we are most interested in for our players analysis are: "firstbloodkill", "position", and "result". These columns represent which position obtained the first blood, the side they played on, and the overall result of the game. This will allow us to analyze how impactful each position's first blood is to winning the game. For our teams analysis, we will analyze the same columns, except we will use "firstblood" rather than "firstbloodkill".

Once cleaned, our players subset had 104,910 rows and 3 columns to analyze. Our teams subset had 20984 rows and 2 columns.

<strong> Descriptions of Columns for Players Subset: </strong>

<ul>
    <li> <strong> "firstbloodkill": </strong> 1 if the player obtained the first blood kill, 0 otherwise </li>
    <li> <strong> "position": </strong> Represents the position each player was playing. Possible values include top, mid, jng, bot, and sup, which represent the 5 roles in the game. </li>
    <li> <strong> "result": </strong> 1 if the team won, 0 otherwise </li>
</ul>

For our teams analysis, we will use the following:

<strong> Descriptions of Columns for Teams subset: </strong>

<ul>
    <li> <strong> "firstblood": </strong> 1 if the team obtained the first blood, 0 otherwise </li>
    <li> <strong> "result": </strong> 1 if the team won, 0 otherwise </li>
</ul>

Using this data, we will answer the question: 

<strong>Does getting the first blood increase a team's chance of winning, and do different positions getting the first blood have varying
imapcts on the overall game? </strong>

## Data Cleaning and Exploratory Data Analysis

### Cleaning our data
First, we dropped all unnecessary columns in both our team and player subset of the datasets.

Then, we created 2 subsets of our data: players and teams. We will use the teams subset to establish a relationship between first bloods and game result. If we observe a relationship, we can use the players subset to analyze each position's first blood impact on the overall result of the game.

Afterwards, we dropped all null values. The very last match had null values for all the first blood values, so we were unable to use that match for our analysis.

Lastly, we converted all floating points to integers so that our row values were consistent across all columns.

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

Using the cleaned datasets, we will look at relationships between variables.

### Interesting Aggregations

Using the players subset, we created a pivot table that shows us the relationship between position, result, and number of first bloods. To do this, we used position as the index, result as the columns, and first blood kills as the values to aggregate. The table shows us that there are more first bloods overall in winning games, which is consistent with our univariate analysis shown below, and also shows us which role has the most first bloods overall for both winning and losing games.


| position   |    0 |    1 |
|:-----------|-----:|-----:|
| bot        | 1001 | 1718 |
| jng        | 1218 | 1852 |
| mid        |  738 | 1179 |
| sup        |  433 |  661 |
| top        |  656 | 1028 |

### Univariate Analysis

Using the teams dataset, we were able to plot the relationship between getting the first blood and winning the game. This plot shows us that winning games have a higher first blood count overall.
<iframe
  src="assets/univariate1.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

We also plotted the distribution of first blood kills across both winning and losing games, and across all positions using our pivot table described above. This plot shows the same general trend across all roles for both winning and losing games, and also shows that ADCs and junglers have the highest first bloods overall.
<iframe
  src="assets/bivariate2.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

## Assesssment of Missingness

### NMAR Analysis
While conducting our analysis, we noticed the dragons and firstdragon columns had many missing values, much more than the other columns. Dragons are an important part of the game, providing powerful buffs to the team that slays them, so it is unlikely that they are missing by design because they can control the pace of the game. However, there could be other reasons that the columns include missing values, such as technical issues with the data collection methodology. Additionally, if the dataset is focusing on match analysis in which objective control is not being considered, the dragons may be missing in this context because it is not being factored into the outcome of the match. If the data is missing due to this reasoning, then additional data could be provided to make it MAR, such as the context in which the analysis will be focused on: objective focused or not.

### Missingness Dependency

In our missingness analysis, we set out to investigate the missingness in the "killsat15" column, specifically exploring whether this missingness could be correlated to missingness "deathsat15" seeing if it could also be independent of the "result" of the match. Through permutation testing, we discovered p-values of 0 for both tests, indicating that the missingness of "killsat15" is likely not random and may indeed depend on the events around the 15-minute mark, as represented by "deathsat15", as well as potentially being influenced by the outcome of the match. This finding was unexpected and led us to speculate about the data collection and recording process. For other columns we tested we also saw consistent p-values of 0, so the non-random pattern of missingness, particularly its dependence on "deathsat15" and its relationship with the match result, suggests there might be a systematic bias or error in how match data is documented. Matches with more dramatic outcomes (either by way of deaths or significant events leading to the result) might be more likely to have complete data, whereas less notable matches might not be fully recorded. It's also possible that the result is influenced by the size of the dataset or peculiarities in the data collection process. For example, data might be missing completely at random due to technical glitches rather than for any meaningful in-game reason.


## Hypothesis Testing

<strong> Question: </strong> Does getting the first blood increase a team's chances of winning?

<strong> Null Hypothesis:</strong> Getting the first blood will <strong>not</strong> increase your chances of winning.

<strong> Alternative Hypothesis:</strong> Getting the first blood will increase your chances of winning.

To answer this question, we conducted a permutation test by randomly shuffling the first blood column for the teams dataset 10000 times. This allows us to see whether the difference in group means for winning vs losing games is purely due to chance, or if there is a significant relationship between first bloods and winning the game. We then used difference of group means to calculate the first blood percentages of winning games and losing games. We chose an alpha level of 0.05 to conduct our test.

Below is an example of one run of the permutation test. We can see that there is a difference between the actual values and shuffled values - losing games have approximately a 39% first blood rate, while winning games have approximately 61%. However, when shuffled, both winning and losing games have 50% first blood rates. Our test statistic is the winning games FB rate - losing games FB rate. The observed value of this statistic was <strong> 0.22</strong>.
We repeated this permutation 10,000 times, and calculated the average amount of times that our permutation had results greater than or equal to our observed value.

|   result |   firstblood |   shuffled |
|---------:|-------------:|-----------:|
|        0 |     0.385209 |   0.498142 |
|        1 |     0.612906 |   0.499952 |


After conducting our test, we obtained a p-value of <strong> 0.0</strong>. This is much below the threshold of 0.05, indicating there may be a significant relationship between obtaining the first blood and winning the game. From our empirical distribution shown below, we can see that our observed value is much greater than any of the values obtained in our permutation test.

<iframe
  src="assets/empirical_dist.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>


Now that we know that there may be a relationship between getting a first blood and winning the game, we can dive deeper into our analysis to observe the potential impact of each position on the overall outcome of the game.

## Framing a Prediction Model

Our results from the previous part indicate that there may be a significant relationship between getting the first blood and winning the game. Now, we will explore the specific position that obtained the first blood and their impact on the game. We will also look at the first blood victims, which will tell us which position was the victim of the first blood.In addition, we will look at the specific champion they were playing to see if that may also have an effect. Our goal is to predict whether a team wins or loses based on which role gets the first blood, and which role was the victim of the first blood.

## Baseline Model

The baseline model focuses on predicting match outcomes based on the occurrence of first blood events, the positions of the players involved, and the champions played. We classify all variables as categorical and nominal, indicating no inherent order among the categories. The model splits the data into an 80% training set and a 20% test set.

For preprocessing, we employ a one-hot encoding strategy within a pipeline to transform categorical inputs into a machine-readable format. We then use a Random Forest Classifier to learn the patterns in the training data.

Upon evaluation, the baseline model achieves an accuracy of approximately 58% on the test dataset. While this initial model offers a reasonable prediction capability, it suggests room for enhancement by incorporating additional, potentially predictive features.

## Final Model

To refine our baseline model, we incorporate a broader set of features, including early-game statistics such as gold, experience (XP), and creep score (CS) differences at the 10 and 15-minute marks, alongside early-game kill, assist, and death counts. These additional numerical features, indicative of early-game performance, are standardized using a StandardScaler to normalize their scale for model training.

The inclusion of both game-phase statistical differences and initial player and champion information aims to create a more nuanced prediction model. By analyzing these early-game indicators, we hypothesize that our model can more accurately predict overall match outcomes, given their significant impact on the game's trajectory.

Using an 80/20 train/test split and after fine-tuning through a grid search to optimize the Random Forest Classifier's hyperparameters, our enhanced model demonstrates a marked improvement, with an accuracy of 75% on the test set. This increase not only underscores the value of incorporating game-phase dynamics into our predictive framework but also highlights the potential for even further model refinement with additional data and analytical techniques.

The progression from our baseline to the final model illustrates the impact of integrating comprehensive game dynamics and statistical indicators on predictive accuracy. By moving beyond first blood events to include early-game performance metrics, we've significantly improved our ability to forecast match outcomes. This reinforces the critical role of feature selection and optimization in building effective predictive models in the context of competitive gaming analytics.

## Fairness Analysis

To analyze the fairness of our model, we will compare "lower" level games to the final matches of the series to assess whether the model performs similarly on the two splits. This will allow us to test the accuracy of our model across varying levels of professional gameplay.

<strong>Null Hypothesis: </strong> Our model is fair. Its accuracy for higher level professional matches is similar to the accuracy for lower level matches.

<strong>Alternative Hypothesis: </strong> Our model is NOT fair. Its accuracy for higher level professional matches is not similar to the accuracy for lower level matches.

We will calculate the difference in the precision of both models in order to determine fairness.

To do this, we ran a permutation test to calculate the absolute difference in the precision of the model after predicting both the spring split and finals split. We use an alpha level of 0.05 to do our analysis.

After running the permutation test 1000 times, we observe a p-value of <strong>0.23</strong>. This is significantly higher than the threshold of 0.05, so we fail to reject the null hypothesis. This means that it is likely our model is fair across different groups of data.
