## Supplement Sales Prediction
- Overview
- Motivation
- Technical Aspect


## Overview
To predict/ forecast the sales of 2 months given sales of 1.5 years of a WOMart retail outlets.

## Motivation
I participated in a hackathon in which this question was asked. To be honest, I didn't know about time series at all that time.
But I took it as a challenge to get acquainted with the concept and give it a shot.
It was fun and good learning at the same time!

## Technical Aspect
- I used XGBoost regressor to train the model.
- For checking, I divided the training data into train and validation sets and evaluation metric as Mean Squared Log Error * 1000 (It was given to us).
- Training data was divided into train and validation sets in the following ratio: -
>
1. Train - All except last 2 months data
2. Validation -- last 2 months data (It is a good practice to maintain validation set at least as large as test data or forecasted results)
- I trained XGBoost model with following parameters:
n_estimators = 1000, objective='reg:linear', ,early_stopping_rounds=50,verbose=True and got MSLE score of 231.2.

## Data-preprocessing / feature engineering ideas
1. I grouped data by the Store_Type, Location_Type and Region_Code individually and found the mean of sales. The category with highest mean got the highest label-encoding value.
2. I did one hot encoding for Discount column and dropped the first column since that might lead to highly correlated independent columns.
3. Final Model
#### Thought-process
- It was a time-series problem. So I first thought of fitting time forecasting models like ARIMA, SARIMAX, VAR and alike. But they seem to be a poor choice since the problem statement was
predicting the sales for a particular store and store location, so individually for store we didn't have enough data.
- Next, I decided to take up Boosting approach "LightGBM" by intoducing lags, rotational, Exponentially Weighted Moving Averages columns in the dataset. Upon, submission the result didn't turn out to be that
good. I could notice some overfitting happening so I did PCA to reduce the dimensions of the dataset.
- After I performed that scores improved but only slightly. So I decided to change my approach.
- I tried taking tree-based approach, since the data was not very large and it is comparatively easy to do hyperparameter tuning on "Random-Forest". moreover, I could notice some outliers in the data which tree
based algorithms could have handled easily. I got a good improvement in my scores this time.
- My curiosity didn't stop there, I wanted to try XGBoost too. Let's give another shot to Boosting!!!
- When I trained that, doing some hyperparameter tuning(the best selection of parameters is in the notebook). I got the best score I got so far!!. 
- XGBoost worked for me this time as well! I trained XGBoost on the given data by doing Label encoding and one hot encoding for categorical columns.
- To, improve my score even more, I tried introducing lag(shift), rotation and other EWMA columns, introduced weekend element also but didn't get very good score
- So, I decided to stick to XGBoost with given variables only.

#### Insights
1. Holidays played a major role in sales. Sales were more on holidays.
2. Sales are more when there are discounts.
3. Sales are least on Monday. So, a strategy to increase sales could be to give more discounts and promotions on Monday. Since sales shoot up on weekends already so no need to give discounts on that day.
4. Since Monday is a working day people will get allured with the offer and try to find time to visit the store. Thereby increasing sales on Monday as well.
5. Give post dated discount coupons of the same store, so promote sales in future and ensure people visit again to buy products.
6. A market basket analysis can be done to put most frequently bought products together on the shelf to further increase the sales.
7. If possible, consider free home delivery options on the less frequented stores to promote sales. They might be situated at a non-residential place (people find it inconvinient to visit). 
8. The cost of free home delivery can leveraged from minimum order approach. If the order exceeds a certain amount, make the delivery free, this will increase the sales of products considerably.
9. Consider doing a customer survey on the most frequented stores to get the names of products they didn't get in the store. If some products that are high in demand but store doesn't sell them, consider selling them but assuring the disatisfied customer that it'll soon be in stock.
10. Try to convince some companies to do their food surveys in the store. Example a new coffee brand can promote their coffee by actually preparing coffee in the store and giving to the customers.
11. This will not only promote their sales and do publicity, it'll be doubly beneficial for the store: -
  - Customers getting to enjoy a cup of coffee while they shop (customers happy).
  - The coffee plant put inside the WOmart can be chargable on hourly basis to fetch more profit.
