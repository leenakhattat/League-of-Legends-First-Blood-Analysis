
# League of Legends Match Predictions
This is a DSC80 final project that analyzes the relationship between position and first blood kills, and tries to use these two features to predict the outcome of a professional League of Legends match.

## Introduction

We are analyzing a League of Legends dataset that contains data from professional competitive League of Legends matches. This dataset contains around 130 features ranging from CS score to dragon data to first blood participation - essentially any metric that can be measured during a League of Legends profesional match. Additionally, the dataset contains over 12,000 rows, with each unique match representing 12 rows: 10 for the players and 2 for the teams.
<br> Using this data, we will analyze two subsets of the data: teams that obtained a first blood, and players that obtained a first blood. We will use the teams subset to analyze whether or not there is a relationship between obtaining a first blood and winning the match. To dig deeper into our data, we will then try to see if there is a relationship between the position that got the first blood and the result of the match using a subset of the data that only includes players. We are interested in these two features specifically because we are curious on whether getting the first blood may truly change the pace of the overall game. Additionally, we want to observe the potential impact of each position getting the first blood: for example, does it have more of an impact on the game if the jungler gets the first blood?
<br> The specific columns we are most interested in for our players analysis are: "firstbloodkill", "position", and "result". These columns represent which position obtained the first blood, the side they played on, and the overall result of the game. This will allow us to analyze how impactful each position's first blood is to winning the game.
<br>
Once cleaned, our players subset had 104,910 rows and 3 columns to analyze. Our teams subset had 20984 rows and 2 columns.
<br> 
<b> Descriptions of Columns: </b>
        - "firstbloodkill": 1 if the player obtained the first blood kill, 0 otherwise
        - "position": Represents the position each player was playing. Possible values include top, mid, jng, bot, and sup, which represent the 5 roles in the game.
        - "result": 1 if the team won, 0 otherwise
<br>
For our teams analysis, we will use the same columns as above, except we will use "firstblood" instead of "firstbloodkill". This column represents whether or not the team got the first blood, using a 1 if they did and 0 if not.
<br>
Using this data, we will answer the question: <b> Does getting the first blood increase a team's chance of winning, and do different positions getting the first blood have varying
imapcts on the overall game? </b>
## Data Cleaning and Exploratory Data Analysis

First, we dropped all unnecessary columns in both our team and player subset of the datasets.



## Assesssment of Missingness

## Framing a Prediction Model

## Baseline Model

## Final Model

## Fairness Analysis