# Customer-Retention
This project is part of a Virtual Internship on Power BI with PwC Switzerland, offered through the Data Analytics Virtual Case Experience by Forage.
# Problem Statement
Janet, the Retention Manager at PhoneNow, has requested the development of a comprehensive dashboard to gain insights into customer retention for the telecom company. While the retention department currently operates reactively by reaching out to customers after they have terminated their contracts, there is a need for a proactive approach to identify at-risk customers before they leave.

The telecom company has previously attempted customer analysis using Excel, but these efforts have not yielded actionable results. To address this, the dashboard should visualize customer data in a clear and self-explanatory way for management.

The aim is to provide a clear overview of customer behavior and retention patterns to help the retention department act preemptively. The dashboard will support the retention team in developing strategies that reduce churn and improve customer loyalty.

Here is an overview of the data preparation process based on the dataset you provided:

# Data Preparation

The customer retention dataset, consisting of 23 columns and multiple observations, was loaded into Power BI Desktop for modeling after transformation in Power Query. Below is a tabulation of the columns and their descriptions:

| **Column Name**      | **Description**                                                            |
| -------------------- | -------------------------------------------------------------------------- |
| customerID           | Represents the unique number of the customer in the dataset                |
| gender               | Describes the gender of the customer                                        |
| SeniorCitizen        | Describes if the customer is a senior citizen                               |
| Partner              | Describes if the customer has a partner                                     |
| Dependents           | Describes if the customer has dependents                                    |
| tenure               | Describes the tenure (number of months as a customer)                       |
| PhoneService         | Describes if the customer has registered a phone service                    |
| MultipleLines        | Describes if the customer has multiple phone lines                          |
| InternetService      | Describes if the customer has registered for internet service               |
| OnlineSecurity       | Describes if the customer has registered for online security                |
| OnlineBackup         | Describes if the customer has registered for online backup                  |
| DeviceProtection     | Describes if the customer has registered for device protection              |
| TechSupport          | Describes if the customer has registered for tech support                   |
| StreamingTV          | Describes if the customer has registered to stream TV                       |
| StreamingMovies      | Describes if the customer has registered to stream movies                   |
| Contract             | Describes the contract type of the customer                                 |
| PaperlessBilling     | Describes if the customer has registered for paperless billing              |
| PaymentMethod        | Describes the customer's payment method                                     |
| MonthlyCharges       | Represents the customer's monthly charges                                   |
| TotalCharges         | Represents the total charges incurred by the customer                       |
| numAdminTickets      | Represents the number of admin tickets opened by the customer               |
| numTechTickets       | Represents the number of tech tickets opened by the customer                |
| Churn                | Describes if the customer is at risk of churn (Yes/No)                      |

**Data Cleansing:**

1. **ServiceCount Column:**
   A new column named `ServiceCount` was created using the following formula to count the number of services each customer has registered for:
   
   ```
   ServiceCount = IF([PhoneService] = "Yes", 1, 0) +
                  IF([MultipleLines] = "Yes", 1, 0) +
                  IF([InternetService] <> "No", 1, 0) +
                  IF([OnlineSecurity] = "Yes", 1, 0) +
                  IF([OnlineBackup] = "Yes", 1, 0) +
                  IF([DeviceProtection] = "Yes", 1, 0) +
                  IF([TechSupport] = "Yes", 1, 0) +
                  IF([StreamingTV] = "Yes", 1, 0) +
                  IF([StreamingMovies] = "Yes", 1, 0)
   ```

2. **Tenure Group Column:**
   Another column named `Tenure Group` was created to classify customers based on their tenure. The formula used for this is:

   ```
   Tenure Group = SWITCH(
       TRUE(),
       [tenure] < 12, "< 1 year",
       [tenure] >= 12 && [tenure] < 24, "1-2 years",
       [tenure] >= 24 && [tenure] < 36, "2-3 years",
       [tenure] >= 36 && [tenure] < 48, "3-4 years",
       [tenure] >= 48 && [tenure] < 60, "4-5 years",
       [tenure] >= 60 && [tenure] < 72, "5-6 years",
       "6+ years"
   )
 

# Data Analysis

Measures used in visualization are:

Churn Rate Calculation:
```dax
Churn Rate = 
DIVIDE(
    COUNTROWS(
        FILTER('01 Churn-Dataset', '01 Churn-Dataset'[Churn] = "Yes")
    ),
    COUNTROWS('01 Churn-Dataset')
)
```

```DAX
DeviceProtectionChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[DeviceProtection] = "Yes" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```

 Internet Service Churn Rate:
```DAX
InternetServiceChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[InternetService] <> "No" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```

Number of Churned Customers:
```DAX
Number of Churned = CALCULATE(COUNTROWS('01 Churn-Dataset'), '01 Churn-Dataset'[Churn] = "Yes")
```

Online Backup Churn Rate:
```DAX
OnlineBackupChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[OnlineBackup] = "Yes" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```

Online Security Churn Rate:
```DAX
OnlineSecurityChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[OnlineSecurity] = "Yes" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```

Phone Service Churn Rate:
```DAX
PhoneServiceChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[PhoneService] = "Yes" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```

Retention Rate:
```DAX
Retention rate = DIVIDE(
    COUNTROWS(
        FILTER('01 Churn-Dataset','01 Churn-Dataset'[Churn] = "No")),
    COUNT('01 Churn-Dataset'[customerID])
)
```

Senior Citizen Churn Rate:
```DAX
SeniorCitizenChurnRate = DIVIDE(
    SUM('01 Churn-Dataset'[SeniorCitizen]),
    COUNT('01 Churn-Dataset'[SeniorCitizen])
)
```

Streaming Movies Churn Rate:
```DAX
StreamingMoviesChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[StreamingMovies] = "Yes" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```

Streaming TV Churn Rate:
```DAX
StreamingTvChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[StreamingTV] = "Yes" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```

Tech Support Churn Rate:
```DAX
TechSupportChurnRate = DIVIDE(
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[TechSupport] = "Yes" &&
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    ), 
    COUNTROWS(
        FILTER(
            '01 Churn-Dataset', 
            '01 Churn-Dataset'[Churn] = "Yes"
        )
    )
)
```
# Insights
- High churn rate with a churn rate of 27%, over a quarter of customers are leaving.
- Churn is slightly higher for males, customers without a partner, and customers without dependents.
- Customers using electronic checks have the highest churn rate.
- Month-to-month contracts show significantly higher churn than one-year or two-year contracts.
- Customers who churn tend to have higher average monthly charges than those who stay
- Customers who use more than 3 services have higher churn rate.
- Senior customers have high churn rate with 25%.
- Customers use fiber optic churn more than DSL customers, which could reflect service quality issues.
- Customers with longer tenures most likely stay with the company
- Customers with a higher number of support tickets generally have a lower churn rate.
 However, churned customers logged significantly more tech support tickets, which may indicate that their issues were not  resolved properly.

# Suggestions
- Investigate the payment process for electronic check users and consider offering more seamless.
- Offer incentives or discounts to encourage customers on month-to-month contracts to switch to longer-term.
- Encourage customers to use additional services by offering bundled packages or cross-promotions, as those who engage with more services tend to have a lower churn rate.
- Introduce loyalty discounts, bundled services, or rewards for customers with higher monthly charges to improve their perceived value and reduce churn.
- mplement targeted strategies for senior citizens, such as simplified plans.
- Investigate and address the reasons for higher churn among fiber optic users, possibly through surveys or service quality assessments.
- Focus on improving the onboarding experience , loyalty discounts and customer support for new customers (with shorter tenures).
- Analyze unresolved tech support issues for churned customers and improve response times, regularly gather feedback from customers, to ensure issues are properly resolved and to identify areas for service improvement.
