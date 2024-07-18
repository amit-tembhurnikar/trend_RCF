# 7-steps to model building for your selected ticker using Random Forest Classifier


#### 7 steps for the models are as follows;
| Steps| Workflow | Remarks|
|:---------|:-----------------|:-------------------|
|Step 1| Ideation | Predict positive up moves (Up trends) above 0.25% (transaction cost)|
|Step 2| Data collection | Download data from Yahoo Finance, store on local drive|
|Step 3| Exploration Data analysis | Study Summary statistics|
|Step 4| Cleaning Dataset | Check for any null values|
|Step 5| Transformation | Perform feature scaling based on EDA|
|Step 6| Modeling | Building and training Random Forest Classifier|
|Step 7| Metrics | Validating the model performance using the score method|


#### Steps in detail for the model.
1. Ideation - The Goal is to predict positive moves (up trends) above 0.05%, which is currently considered to transaction cost for trading for Nifty.

$$
y_t = \left\{ \begin{array}{ll}
         +1 & \mbox{if $p_{t+1} \geq 1.0005*p_t$};\\
         0 & \mbox{if $p_{t+1}Otherwise $}.\end{array} \right.
$$


2. Data collection - Use the input file
3. Exploration Data analysis - Describe the Nifty price.
4. Cleaning Dataset - Check for any null values
5. Feature Engineering and Feature selection
   
| Features| Formula | Description|
|:---------------|:-----------------:|:-------------------| 
|O-C, H-L| Open - Close, High - Low | intraday price range|
|Sign| $sign (rt = ln \frac{P_t}{P_{t-1}})$ | sign of return|
|Past Return| $r_{t-1}, r_{t-2}$ | lagged returns|
|Moving Average| $ SMA_i = \frac{1}{n}\Sigma^{n-1}_{i=0} P_{t-i} $ | simple moving average|
|Momentum| $P_{t} - P_{t-k}$ | price change over k period|

- Feature Selection
![image](https://github.com/user-attachments/assets/fd57f14f-0224-41dc-8c03-6d8767e14a90)

Remove highly correlated features


6. Building and training Random Forest Classifier
7. Validate the model performance using the score method

- Scores of Base model

Confusion matrix of Base Models
<img width="276" alt="Screenshot 2024-07-19 at 4 21 54 AM" src="https://github.com/user-attachments/assets/fbfe184b-a88d-4f6b-aba3-1f8aac9f133e">

The AUC-ROC curve shows a test accuracy of 50%.
<img width="346" alt="Screenshot 2024-07-19 at 4 31 13 AM" src="https://github.com/user-attachments/assets/228ee40f-73db-44b1-9509-d5f9ac52f729">


As the test accuracy is low, we will tune the hyperparameters using RandomizedSearchCV


Confusion matrix of tuned Model
 <img width="278" alt="Screenshot 2024-07-19 at 4 29 53 AM" src="https://github.com/user-attachments/assets/c8d8dae5-fe6b-4fff-aaa4-cdd96b8ccc25">

The AUC-ROC curve shows a test accuracy of 54%<img width="345" alt="Screenshot 2024-07-19 at 4 31 45 AM" src="https://github.com/user-attachments/assets/95d097e6-ccae-4e68-8f85-6fc068502351">


### Observations
1. Test accuracy improved by 4% as compared to the earlier model, also we can see the overfitting is decreased significantly.
2. Recall for Class +1 improved by more than 17% as compared to the earlier model while recall for Class 0 decreased by 12%.
3. Model improved predictor for upside, as we can see the increase in true positive when compared to the downside.
4. Class imbalance skews the prediction and needs to be addressed.


### Back-testing
We will use the predicted signal to buy or sell. We then compare the result of this strategy with the buy and hold and visualize the performance of the RandomForestClassifier Algorithm. We will test the return for last 3 year, starting 6th Nov'23.


<img width="738" alt="Screenshot 2024-07-19 at 4 33 56 AM" src="https://github.com/user-attachments/assets/9ba3e2f0-5324-43bf-bae2-3c583e209650">


#### Observations of back-test:
1. The CAGR return for the out-sample is 23.6%, on the higher side compared to the in-sample.
2. The absolute monthly return by this strategy is negative just for 1 month, while there is also a positive return for absolute annual returns.
3. The distribution of monthly returns from is strategy is left skewed.

