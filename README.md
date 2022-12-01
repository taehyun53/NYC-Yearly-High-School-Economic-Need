# New York City Poverty Analysis

![alt text](https://static01.nyt.com/
/2020/12/09/nyregion/10nytoday1/10nytoday1-mediumSquareAt3X.jpg)

Economic Need Index is the measure of the socioeconomic circumstances of a school’s population and the score is assigned to each school on the number of students eligible for free lunch or public assistance or who live in temporary housing. Through the Demographic Snapshot District data, I extracted useful insights and created visualizations through Tableau, matplotlib and Seaborn. I also performed statistical tests and built machine learning models that can predict the economic need index. 

# 1. Data Source
NYC open data is a open data with free published data in New York City. City organizations and people often utilize this website to gather data to extract useful insights about the city in New York. 

# 2. Method
Used a data sheet with demographics for 1831 schools from years 2016 - 2021. The column economic need index is the target variable which is the key value of this project. I was able to predict which borough will have small or large economic need index value with my domain knowledge. The aim of this project is to provide a solution by building a regression model to predict the economic need index value. 


# 3. Data Wrangling (simple - to understand data)

• Used Describe() and Info() functions to see the types of columns and the statistical summary for numeric values. 
- Changed the data types for columns that weren't having 

• NaN's and Duplicated rows - There were 0 NaN values and no duplicate rows. However, if there were, for the numeric values, it would be better to input mean or median. If the NaN's were categorical, it would be good to use imputation. 

• Column restructuring and filling or removing data gaps 
- 1. Switched the numeric values into percentages for simpler analysis.
- 2. Splitted the columns into separate columns using regex.
- 3. Created another column of economic need index with binary values. No Need support < mean < Need support

• Outliers - Most of the ourliers were within the 95% confidence interval. Eliminating those data points out of the 95 percent qunaitle range would be ideal but I kept it since those data points could be conatining valuable information. 

• Visualization - Quick visualization through histograms to see variances for each columns

![alt text](NewYorkCity_Education/image/scatter_all.jpeg)

• Merged the data sheets

# 4. Exploratory Data Analysis
• Correlation heat map 

![alt text](NewYorkCity_Education/image/heatmap.jpeg)

- Visualized correlations
- Especially the ones that were correlated with Economic Need Index
- Povert rate and Economic Need Index are strongly positively correlated to each other.

• Borough Analysis
- Visualized the relationship between Boroughs and Economic Need Index from years 2016 to 2021. 
- The descending order of economic need index is Bronx -> Brooklyn -> Manhattan -> Queens -> Staten Island
- Visualized the relationship between Grades and Economic Need Index for each boroughs from years 2016 - 2021.
![alt text](NewYorkCity_Education/image/barchart_grades_borough.jpeg)

- Most of the boroughs had decreasing trend of high school students

![alt text](NewYorkCity_Education/image/histplot_dw.jpeg)

- Visualized the relationship between Sex and Economic Need Index for each boroughs from years 2016 - 2021.

![alt text](NewYorkCity_Education/image/scatter_sex.jpeg)

- More males needed support.

![alt text](NewYorkCity_Education/image/sex_box.jpeg)

-Visualized the relationship betwee Economic Need Index and student with disabilities 
![alt text](NewYorkCity_Education/image/scatter_disability.jpeg)


- Visualized the relationship between students who are english learners and Economic Need Index
![alt text](NewYorkCity_Education/image/eng_learners_scatter.jpeg)


• Two tailed ANOVA test

![alt text](NewYorkCity_Education/image/ANOVA_jpeg.png)

- Borough and Year
- The p value obtained from ANOVA analysis for Borough, years, and Economic Need Index are statistically significant (p<0.05). We conclude that type of Borough significantly affects the Economic Need Index outcome, time (years) significantly affects the Economic Need Index outcome, and interaction of both Borough and time (years) significantly affects the yield outcome.

![alt text](NewYorkCity_Education/image/scatter_sex.jpeg)


• Multiple pairwise comparisons (Post-hoc test) Permalink¶
Now, we know that Borough and time (years) differences are statistically significant, but ANOVA does not tell which Borough and time (years) are significantly different from each other. To know the pairs of significant different Borough and time (years), perform multiple pairwise comparison (Post-hoc comparison) analysis using Tukey’s HSD test.

• The null hypothesis is that there are no statistical difference between each groups.

![alt text](NewYorkCity_Education/image/boxplot_econ_ANOVA.jpeg)

- All the years are statistically different to each other by its economic need index by it's p value which is less than 0.05. 

![alt text](NewYorkCity_Education/image/Tukey.jpeg)
![alt text](NewYorkCity_Education/image/Tukey2.jpeg)
![alt text](NewYorkCity_Education/image/Tukey3.jpeg)
![alt text](NewYorkCity_Education/image/Tukey4.jpeg)
- All boroughs are statisticall different to each other by its ecoonomic need index by it's p value which is less than 0.05. 

Tableau Dash Board:
https://public.tableau.com/app/profile/taehyun.yoon/viz/NYCDATA_16676552294860/Dashboard1?publish=yes

# 5. Preprocessing
• Feature Engineering
- Train/Validation/Test set are divided in a 80/10/10 ratio.
- Numeric features - scaling - Standard Scaler
- Categorical features - encoding - One Hot Encoding


# 6. Algorithms and Machine Learning
• Started from High interpretability, low accuracy model to low interpretability, high accuracy model. 
• Simpler the model the better it is because it prevents overfitting, higher interpretability, and more computational efficiency.

Used Optuna (software framework for hyperparamter tuning) to perform Bayesian Optimization instead of GridSearchCV.

Process:
• Started with the linear regression model -> decision tree regressor -> Catboost(boosting) -> Neural Network(Multilayer Perceptron)

The RMSE scores decreased gradually. Catboost had the best RMSE score therefore, Catboost is the best performing model. Multilayer perceptron showed overfitting and the other two models (linear regressiona dn decision tree) showed higher RMSE value than CatBoost. 


# 7. Evaluation 
Cat Boost is the best model that performed with a RMSE value of 4.44

![alt text](NewYorkCity_Education/image/model_eval.jpeg)

These are the feature importance. 

The feature importance Visualization can be done using SHAP values. SHAP (SHapley Additive exPlanations) is a game theoretic approach to explain the output of any machine learning model. 

![alt text](NewYorkCity_Education/image/SHAP_bar.jpeg)

But SHAP values do not provide causality it just tells the contribution. So I had to make sure what determines the economic need index and use SHAP for visualization purposes. 

The descening order of the SHAP value: # White -> # English Language Learners -> Year 2016-2017.

# 8. Solution

![alt text](NewYorkCity_Education/image/Bronx_Staten.jpeg)

I provided three possible solutions to decrease the borough - Bronx. Through Exploratory Data Analysis, I figured the population of white, hispanic, English Learners, grades between 9 - 12 were the major difference between boroughs with high economic need index and low economic need index. So I did some experiments over these values. I adjusted these variables and used my machine learning model to estimate the economic need index. 

The following statements are the result of my experiment.

From decreasing the economic need index from 84 to 76 by decreasing the scale of # English Learners by 0.15, the economic need index decreased by 0.08.

The main goal of this project was to build a predictive regression model that can predict the economc need index and make conclusions to inform the results to NYC.

3 possible solutions which can help decrease the economic need index.
Increase the number of white people
• By increasing the white population by 0.1 in the given condition where mean = 0 and std = 1, there was a decrease in economic need index by 0.069

Decrease the number of black and hispanic people
• By decreasing the black population by 0.3 in the given condition where mean = 0 and std = 1, there was a decrease in economic need index by 0.015

• By decreasing the hispanic population by 0.15 in the given condition where mean = 0 and std = 1, there was a decrease in economic need index by 0.1

Decrease the number of english learners
• By decreasing the english learners by 0.2 in the given condition where mean = 0 and std = 1, there was a decrease in economic need index by 0.08





