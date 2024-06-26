# Hotel score prediction

## Project objective

Create a model for predicting the hotel review scores based on the reviews.

## Dateset

The train and test datasets [can be found in the Kaggle competition here](https://www.kaggle.com/competitions/sf-booking/data).

### Features

- `hotel_address` - hotel address;
- `review_date` — date when the reviewer posted the review;
- `average_score` — average hotel score based on the last review for the last year;
- `hotel_name` - hotel name;
- `reviewer_nationality` - nationality of the reviewer;
- `negative_review` — negative review a reviewer provided to the hotel;
- `review_total_negative_word_counts` — total words in the negative review;
- `positive_review` — negative review a reviewer provided to the hotel;
- `review_total_positive_word_counts` — total words in the positive review;
- `reviewer_score` — score a reviewer gave to the hotel based on their experience;
- `total_number_of_reviews_reviewer_has_given` — total reviews the reviewer wrote in the past;
- `total_number_of_reviews` — total reviews of the hotel;
- `tags` — tags which a reviewer attached to their review;
- `days_since_review` — number of days between the date of checking and the data of cleaning;
- `additional_number_of_scoring` — number of scores without review, since some guests only left the score but didn't provide any feedback;
- `lat` — hotel latitude;
- `lng` — hotel longitude.

## Data cleaning

- Adjusted feature typings
- Removed duplicates
- Filled missing values for `lat` and `lng` with mean values of other hotels in the same area
- Removed outliers based on log-normally distributed `additional_number_of_scoring` and `total_number` of reviews, using **the 3-sigma limits**

## Exploratory data analysis (EDA)

### Data visualization

Please, go to the ipynb notebook in the repo to see visualizations and their interpretations.

### Features engineering

**Engineered features:**

- `country`, `city`, `postcode` and residual `address` of the hotels for more granular address decomposition
- `is_negative` and `is_positive` to evaluate whether each reviewer provided both negative and positive reviews
- `total_scores`, identifying the overall score count, including both scores with and without written reviews
- `positive_negative_word_delta` to measure the difference between the number of words in the positive and negative reviews for the same reviewer
- `year`, `month`, `day_week`, `day_month`, `day_year`, `season` to get more granular on the timings of the reviews
- `same_reviewer_country` to check whether the nationality of the reviwer is the same with the country of the hotel to check for potential review bias
- `negative_sentiment_{}` and `positive_sentiment_{}`, measuring the overall sentiment of both negative and positive reviews. Using `SentimentAnalyzer()` of the nltk lib, calculated the positive, negative, neutral and compound scores for both negative and positive reviews
- `trip_type` - whether the trip was leisure, business or other
- `trip_group` - whether the trip was solo, as a couple, with friends, with familiar with young/older kids or with a pet
- `room` - whether the reviewer stayed in a room, suite or studio
- `nights` - number of nights the reviewer stayed for
- `mobile_submission` - whether the review was submitted from mobile
- `geo_cluster` to create geo clusters for the hotels based on their `lat` and `lng` which are not informative in themselves

### Feature normalization & standartization

- Normalized abnormally distributed numerical features using Robust Scaler
- Standartized normally distributed `positive_negative_word_delta`

### Feature encoding

- Encoded categorical features with less than 5 unique values using OneHot Encoding
- Encoded categorical features with >= 5 unique values using Binary Encoding

### Feature selection

- Using Spearman correlation due to the abnormal distribution of the features, identified features with multicollinearity (correlation score >= 0.7 or <= -0.7) and dropped the less informative of them
- Used F-scores to remove features with F-score below 5 to decrease the risk of overfitting

## Model creation and evaluation

- Split the train dataset into 2: 75% to train the model, 25% to validate
- Used `RandomForestRegressor()` from sklearn with 100 estimators to build the review score prediction model
- Used Mean Absolute Percentage Error (MAPE) to measure the model performance, achieving the MAPE of 0.1206

## Tech stack

### Language & version

- [Python 3.8.10](https://www.python.org/downloads/release/python-3810/)

### Data analysis

- [Pandas 2.0.3](https://pypi.org/project/pandas/)
- [Numpy 1.24.4](https://numpy.org/)

### Data visualization

- [Matplotlib 3.7.2](https://matplotlib.org/3.7.2/)
- [Seaborn 0.13.2](https://seaborn.pydata.org/installing.html)
- [YData Profiling 4.8.3](https://docs.profiling.ydata.ai/latest/)

### Feature engineering

- [NLTK 3.8.1](https://www.nltk.org/install.html)
- [category_encoders 2.6.3](https://pypi.org/project/category-encoders/)

### Feature selection & modelling

- [scikit-learn 1.3.2](https://scikit-learn.org/stable/whats_new/v1.3.html)
