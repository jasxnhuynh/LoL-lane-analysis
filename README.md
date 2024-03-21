# The Winning Edge: A Comprehensive Analysis of Roles in League of Legends

By Jason Huynh (jah037@ucsd.edu) and Jonathan Yi (j8yi@ucsd.edu)

---

## Introduction

Welcome to Summoner’s Rift! Since its release in 2009, League of Legends (LoL) has taken the gaming world by storm, becoming the #1 multiplayer online battle arena (MOBA) game in the world. At its core, LoL pits two teams, each comprising five players, in a strategic battle to destroy the opposing team's Nexus. Players select one of five roles<sup>1</sup>: top, jungle, middle, adc, or support. In such a highly competitive game, players are constantly seeking ways to gain an upper hand, with the choice of role often being a critical factor in securing a win. Our data<sup>2</sup> analysis aims to answer the question: "which role consistently carries their team to victory?"

<sup>1</sup>Throughout our project, the jungle, mid, and support roles will be abbreviated to jng, mid, and sup respectively. As well, the words 'role' and 'position' will be used interchangebly.

<sup>2</sup>Our project uses League of Legends Esports Match Data sourced from [Oracle's Elixir](https://oracleselixir.com/tools/downloads), consisting of 125904 rows and 131 columns.

---

## Data Cleaning and Exploratory Data Analysis

Our data cleaning process began with a check for missing values in our dataset. We discovered that the column 'damagetochampions' contained missing values only when the 'datacompleteness' column was marked as "partial". By removing rows with "partial" datacompleteness, we eradicated all missing values, facilitating our progression to the subsequent cleaning steps.

Next, to ensure consistency in role labeling, we renamed the 'bot' role to 'adc' (Attack Damage Carry), aligning with the standard terminology used in the gaming community.

Then, we implemented the calculation and incorporation of the KDA (Kill-Death-Assist) ratio into our dataset. Utilizing the formula KDA = (kills + assists) / deaths, we computed each player's KDA. However, instances of 0 deaths led to KDA values of 'inf' (infinity). To resolve this issue, we adjusted the formula for these cases, setting KDA = kills + assists.

Lastly, we standardized our dataset's statistics. Employing the z-score formula Z = (X - µ) / σ, we transformed the data into z-scores for each game. To ensure accuracy, we created a custom function and applied it using the .transform(z-score) method after grouping by 'gameid'.

After cleaning, our dataset had 105924 rows and 134 columns. Below are the initial five rows of our dataframe, showcasing the standardized statistics and omitting unnecessary columns for visualization purposes.

|    | gameid                | position   |        KDA |   totalgold |   total cs |   damagetochampions |
|---:|:----------------------|:-----------|-----------:|------------:|-----------:|--------------------:|
|  0 | ESPORTSTMNT06_2753012 | top        |  1.45409   |   -0.181853 |   1.05976  |           -0.546755 |
|  1 | ESPORTSTMNT06_2753012 | jng        | -0.552391  |   -0.511447 |  -0.661093 |           -0.825695 |
|  2 | ESPORTSTMNT06_2753012 | mid        |  0.45085   |   -0.334314 |   0.615204 |           -0.105894 |
|  3 | ESPORTSTMNT06_2753012 | adc        |  2.02737   |   -0.255967 |   0.851821 |            0.412173 |
|  4 | ESPORTSTMNT06_2753012 | sup        |  0.0208899 |   -0.670381 |  -1.65058  |           -0.844755 |

### Univariate Analysis

For our univariate analysis, we aimed to explore how various in-game statistics' z-scores distribute across different player roles. To begin, we grouped the data by the 'position' column, which represents the role each player assumes in the game. We chose not to include the 'team' values, as they do not relate to the positions.

Next, we calculated the average z-score for each statistic within each player role. The statistics we investigated included KDA (Kill-Death-Assist ratio), total gold earned, total creep score (CS), and damage dealt to champions.

To visualize our findings, we utilized bar graphs, where each graph corresponded to a specific player role. In these graphs, each bar on the x-axis represented a different statistic, while the y-axis indicated the average z-score.

<iframe
  src="assets/KDA.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/totalgold.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/total cs.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/damagetochampions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Upon analyzing the graphs, several insights emerged. The middle and adc roles consistently exhibited positive average z-scores across all statistics. In contrast, the top role showed a mix of positive and negative z-scores, indicating varying impact levels. Meanwhile, roles like support and jungle tended to have average z-scores consistently below their counterparts, suggesting a comparatively lesser impact.

In summary, our analysis suggests that the middle and adc roles tend to have a more substantial impact on the game across various statistics, while top, support, and jungle roles exhibit more diverse and sometimes less impactful performances.

### Bivariate Analysis

In our bivariate analysis, we aimed to explore the relationship between different statistics and player positions in the dataset. We employed two main visualization techniques: bar charts and box plots.

<iframe
  src="assets/Average Z-Scores by Position.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The bar graph above illustrates each position’s average z-score across all statistics. The x-axis represents the positions, while the y-axis indicates the average z-score. Each bar was color-coded to distinguish between positions, facilitating a visual comparison of z-scores across roles. Through this visualization, we noticed that the adc role had the highest average z-Score for all statistics, whereas support had the lowest average z-Score for all statistics.  

In addition to the bar charts, we created box plots to further analyze the distribution of z-scores for two specific statistics: KDA and damage to champions. Two separate box plots were generated—one for KDA and another for damage to champions. The x-axis indicated the player positions, while the y-axis represented the z-score of the respective statistic. The box plots provided insights into the central tendency, spread, and presence of outliers in the z-score distribution for each position, allowing for a more detailed examination of the data compared to the bar charts.

<iframe
  src="assets/KDA Distribution Across Positions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Upon closer examination of the KDA box plot, it is evident that all positions demonstrate a relatively similar spread in z-scores, with the median ranging from approximately -0.2 to -0.6. Notably, the box plot for the top position appears slightly lower compared to the others, with the adc role being the only one containing outliers. Additionally, there is a discernible difference in variance, with the top position showing comparatively lower variability than the other positions. This suggests a higher level of consistency in KDA for top players, hinting at a potentially more passive playstyle. Furthermore, the adc role stands out with higher z-scores, as indicated by its box plot being generally higher than those of other positions.

<iframe
  src="assets/Damage to Champions Distribution Across Positions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

In contrast to the KDA box plot, the distribution of z-scores across roles for damage to champions reveals a more diverse pattern. The top, middle, and adc roles exhibit wider ranges, indicating greater variability in performance metrics. Conversely, the jungle and support roles demonstrate narrower ranges, suggesting more consistent performance in this aspect. Moreover, both jungle and support roles feature lower medians at 9.8k and 4.9k, respectively. On the other hand, middle and adc roles consistently maintain higher medians, highlighting their potential for greater impact overall in terms of damage inflicted on champions.

Overall, these visualizations allowed us to compare the performance of players in different positions across various statistics, providing valuable insights into the relationship between player roles and in-game performance metrics.

---

## Interesting Aggregates

To delve deeper into the distribution of z-scores, particularly noting the trend of middle and adc roles consistently exhibiting higher z-scores, we opted to visualize the z-scores for KDA and total damage to champions across all positions using overlaid histograms. These histograms illustrate z-score distributions, with the x-axis representing the z-score and the y-axis indicating the frequency of players within each z-score bin. Each role is color-coded for easy identification of z-score distributions.

<iframe
  src="assets/Distribution of Damage to Champions Across Positions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The overlaid histogram suggests a uniform spread of z-scores for KDA across all roles, contradicting our previous analyses. Each role appears to have a similar count within each z-score bin, prompting us to question whether all roles have an equal impact overall.

In contrast, this visualization reinforces our earlier observations, indicating that support generally has the lowest z-scores, while middle, adc, and top roles tend to have the highest z-scores.

---

## Assessment of Missingness

### NMAR Analysis

We hypothesize that the absence of values in the 'ban5' column of our dataset is not missing at random (NMAR). The missing values in this column appear to lack a discernible pattern, suggesting potential scenarios where certain teams may have either overlooked banning a fifth champion or deemed it unnecessary. This dependence on the actual value of the missing data points renders the missingness NMAR.

To mitigate this issue and transform the column into one with missingness at random (MAR), we propose the introduction of a new column named 'bancompleteness'. This new column would capture whether the total number of bans for each team, including the fifth ban, was complete. By incorporating this additional data, the 'ban5' column would no longer rely on the specific value of the missing data points but instead on the completeness of bans recorded in 'bancompleteness'.

### Missingness Dependency

When we began this project, our curiosity was piqued by both the comprehensive dataset and the subset of data collected at the 15-minute mark. Intriguingly, we observed numerous missing values in columns corresponding to this specific time point. Consequently, we set out to scrutinize the relationship between missingness and the presence of data collected at the 15-minute mark, particularly focusing on the 'killsat15' column.

In our investigation, we chose to assess the missingness dependency against the 'datacompleteness' column.

Null Hypothesis: The distribution of 'datacompleteness' when 'killsat15' is missing mirrors the distribution when 'killsat15' is not missing.

Alternative Hypothesis: The distribution of 'datacompleteness' when 'killsat15' is missing differs from the distribution when 'killsat15' is not missing.

The graph below illustrates the distribution of 'datacompleteness' when 'killsat15' is and is not missing.

<iframe
  src="assets/Distribution of Datacompleteness Based on Killsat15 Missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We conducted a permutation test to ascertain the dependency of missingness in the 'killsat15' column on the 'datacompleteness' column, using the total variation distance (TVD) as our test statistic. The plot presented below depicts the empirical distribution of TVDs, with our observed TVD indicated by a red vertical line.

<iframe
  src="assets/Permutation Test Distribution of 'datacompleteness' TVD.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

After calculating the p-value by comparing the observed TVD to the TVDs derived from permutation testing, we obtained a p-value of 0. Because this value is less than the significance level of 0.05, we reject the null hypothesis, indicating a dependency between missingness in 'killsat15' and 'datacompleteness'.

Subsequently, we investigated missingness against the 'position' column, establishing the following hypotheses:

Null Hypothesis: The distribution of 'position' when 'killsat15' is missing is identical to when it's not missing.

Alternative Hypothesis: The distribution of 'position' when 'killsat15' is missing differs from when it's not missing.

Following a similar procedure, we plotted the distribution of 'position' in relation to missingness in 'killsat15' and conducted another permutation test employing TVD as our test statistic.

<iframe
  src="assets/Distribution of Position Based on Killsat15 Missingness.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/Permutation Test Distribution of 'position' TVD.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Upon comparing the Total Variation Distances (TVDs) obtained from permutation testing with the observed TVD, we arrived at a p-value of 1. Given that this p-value exceeds the significance threshold of 0.05, we fail to reject the null hypothesis. Therefore, it appears that the missingness of data in the killsat15 is not dependent upon position.

---

## Hypothesis Testing

We now aim to answer this question: do all roles have an equal overall impact?

Null Hypothesis: All roles have an equal impact overall.

Alternative Hypothesis: All roles do not have an equal impact overall.

Test statistic: Total Variation Distance (TVD)

Significance level: 5% (0.05)

Impact variables: KDA, damagetochampions, totalgold

We employed the Total Variation Distance (TVD) as our test statistic due to its suitability for comparing categorical distributions of different positions (top, jungle, middle, adc, and support). Choosing the conventional significance level of 0.05, a p-value below this threshold indicates strong evidence against the null hypothesis, suggesting that observed results are unlikely to occur by chance.

For this hypothesis test, we excluded "total cs" as it closely correlates with "totalgold." Subsequently, we conducted three distinct hypothesis tests, each focusing on one of the three impact variables: KDA, damagetochampions, totalgold.

<iframe
  src="assets/Empirical Distribution of 'KDA' TVD.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

'KDA' results:

P-value: 0.001

Conclusion: Because the p-value is less than the significance level of 0.05, we reject the null hypothesis. This suggests that all roles do not have the same average KDA z-score.

<iframe
  src="assets/Empirical Distribution of 'damagetochampions' TVD.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

'damagetochampions' results:

P-value: 0.001

Conclusion: Because the p-value is less than the significance level of 0.05, we reject the null hypothesis. This suggests that all roles do not have the same average damage to champions z-score.

<iframe
  src="assets/Empirical Distribution of 'totalgold' TVD.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

'totalgold' results:

P-value: 0.001

Conclusion: Because the p-value is less than the significance level of 0.05, we reject the null hypothesis. This suggests that all roles do not have the same average total gold z-score.

## Framing a Prediction Problem

The central question we aim to address is: Can we predict a player's role (top, middle, jungle, adc, support) given their post-game data?

This predictive task entails constructing a multiclass classification system capable of assigning players to one of five distinct positions: top, mid, jungle, adc, or support. The player's position serves as the target variable, crucial for understanding team dynamics and strategic planning.

Earlier, we delved into the interplay between various in-game metrics—such as kills, assists, deaths, damagetochampions, and totalgold—and player position. Our analysis revealed discernible correlations between these metrics and player roles, prompting us to investigate the feasibility of leveraging these metrics for predictive purposes.

Our model's performance evaluation hinges on precision—a metric selected for its efficacy in minimizing false positives. By prioritizing precision, we aim to ensure the model's predictions maintain a high level of accuracy.

In our design, we took great care to ensure our predictive framework operates under the constraint of utilizing only in-game statistics available at the "time of prediction." Thus, during model training, we meticulously excluded any data points inaccessible during real-time gameplay.

### Baseline Model

Our baseline model utilized a random forest classifier to predict positions based on three key features: KDA, total damage to champions, and total gold. These features, all quantitative variables, collectively encapsulate all important aspects of player performance. No further alterations or encodings were applied to these values. 

The model's reported performance stands at 0.57, indicating that it predicts the correct position for 57% of the observations. Being that this accuracy level falls below the 60% threshold, it leaves lots of room for improvement. To raise this score, we can utilize additional relevant features or optimize the parameters of the random forest classifier.

### Final Model

In our final model iteration, we introduced several new features to enrich our dataset: 'kills', 'deaths', 'assists', 'damagetochampions', 'totalgold', 'earnedgoldshare', 'minionkills', 'monsterkills', and 'visionscore.’ These additions offer deeper insights into player performance, enabling the model to better distinguish between different positions.

The inclusion of kills, deaths, and assists alongside the KDA feature enhances specificity in evaluating player performance. By incorporating these individual metrics, we retain valuable details that contribute to the overall KDA calculation. Exclusively relying on KDA overlooks the nuanced information inherent in kills, deaths, and assists, potentially losing valuable distinctions such as players with a high number of assists but relatively fewer kills and deaths.

Moreover, the integration of earned gold share, minion kills, monster kills, and vision score aims to capture the player's role within the team context. For instance, roles like middle and adc typically exhibit higher gold shares, while support and jungle tend to have lower minion kills. Similarly, jungle and mid positions often entail higher monster kills, whereas jungle and support may prioritize a higher vision score.

To preprocess the minion kills, monster kills, and vision score features, we opted to binarize these statistics using their respective median values as thresholds. This choice of using medians mitigates the influence of outliers, which could skew results if the mean were employed. Consistent with our previous model, all features utilized remained quantitative in nature.

We chose to employ a random forest classifier for our final model because of its robustness in handling a multitude of input features while maintaining strong performance. Given the diverse range of features involved in predicting a player’s position—ranging from kills and deaths to assists and gold—the random forest algorithm proves particularly adept at managing such complexity.

Furthermore, the algorithm's capacity to discern intricate relationships between input features and the target variable enhances model accuracy. This ability to detect nuanced patterns contributes significantly to improving predictive outcomes, crucial in the context of player position prediction.

Notably, random forests offer resilience against overfitting, a common concern with complex datasets. This resilience ensures the model generalizes well to unseen data, bolstering its reliability and applicability.

Hyperparameter tuning, facilitated through grid search, played a pivotal role in optimizing model performance. By exhaustively exploring various combinations of hyperparameters, we identified the most effective configuration: a criterion of ‘gini’, a maximum depth of 15, and a minimum samples split of 10.

The culmination of these efforts resulted in a noteworthy accuracy of 0.746—a remarkable 17.6% improvement over our baseline model. This substantial enhancement suggests that our process of incorporating additional features and fine-tuning hyperparameters was effective in improving the model’s performance. Overall, our final model has a higher chance of correctly predicting a player’s position based on the given data values than our baseline model.

---

## Fairness Analysis

Fairness analysis aims to determine if a predictive model exhibits bias against specific groups within a population. In our analysis, Group X and Group Y were designated to represent players on the 'red' and 'blue' sides, respectively, to examine potential biases in our model's performance.

Null Hypothesis: The model is unbiased, suggesting that the precision scores for the blue and red teams are comparable, with any observed variations attributable to random fluctuations.

Alternative Hypothesis: The model exhibits bias, implying that the precision for the blue team notably exceeds that of the red team.

Precision was selected as the metric for evaluation, focusing on the disparity in precision scores between the red and blue team predictions. We established a significance threshold of 0.05 and conducted 500 permutation tests.

<iframe
  src="assets/Permutation Test Distribution of Precision Difference (Blue - Red).html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Conclusion

P-value: 0.51

Because the p-value is greater than the significance level of 0.05, we fail to reject the null hypothesis. As such, there's insufficient evidence to assert that our model favors the blue team over the red team regarding precision scores. This outcome suggests that our model operates without detectable bias, and any differences in precision between the two groups likely stem from chance.