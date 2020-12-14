# CS 410 CourseProject

## Table of Contents
- [1. An overview of the function of the code](#part1) 
- [2. Documentation of how the software is implemented](#part2)
- [3. Documentation of the usage of the software](#part3)
- [4. Brief description of the contribution of each team member](#part4)
- [5. References](#part5)

## 1. An overview of the function of the code (i.e., what it does and what it can be used for) <a name="part1"></a>

We chose to reproduce the first experiement of the paper, Mining Causal Topics in Text Data: Iterative Topic Modeling with Time Series Feedback, which is where the authors of the paper examine the 2000 U.S. Presidential Election. We do this by:

- First, we parse through both the Time Series Prices data, which was from the Iowa Electronic Markets(IRM) Time Series Data, as well as the NYT_Corpus data. Then we start implementing the Iterative Topic Modeling Framework with Time Series Feedback as explained in the paper. 

- Using this data we are able to implement a general text mining framework for discovering causal topics from text by combining the probabilistic topic model with time series causal analysis to discover topics that are semantically coherent as well as correlated with the time series data. By iterating through the data, we can refine the topics which increase the correlation of the topics with the time series. We can use this function in order to analyze textual topics in conjunction with external time series variables such as stocks

What we implemented can be used to find the causal relationships between text data and non-text data, between media coverage and public opinion. Thus, our code can potentially be modified in order to identify target paragraphs for topics relating to not just the 2000 U.S. Presidential Election but instead can be used for other things such as measuring the relationship between the public's response to topics such as climate change, corona virus, as well as other issues. 

## 2. Documentation of how the software is implemented with sufficient detail so that others can have a basic understanding of your code for future extension or any further improvement <a name="part2"></a>

- For the Time Series Prices data, we converted the data that we found online on this site into a CSV file which we read using pandas.

- For the NYT_Corpus data, we loop through all the folders in order to reach the XML files and search for all the paragraphs that include the words “Gore” and “Bush” and use that to filter out the non-relevant documents.

- The Iterative Topic Modeling Framework with Time Series Feedback function can be broken down into the following six parts

  - First, we get the topics by applying the topic modeling method (Latent Dirichlet Analysis) to the set of documents with time stamps, let's call this set D
    
    - The original experiment used PLSA, but according to lecture, PLSA and LDA perform the same. We wanted to use LDA since the gensim library has an LDA function, and our PLSA implementations take a very long time and are possibly incorrect. 
    
  - Second, we use the Granger Tests to find topics with significant values greater than 1 - the output of the Granger Tests. Then we can get the set of candidate causal topics with lag, let’s call this set CT.
  
  - Third, we apply the Granger Tests for each candidate topic in CT in order to find the most significant causal words. Once we find those values, we record them.

  - Fourth, we define a prior on the topic model parameters using significant terms and their values
    
    - First, we need to separate the positive valued terms from the negative valued terms. We can ignore terms with values less than 10%
    
    - Then, we can assign the prior probability proportions according to the significance levels
    
  - Fifth, we use the prior from step 4 to apply LDA to D 
    - This is the part that uses the feedback signals and guides LDA to form topics that better correlate with the time series

  - Sixth, we repeat steps two through five until the stopping criteria. Once we reach the stopping criteria, the process stops and the function outputs CT, which is the output causal topic list

## 3. Documentation of the usage of the software including either documentation of usages of APIs or detailed instructions on how to install and run the software, whichever is applicable <a name="part3"></a>

This project was run using python version 3.8.

Make sure you have access to Jupyter Notebook either by installing Jupyter by following the directions at this [link](https://jupyter.org/install) or by installing Anaconda by following the directions at this [link](https://docs.anaconda.com/anaconda/install/)

For parsing, we use the libraries: os, tarfile, xml.dom.minidom, pandas, xml.etree. These libraries should already be included in python.

For the Iterative Topic Modeling Framework with Time Series Feedback function, we use the libraries: gensim, nltk, re, pprint, spacy. To use these libraries, you need to install them which you can do by doing the following commands in the command prompt (the one we used was the Anaconda Prompt): 

  ```
  - conda install gensim
  - conda install nltk
  - conda install re
  - conda install pprint
  - conda install -c conda-forage spacy
  ```

After installing all the libraries, once you launch the notebook, you should be able to run each cell in the notebook by pressting the "Restart and Run All" button or you can run each cell one at a time by pressing the "Run" button.

## 4. Brief description of the contribution of each team member in case of a multi-person team <a name="part4"></a>

In general, we worked together as a group on a call and gave each other advice and helped when possible whether it be by looking at the current issue, clarify what the Iterative Topic Modeling Framework with Time Series Feedback function is doing, or Googling resources such as libraries to use. Down below is what we were tasked with, but we worked on things outside of what we were in charge of.

- Nikhil Deenamsetty (ndeena2)
  - In charge of the Iterative Topic Modeling Framework with Time Series Feedback function parts 2 and 3
  
- Peter Wasala (pwasal3)
  - In charge of the Iterative Topic Modeling Framework with Time Series Feedback function parts 1, 4, 5
  
- Angela Jaw (ajaw2)
  - In charge of parsing data, wrote up the documentation, helped when asked, did tasks assigned to me

- Jiahua He (jiahuah2)
  - In charge of parsing data, worked on the presentation, helped when asked, did tasks assigned to me

## 5. References <a name="part5"></a>

- [Topic Modeling with Gensim (Python)](https://www.machinelearningplus.com/nlp/topic-modeling-gensim-python/)

- [Topic Distribution](https://stackoverflow.com/questions/20984841/topic-distribution-how-do-we-see-which-document-belong-to-which-topic-after-doi)

- [statsmodel documentation](https://www.statsmodels.org/stable/generated/statsmodels.tsa.stattools.grangercausalitytests.html)
