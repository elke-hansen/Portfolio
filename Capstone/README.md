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
 Notebook containing the bare minimum pieces of code to get recommendations.


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
  
###### Basic Recommendation Engine

  This model is the very bare bones of a recommendation engine. It takes the list of products that a customer has purchased and compares it to all the other customers and what they have purchased to be able to make a suggestion based on collaborative filtering. The model uses a weighted jaccard score which takes the similar items beteen customers but also weights the items by how popular they are. This way you won't just get recommendations of the most popular items each time. It then takes the score between the key customer and finds the weights between that customer and all other customers, sorts from highest to lowest and returns the items those other customers have purchased that the key customer has not. 
  
  This model would work quite well for a large set of customers that have made several purchases each. However, my dataset has 80% of the customers only purchased once so most of the customers were only being recommended one item, or in some cases none, so I needed to find something a bit more extensive. This is actually a positive for the project as a whole as it gives me an opportunity to explore solutions to the cold start problem. That is, what do you recommend to new customers that you have little to no information about? 



 *From the initial base recommendation engine, I decided to take a look at some other types of models to see what else I could predict and whether or not these could be included in the model. After the initial EDA, I had noticed that the review score was affected by whether or not an order was delivered on time but not so much affected by the actual delivery time which leads me to believe that the review score is highly based on product given that the order is delivered on time. As such, I wanted to model these two features.* 

  
###### Review Score Prediction

 For this model I decided to start with regression models to see what kind of results I could get. The idea being that if I can predict review score in a linear fashion then I could rank products based on their score and be able to recommend in that manner. However, after trialling a few different methods including linear regression, random forest and lasso with cross validation, the best accuracy score these models could manage was 26.8% so I quickly decided to move on to classification models.
 
 For classification, I used both logistic regression and random forest and trialled some other models such as an SGD classifier but found the best model to be the random forest combined with a truncated SVD for feature reduction. This model had an f-1 score of 64% compared to the baseline of 57.3% and while this is not a huge lift, it has significantly improved the accuracy of predicting the extreme scored of 1 and 5. After plotting the number of SVD components against the explained variance ratio, I found the optimal components to be 115, which covers ~92% of the variance while significantly decreasing the features, preventing overfitting. It also gave me an extra point on my f-1 score increasing it to 65%.

###### On Time Status Target

For this model I had extremely unbalanced classes so the baseline model was already giving 92%. After trialling some different models, I found the best one to be the same model used for review score, a random forest with truncated SVD and the same amount of optimal components. This gives an average f-1 score of 96%. The main thing I like about this model is that it improved the 0 classification (an order is delivered late compared to provided expected delivery time) from 8% to 69% which is really good as we do not want to recommend items that are likely to be late.

###### Clustering: Customer Segmentation

Next I wanted to see if I could use an SVD to reduce the features in order to cluster the customers to recommend purchases based on customers like them. In this case, I was unable to find any well performing clusters so I quickly moved on.


###### Collaborative Filtering Using SVD and TFIDF

After trialling different models and seeing that SVD has worked well on this data, I decided to try and use this technique for a more in depth collaborative filtering model. By using a tfidf vectorizer to count all products purchased in a matrix against all customers, I was able to replicate the weights penalizing the popular products that I had used in my basic model. This time I have all the information in a matrix that can be plugged in to an SVD. After retrieving the most important 1000 components in this SVD, I can take a csoine similarity between any two customers to see how likely they are to order the same items. In this case,it's not as important to calculate the optimal number of components as I am essentially just using it in order to rank each customer in reference to another.


###### Intermediate Recommendation Engine

Now that I have the method by which to calculate the similarities between each customer, I need to go about the task of actually doing so. For each customer, to calculate all other customers and their cosine simlarity it takes around half an hour. This means that in order to get a starting database of all customers, it will take 127 days as I have 80,000+ unique customers in the database. In order to be able to only have to do this task once, the best method is to create a clustered dictionary with each customer as the key and then the value be a dictionary of the similarities with all other customers. This way when a new customer purchases something, it would only need that half hour calculation and a further calculation to insert into all other customers' dictionaries. Up until this point, this new customers recommendations will be made using other metrics such as most popular items for their demographic etc. It also means this calculation does not need to be done on the spot as recommendations need to be made instantaneously. This initial processing time and general data storage capacity is one of the biggest factors to take into consideration when creating a recommendation such as this.

Once you have this dictionary per customer of their similarities to other customers, the model is really simple. It sorts the dictionary to find the most similar customers and then recommends based on which items those similar customers have bought which the key customer has not yet purchased.



#### Next Steps:
* Combination of models and increased complexity. Take the SVD components that predict review score and use those to make recommendations. Or use it to exclude low review predicted products
* Strategy for how to track and incorporate (hypothetical) future sales and implementation of recommending to brand new customers
* Use of SQL queries to emulate a real business data structure
* Creation of a working flask application for predictions

