# Credit Score classification

# Background:

The role of banks in modern market economies is pivotal, as they wield the power to decide who qualifies for financial support and under what terms. Their decisions can significantly influence investment choices, shaping the trajectory of both individuals and businesses. For a market-driven society to thrive, it is essential that individuals and companies have access to credit. To facilitate this access, banks employ credit scoring algorithms, which assess the likelihood of default, aiding in the determination of loan eligibility.

This project delves into a dataset obtained from Kaggle, sourced from a global finance company. Over the years, this company has compiled a wealth of data encompassing basic banking details and an extensive array of credit-related information. This comprehensive data repository has been assembled by the company's management with the strategic intent of constructing an intelligent system. This system aims to categorize individuals into distinct credit score brackets, with the ultimate goal of streamlining and automating the credit evaluation process, thus reducing the need for manual assessments and interventions.

# Goal:

Given a person’s credit-related information, build a machine learning model that can classify the credit score.

# Data 

## Details:

28 columns/ caracteristics 

100000 entries

## Data Disctionary:

- **ID:** Represents a unique identification of an entry
- **Customer_ID:** Represents a unique identification of a person
- **Month:** Represents the month of the year
- **Name:** Represents the name of a person
- **Age:** Represents the age of the person
- **SSN:** Represents the social security number of a person
- **Occupation:** Represents the occupation of the person
- **Annual_Income:** Represents the annual income of the person
- **Monthly_Inhand_Salary:** Represents the monthly base salary of a person
- **Num_Bank_Accounts:**Represents the number of bank accounts a person holds
- **Num_Credit_Card:** Represents the number of other credit cards held by a person
- **Interest_Rate:**Represents the interest rate on credit card
- **Num_of_Loan:** Represents the number of loans taken from the bank
- **Type_of_Loan:** Represents the types of loan taken by a person
- **Delay_from_due_date:** Represents the average number of days delayed from the payment date
- **Num_of_Delayed_Payment:** Represents the average number of payments delayed by a person
- **Changed_Credit_Limit:** Represents the percentage change in credit card limit
- **Num_Credit_Inquiries:** Represents the number of credit card inquiries
- **Credit_Mix:** Represents the classification of the mix of credits
- **Outstanding_Debt**:** Represents the remaining debt to be paid
- **Credit_Utilization_Ratio:** Represents the utilization ratio of credit card
- **Credit_History_Age:** Represents the age of credit history of the person
- **Payment_of_Min_Amount:** Represents whether only the minimum amount was paid by the person
- **Total_EMI_per_month:** Represents the monthly EMI payments
- **Amount_invested_monthly:** Represents the monthly amount invested by the customer
- **Payment_Behaviour:** Represents the payment behavior of the customer
- **Monthly_Balance:**Represents the monthly balance amount of the customer
- **Credit_Score:**Represents the bracket of credit score (Poor, Standard, Good) ((Target))

# Data Cleaning steps:

## ID and Name Columns
These columns represented the name and the unique identification number of a client. We removed the "ID" and "Name" columns as they didn't contribute to the model.

## Customer_ID Column
Column description: Represents a unique identification of a person

This column was Initially used for filling missing values with a fuction we generated called "replace_missing_Values" (per customer ID backward fill, if not having value-> forward fill); we later removed this column after its purpose was fulfilled.

## Month Column
Column Description: Represents the month of the year

We converted the "Month" column from object to integer by imputing the months with their respective numerical values. We discovered that the dataset uniformly represented 12,500 unique customer IDs each month, suggesting recurring interactions with this specific group of customers.

## Age Column
Column description: Represents the age of the person

We cleaned the "Age" column by removing non-numeric characters and replacing ages over 90 and under 10 with NaN. Missing values were imputed using customer IDs.

## SSN Column
Column description: Represents the social security number of a person

We deleted the "SSN" column, as it contained sensitive information irrelevant to our analysis.

## Occupation Column
Column description: Represents the occupation of the person

We replaced blank occupation values with NaN and filled missing values with random choices. Later, we used label encoding to convert this column into numerical format.

## Annual_Income Column
Column description: Represents the annual income of the person

We removed non-numeric characters and changed the data type from integer to float. Outliers were retained, as the company's global nature resulted in varied income levels based on location and other factors.

## Monthly_Inhand_Salary Column
Column description: Represents the monthly base salary of a person

We imputed missing values using customer IDs by applying the "replace_missing_Values" fuction and retained a few outliers due to global income variations.

## Num_Bank_Accounts Column
Column description: Represents the number of bank accounts a person holds

We treated negative values as zero and outliers by setting values over 10 to NaN, as they were considered unrealistic. We finalized by using the “replace_missing_values” function to replace the missing values using the customer ID (per customer ID backward fill, if not having value-> forward fill).

## Num_Credit_Card Column
Column description: Represents the number of other credit cards held by a person

Similar to bank accounts, we replaced values over 10 with NaN and used the “replace_missing_values” function.

## Interest_Rate Column
Column description: Represents the interest rate on credit card

Values over 48% were set to NaN, as they were considered invalid. Outliers were treated similarly with the "replace_missing_values" function.

## Num_of_Loan Column
Column description: Column description: Represents the number of loans taken from the bank

Values over 100 were set to NaN, and the column was later converted to categorical, creating a new column named "Loan_Category" to reduce the noise caused by the significant number of outliers.

## Loan_Category Column
Column description: This is a categorical column created from the Num_of_Loan column and distributes the numbers of loans between Low, Medium and High.

This categorical column was converted to numerical format.

## Type_of_Loan:
Column description: Represents the types of loan taken by a person

We opted to eliminate the Type_of_Loan column due to its inherent complexity, coupled with its redundancy when compared to the Num_of_Loan column. This decision enables us to focus our resources on other aspects of the analysis while maintaining the overall quality and usability of our dataset.

## Delay_from_due_date
Column Description: Represents the average number of days delayed from the payment date

We had a couple negative values in this column but having a negative number didn’t make sense considering we’re talking about the number  of days delayed from the payment date. For that reason we decided to set negative values to zero

## Num_of_Delayed_Payment
Column description: Represents the average number of payments delayed by a person

Missing values were imputed using customer IDs, and the column was later eliminated to be replaced by a new column named "Delayed_Payment_Category" to reduce the noise caused by the outliers. 

## Delayed_Payment_Category
Column description: This categorical column, derived from the "Num_of_Delayed_Payment" column, consists of three distinct categories (High, Medium, Low), each reflecting the level of loans acquired by a client

This categorical column was converted to numerical format.

## Changed_Credit_Limit:
Column description: Represents the percentage change in credit card limit

We treated missing values by replacing them with the mean and set negative values to NaN. Outliers were retained

## Num_Credit_Inquiries
Column description: Represents the number of credit card inquiries

We retained data below the mean and set higher values to NaN, as they appeared illogical. We proved the data was illogical by visualizing the behavior of the feature by clients by month. We took client CUS_0xa5f9 as an example. We generated a table that showed the number of credit inquiries for the client went from 12 in January to 1044 in february

## Credit_Mix column
Column description: Represents the classification of the mix of credits (Good, Bad and Standard)

We replaced "_" values with NaN and imputed missing values using customer IDs. The column was converted to numerical format.

## Outstanding_Debt column
Column description: Represents the remaining debt to be paid

Used the filter_col function to remove the “_” and “-” of the values. No further cleaning was required.

## Credit_Utilization_Ratio
Column description: Represents the utilization ratio of credit card

We adjusted decimal places but retained outliers

## Credit_History_Age:
Column description: Represents the age of credit history of the person

We extracted months and years into separate columns, "Credit_Age_Years" and "Credit_Age_Months," and deleted the original column.

## Payment_of_Min_Amount
Column Description: Represents whether only the minimum amount was paid by the person

We converted this column from categorical to numerical.

## Total_EMI_per_month:
Column description: Represents the monthly EMI payments

Outliers were retained.

## Amount_invested_monthly:
Column description: Represents the monthly amount invested by the customer

We replaced "__10000__" values with NaN and imputed missing values using customer IDs. Outliers were retained.


## Payment_Behaviour:
Column description: Represents the payment behavior of the costumer

We replaced special characters with NaN, imputed missing values using customer IDs, and converted categories to numerical values:

'Low_spent_Small_value_payments'= 1

'Low_spent_Medium_value_payments'= 2

'Low_spent_Large_value_payments' = 3

'High_spent_Small_value_payments'= 4

'High_spent_Medium_value_payments'=5

'High_spent_Large_value_payments'= 6

## Monthly_Balance column
Column description: Represents the monthly balance amount of the customer

We replaced "-333333333333333333333333333" values with NaN, filled missing values with the mean, and retained outliers because we’re talking about a global company and the income of their clients vary depending on their location so it makes sense to find the same behavior in their monthly balance.

## Credit_Score(Y):
Column description: Represents the bracket of credit score (Poor, Standard, Good).

This target column remained unchanged.

# Data Visualizations:

## Histograms

![Histograms](https://github.com/carmeniturbe/credit_score/assets/98364829/7af353c1-e066-4889-95e8-752aa3c724fd)

- Month: The histogram of the 'Month' column in our dataset reveals a unique and intriguing distribution pattern. The 'Month' column represents the timeline of our data, and typically, one might expect to observe variations in the frequency of events or transactions across different months. However, in our case, we have consistently recorded 12,500 occurrences for each month, from January to August. This uniform distribution is a result of our data capturing a specific group of 12,500 unique customer IDs each month. This phenomenon suggests that the dataset contains recurring interactions with this set of customers, leading to an equal representation of each month's data. This finding provides valuable insight into our data collection process and underscores the importance of understanding the context and composition of our dataset when interpreting its distribution characteristics

- Age: The histogram of Age reveals an interesting distribution pattern. The graph exhibits a slight right skew, suggesting that the majority of our clients are younger than 50 years old. Notably, there is a pronounced peak in the number of clients around the ages of 37, indicating a significant concentration of clients within this age group. This insight can be valuable for tailoring our services or marketing strategies to cater to the needs and preferences of this specific demographic. Additionally, it's essential to consider the implications of this age distribution when performing further analyses or building predictive models, as age may be a relevant factor in understanding client behavior or outcomes

- Annual_Income: The histogram representing annual income can initially appear challenging to interpret due to its format in scientific notation (1e7). However, a more comprehensive examination, as demonstrated by the boxplot we reviewed earlier, reveals a wide range of income levels among our clients. The mean annual income is approximately 176,415, but what stands out is the presence of numerous outliers, with some clients reporting annual incomes as high as 24,198,060. This variation in income aligns with our understanding that clients' income levels can significantly differ based on factors such as their geographic location and client type, whether they are individuals or organizations.

- Monthly_Inhand_Salary: Much like the annual income, the histogram for monthly in-hand salary exhibits a noticeable right skew, suggesting that the majority of clients have relatively lower monthly salaries. The mean monthly salary stands at 4,198, with a substantial number of clients reporting salaries exceeding 15,000. This skewness reflects the income disparities among our clients, which can be attributed to various factors such as their occupation, seniority, or location.

- Num_Bank_Accounts, Num_Credit_Card, Credit_Utilization_Ratio, Credit_Age_Years: The histograms for these columns reveal a distinctive and often desirable distribution pattern - a bell-shaped or approximately normal distribution. In such distributions, the majority of clients tend to cluster around the central values, resulting in a symmetrical appearance with a peak at the mean. This suggests that a significant portion of our clients exhibit average or typical behaviors in terms of the number of bank accounts, the number of credit cards, credit utilization ratios, and credit age in years. This bell-shaped pattern indicates that most clients fall within a similar range, making these columns valuable for predictive modeling as they can help capture typical client profiles.

- Interest_Rate, Delay_from_due_date, Changed_Credit_limit, Num_Credit_Inquiries, Outstanding_Debt, Amount_Invested_Monthly, Monthly_Balance: These columns exhibit right-skewed distributions with notable commonalities. The majority of clients tend to have low values across these attributes, signaling a prevailing trend towards lower interest rates, minimal delays from due dates, negligible changes in credit limits, and limited outstanding debt. Additionally, clients typically report low levels of credit inquiries, modest monthly investments, and conservative monthly balances. This skewness suggests that our client base is characterized by a substantial number of individuals or organizations with conservative financial behaviors and relatively lower financial activity within these categories. While these attributes may dominate, it's essential to explore the tails of these distributions, as they may contain valuable insights into clients with unique financial characteristics or behaviors.

- Payment_of_min_amount and Loan_Category: These columns shed light on the distribution of clients across various loan categories and their payment behaviors. Notably, a substantial proportion of clients gravitate towards the medium range of the number of loans, indicating a common preference for a moderate level of loan engagement. Furthermore, the majority of clients demonstrate a consistent practice of paying the minimum required amount on their credits. This insight underscores the importance of offering a range of loan products that align with the preferences and financial capacities of our diverse client base. Additionally, it highlights the need for tailored strategies and communication to encourage clients to explore other payment options and financial management approaches.

## Heatmap

![Heatmap](https://github.com/carmeniturbe/credit_score/assets/98364829/67e09c80-56c6-40be-9870-9a39031f8c6c)

In our correlation map (heatmap), several noteworthy correlations emerge. First, we observe positive correlations between the Monthly_Inhand_Salary and both the Amount_Invested_Monthly and Monthly_Balance. This association is intuitive, suggesting that clients with higher monthly incomes tend to invest more and maintain higher monthly balances, which aligns with sound financial practices.

Another notable correlation is the positive relationship between Credit_Mix and Credit_Age_Years. This finding implies that clients with a diverse mix of credit types tend to have longer credit histories. This correlation underscores the potential benefits of maintaining a varied credit portfolio over time.

Furthermore, we identify a smaller yet meaningful positive correlation between Outstanding_Debt and both Interest_Rate and Delay_from_due_date. The latter metric represents the average number of days delayed from the payment date. This association suggests that clients with higher outstanding debts tend to face relatively higher interest rates and more frequent delays in payment, indicating a potential area of concern for further investigation.

**Target Feature - Credit Score:**

Turning our attention to the target feature, the credit score, we observe that it doesn't exhibit strong correlations with most other features in our dataset. However, the most significant positive correlations seem to originate from the Num_Credit_Inquiries, Interest_Rate, and Delay_from_due_date features. There is also a significant negative correlation with the Credit_Mix.

These correlations provide valuable insights into the relationships between key financial variables within our dataset, offering a foundation for deeper analyses and data-driven decision-making in our financial services and strategies.

## Credit score distribution

![Credit score](https://github.com/carmeniturbe/credit_score/assets/98364829/9b3f8448-b15c-4a13-80d1-27bccf72f599)

We observe that the dataset encompasses a diverse range of credit scores, with the majority of individuals falling into the 'Standard' category, accounting for 53,174 instances. Additionally, we note a significant representation of individuals classified as 'Poor,' with 28,998 instances. Finally, the 'Good' credit score category was observed for 17,828 individuals.

It's important to note that in this dataset, individual clients may appear in multiple instances, reflecting various interactions over time.

# Balanced Data

In order to address the class imbalance present in our dataset, we employed the Synthetic Minority Over-sampling Technique (SMOTE). Class imbalance, where one class is significantly underrepresented compared to others, can impact the performance of machine learning models. SMOTE is a widely used technique that helps rectify this imbalance by generating synthetic instances for the minority class. By doing so, we aimed to create a more balanced dataset, allowing our models to learn from a representative distribution of the classes. This approach is essential to prevent biases and improve the overall predictive accuracy and reliability of our models. We applied SMOTE as part of our data preprocessing strategy to ensure a more equitable representation of classes during model training and evaluation.

## Credit score before SMOTE

![Credit score Before](https://github.com/carmeniturbe/credit_score/assets/98364829/a379b755-312c-414d-8a5e-82a7866c4149)

## Credit score after SMOTE

![Credit score after](https://github.com/carmeniturbe/credit_score/assets/98364829/61a0f4dd-b6e7-4649-9e59-d3953ad6a1c6)

# Machine Learning

## Models
During the course of our analysis, we aimed to leverage the powerful LazyPredict library to quickly assess and compare the performance of various machine learning models. However, we encountered a challenge related to hardware limitations. Regrettably, our computing environment, which included limited RAM capacity, posed a constraint on our ability to fully utilize LazyPredict.

In light of this limitation, we made a deliberate choice to proceed with alternative modeling techniques that were better suited to our resource constraints. Specifically, we opted to implement the Random Forest Classifier, Xgboost Classifier and the K-Nearest Neighbors (KNN) Classifier for our dataset. These models were selected for their ability to handle our data effectively while requiring fewer computational resources.

## Confusion matrix of best model 

![Confusion matrix](https://github.com/carmeniturbe/credit_score/assets/98364829/25116ac0-f5a7-4e2f-8090-b9f111f20873)


## Classification report of best model

![Screen Shot 2023-09-21 at 18 57 40](https://github.com/carmeniturbe/credit_score/assets/98364829/8df04856-429f-4b97-bad3-56531d5b6523)


## Feature importance

We created this bar chart to evaluate the model's features importance. It's evident there are a few features with very little significance, which could allow us to develop another dataframe for future iterations of the model. 

![Feature importance](https://github.com/carmeniturbe/credit_score/assets/98364829/a30cd6a4-ee12-4f63-83e1-b4b0c37668ec)

# Conclusion

This Credit Score classification project involved extensive exploratory data analysis (EDA) and the evaluation of three different machine learning models: KNN classifier, Random Forest Classifier, and XGBoost classifier. Among these models, the Random Forest classifier emerged as the top performer, showcasing its effectiveness in predicting credit score categories based on client features.

To further assess the model's performance, we analyzed the recall, precision, and F1 Scores obtained from the classification report for the Random Forest classifier:

Recall, which measures the correct identification of specific classes, demonstrated that the model successfully identified approximately 94% of clients with a Good credit score, approximately 75% of clients with a Standard credit score, and 87% of clients with a Poor credit score.

Precision, representing the accuracy of predictions for each class, indicated that the model achieved precision rates of 84% for Good credit score predictions, 87% for Standard credit score predictions, and 85% for Poor credit score predictions.

The F1-score, a balanced metric between precision and recall, provided insight into the model's overall accuracy for each credit score category. The model achieved an F1-score of 89% for Good credit score, 80% for Standard credit score, and 86% for Poor credit score. These three F1-scores contributed to an overall accuracy of 85%.

In summary, this project resulted in the creation of a predictive model with an overall accuracy of 85%, capable of categorizing clients into Good, Standard, or Poor credit score classes based on their individual features. Notably, the Random Forest classifier outperformed the other models, excelling in its ability to accurately classify clients across all credit score categories, with a particular emphasis on effectively identifying clients with a Good credit score—a crucial aspect of the model's success.
