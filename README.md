# Implementation of end-to-end topic modeling solution

Topic modeling is a part of natural language processing (NLP) which enables end-users to 
identify themes and topics within a collection of documents. It has applications in multiple industries for text mining and gaining relevant insights from textual data. 
This repository can be used as a reference to undertsand the math behind topic modeling, 
do a hands-on exercise of extracting topics from a sample data, use both pandas and
pyspark libraries to perform data processing and topic modeling,
how the end user can leverage these outcomes to take actionable insights and lastly,
what are the necessary infrastructure required to implement a large-scale
end-to-end solution.

- Section [Topic modeling algorithms](#algo) - explains the different topic
  modeling algorithms available.

  Additional reading: Medium article on different
  [topic modeling algorithms like LDA, NMF, BERTopic](https://medium.com/@m.nath/topic-modeling-algorithms-b7f97cec6005).

- Section [Dataset used](#data) - provides information on the sample data that is
  used in this repository to perform topic modeling.

- Section [Example results](#results) - shows the output of topic
  modeling on the above dataset. The analysis is done using both
  [pandas LDA](#pandasresults) and [SparkNLP LDA](#sparkresults) modules.

- Section [End-to-end implementation](#end) showcases an example of a
  sample arcitecture of deploying such a large-scale solution to production.
  This also shows how multiple teams collaborate together for
  an enterprise-wide to build such solutions.
  
  Additonal reading: [Medium article on Implementation of end-to-end ML
  solution](https://medium.com/@m.nath/implementation-of-end-to-end-machine-learning-solution-aebdbc024c4e)


## <a name = "algo"> </a> Topic modeling algorithms

- an unsupervised machine learning problem.
- does not aim to find similarities in documents,
  unlike text classification or clustering.
- makes clusters of three types of words – co-occurring words,
  distribution of words, and histogram of words topic-wise.
- Conventional and well-known approaches to topic modeling are:
  
    § Latent Semantic Analysis (LSA)
  
    § Probabilistic Latent Semantic Analysis (pLSA)

    § Latent Dirichlet Allocation (LDA)

    § Hierarchical Dirichlet Process (HDP)

    § Non-Matrix Factorization (NMF)

    § BERTopic

- basic principle behind the search of latent topics is the
  **decomposition of the Document-Term Matrix (DTM) into a document-topic**
  **and a topic-term matrix**. The three methods differ in how
  they define and reach this goal.

**STEP 1**:

Start with the conversion of a textual corpus into 
a Document -Term Matrix (DTM), a table where each row is a document, 
and each column is a distinct word.

Consider this corpus:
| | |
| --- | --- |
| document 1 | I like books |
| document 2 | I recently read two bestseller books |
| document 3 | Some movies are based on bestseller books |

The **`DOCUMENT-TERM MATRIX (DTM)`** for this corpus is

| | I | like | books | recently | read | two | bestseller | some | movies | are | based | on |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| doc1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| doc2 | 1 | 0 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| doc3 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 1 |

This matrix basically does a count of frequency of each of the words/terms
in the documents. This is called the ***Term Frequency (TF)***.

As an alternative, ***Term Frequency – Inverse Document Frequency (TF-IDF)*** 
can be considered.

**STEP 2**:

Decompose the **`Document-Term Matrix DTM`** and extract topics.
- LSA uses matrix factorization - Singular Value Decomposition (SVD)
- pLSA uses probabilistic model, calculates the joint probability
  of seeing a word and a document together as a mixture of conditionally
  independent multinomial distributions
- LDA uses Dirichlet priors to estimate the document-topic and term-topic
  distributions in a Bayesian approach

The goal is to fins the `Topic-Term Matrix` by solving the equation

`DTM` (dim: $m \times n$) = `Document-Topic Matrix`  (dim: $m \times t$)
`Topic-Importance Matrix` (dim: $t \times t$) `Topic-Term Matrix` (dim: $t \times n$)

**`Document-Term Matrix (DTM)`** (dim: $m \times t$)

| | term 1 | term 2 | ... | term n |
| --- | --- | --- | --- | --- |
| doc 1 | 1 | 0 | | |
| doc 2 | 0 | 1 | | |
| ... | | | | |
| doc m | | | | |

**`Document-Topic Matrix`** (dim: $m \times t$) 

| | topic 1 | topic 2 | topic 3 |
| --- | --- | --- | --- |
| doc 1 | | | |
| doc 2 | | | |

**`Topic-Importance Matrix`** (dim: $t \times t$)

| | topic 1 | topic 2 | topic 3 |
| --- | --- | --- | --- |
| topic 1 | | | |
| topic 2 | | | |
| topic 3 | | | |

**`Topic-Term Matrix`** (dim: $t \times n$)
| | term 1 | term 2 | term 3 |
| --- | --- | --- | --- |
| topic 1 | | | |
| topic 2 | | | |
| topic 3 | | | |

where, $m$ = Number of documents, $n$ = Number of words and $t$ = Number of topics

---

### Latent Dirichlet Allocation (LDA)
LDA is a ***generative probabilistic*** model for 
collections of discrete data such as text corpora.
* A 3-level hierarchical Bayesian model, in which each
  item of a collection is modeled as a finite mixture over an underlying set
  of topics.
* Each topic is modeled as an infinite mixture over an underlying set of topic probabilities.
* The topic probabilities provide an explicit representation of a document.

Pros:
* better performances than LSA and pLSA - pLSA is prone to overfitting
* can assign a probability to a new document thanks
  to the document-topic Dirichlet distribution
* topics are open to human interpretation
  
Cons:
* number of topics must be known/set beforehand
* bag-of-words approach disregards the semantic
  representation of words in a corpus, similar to LSA and pLSA
* estimation of Bayes parameters lies under the
  assumption of exchangeability for the documents
* requires an extensive pre-processing phase
  to obtain a significant representation from the textual input data
* studies report LDA may yield too general (Rizvi et al., 2019)
  or irrelevant (Alnusyan et al., 2020) topics.
  Results may also be inconsistent across different executions (Egger et al., 2021).

## <a name = "data"> </a>  Dataset used

The Kaggle dataset is downloaded from 
[here](https://www.kaggle.com/datasets/blessondensil294/topic-modeling-for-research-articles?resource=download&select=train.csv).
It has abstract and title for a set of research articles
and each article is assigned to one or more of the following topics:
- Computer Science
- Physics
- Mathematics
- Statistics
- Quantitative Biology
- Quantitative Finance

### <a name = "pandasresults"> </a> Results using pandas
Refer to [this notebook](https://github.com/madhurima-nath/topicModeling/blob/main/pandasTopicModeling.ipynb)
for analysis in pandas.

### <a name = "sparkresults"> </a> Results using pyspark

**SparkNLP module**
SparkNLP is developed and maintained by 
[John Snow Labs](https://nlp.johnsnowlabs.com).
It is an open-source text processing module
built on top of Apache Spark and spark ML library.
It supports Python, Scala and Java.

**PySpark ML module**
This contains dataframe-based ML Pipeline APIs
which lets users quickly assemble and configure
ML solutions. It is fast and uses distributed computing.
To learn more about PySpark ML package, refer
[here](https://spark.apache.org/docs/2.3.0/api/python/pyspark.ml.html).

Refer to [this notebook](https://github.com/madhurima-nath/topicModeling/blob/main/pysparkTopicModeling.ipynb)
for analysis in PySpark

## <a name = "results"> </a> Example results
Depending on the requirements of the end-users, the output format of the topics obtained
may change. Usually, when topic modeling is done, the topic and the distribution of
terms/words in the topic is obtained and presented as result. 
| Topic | Topic terms | Topic weights/probablities | 
| --- | --- | --- | 
| 1 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
| 2 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
| 3 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |

This can be then visualized in the notebook itself as shown below.
![image](https://github.com/madhurima-nath/topicModeling/blob/main/topicsImage.jpg)

However it is also possible to map these output topics back to the
original dataset, with each row showing which set of topic(s) it might belong to.
This can be used if the requirement is that the end users want to drill down 
into each of the comments details. 

| ID | Title | Abstract | Topic | Topic terms | Topic weights/probabilities | 
| --- | --- | --- | --- | --- | --- |
| 1 | title 1 | abstract text | 1 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
| 2 | title 2 | abstract text | 3 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
...
| 500 | title 100 | abstract text | 5 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |

Both these output formats can be easily fed into one of the data 
visualization platforms like MS Power BI or Tableau to provide 
the end users with usable dashboards.


## <a name = "end"> </a> End-to-end implementation
There are a lot of pieces involved while designing an end-to-end solution. 
![image](https://github.com/madhurima-nath/topicModeling/blob/main/dataSolution.jpg)

### Data architecture
This is a sample data architecture of an end-to-end solution which is scheduled for refreshes.
![image](https://github.com/madhurima-nath/topicModeling/blob/main/dataArchitecture.jpg)
The medallion structure is a best practice in industry nowadays. 

# References:
1. Deerwester, Scott, et al. “Indexing by latent semantic analysis.” Journal of the American society for information science 41.6 (1990): 391–407.
2. Hofmann, Thomas. “Probabilistic latent semantic indexing.” In Proceedings of the 22nd annual international ACM SIGIR conference on Research and development in information retrieval. 1999.
3. Blei, David M., Andrew Y. Ng, and Michael I. Jordan. “Latent dirichlet allocation.” Journal of machine Learning research 3, no. Jan (2003): 993–1022.
4. Lee, Daniel, and H. Sebastian Seung. “Algorithms for non-negative matrix factorization.” Advances in neural information processing systems, 13 (2000).
5. Lee, Daniel, and H. Sebastian Seung. “Learning the parts of objects by non-negative matrix factorization.” Nature 401, no. 6755 (1999): 788–791.
6. Egger, Roman, and Joanne Yu. “A topic modeling comparison between lda, nmf, top2vec, and berttopic to demystify twitter posts.” Frontiers in sociology 7 (2022): 886498.
7. Grootendorst, Maarten. “BERTopic: Neural topic modeling with a class-based TF-IDF procedure. arXiv preprint arXiv:2203.05794 (2022).
8. Devlin, Jacob, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. “Bert: Pre-training of deep bidirectional transformers for language understanding.” arXiv preprint arXiv:1810.04805 (2018).
9. Shi, Tian, Kyeongpil Kang, Jaegul Choo, and Chandan K., Reddy. “Short-text topic modeling via non-negative matrix factorizationenriched with local word-context correlations.” In Proceedings of the 2018 World Wide Web Conference, pp. 1105–1114, 2018.
10. Choo, Jaegul, Changhyun Lee, Chandan K. Reddy, and Haesun Park. “Utopian: User driven topic modeling based on interactive nonnegative matrix factorization.” IEEE transactions on visualization and computer graphics 19, np. 12 (2013): 1992: 2001.
