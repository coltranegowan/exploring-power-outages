# Exploring Power Outages
Coltrane Gowan



## Introduction
This project analyzes the dataset Power Outages from Purdue University’s Laboratory for Advancing Sustainable Critical Infrastructure, which contains data on major power outage events in the continental U.S. from January 2000 to July 2016. All outages part of this dataset affected at least 50,000 customers or caused an energy loss of 300 MegaWatts or more. Information on each outage includes the geographical location of the outages, date and time of the outages, regional climatic information, land-use characteristics, electricity consumption patterns and economic characteristics of the states affected by the outages.

This dataset contains 57 columns and 1534 rows, but for the purposes of this project, these are the columns I will focus on:
| Column Name | Information |
| ----------- | ----------- |
| 'YEAR' | Indicates the year when the outage event occurred |
| 'MONTH' | Indicates the month when the outage event occurred |
| 'U.S._STATE' | Represents all the states in the continental U.S. |
| 'CLIMATE.REGION' | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.) |
| 'ANOMALY.LEVEL' | This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W) |
| 'CLIMATE.CATEGORY' | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI) |
| 'OUTAGE.START.DATE' | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region) |
| 'OUTAGE.START.TIME' | This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region) |
| 'OUTAGE.RESTORATION.DATE' | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region) |
| 'OUTAGE.RESTORATION.TIME' | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region) |
| 'CAUSE.CATEGORY' | Categories of all the events causing the major power outages |



The question I want to center this exploration on is:
How have characteristics of major power outages changed over time, and specifically how has the changing climate influenced how the distribution of these outages?

In researching this question, we can better understand the role of the climate in our energy grid, and determine whether more intense precautions need to be put into place to curb the frequency of these events.


## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
First, I need to clean the dataframe to prepare it for the upcoming analysis.

- I only want to keep the columns listed above: 'YEAR', 'MONTH', 'U.S._STATE', 'CLIMATE.REGION', 'ANOMALY.LEVEL', 'CLIMATE.CATEGORY', 'OUTAGE.START.DATE', 
    'OUTAGE.START.TIME', 'OUTAGE.RESTORATION.DATE', 'OUTAGE.RESTORATION.TIME', and 'CAUSE.CATEGORY'. All other columns are dropped. This way, I do not waste space and computing time with columns that are not relevant.

- Then, I combine the 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' columns into one 'OUTAGE.START' column in datetime format for efficiency.

- I do the same for the 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME' columns, combining them into 'OUTAGE.RESTORATION' in datetime format.

- After, I drop the old columns '.DATE' and '.TIME' respectively, since they are no longer needed.


The first couple rows of the cleaned dataframe are shown below:

|   OBS |   YEAR |   MONTH | U.S._STATE   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | OUTAGE.START        | OUTAGE.RESTORATION   |   OUTAGE.DURATION | SEASON   |
|------:|-------:|--------:|:-------------|:-------------------|----------------:|:-------------------|:-------------------|:--------------------|:---------------------|------------------:|:---------|
|     1 |   2011 |       7 | Minnesota    | East North Central |            -0.3 | normal             | severe weather     | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |        51         | Summer   |
|     2 |   2014 |       5 | Minnesota    | East North Central |            -0.1 | normal             | intentional attack | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |         0.0166667 | Spring   |
|     3 |   2010 |      10 | Minnesota    | East North Central |            -1.5 | cold               | severe weather     | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |        50         | Fall     |
|     4 |   2012 |       6 | Minnesota    | East North Central |            -0.1 | normal             | severe weather     | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |        42.5       | Summer   |
|     5 |   2015 |       7 | Minnesota    | East North Central |             1.2 | warm               | severe weather     | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |        29         | Summer   |


### Univariate Analysis
To start off, I explore some univariate data to identify patterns in the data.

I first wanted to visualize the amount of outages per year to identify potential outliers:

<iframe
  src="outages_per_year.html"
  width=800
  height=600
  frameborder=0
></iframe>


I then looked at the amount of outages by cause in the dataset overall:

<iframe
  src="assets/outages_by_cause.html"
  width=800
  height=600
  frameborder=0
></iframe>

### Bivariate Analysis
Next, I look at the possible relationships between columns, which will help guide the direction in which I will explore this data. Here are a couple I found relevant to my question.

Here I looked at the the amount of outages per year and cause combined, to get a sense of the distribution of the data as a whole:

<iframe
  src="assets/outages_by_year_cause.html"
  width=800
  height=600
  frameborder=0
></iframe>


I also singled out the amount of severe weather outages in the dataset to see if there was a clear trend worth diving in to, but there was no clear trend in the data:

<iframe
  src="assets/severe_outages.html"
  width=800
  height=600
  frameborder=0
></iframe>

### Interesting Aggregates
I then made several pivot tables, grouping by different statistics that I thought would be telling in understanding the distribution of the data. In the dataframe below, I grouped by 'CLIMATE.CATEGORY' and 'CAUSE.CATEGORY', seeing if there was a meaningful difference in distribution based on the climate:

| CLIMATE.CATEGORY   |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |\n|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|\n| cold               |                  19 |                      19 |                  122 |          15 |              22 |              239 |                              37 |\n| normal             |                  28 |                      26 |                  226 |          17 |              34 |              354 |                              59 |\n| warm               |                  10 |                       5 |                   70 |          14 |              13 |              166 |                              30 |


## Assessment of Missingness
### NMAR Analysis
I believe that 'OUTAGE.RESTORATION' is NMAR because its missingness appears to depend on the nature of the outage itself. Specifically, when an outage is labeled as an 'intentional attack' in the 'CAUSE.CATEGORY' column, the 'OUTAGE.RESTORATION' column is often missing. This suggests that the reason for missingness is related to the restoration process itself - perhaps in cases of intentional attacks, restoration times are either classified, unknown, or handled differently, rather than being missing at random. To determine if the missingness is actually MAR, I could collect additional data such as the severity of the attack or security-related restrictions that might explain why restoration times are not recorded for these cases.

### Missingness Dependency
To test the missingness dependency, I performed a permutation test to determine whether the missingness in the 'OUTAGE.RESTORATION' column is dependent on the 'CAUSE.CATEGORY' column. Specifically, I tested whether the variance of missingness proportions across different outage causes is significantly higher than what would be expected under the assumption that missing values are independent of the outage cause.



Since the p-value is 0, I now have evidence that missingness is not random with respect to 'CAUSE.CATEGORY'. This suggests that outages categorized as intentional attacks may systematically lack restoration time data. 


I also tested to see whether the 'OUTAGE.RESTORATION' column was dependent on the 'MONTH' column. In this permutation test, the p-value was 0.49, reaffirming my idea that the 'OUTAGE.RESTORATION' column is not dependent on a certain month or time but rather on the cause of the outage.

## Hypothesis Testing
I next wanted to test if there is a trend to the amount of intentional attacks per year in the dataset, so I performed a hypothesis test using chi-squared as the test statistic to see if the distribution is uniform.

Null Hypothesis: The amount of outages caused by intentional attacks is uniform throughout the years in the dataset.

Alternative Hypothesis: The amount of outages caused by intentional attacks is not uniform throughout the years in the dataset.

Performing this hypothesis test, I got a chi square statistic of 316.59, which gives a p-value of near 0. With this, there is evidence that the amount of intentional attacks per year is not uniform, and therefore has some trend to the data.

## Framing a Prediction Problem
My goal is to predict the cause of a power outage based on temporal featuresusing logistic regression. This is a multiclass classification problem because the response variable, 'CAUSE.CATEGORY', has multiple possible categories, like severe weather and intentional attack.

I will be using 'YEAR' and 'MONTH' as predictors because at the time of prediction, we would know the year and month but not details about the outage itself (e.g., severity, location, restoration time) since those are only known after the fact. Using only temporal features ensures we are making predictions based on information available before an outage occurs.

For analysis, I will be using mean accuracy for my simple model and later F1-Score to optimize my prediction and balance both the precision and recall, making my analysis more accurate.

## Baseline Model
My model is a logistic regression classifier designed to predict the cause of an outage using year and month as predictors, both of which are quantitative. There are no ordinal or nominal features in this model, meaning no categorical encoding was required. To preprocess the data, I handled missing values using mean imputation and standardized the numerical features using StandardScaler to ensure they were on a comparable scale. Since logistic regression is sensitive to feature scaling, this step was crucial for improving model performance.

To evaluate my model, I performed 5-fold cross-validation and computed mean accuracy as my primary metric. The mean accuracy across the folds was 53%. Given that the model relies solely on year and month, it has significant limitations in distinguishing between different outage causes. If the accuracy is close to what I would expect from random guessing, the model is likely not effective, and additional features would be needed to improve performance. However, if the model does outperform random guessing, it suggests that some seasonal trends such as varying weather may influence outage causes.

Despite being a simple baseline model, its current form is likely too limited to be considered a good model. Outage causes are complex and influenced by many external factors beyond time alone. To improve the model, I could incorporate additional predictive features, explore alternative evaluation metrics such as F1-score, and experiment with more advanced models like decision trees to capture nonlinear patterns in the data. Overall, while my logistic regression model provides a starting point, it is unlikely to be sufficient for accurate predictions without further refinement.

## Final Model
For my final model, I expanded the feature set by incorporating 'CLIMATE.REGION' and 'ANOMALY.LEVEL'. These additions were based on domain knowledge of outages—climate conditions and anomaly severity are likely to influence the cause of an outage. 'CLIMATE.REGION' provides categorical information about geographic areas, which is important since certain causes of outages, like severe weather, are geographically dependent. 'ANOMALY.LEVEL' captures deviations from normal conditions, which may also help distinguish between different causes of outages.

To preprocess the data, I applied different transformations based on feature types. Numerical features (YEAR, MONTH, ANOMALY.LEVEL) were imputed with their mean values and scaled using StandardScaler. I also applied a QuantileTransformer to normalize distributions and PolynomialFeatures (degree=2, interaction_only=True) to capture potential interactions between numerical features. For the categorical feature CLIMATE.REGION, I imputed missing values using the most frequent category and applied OneHotEncoder to convert it into a format suitable for machine learning.

For the modeling algorithm, I selected a Random Forest Classifier, a robust ensemble method that is well-suited for handling both numerical and categorical features while automatically managing feature importance and interactions. Through GridSearchCV, I tuned the hyperparameters using 5-fold cross-validation. The best hyperparameters found were:

- Number of Estimators (n_estimators): 100
- Maximum Depth (max_depth): 20
- Minimum Samples Split (min_samples_split): 2
- Minimum Samples Leaf (min_samples_leaf): 2

These hyperparameters balance model complexity and generalization, while preventing overfitting and maintaining the predictive power of my model.

Compared to the baseline logistic regression model, my final model showed a decent improvement in performance. The baseline model, which only used YEAR and MONTH, achieved an accuracy of 53%, suggesting that outages are not purely determined by temporal trends. By incorporating CLIMATE.REGION and ANOMALY.LEVEL, the final model achieved a cross-validation accuracy of 67.2%, a substantial improvement over the baseline. This indicates that environmental and climate-related factors are important predictors of outage causes.

## Fairness Analysis
For this fairness analysis, I examined whether the model’s performance differed significantly across two groups based on the 'CLIMATE.REGION' feature. Specifically comparing the Southeast (Group X) and Northwest (Group Y) regions. My evaluation metric was the weighted F1-score, which accounts for both precision and recall while adjusting for imbalances in the dataset. To determine if the observed difference in F1-scores was statistically significant, I conducted a permutation test with 1,000 shuffles.

Null Hypothesis: My model is fair. Its precision for outages in the Southeast and outages in the Northwest are roughly the same, and any differences are due to random chance.

Alternative Hypothesis: My model is unfair. Its precision for outages in the Southeast is different than outages in the Northwest.

The test statistic used was the difference in weighted F1-scores between Group X and Group Y. The observed difference was 0.0172, with Group X achieving an F1-score of 0.7498 and Group Y an F1-score of 0.7326. After running the permutation test, I computed a p-value of 0.928, which is far greater than the standard significance threshold of 0.05.

Since the p-value is so high, I fail to reject the null hypothesis. This result suggests that the observed difference in F1-scores is likely due to random chance rather than systemic bias in the model’s performance across these climate regions. In other words, the model appears to classify outages fairly between these two groups. However, if fairness remains a concern, I could explore additional fairness metrics such as demographic parity or equalized odds, or extend the analysis to other regions or features that may impact predictions.
