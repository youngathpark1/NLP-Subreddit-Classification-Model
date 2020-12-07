## Problem Statement

Can NLP machine learning enable companies to better connect with their customers?

One commonality that every successful companies share without exception is that they are all customer-centric. In a world that is constantly changing and shifting, creating a clear connection with your customer-base is a critical business strategy that every company should strive to achieve as part of their core competency. Capability to understand what your customers are saying about your product or service, what their needs are, and how to meet those needs, can significantly improve brand power and profitability.

As a small step forward to explore this question, I set out to build a NLP classification model prototype using two separate subreddit posts. For this prototype, I decided to focus on posts for two mega-stores r/Target and r/Walmart. By feeding, training, and testing various classification models, I set out to see how NLP machine learning can be used for various business applications, specifically to help companies evalute in real time the voice of their customers. While this prototype is simpy a classification model predicting between the two subreddits, the concept behind this model can be expanded into other areas of considersations such as pinpointing topic of trend, deeper sentiment analysis of what the customers are saying, and building out a real-time response algorithm that can respond to customers in seconds based on their feedback.

## Data Dictionary
|Data|Type|Source|Description|
|---|---|---|---|
|**Subreddit**|str|df|Origin of reddit posts (Target or Walmart)|
|**Selftext**|str|df|Text extracted from the post section|
|**Title**|str|df|Text extracted from the title section|
|**Subreddit Indicator**|int|df|1 Target, 0 Walmart|
|**Selftext_length**|int|df|Length of selftext|
|**Selftext_word_count**|int|df|Count of words in Selftext|

## Methodology
1. Data Extraction & Cleaning
- 10,000 posts in total were extracted through Reddit's api. 5,000 from r/Target and 5,000 from r/Walmart. During the cleaning process, I noticed that Selftext had a lot of null values. To account for this, I replaced null with text from the title section.

2. CountVectorizer and TfidVectorizer
- I transformed the texts using CountVectorizer and TfidVectorizer. While they are both text transformers, they apply different methodology in terms of how they weigh tokens so I decided to apply and measure their effects. I also decided to apply these transformers both with keywords included and keywords excluded. I also slightly modified stopwords list by adding keyterms "Target" and "Walmart" to see the impact on model performance.

3. Classification Models
- I decided to explore three different classification models to measure their performance. Logistic Regression w/ Bagging, Random Forest, and Voting Classifier comprised of GradientBoosting, AdaBoost, and XGBoost. For each model, I calculated metrics such as accuracy, recall, precision, f1, and roc scores. I also created confusion matrix and roc auc curve for each model to visually represent their performance.

## Conclusions & Recommendations

Generally speaking, across all combination of models and transformers, I was able to achieve accuracy score in the 70% range, with Random Forest/CountVectorizer (including stopwords) scoring accuracy score as high as 77% and roc auc score of 87%. Excluding stopwords, Logistic Regression with TfidVectorizer achieved the highest accuracy with 73% and roc auc score of 82%.

Modified stopwords (including words like 'Target' and 'Walmart') seemed to play critical role in improving model performance. When comparing measurements with and without modified stopwords, accuracy scores dropped by about 4-5 pts. In terms of transformers, there doesn't appear to be a particular preference in terms of performance. It appears they both perform differently based on the environment that they operate under.

While 77% accuracy score is not on the high end of the scale, given that posts from these two subreddits are similar in nature and hard to distinguish even with a human eye, I found the score to be fairly impressive. In addiiton, roc auc score of 87% was a promising indicator that additional intervention with tuning hyperparamters could potentially lead to better performance.

**Based on these observations, here are some recommendations that could potentially lead to better performance**

- Some of the classification models were complex in nature with many different hyperparameters that you can tune which may/may not improve model performance. While I was unable to tune many of the hyperparameters due to lack of computational power, it would be beneficial to leverage AWS or other remote server providers to run these complex models in order to fine tune over many of the hyperparameters for better result.

- For demonstration and learning purposes, I intentionally chose two subreddits that I thought were similar in nature. In this particular case, I trained the models based on 10,000 posts that I extracted from Reddit api. While I would've liked to consider more data, due to computational restriction, I was unable do so. With more computational power, I would definitely bring in more data in order to feed more information to these models.

- Another model that I attempted to was SVM. However, again, due to lack of computational power, I was unable to successfully fit and train the SVM model. With access to more powerful computer, one approach to consider would be to bag all the models into one Voting Classifier, similar to how I bagged Gradient, Ada, and XG Boost models, and run all the hyperparamters at the same time.

- There are many business applications that NLP machine learning can make an impact. Here are some potential ideas:
    1. Pinpoint topic of interests. What are you customers interested in?
    2. Deeper sentiment analysis. How are your customers feeling?
    3. Real-time response algorithm. Respond to your customers right away.