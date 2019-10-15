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

## Notebooks
  This project contains several notebooks detailing the processes used to create the final model:
  
#### Loading and Combining Data: 
 Shows the process of merging the 9 dataframes in the database. This was a good exercise that really helped me to understand the dataset and think about all the aspects of this business. In practise, the model would not require this combining of the data and it would just run on SQL queries.

#### Cleaning and EDA:
 This outlines the meat of the database and how the variables relate to each other. Through this, I could make decisions on which variables might be useful and what kinds of models I might be able to create from the dataset. I also used this to engineer some new features that I know to be important through domain knowledge.
 
#### Model Trial and Error:
 This is the main notebook where I tested and trained different models to decide on aspects of the data that can be predicted to inform good recommendations.

#### Final Model:
 Notebook containing the final model with the bare minimum pieces of code to get recommendations as well as some additional code for trial and error to create the flask app and flourish visualisation.


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
  
#### Modelling

  For the modelling aspect of this project I took a few approaches to see what kind of information I could predict to varying accuracy.

  1. Basic recommendation engine
  2. Review Score Target
  3. On Time Status Target
  4. Customer segmentation using clustering
  5. Customer segmentation using SVD and TFIDF
  6. Customer-Customer Collaborative Filtering
  7. Item-Item Collaborative Filtering

 For more information about the processes regarding the first 6 modelling trials, please refer to the Model Trial and Error notebook.

 #### Item-Item Collaborative Filtering

  After initially starting this project thinking that the customer based collaborative filtering model was the way to go, I slowly realised that an item-item filtering would be better for the following reasons:

  1. The number of products a company has is finite while the number of customers is potentially infinite.
  2. If I visualise the products connected to each other, you can see clustering when it comes to category and other product attributes.
  3. Most importantly, this particular dataset has 91,000 unique customers and only 31,000 unique products. This means:
    - more connections between the products as they have more frequency of purchase
    - 30 times shorter calculation time for the full product set (64 days versus 1,895)
    - smaller file size for storage
  
  The model works by calculating the cosine similarities between each product and then recommending based on the customers prior purchases and their most similar products

#### Flask Application 
  In order to show the working model, I created a flask application which combines a Flourish representation of the item-item network (note, it only includes the top ~ 400 products for ease of understanding) and the ability to select hypothetically purchased items and get the top 5 recommendations for whichever combination is selected along with their images. I am currently in the process of turning this into a web hosted application so please watch this space.
  


#### Next Steps:
* Combination of models and increased complexity. Take the SVD components that predict review score and use those to make recommendations. Or use it to exclude low review predicted products
* Strategy for how to track and incorporate (hypothetical) future sales and implementation of recommending to brand new customers

