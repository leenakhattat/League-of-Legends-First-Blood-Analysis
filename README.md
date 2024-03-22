
# League of Legends First Blood Analysis
This is a DSC80 final project that analyzes the relationship between position and first blood kills, and tries to use these two features to predict the outcome of a professional League of Legends match.

<strong> Names: </strong> Arnav Saxena, Leena Khattat

## Introduction

We are analyzing a League of Legends dataset that contains data from professional competitive League of Legends matches. This dataset contains around 130 features ranging from creep score to dragon data to first blood participation - essentially any metric that can be measured during a League of Legends profesional match. Additionally, the dataset contains over 12,000 rows, with each unique match representing 12 rows: 10 for the players and 2 for the teams.

Using this data, we will analyze two subsets of the data: <strong>teams</strong> and <strong>players</strong>. We will use the teams subset to analyze whether or not there is a relationship between obtaining a first blood and winning the match. To dig deeper into our data, we will then try to see if there is a relationship between the position that got the first blood and the result of the match using a subset of the data that only includes players. We are interested in these two features specifically because we are curious on whether getting the first blood may change the outcome of the game. Additionally, we want to observe the potential impact of each position getting the first blood: for example, does it have more of an impact on the game if the jungler gets the first blood?

The specific columns we are most interested in for our players analysis are: "firstbloodkill", "position", and "result". These columns represent which position obtained the first blood, the side they played on, and the overall result of the game. This will allow us to analyze how impactful each position's first blood is to winning the game. For our teams analysis, we will analyze the same columns, except we will use "firstblood" rather than "firstbloodkill".

Once cleaned, our players subset had 104,910 rows and 3 columns to analyze. Our teams subset had 20984 rows and 2 columns.

<strong> Descriptions of Columns: </strong>

<ul>
    <li> <strong> "firstbloodkill": </strong> 1 if the player obtained the first blood kill, 0 otherwise </li>
    <li> <strong> "position": </strong> Represents the position each player was playing. Possible values include top, mid, jng, bot, and sup, which represent the 5 roles in the game. </li>
    <li> <strong> "result": </strong> 1 if the team won, 0 otherwise </li>
</ul>

For our teams analysis, we will use the following:

<strong> Descriptions of Columns: </strong>

<ul>
    <li> <strong> "firstblood": </strong> 1 if the team obtained the first blood, 0 otherwise </li>
    <li> <strong> "result": </strong> 1 if the team won, 0 otherwise </li>
</ul>

Using this data, we will answer the question: 

<strong>Does getting the first blood increase a team's chance of winning, and do different positions getting the first blood have varying
imapcts on the overall game? </strong>

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

Using the cleaned datasets, we will look at relationships between variables.

## Interesting Aggregations

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

Our baseline model will first use the first blood victim, as well as the position they were playing and result columns, in order to try and predict whether a team wins or loses based on who was first blooded (who was killed first). All features we are observing are categorical nominal features, as they have no ordering to them.

We are using 80% of our data to train the model, and 20% to test the model.

To train our model, we created a preprocessing pipeline that uses one-hot-encoding to convert our categorical features to trainable values. Then, we fit the model to a Random Forest Classifier using our training data.

After training the model, we tested it on our test data. It proved accurate approximately 58% of the time. This model is fairly accurate, and uses appropriate metrics to predict the outcome of the game - however, with more features the accuracy can be improved. //idk wtf im saying

## Final Model

To improve our baseline model, we added several features from our original dataset that correspond to phases of the game. This features include: "golddiffat10", "xpdiffat10", "csdiffat10", "killsat10", "assistsat10", "deathsat10", "golddiffat15", "xpdiffat15", and "csdiffat15". The new features we added are all numerical and represent differences of varying game statistics between the two teams. We believe that these features will improve our model because they represent important game statistics throughout the phases of the game. For example, golddiffat10 represents the difference in gold between the two teams. If there is a high difference, that means one team is likely winning very strongly over the other at that current moment in time, which will be a large factor in determining whether that team wins overall or not. 

To process these new numerical features, we standardized them through StandardScaler. //explain why this helps ?

Once again, we are using 80% of our data to train the model and 20% to test.

After processing all of our features, we fit the model to a Random Forest Classifier. This time, we used a Grid Search CV to obtain the best hyperparameters for our model. Once the best hyperparameters were determined, we tested our new model using our testing dataset, and we achieved an accuracy of 75%. //idk jsut explain more
## Fairness Analysis
