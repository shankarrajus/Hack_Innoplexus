
# Innoplexus Hackathon 

My approach and codes for this hackathon (Leaderboard score: Within top 20%)

## Problem Statement

A publication firm has presented you with a problem of identifying the articles that another article will cite as a reference.

For a particular article, the client has provided information such as its abstract, author name, title of the article etc. More details on the information can be found below. Also, it is worth noting that, research articles in both train and test are divided on the basis of set. A research article belonging to say set 1 can only cite articles from set 1 and not any other set.


Training data set has 9 sets in total, whereas Test data set has 9 sets. The Test has been further divided into public and private data set each containing 5 and 4 sets respectively.


Note: All sets irrespective of whether in train, public or private are independent of each other and share no article ids(pmid) in common.

## Data dictionary

* information_train.csv, information_test.csv - tab separated files

Variable | Meaning
---------- | -----------
abstract | Text containing the abstract of the article
article_title| Title of the article
author_str|Name of the Authors of the article
pmid|Id for the article
pub_date|Publication date of the article/manuscript
set|The set to which the article belongs
full_Text|The complete text of the research article

* Train.csv - comma separated file

Variable | Meaning
---------- | -----------
pmid|Id for the article
ref_list|pmid of the articles that this article has cited

* Test.csv - comma separated file

Variable | Meaning
---------- | -----------
pmid|Id for the article

## Evaluation Metric

The Evaluation Metric for this competition is f1 weighted by samples. An example of the same is provided below.

Actuals: Both rows belong to same set.

Row1: [1,3]
Row2: [1,4]

Predicted:
Row1 : [1,4]
Row2: [2,3]
 
F1_score for Row1 = 0.5
F1_score for Row2 = 0
Average F1_score = (0.5+0)/2 = 0.25

Final F1_score is calculated by taking average of Average F1_score across sets.

## Submission Format

Entries in the ref_list column should be only a List of pmids(integer or string)

eg - [1234,657389]

# My approach


## Observation on dataset
* At first, it looked like it was a small training dataset 3522 entries
* Totally 18 sets(read as category) were given. Training and test datasets "sets" are mutually exclusive.
* It has title, abstract and text fields .. so NLP is needed. 

## Data transformations

* Noticed there are very little features available. So spent most of the time in finding tf-idf between each document. 

* Then based on the tf-idf calculated the pairwise similarity between each documents. This provided good public score initially. But after that I could not make much improvement. 

* After struggling much decided to go with different approach. That is to de-normalize the dataset such that permutation of all documents and whether it is referenced or not. By doing that training dataset expanded to ~700k and provided lot of options for feature engineering ... then the fun started .



## Feature engineering

* Calculated number of authors in each document and the document it is referred
* Similarly found the authors which are common between them. Same author for both document and it's referred one.
* If more than one author is present, then extract maximum number of documents by a single author
* Publication date differences are calculated. There was a confusion as OLD document referred NEW document! But in the discussion board some explanation was given. So, did not spent much time on it.
* One key point is all the documents are referred only within same set(category). So, all the feature engineering was made only within same set

## Model selection

* I tried with simple classifier algorithms and came to conclusion that AdaBoostClassifier is better
* Also the probability for selecting the referred document is based on the custom F1-score and trail/error on leaderboard score!

## What has worked

* De-normalizing the data definitely helped
* Text similarity between documents. PorterStemm and stopwords improved the similarity score
* Common authors between documents

## What did not worked

* Spent so much time on Doc2Vec exploration but it did not helped
* NLP lemma did not helped
* Calculating set level reference range did not helped

## I wanted to try but could not

* Try different boosting algorithms and parameter tuning ... definitely missed it
* Algorithm ensembling
* Additional feature engineering like min,mean number of documents by authors, etc
* Refining custom f1-score to sync with public leader board in order to improve the probability threshold


```python

```
