# Credit Score classification

# Background:

The role of banks in modern market economies is pivotal, as they wield the power to decide who qualifies for financial support and under what terms. Their decisions can significantly influence investment choices, shaping the trajectory of both individuals and businesses. For a market-driven society to thrive, it is essential that individuals and companies have access to credit. To facilitate this access, banks employ credit scoring algorithms, which assess the likelihood of default, aiding in the determination of loan eligibility.

This project delves into a dataset obtained from Kaggle, sourced from a global finance company. Over the years, this company has compiled a wealth of data encompassing basic banking details and an extensive array of credit-related information. This comprehensive data repository has been assembled by the company's management with the strategic intent of constructing an intelligent system. This system aims to categorize individuals into distinct credit score brackets, with the ultimate goal of streamlining and automating the credit evaluation process, thus reducing the need for manual assessments and interventions.

# Goal:

Given a person’s credit-related information, build a machine learning model that can classify the credit score.

# Data Disctionary:

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

##ID and Name Columns
These columns represented the name and the unique identification number of a client. We removed the "ID" and "Name" columns as they didn't contribute to the model.

##Customer_ID Column
Column description: Represents a unique identification of a person

This column was Initially used for filling missing values with a fuction we generated called "replace_missing_Values" (per customer ID backward fill, if not having value-> forward fill); we later removed this column after its purpose was fulfilled.

##Month Column

Column Description: Represents the month of the year

We converted the "Month" column from object to integer by imputing the months with their respective numerical values. We discovered that the dataset uniformly represented 12,500 unique customer IDs each month, suggesting recurring interactions with this specific group of customers.

##Age Column
Column description: Represents the age of the person

We cleaned the "Age" column by removing non-numeric characters and replacing ages over 90 and under 10 with NaN. Missing values were imputed using customer IDs.

##SSN Column
Column description: Represents the social security number of a person

We deleted the "SSN" column, as it contained sensitive information irrelevant to our analysis.

##Occupation Column
Column description: Represents the occupation of the person

We replaced blank occupation values with NaN and filled missing values with random choices. Later, we used label encoding to convert this column into numerical format.

##Annual_Income Column
Column description: Represents the annual income of the person

We removed non-numeric characters and changed the data type from integer to float. Outliers were retained, as the company's global nature resulted in varied income levels based on location and other factors.

##Monthly_Inhand_Salary Column
Column description: Represents the monthly base salary of a person

We imputed missing values using customer IDs by applying the "replace_missing_Values" fuction and retained a few outliers due to global income variations.

##Num_Bank_Accounts Column
Column description: Represents the number of bank accounts a person holds

We treated negative values as zero and outliers by setting values over 10 to NaN, as they were considered unrealistic. We finalized by using the “replace_missing_values” function to replace the missing values using the customer ID (per customer ID backward fill, if not having value-> forward fill).

##Num_Credit_Card Column
Column description: Represents the number of other credit cards held by a person

Similar to bank accounts, we replaced values over 10 with NaN and used the “replace_missing_values” function.

##Interest_Rate Column
Column description: Represents the interest rate on credit card

Values over 48% were set to NaN, as they were considered invalid. Outliers were treated similarly with the "replace_missing_values" function.

##Num_of_Loan Column
Column description: Column description: Represents the number of loans taken from the bank

Values over 100 were set to NaN, and the column was later converted to categorical, creating a new column named "Loan_Category" to reduce the noise caused by the significant number of outliers.

##Loan_Category Column
Column description: This is a categorical column created from the Num_of_Loan column and distributes the numbers of loans between Low, Medium and High.

This categorical column was converted to numerical format.

##Type_of_Loan:
Column description: Represents the types of loan taken by a person

We opted to eliminate the Type_of_Loan column due to its inherent complexity, coupled with its redundancy when compared to the Num_of_Loan column. This decision enables us to focus our resources on other aspects of the analysis while maintaining the overall quality and usability of our dataset.

##Delay_from_due_date
Column Description: Represents the average number of days delayed from the payment date

We had a couple negative values in this column but having a negative number didn’t make sense considering we’re talking about the number  of days delayed from the payment date. For that reason we decided to set negative values to zero

##Num_of_Delayed_Payment
Column description: Represents the average number of payments delayed by a person

Missing values were imputed using customer IDs, and the column was later eliminated to be replaced by a new column named "Delayed_Payment_Category" to reduce the noise caused by the outliers. 

##Delayed_Payment_Category
Column description: This categorical column, derived from the "Num_of_Delayed_Payment" column, consists of three distinct categories (High, Medium, Low), each reflecting the level of loans acquired by a client

This categorical column was converted to numerical format.

##Changed_Credit_Limit:
Column description: Represents the percentage change in credit card limit

We treated missing values by replacing them with the mean and set negative values to NaN. Outliers were retained

##Num_Credit_Inquiries
Column description: Represents the number of credit card inquiries

We retained data below the mean and set higher values to NaN, as they appeared illogical. We proved the data was illogical by visualizing the behavior of the feature by clients by month. We took client CUS_0xa5f9 as an example. We generated a table that showed the number of credit inquiries for the client went from 12 in January to 1044 in february

##Credit_Mix column
Column description: Represents the classification of the mix of credits (Good, Bad and Standard)

We replaced "_" values with NaN and imputed missing values using customer IDs. The column was converted to numerical format.

##Outstanding_Debt column
Column description: Represents the remaining debt to be paid

Used the filter_col function to remove the “_” and “-” of the values. No further cleaning was required.

##Credit_Utilization_Ratio
Column description: Represents the utilization ratio of credit card

We adjusted decimal places but retained outliers

##Credit_History_Age:
Column description: Represents the age of credit history of the person

We extracted months and years into separate columns, "Credit_Age_Years" and "Credit_Age_Months," and deleted the original column.

##Payment_of_Min_Amount
Column Description: Represents whether only the minimum amount was paid by the person

We converted this column from categorical to numerical.

##Total_EMI_per_month:
Column description: Represents the monthly EMI payments

Outliers were retained.

##Amount_invested_monthly:
Column description: Represents the monthly amount invested by the customer

We replaced "__10000__" values with NaN and imputed missing values using customer IDs. Outliers were retained.


##Payment_Behaviour:
Column description: Represents the payment behavior of the costumer

We replaced special characters with NaN, imputed missing values using customer IDs, and converted categories to numerical values:

'Low_spent_Small_value_payments'= 1

'Low_spent_Medium_value_payments'= 2

'Low_spent_Large_value_payments' = 3

'High_spent_Small_value_payments'= 4

'High_spent_Medium_value_payments'=5

'High_spent_Large_value_payments'= 6

##Monthly_Balance column
Column description: Represents the monthly balance amount of the customer

We replaced "-333333333333333333333333333" values with NaN, filled missing values with the mean, and retained outliers because we’re talking about a global company and the income of their clients vary depending on their location so it makes sense to find the same behavior in their monthly balance.

##Credit_Score(Y):
Column description: Represents the bracket of credit score (Poor, Standard, Good).

This target column remained unchanged.


