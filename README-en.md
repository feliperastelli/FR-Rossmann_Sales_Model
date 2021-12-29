### :pushpin: [__Read in Portuguese__](https://github.com/feliperastelli/FR-Rossmann_Sales_Model/blob/main/README.md)

# Rossmann Sales Model

The goal of this project is to provide the Rossmann Drug Stores CFO a **sales forecast model** for the next six weeks so that he can set a specific budget for store renovations. The prediction model used does not meet the company's needs, therefore, the machine learning model developed in this project came as an exact solution for this business problem.

The project was developed using the CRISP-DM technique, and at the end of the first development cycle it was possible to produce a prediction model with a **MAPE Error index of 9%** using the **XGBoost** algorithm.

In terms of business, the result of this forecasting model can be summarized with the numbers below:

| __Scenarios__ | __Values__ |
| ------------- | -----------|
| predictions	| US$ 282,662,848.00 |
| worst scenario | US$ 281,907,880.11 |
| best scenario	| US$ 283,417,771.65 |

*The worst scenario considers the calculated model error (MAE) negatively and the best scenario positively.

To view the result of each store's forecast, a bot was built in the Telegram application, where the user can enter the store number and will have the return of the forecast calculated by the model that was put into production in Heroku. In other words, the model and bot were deployed in production so they can be accessed from anywhere.

To access, just have the app installed on your smartphone or PC, create an account, and ask the Bot contact for the store number. Ex: '/22', '/50'. Do the test:

[<img alt="Telegram" src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"/>]( http://t.me/vs_rossmannbot)

## 1. About Rossmann Drug Store

### 1.1 Business Context:

Rossmann is one of the largest drugstore chains in Europe, with approximately 56,200 employees and more than 4000 stores in several countries, such as Germany, Poland, Hungary, Czech Republic, Turkey, Albania, Kosovo and Spain. It is a company with a wide range of products that are offered to its customers, including its own products. The company is expanding rapidly and at a high pace, with large investments.

### 1.2 Business Question:

As mentioned above, the project was developed from the need of the company's CFO to have a sales forecast for the next six weeks, to guarantee and evaluate the specific budget for possible renovations to the stores. In the past, it did not have a forecasting model and the method used was not satisfactory.

### 1.3 About the Data:

The data were made available by the company on the Kaggle platform: https://www.kaggle.com/c/rossmann-store-sales/data

|***Attribute*** | ***Description*** |
| -------- | --------- |
|**Id** | an Id that represents a (Store, Date) duple within the test set |
|**Store** | a unique Id for each store |
|**Sales** | the turnover for any given day (this is what you are predicting) |
|**Customers** | the number of customers on a given day |
|**Open** | an indicator for whether the store was open: 0 = closed, 1 = open |
|**StateHoliday** | indicates a state holiday. Normally all stores, with few exceptions, are closed on state holidays. Note that all schools are closed on public holidays and weekends. a = public holiday, b = Easter holiday, c = Christmas, 0 = None |
|**SchoolHoliday** | indicates if the (Store, Date) was affected by the closure of public schools |
|**StoreType** | differentiates between 4 different store models: a, b, c, d | 
|**Assortment** | describes an assortment level: a = basic, b = extra, c = extended | 
|**CompetitionDistance** | distance in meters to the nearest competitor store | 
|**CompetitionOpenSince[Month/Year]** | gives the approximate year and month of the time the nearest competitor was opened| 
|**Promo** | indicates whether a store is running a promo on that day | 
|**Promo2** | Promo2 is a continuing and consecutive promotion for some stores: 0 = store is not participating, 1 = store is participating | 
|**Promo2Since[Year/Week]** | describes the year and calendar week when the store started participating in Promo2 | 
|**PromoInterval** | describes the consecutive intervals Promo2 is started, naming the months the promotion is started anew. E.g. "Feb,May,Aug,Nov" means each round starts in February, May, August, November of any given year for that store | 

### 1.4 Business Assumptions:

- The days when stores were closed (Open) were removed from the analysis.
- Only stores with sales values bigger than 0 were considered.
- For stores which did not have Competition Distance information, it was considered that the distance should be the longest distance observed in the data set.

## 2. Solution planning:

The project was developed using the CRISP-DM method, applying the following steps:

**Step 01 - Data Description:** At this stage, the objective was to know the data, its types, use statistical metrics to identify outliers in the business scope and also analyze basic statistical metrics such as: mean, median, maximum, minimum, range, skew, kurtosis and standard deviation. At this stage, some adjustments were also made to dataset features, such as filling NA's for example.

**Step 02 - Feature Engineering:** At this stage, a mental map was developed to analyze the phenomenon, its variables and the main aspects that impact each variable. From the characteristics of the hypotheses and the need for new attributes, new features were raised from the original variables, in order to improve the phenomenon of being modeled.

**Step 03 - Data Filtering:** The purpose of this step was to filter lines and exclude columns that are not relevant to the model or are not part of the business scope, such as disregarding days when stores were not operating and/or there were no sales.

**Step 04 - Exploratory Data Analysis:** The purpose of this step was to explore the data to find insights, better understand the relevance of the variables in learning the model. Univariate, bivariate and multivariate analyzes were performed, using numerical and categorical data from the set.

**Step 05 - Data Preparation:** At this stage, the data were prepared for the beginning of machine learning model applications. Techniques such as Rescaling and Transformation were used, through encodings and nature transformation.

**Step 06 - Feature Selection:** The objective of this step was to select the best attributes to train the model. The Boruta algorithm was used to select the variables, highlighting those that were more relevant to the phenomenon.

**Step 07 - Machine Learning Modeling:** At this stage, tests and training of some machine learning models were carried out, where it was possible to compare their respective performance and to choose the ideal model for the project. The Cross Validation technique was even used to guarantee the real performance on the selected data.

**Step 08 - Hyperparameter Fine Tunning:** Having chosen the XBoost algorithm in the previous step, an analysis was performed using the Randon Search method to choose the best values for each of the model's parameters. At the end of this step, it was possible to obtain the final values of the model's performance.

**Step 09 - Error Translation and Interpretation:** The objective of this stage was in fact to demonstrate the result of the project, where it was possible to evaluate the performance of the model with a business bias, demonstrating the financial result that can be expected if the developed model is applied.

**Step 10 - Deploy Model to Production:** After successful execution of the model, the objective was to publish it to a cloud environment so that other people or services can use the results to improve the business decision. The chosen cloud application platform was Heroku.

**Step 11 - Telegram Bot:** The final stage of the project was to create a bot in the messaging app - Telegram, which makes it possible to consult forecasts at any time and place, as it was also deployed on the cloud platform.

## 3. Key Insights:

**Hypothesis 1:** Stores with larger assortments should sell more.
  **False:** Stores with a larger assortment sell LESS.

![image](https://user-images.githubusercontent.com/77105763/142774353-0b11753d-f737-4cd9-ba9c-9075dc34ee0e.png)

**Hypothesis 2:** Stores should sell more over the years.
  **False:** Stores sell less over the years.

![image](https://user-images.githubusercontent.com/77105763/142774441-47a4439b-f8c3-458d-a93c-8b22f544a6ce.png)

**Hypothesis 3:** Stores should sell more in the second half of the year.
  **False:** Stores sell less in the second half of the year.
  
  ![image](https://user-images.githubusercontent.com/77105763/142774550-6be3c362-d896-46c7-a57a-3b17914b68c1.png)

*Other insights can be found in the project notebooks.*

## 4. Machine Learning Models Performance:

The project data were tested with linear and non-linear models. The strategy of selecting 5 types of models was used: Mean model, two linear models, and two non-linear models. The average for example served as a reference base. Linear models serve to assess the learning complexity of the dataset. If the performance was bad, I could understand that a more complex model would be needed.

**- Linear Models:**

   - Average
   - Linear Regression 
   - Linear Regression Regularized

**- Nonlinear Models:**

   - Random Forest Regressor 
   - XGBoost Regressor

**Comparing the performance of models:**

***Model Name*** | ***MAE CV*** | ***MAPE CV*** | ***RMSE CV*** |
| ---------------- | ---------- | --------- | ---------- |
|Random Forest Regressor | 842.56 +/- 220.07 | 0.12 +/- 0.02	 | 1264.33 +/- 323.29 |
|XGBoost Regressor | 1048.45 +/- 172.04 | 0.14 +/- 0.02	 | 1513.27 +/- 234.33 |
|Average Model | 1354.80 | 0.45	 | 1835.13 |
|Linear Regression | 2081.73 +/- 295.63 | 0.3 +/- 0.02	 | 2952.52 +/- 468.37 |
|Lasso | 2116.38 +/- 341.5 | 0.29 +/- 0.01	 | 3057.75 +/- 504.26 |

**Final performance of the chosen model after Hyperparameter Fine Tuning:**

***Model Name*** | ***MAE*** | ***MAPE*** | ***RMSE*** |
| -------- | --------- | --------- | --------- |
|XGBoost Regressor | 673.394631 | 0.097298	 | 965.731681 |

## 5. Final result - Model performance vs Business Values

The final result of the project was satisfactory for most of the stores covered in the data, as shown in the graph below (These stores in particular may contain particularities and possibly in a second cycle of this project, something could be done to improve performance and prediction for them).

![image](https://user-images.githubusercontent.com/77105763/143149982-0e6c1f18-3874-412a-a82f-01ff03b13c85.png)

Most stores had the MAPE error very close to the error performed in the model - **MAPE Error 9%**

As indicated in the previous project summary, the result that can be obtained using the model, considering the best and worst scenario, is as follows:

| __Scenarios__ | __Values__ |
| ------------- | -----------|
| predictions	| US$ 282,662,848.00 |
| worst scenario | US$ 281,907,880.11 |
| best scenario	| US$ 283,417,771.65 |



We can observe the performance of the model, evaluating the relationship between sales (test data) and predictions:

![image](https://user-images.githubusercontent.com/77105763/143151060-c9ef9bcd-a266-4a1a-9457-e99a203d77d6.png)

## 6. Conclusion

The developed project was successfully concluded, where it was possible to project sales for the coming weeks so that the CFO has real information to create the stores' budget, being able to consult each prediction in real time.

- The deployment of the developed model and the Telegram Bot application were built in the **Heroku** cloud environment and are in operation.

- All project documentation can be consulted in the repository, including developed notebooks and all final scripts for web applications.
