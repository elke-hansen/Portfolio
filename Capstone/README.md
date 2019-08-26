## Capstone - Recommendation Engine

#### Overview

  This project is based around my keen interest in machine learning models for retail and ecommerce. The central ingredient for any retail related machine learning algorithm is a good Customer Relationship Management (CRM) software and related database. If you have in depth knowledge of who your current and potential customers are, you have already won half the battle.
From this, I had several ideas for models that can be created from a good customer database:

###### Starting Point: Customer Segmentation

1. Recommendation Engine
2. Predicting Trends : Sales Forecasting for Inventory Purchase
3. Targeted Marketing
4. Promotional Optimisation
5. Store Allocation (logistical process optimisation)

  I decided to create a recommendation engine as they can have a great impact on sales and profit margins when used properly. They also can be created very easily but can range from very simple to extremely complex so can be implemented quickly and improved over time.

  This is a work in progress - reach out if you're interested to know more, or if you have suggestions on further directions I could take.

## Current Progress

#### Data Collection

  Within the retail industry, customer sales data is a highly valued commodity so it is extremely difficult to come across datasets with enough customer and product information to be able to create this kind of product.
  The dataset I have chosen is a kaggle dataset on Brazilian Ecommerce Data and can be found [here](https://www.kaggle.com/olistbr/brazilian-ecommerce).
  I initially thought that this would be a good dataset to work on as it has transactional data with unique customer, seller and product information and the dataset is well structured (see data schema for more information).
 
#### EDA: Exploratory Data Analysis
  The initial cleaning and exploration process exposed some interesting features of the data (see attached data dictionary for full explanations of each variable):
  1. Extensive shipping and geolocation data: would be good for looking into logistics modelling. These variables also proved to be highly valuable when looking at the correlation between shipping times and review scores. 
  2. Incomplete Time Series data: The sales are recorded over one and a half years, with two months showing little to no data in the busiest periods (Nov/Dec) which could pose problems for trying to run future sales predictions.
  3. Categorical variables (such as product ids, customer ids, and review scores) with highly unbalanced classes.
  
  Upon further exploration of the data, I have discovered that it is a little more difficult than I had initially anticipated to use for recommendations as the business model of this particular company is more like that of an intermediary between sellers and marketplaces. This means that while the company does have transactional data, the customer loyalty generally belongs to the marketplace so there are not so many repeat purchases, making it hard to predict customer behaviour. 
  This will pose a further challenge in creating the recommendation engine but I think I may be able to get some useful information from the product category and rating data.
  
#### Next Steps:
* Clustering of data to see if there are any obvious customer or product segmentation
* Trial and error of simple models
* Combination of models and increased complexity
* Strategy for how to track and incorporate (hypothetical) future sales
