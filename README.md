# Twitter Bot Classifier

**Authors:** Nabil Abbas & Mark Brennan

*“In line with our principles of transparency and to improve public understanding of alleged foreign influence campaigns, **Twitter is making publicly available archives of Tweets and media that we believe resulted from potentially state-backed information operations on our service.**”*   - Twitter (Stated approach on election integrity)

## Goals

Using a publicly available dataset provided by Twitter, create a Classification Machine Learning Model to classify twitter accounts as originating from Humans or Bots.  

The datset includes: *"all public, nondeleted Tweets and media (e.g., images and videos) from accounts Twitter believes are connected to state-backed information operations."*

Once the model has been created, identify key terminology that may differentiate between bot and humans.

## Tech Stack
- Python
- SpaCy
- NLTK
- Scikit-Learn
- Twint
- Matplotlib
- Pandas


## Strategy

The dataset provided 8,000,000 flagged / removed / suspended tweets, over several languages.  The dataset was sliced down to 3,000,000 tweets so that we would only be looking at English accounts, and English language tweets.  The dataset was then randomly sampled to ~ 120,000 tweets so that it would be a workable size.

These ~ 120,000 tweet would be identified as our positive "BOT" classification.

Using Twint library / Twitter Api we grabbed ~120,000 tweets using  terms/phrases: “equality, trigger, snowflake, swamp, border, politics, activists, liberal, corrupt, conservative, police officer“ from verified accounts.

These tweets were classified as the negative class i.e. "Human" classification.

Final dataset contains ~240,000 observations (tweets)

## Understanding Our Data

The bar graphs below will highlight the most occurring key words amongst both classes as well as show that our classes are balanced.

![](/plots/tweeted_word_counts.png)
![](/plots/class_count.png) 

## Our Model
Following a train test split of our sample data, we vectorized our tweets, using both Scikit-Learn’s Count Vectorizer and TF-IDF Vectorizer.  This yielded 19K+ features, so we subsequently reduced our dimensions using singular value decomposition (SVD) techniques through Scikit-Learn’s TruncatedSVD class, reducing our dimensions to 100.

We fit Scikit-Learn’s  multinomial naive bayes model with our TF-IDF vectorized data, but subsequently focused on modeling our reduced dimensions with Scikit-Learn’s DecisionTree and RandomForest estimators.
## Model Performance
A baseline “dummy” model yielded precision, recall, accuracy and F1 of .47-.50, which is to be expected given our evenly distributed classes.

The multinomial naive bayes model with our TF-IDF vectorized data yielded a precision score of .91, a recall score of .78, an accuracy score of .86, and an F1 score of .84.

But predictions using our RandomForest model fitted with our reduced feature dimensions yielded staggeringly good results, with a precision score of .96, a recall score of .93, an accuracy score of .94, and an F1 score of .94.

![](/plots/RF_Confusion.png)
![](/plots/ROC_RF.png)

## Feature Importance
Not surprinsgly, given the subjects/categories used for capturing verified (non-bot) tweets, and illustrated in the pictures below, certain “hot-button” words/topics are prominent in their importance in driving model performance, but other words, such as “man”, “woman”, “trump”, “new”, and “year”, are also important in their impact on the model’s latent semantic analysis.

![](/plots/svd_features.png)
![](/plots/tf_idf_feature_counts.png)


## Takeaways
The model performed exceptionally well with reduced dimension word vectors and shed light on words that helped drive its latent semantic analysis.
## Future Consideration
- Consider varying opinions of our approach to see if there are any overlooked factors that led to the success of our classifier.
- Tagging may yield additional insights.
- Explore options to include tweets from “non-verified” twitter users.
- Use GridSearchCV to optimize parameters.
- Test additional classification models.
- Kernel kept crashing when trying to visualize distribution of key words over the two classifications. Visualize on a more powerful machine.
- The key words selected to obtain the "Human" classification could be found through additional EDA of BOT tweets.

## Sources
Data Source: Twitter Election Integrity Data Set with 8,000,000 flagged, removed, and suspended tweets

https://about.twitter.com/en_us/values/elections-integrity.html#data
