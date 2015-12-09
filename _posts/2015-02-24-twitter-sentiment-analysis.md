---
layout: post
title: "Twitter Sentiment Analysis"
excerpt: "Post about sentiment analyis with special example of twitter."
tags: [machine learning, data science, text mining, NLP, Sentiment Analysis, Bayes Theorem]
comments: true
image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

Twitter represents a fundamentally new instrument to make social measurements. Millions of people voluntarily express opinions across any topic imaginable - this data source is incredibly valuable for both research and business. This means we can use the vast amount of data from Twitter to generate "public opinion" towards certain topics by aggregating the individual tweet results over time. 
<p>
Sentiment Analysis aims to determine how a certain person or group reacts to a specific topic. For Twitter, it works by extracting tweets containing references to the desired topic, computing the sentiment polarity and strength of each tweet and then aggregating the results for all such tweets.  
</p><p>
We can also track changes in the users opinion towards these topics over time, allowing us to identify the events that caused these changes. e.g the episode *The Rains of Castamere* in the TV series *Game of Thrones* had volcanic effect on the public sentiment. 
Also we can look at the geocoded information in the tweets and analyze the relation between location and mood. 
</p>
<br><br>
**Techniques:** There are broadly two categories of sentiment analysis: 
<br><br>
**Lexical Methods:**
<br> These techniques employ dictionaries of words annotated with their semantic polarity and sentiment strength. This is then used to calculate a score for the polarity and/or sentiment of the document. Usually this method gives high precision but low recall.
<br><br>
**Machine Learning Methods:**
<br> Such techniques require creating a model by training the classifier with labeled examples. This means that you must first gather a dataset with examples for positive, negative and neutral classes, extract the features from the examples and then train the algorithm based on the examples. These methods are used mainly for computing the polarity of the document. 
<br><br>
Choice of the method heavily depends on the application, domain and language. Using lexicon based techniques with large dictionaries enables us to achieve very good results. Nevertheless they require using a lexicon, something which is not always available in all languages. 
On the other hand Machine Learning based techniques deliver good results but they require obtaining training on labeled data.
<br><br>
Now I'll discuss an example of each of the above techniques. 
<br><br>
**AFINN Model:** 
<br>
In the [AFINN model](http://www2.imm.dtu.dk/pubdb/views/publication_details.php?id=6010), the authors have computed sentiment scores for a list of words. The sentiment of a tweet is computed based on the sentiment scores of the terms in the tweet. The sentiment of the tweet is defined to be equal to the sum of the sentiment scores for each term in the tweet. The AFINN-111 dictionary contains 2477 English words rated for valence with an integer value between -5 and 5. The words have been manually labelled by Finn Arup Neilsen in 2009-2010. Some of the words are the grammatically different versions of the same stem e.g. "favorite" and "favorites" are listed as two different words with different valence scores. 
<br>
An implementation of the AFINN model can be [found here](https://github.com/Jverma/Twitter-Sentiment-Analysis).
<br><br>
**Naive Bayes Classifier:**
<br>
The [Naive Bayes classifier](http://en.wikipedia.org/wiki/Naive_Bayes_classifier)can be trained on a corpus of labeled (+ve, -ve, neutral) tweets and then employed to assign polarity to a new tweet. The features used in this model are the words or bi-grams with their frequencies in the tweet strings. You may want to keep or remove URLs, emoticons and short tokens depending on the application. 
<br><br>
If \\( f = (f\_{1}, f\_{2}, \ldots, f\_{n}) \\) be the vector of features of a tweet ( T ), then the probability of the tweet to be +ve according to Bayes Rule is:  
$$ p(+ve|f ) = \( p(f|+ve) * p(+ve) \) / p(f) $$
<br>
Now basic theory of probability tells us that: 
<br>
$$
p(f|+ve) =  p(f\_{1}, f\_{2}, \ldots, f\_{n} | +ve)
$$
<br>
and 
<br>
$$ p(f) = p(f\_{1}, f\_{2}, \ldots, f\_{n}) $$
<br>
The Naive Bayes asserts that all the features are independent and thus: 
<br>
$$ p(f) = \prod\_{i} p(f\_{i}) $$
<br>
$$ p(f|+ve) = \prod\_{i} p(f\_{i}|+ve) $$
<br>
And similarly for -ve and neutral tweets. 
<br><br>
Using the pre-estimated values of these probabilities, one can compute the probability of a tweet to be positive, negative and neutral. 
Whenever a new tweet is fed to the classifier, it will predict the polarity of the tweet based on the probability of its having that polarity. 
<br>
An implementation of Naive Bayes classifier for classifying spam and non-spam messages can be found [here](https://github.com/Jverma/naive-bayes). 
<br><br>
Several other methods in both the categories are prevalent today. Lots of companies using sentiment analysis employ lexical methods where they create dictionaries based on their trade algorithms and the domain of the application. 
<br><br>
For machine learning based analysis, instead of Naive Bayes, one can use more sophisticated algorithms like SVMs. 
<br><br>
**Challenges:**
<br>
Sentiment analysis is a very useful, but there are many challenges that need to be overcome to achieve good results. The very first step in opinion mining, something which I swept under the rug so far, is that we have to identify tweets that are relevant to our topic. Tweets containing the given word can be a decent choice, although not perfect. Once we have identified tweets to be analyzed, we need to sure that the tweets DO contain sentiment. Neutral tweets can be a part of our model, but only polarized tweets tell us something subjective. Even though the tweets are polarized, we still need to make sure that the sentiment in the tweet is related to the topic we are studying. For example, suppose we are studying sentiment related to a movie Mission Impossible, then the tweet: “Tom Cruise in Mission Impossible is pathetic!”.
<br>
Now this tweet has a negative sentiment, but is directed at the actor rather than the movie. This is not a great example, as the sentiment of the actor and movie is related.
<br>
The main challenge in Sentiment analysis using lexical methods is to build a dictionary that contains words/phrases and their sentiment scores. It is very hard to do so in full generality, and often the best idea is to choose a subject and build a list for that. Thus sentiment analysis is highly domain centric, so the techniques developed for stocks may not work for movies.
<br>
To solve these problems, you need expertise in NLP and computational linguistics. They correspond to entity extraction, NER, and entity pattern extraction in NLP terminology.
<br><br>
**Beyond Twitter:**
<br>
Facebook performed an experiment to measure the effect of removing positive (or negative) posts from the people's news feeds on how positive (or negative) their own posts were in the days after these changes were made. They found that the people from whose news feeds negative posts were removed produced a larger percentage of positive words as well as a smaller percentage of negative words in their posts. The group of people from whose news feeds negative posts were removed showed similar tendencies. The procedure and results of this experiment were a paper in the Proceedings of the National Academy of Sciences. Though, I don’t subscribe to the idea of using users are subjects to a physiological experiment without their knowledge, this is a cool application of sentiment analysis subject area.
<br><br>

**Resources:**
-  Liu, Bing. "Sentiment analysis and subjectivity." Handbook of natural language processing 2 (2010): 627-666.

- Liu, Bing, and Lei Zhang. "A survey of opinion mining and sentiment analysis."Mining Text Data. Springer US, 2012. 415-463.

-  Liu, Bing. "Sentiment analysis and opinion mining." Synthesis Lectures on Human Language Technologies 5.1 (2012): 1-167.

-  [Opinion mining and sentiment analysis](http://www.cs.cornell.edu/home/llee/opinion-mining-sentiment-analysis-survey.html)

5. [Twitter sentiment analysis using Python and NLTK](http://www.laurentluce.com/posts/twitter-sentiment-analysis-using-python-and-nltk/)