# Predicting Match Outcomes in League of Legends Based on Team Performance and Player Statistics

Please access the project notebook [here](https://nbviewer.org/github/Mohamed-Aymaan-Zahir/League-of-Legends-Match-Analysis/blob/main/League%20of%20Legends%20Match%20Analysis%20and%20Predictive%20Model.ipynb). For a summary of the work done and results, the purpose of this project, and some useful information about the game, please continue reading!

## Introduction

As both an aspiring data scientist and a regular player of League of Legends, this project provides the perfect opportunity to refine my data science skills while also obtaining useful insights about a topic I am heavily interested in. Leveraging the techniques I have learned throughout my degree while also employing new models and visualisation methods, I aim to bridge my analytical expertise with my domain knowledge of the game. This analysis will be useful for both players who are eager to gain more understanding about the factors that contribute towards winning games and other data scientists who are interested in the different machine learning and visualisation techniques I employ to extract information and convey it in an engaging manner. 

To ensure clarity, the analysis notebook is written in a way that walks readers through each step, making it accessible even for those without a technical background. For those unfamiliar with League of Legends, a brief summary of the game is included below to provide necessary context. Additionally, the project is structured with modular code aided by literate programming, allowing fellow data scientists to follow along, replicate the analysis, or apply similar techniques to their own projects.

---

## What is League of Legends?
![league pic](league_pic.png)
_Credit: Karin Richter Gómez / Red Bull Content Pool_

League of Legends is a highly popular 5v5 multiplayer online battle arena (MOBA) game developed and published by Riot Games that is played by over 30 million players daily. Players choose different characters, known as champions, each with a unique set of abilities, and battle to earn gold, which can be used to buy items to enhance their champions' strengths. The primary objective is to work as a team to destroy the enemy base, which is defended by turrets and the opposing team. Before each game, every player chooses one of five different position:

1. **Top lane:** Primarily a 1v1 role focused on gaining an experience lead, often played with durable tank champions.
2. **Jungle:** in charge of roaming between lanes to assist teammates and secure neutral objectives..
3. **Middle Lane:** Similar to top lane but with easier access to infleunce the map, usually played by damage-dealing champions.
4. **ADC (Attack Damage Carry):** A long-range damage dealer who scales with items, supported by a teammate.
5. **Support:** in charge of providing utility to the team in terms of healing, shielding, crowd control (abilities that hinder the enemy's ability to fight or move) and gaining vision of the enemy team.

The game features three main modes: Summoner’s Rift (Classic mode), ARAM (All Random All Mid), and Swiftplay. This analysis focuses solely on Summoner’s Rift, which has two variations:

- Draft Pick – A casual mode with no ranking consequences.
- Ranked – A competitive mode where players are matched by skill level (ranks). Ranks range from Iron to Challenger, with four subdivisions per tier. Players gain or lose LP (League Points) based on match outcomes. Ranked mode is further divided into:
    - Solo/Duo Queue – For individuals or duos.
    - Flex Queue – For full or partial teams.

      
Any additional game-related terminology is explained in context throughout the notebook.

---

## Data Pipeline

### a) Raw Data

The data that was used is the League of Legends Match Dataset (2025) by user Jacob Krasuski from [Kaggle.com](https://www.kaggle.com/datasets/jakubkrasuski/league-of-legends-match-dataset-2025) under the license  CC BY 4.0 (Creative Commons Attribution 4.0 International). The dataset is based on game data from Riot Games, the developer and publisher of League of Legends. Please refer to the original dataset source and Riot Games' policies for attribution and licensing details.

The original data consists of 40412 rows and 94 columns, with each row representing a unique player in a match and each column representing information about various aspects regarding the match and the player's performance within the match.

### b) Data Cleaning

The following steps were taken to clean the data:

1. Dropping columns that do not convey much useful information, are not relevant or columns that visibly have a lot of missing data.
2. Converting data types
3. Changing naming semantics
4. Filtering for the subset of the data to be used
5. Filling in and dropping null values
6. Feature engineering: splitting the date column, calculating derived performance metrics.

### c) Descriptive Analytics

In order to explore the data and gain information about its overall characteristics, the following actions were done:

1. Calculating summary statistics such as the mean, median, min, max and spread for key features
2. Visualising the correlation between key features using a correlation map
3. Visualising the distribution of summoner levels using a histogram

### d) Data Analysis

These are the various aspects of the data that I examined and visualised:

1. Performance of players based on their role: I checked the amount of data available for each role and the win rate of each position using a bar chart and stacked bar chart respectively. To further inspect win rates, I plotted a line graph for the Jungle and Support role win rates over different game versions. Additionally, I plotted the distribution of eliminations, deaths and assists for each role using boxplots.
2. Performance of players based on game duration: I examined how player performance changes depending on how long the game is by plotting scatterplots for the gold per minute against duration and the KDA against duration.
3. Performance of players based on solo rank: this involved visualising the differences in performance metrics between player ranks using a heatmap.


### e) Building the Classifier Model

I used a Catboost classifer model to predict match outcomes and win probabilities due to the existence of complex categorical columns in the data. I employed a 80:20 train-test split and fit the model with some slight manual hyperparameter tuning, and then calculated performance metrics such as the area under the receiver operating characteristic curve (AUC), accuracy and logloss. Further testing the robustness of the test performance metrics, I performed 5-fold cross validation.

### f) Analysing the Classifier Model

I used the Shapley Additive Explanations library to plot the most important features in the model and analysed possible reasons for their significance. I also went further as to examine which champions have the most influence on improving and worsening win probabilities.

---
## Summary of Results

### **_Key Insights:_**
1. Achieved an 88% accuracy, 95% AUC and 0.27 logloss. These metrics did not change much when conducting cross validation.
2. Win rates do not differ by roles over time, even though on specific game versions some roles may have higher or lower win rates.
4. As expected, the amount of eliminations and assists that players get differ by roles, with support players having more assists on average and ADC players normally leading the elimination chart. However, the amount of deaths follows an almost identical distribution for each role, contrary to the belief that support and top lane players die more to save their team.
5. Player performance tends to peak when games are not too long, and diminishes as games take longer, seeing that player KDAs and gold per minute tend to be high for average length games and lower for very long games.
6. The performance of higher ranked players do differ quite a lot from lower ranked players, with higher ranked players outperforming them on almost every performance metric on average.
7. The most important features that contribute towards predicting a win probability are the number of assists, damage dealt to objectives and damage dealt to turrets, all of which potentially indicate better team coordination and focus on gaining advantages over the enemy team.
8. Champions that are usually easier and more accessible for lower ranked players might improve win probability more than obscure champions or champions with high skill expression.

### **_Example Graphs:_**

1.
 ![boxplot](boxplot.png)
_Distributions of Eliminations, Deaths and Assists By Role_

2.
![heatmap](heatmap.png)
_Difference in Player Performance Based on Rank_

3.
![bar2](bar2.png)
_Top 10 Champions That Positively And Negatively Influence Win Probability_

---

### **_Potential Limitations and Areas For Improvement:_**
1. All of the data related to players on the EU Nordic and East server. While not being a small server by any means, it is still not as popular as servers like Europe West, North America and Korea. As a result, the results obtained from this analysis may not be representative of the entire League of Legends player base, and may only be representative of players on this specific server.
2. Even though our predictive model ended up using a large number of predictors, there are still many more features that could have provided useful information to improve the model performance. These include the number of minions killed, which is a very important feature that was not included in the dataset, and the flex rank columns, which were removed due to the abundance of missing data.
3. With more time and computational resources, a better model may be obtained through automated hyperparameter tuning, but given that a good model was fit with relatively little tuning, this limitation is not so severe.
4. This project only focused on the Classic Summoner's Rift mode, but there is still data on the ARAM and Swiftplay modes available, which may be useful to analyse in the future.

---

## Acknowledgements

Please refer to the [Acknowledgements Wiki Page](https://github.com/aymaan-kj/League-of-Legends-Match-Analysis/wiki/Acknowledgements) for a full list of resources used in this project.
