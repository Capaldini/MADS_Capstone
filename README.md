# MADS_Capstone_Generating URLs for Coursera Videos
Master of Applied Data Science Capstone Project 

University of Michigan School of Information

Team Members: 
* [Nick Capaldini](https://www.linkedin.com/in/nick-capaldini/), nickcaps@umich.edu
* [Wei Zhou](https://www.linkedin.com/in/wei-zhou-5aab0b102/), weiwzhou@umich.edu


## Table of Contents: 

- [1. Introduction](#1-introduction)
- [2. Data Acquisition](#2-data-acquisition)
- [3. Methodology](#3-methodology)
- [4. Results & Discussion](#4-results---discussion)
- [5. Conclusion & Future Direction](#5-conclusion---future-direction)



### 1. Introduction

•	Background

As students from the MADS program, we’ve completed the whole program online. The online study offers great flexibility to students because we could study whenever and wherever works the best. For example, we could pause the lecture videos when we want to dig deeper before moving forward and we could do some online research while the videos are paused. So, a nice online study platform feature would be to provide users the links to useful resources to better understand the concepts and save time searching around. We found that clickable URLs are extremely convenient for the users. We decided to focus our capstone project on this product idea and figure out a minimum viable product that automatically extracts key words/phrases from original transcripts and generates URLs.

•	Audience & Motivation

Users of this product are users of online courses and video conference call recordings. This product feature could not only be applied to the online learning platform, but also in business context. For example, when users of business organizations watch previously recorded video, automatically generated URLs will pop up under the video based on the video’s content (transcripts).

### 2. Data Acquisition

•	Data Source & Data Acquisition Tool

We used a Python library called coursera-dl to download MADS course transcripts that we’re authorized to access from the MADS program. The dataset we created for analysis has two columns, one column is the course number, the other column is the text transcripts. 
Here is a [list](https://www.si.umich.edu/programs/master-applied-data-science-online/curriculum/mads-courses) of MADS curriculum and course names. Feel free to click the [link](https://github.com/coursera-dl/coursera-dl#coursera-downloader) to GitHub of Coursera Downloader if you’re interested in exploring this tool.


### 3. Methodology

•	Data Cleaning

Prior to data cleaning, we did a LDA (LatentDirichletAllocation)analysis with n_components=10 and was hoping to understand the original text before removing stop words and lemmatization. Our setup for the TfidfVectorizer is min_df=3, ngram_range=(3,4).  The top 10 topics list helped us defined a more specific stop words list. For example, “just little bit”, “make sense”, “want make sure”, “little bit”, “blah blah blah blah”, “let look example”, etc. Those phrases frequently appear in our daily spoken English, but do not add too much value to a student having a professional course. So besides the stop words from NLTK library, we also created another stop words list to deal with domain knowledge, like “data” and “science”, and frequently used words, like “going” and “want”. 

After that, we created a new column to store the token transcripts. Then we ran LDA again with tokenized transcripts and the top 10 topics made more sense. We got rid of filler words and frequently used phrases in spoken English. However, one issue we realized was that LDA was not able to identify grammatically correct phrases, which are important input as we fetch URLs for those phrases. 

•	Feature Engineering

We introduced BERT model to help extract key words and key phrases. We ran KeyphraseCountVectorizer, and the got a list of key words with their cosine similarity to each original document. Finally, we choose the key word with the highest cosine similarity. With the function, we extracted key words for both original transcripts and token transcripts. By reading through the outputs, we believe the key words extracted from original transcripts make more sense. 

•	Wikipedia library

The next step is to search query and return URLs for key words extracted. This is the most complex part of our project.  We used the wikipedia library, which takes word and return URL and page summary as requested. However, there’re two types of difficulties (Python errors) we need to solve: Disambiguation Error and Page Error. Disambiguation Error occurs when the key word may refer to more than one terms, for example, ‘Sample’ may refer to Sample (statistics) or Sample (signal) or Sample (material), etc. Page Error occurs when key word does not match any page id, for example, key phrase ‘harry plotter’ will cause Page Error. To solve the Disambiguation Error, we used DisambiguationError.options  to get all the options that wikipedia.org could find, and then we calculated cosine similarity between input key word and every option in the options list, and finally we return the option with the highest cosine similarity. In the above process, we transformed original extracted key words to options that Wikipedia has pages for. We finally got 1053 out of 1209 automatically generated URLs through our model. 

### 4. Results & Discussion

•	Accuracy Score

To evaluate the accuracy of model, we decided to go through a manual review process. We went through the key words and URLs one by one and gave a score of 1 if the URL is relevant, otherwise 0. This is a subjective evaluation process and we’ve reached a conclusion that 83.71% of the automatically generated URLs make sense. 



### 5. Conclusion & Future Direction

•	Audience Testing

We’ll recruit classmates from MADS program to help us evaluate the accuracy of models as they’re the target users.  Typically, a participant will get 10 records randomly, with original key words and URLs. And user will score 1 to URLs if they’re relevant to key words, otherwise they’ll score 0. We’ll collect the survey responses and calculate an overall accuracy score evaluated by target audience. 

•	Microsoft Teams Extension

A potential commercial application of this project would be an extension to Microsoft Teams. 
