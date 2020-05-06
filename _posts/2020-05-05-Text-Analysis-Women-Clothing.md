
---
layout: post
title:  "Text Analysis Women Clothing"
date:   2020-05-05 12:07:25 +0000
categories:
  - data
---
# Text Analysis

In this post, we will discuss how one can make sentiment analysis. Such analysis is crucial to a variety of stakeholders. It is of no surprise by now to see comments and reviews at the end of posts, online purchases, feedback for apps and/or websites. Therefore it is insightful for bloggers, online markets, app-devlopers etc to have the abilty to analyse the feedback/s they are getting from respective customers and/or users. Sentiment analysis employes techniques to interpret and classify text data as **positive**, **negative**, and **neutral** based on polarity.  This ability to classify text data based on emotions reflected by words helps online bussiness providers improve their services and/or products. 

The analysis is based on dividing paragraph, sentences into smaller units, words and based on its library assigns emotions to these unit words. Python has such library and we are going to make use of it in this post. One such library is the Natural Language Toolkit Library (NLTK). The library looks at each words and sentences in a text and cleans it unneccessary features from the text data to prepare for sentiment analysis. This. cleaning processes for example includes the exclusion of punctuation and english stop words. Without this step the text would be dominated by words like _the_, _is_, _are_, etc. 

It is worth noting here that breaking paragraph or sentence into individualized words can be achieved by python's builtin function _.split()_. We demonstrate below how to acheive it 


```python
sentence = "The quick brown fox jumps over the lazy dog!"
```

We now call the `_.split()_` function on sentence and break down it into individualized words. In doing so we get


```python
sentence.split()
```




    ['The', 'quick', 'brown', 'fox', 'jumps', 'over', 'the', 'lazy', 'dog!']



We also can apply the same function on a bigger text data, say the following paragraph


```python
paragraphs = "Here we go this is my first post. Please note that I followed Jonathan McGlone steps \
to build this page. The steps provided by Jonathan McGlone, are very clearly outlined, and I say, \
in case you want to build your own website, I highly recommend implementing them faithfully. \
I have been thinking to own a page for a while, now because of COVID-19 at  last I got the courage and \
the time to putting this website together. This is hosted by Github pages, and the interesting feature  \
of it is that it is powered by Jekyll and I can use Markdown to author my posts. As Jonathan says \
It actually is a lot easier than  I thought it was going to be. I expected a lot of hurdle to build the web,\
but the steps outlined -- though I didn not completely understand what it is -- seems to be effective \
in helping me build my site."

```


```python
#paragraphs.split()
```

Again when we implement the function it splits 'paragraph' into its indivisualised units. In this particular example we have long list of words, 156 to be specific. You can use the `len()` function to determine the length ```python len(paragraphs.split())```. To help us visualize what we do next can be thought of as two steps. First we pass the result obtained by `paragraphs.split()` into a `Counter` that counts each _key_ of the dictionary and returns the number of count as value. Following which we form a DataFrame, using pandas DataFrame, and put the key items of the dictionary along with thier corresponding count in. At last comes displaying the result, as we can see we have listed the top 20 words. It is interesting to see the top five words are english stop words. In fact with the exception of *build*, *Jonathan*, *steps*, and *lot* the top twenty words seem to be dominated by english stop words including punctuation, i.e '--'. 


```python
from pandas import DataFrame
from collections import Counter
df_dict = Counter(paragraphs.split())
#df_dict
```


```python
df_counter = DataFrame(list(df_dict.items()),columns = ['words','count']) 
df_counter.sort_values(by=['count'], ascending=False).reset_index().head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>words</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>11</td>
      <td>I</td>
      <td>9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16</td>
      <td>to</td>
      <td>8</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>is</td>
      <td>6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>55</td>
      <td>the</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>27</td>
      <td>and</td>
      <td>4</td>
    </tr>
    <tr>
      <th>5</th>
      <td>44</td>
      <td>a</td>
      <td>4</td>
    </tr>
    <tr>
      <th>6</th>
      <td>17</td>
      <td>build</td>
      <td>4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>67</td>
      <td>it</td>
      <td>4</td>
    </tr>
    <tr>
      <th>8</th>
      <td>50</td>
      <td>of</td>
      <td>3</td>
    </tr>
    <tr>
      <th>9</th>
      <td>3</td>
      <td>this</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>5</td>
      <td>my</td>
      <td>3</td>
    </tr>
    <tr>
      <th>11</th>
      <td>21</td>
      <td>by</td>
      <td>3</td>
    </tr>
    <tr>
      <th>12</th>
      <td>13</td>
      <td>Jonathan</td>
      <td>3</td>
    </tr>
    <tr>
      <th>13</th>
      <td>15</td>
      <td>steps</td>
      <td>3</td>
    </tr>
    <tr>
      <th>14</th>
      <td>34</td>
      <td>own</td>
      <td>2</td>
    </tr>
    <tr>
      <th>15</th>
      <td>90</td>
      <td>--</td>
      <td>2</td>
    </tr>
    <tr>
      <th>16</th>
      <td>29</td>
      <td>in</td>
      <td>2</td>
    </tr>
    <tr>
      <th>17</th>
      <td>79</td>
      <td>lot</td>
      <td>2</td>
    </tr>
    <tr>
      <th>18</th>
      <td>10</td>
      <td>that</td>
      <td>2</td>
    </tr>
    <tr>
      <th>19</th>
      <td>63</td>
      <td>Github</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




#### Tokenizing Words and Sentences

Above we used the `.split()` function to break a sentence and a paragraph apart. But python has already a neat way of spliting sentences and paragraphs into units by tokenizing them. Tokenization is the a technique by which big chunck of text is divided into its smaller units known as _tokens_. Doing the splitting allows us to exclude punctuations and stopwords. Ones we chopped the text data, and extracted the relevant pieces what follows is implementing sentiment analysis on the words. 

Doing Sentiment analysis on text data that contains opinion help us understand how people feel about something. The VADER (Valence Aware Dictionary and sEntiment Reasoner) Sentiment Intensity Analyzer returns a score between -1 to 1. Accordingly, scores closer to -1 have a negative sentiment, scores closer to 1 have a positive sentiment, and scores around 0 are considered neutral.

First thing to do before prceeding onwards is import the necessary library and modules as follows:


```python
import nltk
from nltk.tokenize import word_tokenize
from nltk.tokenize import sent_tokenize
from nltk.tokenize import TweetTokenizer
from nltk.probability import FreqDist
from nltk.corpus import stopwords
from nltk.stem import WordNetLemmatizer
from nltk.sentiment.vader import SentimentIntensityAnalyzer 

#this is sample data
from nltk.corpus import names  

from string import punctuation

#initilize function to do sentiment analysis
sid = SentimentIntensityAnalyzer()
```

Just in case we are interested at looking the english stop words in the `ntlk.corpus` library we use the `stopwords.words('english')`, this yields an array of words that the library considers to be stopwords. 


```python
#list of english stopwords 
eng_stopwords = stopwords.words('english')
eng_stopwords
```




    ['i',
     'me',
     'my',
     'myself',
     'we',
     'our',
     'ours',
     'ourselves',
     'you',
     "you're",
     "you've",
     "you'll",
     "you'd",
     'your',
     'yours',
     'yourself',
     'yourselves',
     'he',
     'him',
     'his',
     'himself',
     'she',
     "she's",
     'her',
     'hers',
     'herself',
     'it',
     "it's",
     'its',
     'itself',
     'they',
     'them',
     'their',
     'theirs',
     'themselves',
     'what',
     'which',
     'who',
     'whom',
     'this',
     'that',
     "that'll",
     'these',
     'those',
     'am',
     'is',
     'are',
     'was',
     'were',
     'be',
     'been',
     'being',
     'have',
     'has',
     'had',
     'having',
     'do',
     'does',
     'did',
     'doing',
     'a',
     'an',
     'the',
     'and',
     'but',
     'if',
     'or',
     'because',
     'as',
     'until',
     'while',
     'of',
     'at',
     'by',
     'for',
     'with',
     'about',
     'against',
     'between',
     'into',
     'through',
     'during',
     'before',
     'after',
     'above',
     'below',
     'to',
     'from',
     'up',
     'down',
     'in',
     'out',
     'on',
     'off',
     'over',
     'under',
     'again',
     'further',
     'then',
     'once',
     'here',
     'there',
     'when',
     'where',
     'why',
     'how',
     'all',
     'any',
     'both',
     'each',
     'few',
     'more',
     'most',
     'other',
     'some',
     'such',
     'no',
     'nor',
     'not',
     'only',
     'own',
     'same',
     'so',
     'than',
     'too',
     'very',
     's',
     't',
     'can',
     'will',
     'just',
     'don',
     "don't",
     'should',
     "should've",
     'now',
     'd',
     'll',
     'm',
     'o',
     're',
     've',
     'y',
     'ain',
     'aren',
     "aren't",
     'couldn',
     "couldn't",
     'didn',
     "didn't",
     'doesn',
     "doesn't",
     'hadn',
     "hadn't",
     'hasn',
     "hasn't",
     'haven',
     "haven't",
     'isn',
     "isn't",
     'ma',
     'mightn',
     "mightn't",
     'mustn',
     "mustn't",
     'needn',
     "needn't",
     'shan',
     "shan't",
     'shouldn',
     "shouldn't",
     'wasn',
     "wasn't",
     'weren',
     "weren't",
     'won',
     "won't",
     'wouldn',
     "wouldn't"]



## Women's Clothing

Let us now chose a dataset to apply the analysis. We retrive a dataset, that is in a csv format, which you can download from kaggle [women's e-commerce clothing reviews](https://www.kaggle.com/nicapotato/womens-ecommerce-clothing-reviews). The context of the dataset is explained in kaggle as:

>This is a Women’s Clothing E-Commerce dataset revolving around the reviews written by customers. Its nine supportive features offer a great environment to parse out the text through its multiple dimensions. Because this is real commercial data, it has been anonymized, and references to the company in the review text and body have been replaced with “retailer”.

The target is to analyse the reviews, we achieve this by creating a function that will analyze the "Review Text" column and calculate the corresponding sentiment value. We create a new column in the dataframe in which we will put the sentiment value for each review.

First thing first, let us read our file and make sure it is clean and in a desired format. I already downloaded the csv file entitled `women_clothing_review.csv` into my dataset folder. To open the file and read it we need to import `pandas` library. 


```python
import pandas as pd

#load the data from the Reviews.csv file
filepath = "datasets/women_clothing_review.csv"
df = pd.read_csv(filepath, encoding = "latin-1") #this file is encoded differently

df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>Clothing ID</th>
      <th>Age</th>
      <th>Title</th>
      <th>Review Text</th>
      <th>Rating</th>
      <th>Recommended IND</th>
      <th>Positive Feedback Count</th>
      <th>Division Name</th>
      <th>Department Name</th>
      <th>Class Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>767</td>
      <td>33</td>
      <td>NaN</td>
      <td>Absolutely wonderful - silky and sexy and comf...</td>
      <td>4</td>
      <td>1</td>
      <td>0</td>
      <td>Initmates</td>
      <td>Intimate</td>
      <td>Intimates</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1080</td>
      <td>34</td>
      <td>NaN</td>
      <td>Love this dress!  it's sooo pretty.  i happene...</td>
      <td>5</td>
      <td>1</td>
      <td>4</td>
      <td>General</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1077</td>
      <td>60</td>
      <td>Some major design flaws</td>
      <td>I had such high hopes for this dress and reall...</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>General</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>1049</td>
      <td>50</td>
      <td>My favorite buy!</td>
      <td>I love, love, love this jumpsuit. it's fun, fl...</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>General Petite</td>
      <td>Bottoms</td>
      <td>Pants</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>847</td>
      <td>47</td>
      <td>Flattering shirt</td>
      <td>This shirt is very flattering to all due to th...</td>
      <td>5</td>
      <td>1</td>
      <td>6</td>
      <td>General</td>
      <td>Tops</td>
      <td>Blouses</td>
    </tr>
  </tbody>
</table>
</div>



We point out here that as the file is encoded some how differently we enforce `encoding = "latin-1"` to help us open and read it properly. The next thing we should do is make sure the data frame doesn't have a sizeable amount of `NaN` or `null` values. Specially the column we are interested, i.e __Review Text__, in must be checked before proceeding to make analysis.  


```python
df.isnull().mean()*100
```




    Unnamed: 0                  0.000000
    Clothing ID                 0.000000
    Age                         0.000000
    Title                      16.222430
    Review Text                 3.597888
    Rating                      0.000000
    Recommended IND             0.000000
    Positive Feedback Count     0.000000
    Division Name               0.059610
    Department Name             0.059610
    Class Name                  0.059610
    dtype: float64



As you can see about 3.6% of the entries in column that is of interest to us is missing, as there is no way of replacing these missing values even with less accuracy, we decide to drop these missing values. This is done by the following line of code.


```python
df.dropna(inplace=True)
df.reset_index(drop=True)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>Clothing ID</th>
      <th>Age</th>
      <th>Title</th>
      <th>Review Text</th>
      <th>Rating</th>
      <th>Recommended IND</th>
      <th>Positive Feedback Count</th>
      <th>Division Name</th>
      <th>Department Name</th>
      <th>Class Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2</td>
      <td>1077</td>
      <td>60</td>
      <td>Some major design flaws</td>
      <td>I had such high hopes for this dress and reall...</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>General</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>1049</td>
      <td>50</td>
      <td>My favorite buy!</td>
      <td>I love, love, love this jumpsuit. it's fun, fl...</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>General Petite</td>
      <td>Bottoms</td>
      <td>Pants</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>847</td>
      <td>47</td>
      <td>Flattering shirt</td>
      <td>This shirt is very flattering to all due to th...</td>
      <td>5</td>
      <td>1</td>
      <td>6</td>
      <td>General</td>
      <td>Tops</td>
      <td>Blouses</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>1080</td>
      <td>49</td>
      <td>Not for the very petite</td>
      <td>I love tracy reese dresses, but this one is no...</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>General</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6</td>
      <td>858</td>
      <td>39</td>
      <td>Cagrcoal shimmer fun</td>
      <td>I aded this in my basket at hte last mintue to...</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>General Petite</td>
      <td>Tops</td>
      <td>Knits</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>19657</th>
      <td>23481</td>
      <td>1104</td>
      <td>34</td>
      <td>Great dress for many occasions</td>
      <td>I was very happy to snag this dress at such a ...</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>General Petite</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
    <tr>
      <th>19658</th>
      <td>23482</td>
      <td>862</td>
      <td>48</td>
      <td>Wish it was made of cotton</td>
      <td>It reminds me of maternity clothes. soft, stre...</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>General Petite</td>
      <td>Tops</td>
      <td>Knits</td>
    </tr>
    <tr>
      <th>19659</th>
      <td>23483</td>
      <td>1104</td>
      <td>31</td>
      <td>Cute, but see through</td>
      <td>This fit well, but the top was very see throug...</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>General Petite</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
    <tr>
      <th>19660</th>
      <td>23484</td>
      <td>1084</td>
      <td>28</td>
      <td>Very cute dress, perfect for summer parties an...</td>
      <td>I bought this dress for a wedding i have this ...</td>
      <td>3</td>
      <td>1</td>
      <td>2</td>
      <td>General</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
    <tr>
      <th>19661</th>
      <td>23485</td>
      <td>1104</td>
      <td>52</td>
      <td>Please make more like this one!</td>
      <td>This dress in a lovely platinum is feminine an...</td>
      <td>5</td>
      <td>1</td>
      <td>22</td>
      <td>General Petite</td>
      <td>Dresses</td>
      <td>Dresses</td>
    </tr>
  </tbody>
</table>
<p>19662 rows × 11 columns</p>
</div>




```python
df.isnull().sum()
```




    Unnamed: 0                 0
    Clothing ID                0
    Age                        0
    Title                      0
    Review Text                0
    Rating                     0
    Recommended IND            0
    Positive Feedback Count    0
    Division Name              0
    Department Name            0
    Class Name                 0
    dtype: int64



Now that our data is clean and ready for analysis, let us build a function that cleans the reviews, that are in our **Review Text** column. To begin with we make every word be in lower case, following which we break the reviews into individualized form. After breaking the text into bits of words, we then remove punctuations and english stopwords from the tokenized format. Next is to collect the remaining tokenized words into a list for further use. Once we gather these individualized words, what follows is to form back the reviews in a sentences or paragraph form (without the punctuations and stopwords that is). At last we calculate the sentiment polarity  and return the sentiment value for each review. What follows is a function that can implement all these processes outlined.  


```python
#create a function to clean up each review
#then it will analyze and assign a sentiment polarity
def reviewSentiment(review):
    
    #make text lowercase
    review = review.lower()
    
    #tokenize the review
    #tknz_review is alist
    tknz_review = word_tokenize(review)
    
    #remove puntuation
    for token in tknz_review:
        if token in punctuation:
            tknz_review.remove(token)
    
    #empty list to hold "cleaned" tokens
    clean_tokens = []
    
    #remove filler words
    for token in tknz_review:
        if token not in eng_stopwords:
            clean_tokens.append(token)
            
    #put sentence back together with remaining clean words
    clean_review = ' '.join(clean_tokens)
    
    #get the polarity scores dictionary
    sid_rev = sid.polarity_scores(clean_review)
    
    #get sentiment polarity from the "compound" key in the sid_rev dictionary
    r_comp = sid_rev['compound']
    
    #return the sentiment value
    return r_comp
```

As our interest is to apply the above function for each review and obtaing the corresponding sentiment value, it requires that we create a column to put the sentimnet values for each review. Let us name the new column _review_sentiment_. We see below the newly formed column in the dataframe.


```python
#create a new column to hold sentiment value from function
df['review_sentiment'] = df['Review Text'].apply(reviewSentiment)

```


```python
#erify sentiment values in new column
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>Clothing ID</th>
      <th>Age</th>
      <th>Title</th>
      <th>Review Text</th>
      <th>Rating</th>
      <th>Recommended IND</th>
      <th>Positive Feedback Count</th>
      <th>Division Name</th>
      <th>Department Name</th>
      <th>Class Name</th>
      <th>review_sentiment</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>1077</td>
      <td>60</td>
      <td>Some major design flaws</td>
      <td>I had such high hopes for this dress and reall...</td>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>General</td>
      <td>Dresses</td>
      <td>Dresses</td>
      <td>0.9062</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>1049</td>
      <td>50</td>
      <td>My favorite buy!</td>
      <td>I love, love, love this jumpsuit. it's fun, fl...</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>General Petite</td>
      <td>Bottoms</td>
      <td>Pants</td>
      <td>0.9464</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>847</td>
      <td>47</td>
      <td>Flattering shirt</td>
      <td>This shirt is very flattering to all due to th...</td>
      <td>5</td>
      <td>1</td>
      <td>6</td>
      <td>General</td>
      <td>Tops</td>
      <td>Blouses</td>
      <td>0.9117</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>1080</td>
      <td>49</td>
      <td>Not for the very petite</td>
      <td>I love tracy reese dresses, but this one is no...</td>
      <td>2</td>
      <td>0</td>
      <td>4</td>
      <td>General</td>
      <td>Dresses</td>
      <td>Dresses</td>
      <td>0.9153</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>858</td>
      <td>39</td>
      <td>Cagrcoal shimmer fun</td>
      <td>I aded this in my basket at hte last mintue to...</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>General Petite</td>
      <td>Tops</td>
      <td>Knits</td>
      <td>0.6361</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.dtypes
```




    Unnamed: 0                   int64
    Clothing ID                  int64
    Age                          int64
    Title                       object
    Review Text                 object
    Rating                       int64
    Recommended IND              int64
    Positive Feedback Count      int64
    Division Name               object
    Department Name             object
    Class Name                  object
    review_sentiment           float64
    dtype: object



Although getting sentiment value is a step in the right direction, that in itself is not that much infromative. To paint a clear picture we need to chnage the category into numerics using the following function. The function help us chnage the numbers into positive, negative, and neutral categories. We indicate the categories by creating another column to indicate the category. 


```python
#create a function to assign a polarity category to the sentiment
def sentimentCategory(sent_num):
    if sent_num >= 0.2:
        return "positive"
    if sent_num <= -0.2:
        return "negative"
    else:
        return "neutral"
```


```python
#create a new column to hold sentiment category
df['sentiment_category'] = df['review_sentiment'].apply(sentimentCategory)
```

For the purpose of this post our interest lies in analysing the reviews, and the relevant columns as far as we are concerned can then be extracted to establish a new DataFrame that consists of these relevant columns. The following code line takes care of that. It is worth noting here that we need to reset index as we will use index to show case the reviews in each category.


```python
df = df[['Review Text','review_sentiment','sentiment_category']]
df = df.reset_index(drop=True)
```


```python
df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Review Text</th>
      <th>review_sentiment</th>
      <th>sentiment_category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>I had such high hopes for this dress and reall...</td>
      <td>0.9062</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>1</th>
      <td>I love, love, love this jumpsuit. it's fun, fl...</td>
      <td>0.9464</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>2</th>
      <td>This shirt is very flattering to all due to th...</td>
      <td>0.9117</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>3</th>
      <td>I love tracy reese dresses, but this one is no...</td>
      <td>0.9153</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>4</th>
      <td>I aded this in my basket at hte last mintue to...</td>
      <td>0.6361</td>
      <td>positive</td>
    </tr>
  </tbody>
</table>
</div>



Looking at the above table, one might wonder if every review is categorized as positive, to check that doubt we now categorize the dataFrame based on category and request to get value counts for each category. In doing so we note that, see below, majority of the reviews are categorized as postive.   


```python
#compare frequency of positive, negative, and neutral reviews
df['sentiment_category'].value_counts()
```




    positive    18596
    neutral       614
    negative      452
    Name: sentiment_category, dtype: int64



Let us peek in at each category, within our data, by extracting entries that fullfills the condition that we are looking for. By this what we mean is that let us collect entries from df, that satisfies the condition 
```python
df['sentiment_category']== 'positive' | 'negative' | 'neutral'
```
The results respectively are seen in the followng three consecutive tables.


```python
df[df['sentiment_category']== 'positive'].head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Review Text</th>
      <th>review_sentiment</th>
      <th>sentiment_category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>I had such high hopes for this dress and reall...</td>
      <td>0.9062</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>1</th>
      <td>I love, love, love this jumpsuit. it's fun, fl...</td>
      <td>0.9464</td>
      <td>positive</td>
    </tr>
    <tr>
      <th>2</th>
      <td>This shirt is very flattering to all due to th...</td>
      <td>0.9117</td>
      <td>positive</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df['sentiment_category']== 'negative'].head(3)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Review Text</th>
      <th>review_sentiment</th>
      <th>sentiment_category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>60</th>
      <td>The zipper broke on this piece the first time ...</td>
      <td>-0.2263</td>
      <td>negative</td>
    </tr>
    <tr>
      <th>79</th>
      <td>The fabric felt cheap and i didn't find it to ...</td>
      <td>-0.3724</td>
      <td>negative</td>
    </tr>
    <tr>
      <th>84</th>
      <td>This is so thin and poor quality. especially f...</td>
      <td>-0.3892</td>
      <td>negative</td>
    </tr>
  </tbody>
</table>
</div>




```python
df[df['sentiment_category']== 'neutral'].head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Review Text</th>
      <th>review_sentiment</th>
      <th>sentiment_category</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>19</th>
      <td>First of all, this is not pullover styling. th...</td>
      <td>0.1027</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>76</th>
      <td>At first i wasn't sure about it. the neckline ...</td>
      <td>0.1106</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>159</th>
      <td>Nice weight sweater that allows one to wear le...</td>
      <td>0.1999</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>160</th>
      <td>I loved this top; it reminded me of one i have...</td>
      <td>0.0258</td>
      <td>neutral</td>
    </tr>
    <tr>
      <th>235</th>
      <td>I wore this dress for the first time yesterday...</td>
      <td>0.1882</td>
      <td>neutral</td>
    </tr>
  </tbody>
</table>
</div>



It is note worthy mentioning here that instead of listing df's entries by querying a specific condition as we just did, we can alternatively use the following line of code to extract the indices of the entries we are interested in, and use the index inturn to get the exact entry by using index position: 

```python 
df.index[df['sentiment_category'] == 'neutral'].tolist()
```

Let us now see one review entry from each category along with their sentiment value and allocated category.

### Positive


```python
df['Review Text'].iloc[0]
```




    'I had such high hopes for this dress and really wanted it to work for me. i initially ordered the petite small (my usual size) but i found this to be outrageously small. so small in fact that i could not zip it up! i reordered it in petite medium, which was just ok. overall, the top half was comfortable and fit nicely, but the bottom half had a very tight under layer and several somewhat cheap (net) over layers. imo, a major design flaw was the net over layer sewn directly into the zipper - it c'




```python
df['review_sentiment'].iloc[0]
```




    0.9062




```python
df['sentiment_category'].iloc[0]
```




    'positive'



**It is interesting to note here that, although reading the review doesn't imply that the review is by and large positive we can see that it is categorized as postive with a very high value, i.e 0.9**

### Nagative


```python
df['Review Text'].iloc[79]
```




    "The fabric felt cheap and i didn't find it to be a flattering top. for reference i am wearing a medium in the photos and my measurements are 38-30-40."




```python
df['review_sentiment'].iloc[79]
```




    -0.3724




```python
df['sentiment_category'].iloc[79]
```




    'negative'



**Here our judgement and sentiment-analysis's conclusion seems to be aligned**

### Neutral


```python
df['Review Text'].iloc[19]
```




    "First of all, this is not pullover styling. there is a side zipper. i wouldn't have purchased it if i knew there was a side zipper because i have a large bust and side zippers are next to impossible for me.\n\nsecond of all, the tulle feels and looks cheap and the slip has an awkward tight shape underneath.\n\nnot at all what is looks like or is described as. sadly will be returning, but i'm sure i will find something to exchange it for!"




```python
df['review_sentiment'].iloc[19]
```




    0.1027




```python
df['sentiment_category'].iloc[19]
```




    'neutral'



**In this particular case also our conclusion and the analysis's verdict diverges, we would get a negative feel from comment. The reviewer seems to be telling us that she is tricked into buying the dress. Reading the comment we would take it as a negative review, yet the analysis thinks it is neutral**

### Contradictory?


```python
df['Review Text'].iloc[24]
```




    "The colors weren't what i expected either. the dark blue is much more vibrant and i just couldn't find anything to really go with it. fabric is thick and good quality. has nice weight and movement to it. the skirt just wasn't for me, in the end."




```python
df['review_sentiment'].iloc[24]
```




    0.8442




```python
df['sentiment_category'].iloc[24]
```




    'positive'



**Although this may not as be straight forward as the previous cases, the overall vibe of the comment is inclined towards negative than postive. One would have put this as neutral, taking into cosideration the mixed message sengt by the comment**

# Visualizing Sentiments

Whenever it is possible it is a good practice to include visual presentation while sharing your work. To this end we here visualize the categories using pie chart. Generally speaking it is not advisable to use pie-charts if you have more than forur items in category, as it could bring (at times) difficultly in telling the difference between the size of the slices in the chart. A very promising and appealing visual display is given below, it is one of my favorites when you have fewer categories to compare. This visual fihure is a variant of pie-chart and is commonly known as donut-like pie chart.  

In the following code cell the magic line `%matplotlib inline` ensures that the images to be displayed in this notebook. The semicolon (;) at the end of the plot command avoids the display of `<matplotlib.axes._subplots.AxesSubplot at 0x1a1ec10290>` in the output.


```python
%matplotlib inline
df.groupby(['sentiment_category']).size().plot.pie(label="");
```


![png](img/Text_Analysis_Women_Clothing_files/Text_Analysis_Women_Clothing_62_0.png)


As we can see from the pie chart the majority of the reviews are categorized to be postive. Set aside as to the resason why we got such large proportion of postive category as it is not the the scope of the present post. But it is worth pointing out that the analysis needs further refining. This could be related because of the limitation of the function to anlayse words like `wouldn't, couldn't, isn't` and the likes. 

Another point worth raising here that from the look of it the proportions of neutral and negative are close enough it is hard to see the difference from the graph. A best solution in such case is to include percentage value alongside the chart. One of the ways to achieve this leads me to introduce my preference when it comes to versions of pie-charts. Instead of pie chart, I think donut-like pie chart is visually more attractive, you can find it and other interesting, visually appealing pie-charts that uses Python's `Plotly` in [donut-pie](https://plotly.com/python/pie-charts/). 

```python
import plotly.graph_objects as go
%matplotlib inline


df_sentiments = df.groupby('sentiment_category').size()/df['sentiment_category'].count()*100
labels = df_sentiments.index
values = df_sentiments.values

# Use `hole` to create a donut-like pie chart
fig = go.Figure(data=[go.Pie(labels=labels, values=values, hole=.7)])
fig.show()
```
![png](img/Text_Analysis_Women_Clothing_files/Text_Analysis_Women_Clothing_63_0.png)

To get clear picture of the neutral and negative catagories let us make a histogram plot. To do so we need to convert the categories into numbers using the following function. Where we assigned positive =1, negative =-1, and neutral = 0.


```python
#create a function to assign a number to category of the sentiments
def sentimentCategory_num(sent_num):
    if sent_num == "positive":
        return 1
    if sent_num == "negative":
        return -1
    else:
        return 0
```


```python
#create a new column to hold sentiment category numbers
df['sentiment_category_num'] = df['sentiment_category'].apply(sentimentCategory_num)
```


```python
df['sentiment_category_num'].plot(kind='hist');
```


![png](img/Text_Analysis_Women_Clothing_files/Text_Analysis_Women_Clothing_68_0.png)



```python
# zoom_in plots
df['sentiment_category_num'].plot(kind='hist', ylim=(0,700));
```


![png](img/Text_Analysis_Women_Clothing_files/Text_Analysis_Women_Clothing_69_0.png)


Overall, it seems that most buyers feel positive.


```python

```
