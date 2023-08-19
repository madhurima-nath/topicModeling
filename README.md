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

- Section [Dataset used](#data) - provides information on the sample data that is
  used in this repository to perform topic modeling.

  The sub-section
  [Data processing/transformation using pandas](#pandas) provides the
  well-known data processing steps. Data processing can also be done
  using spark when hnadling datasets of large sizes.

  The sub-section
  [Data processing/transformation using spark](#spark) discusses the
  advantages of using Spark and how it can be used on dataframes.

- Section [Example results](#results) - shows the output of topic
  modeling on the above dataset. The analysis is done using both
  pandas LDA modules and SparkNLP modules.

- Section [End-to-end implementation](#end) showcases an example of a
  sample arcitecture of deploying such a large-scale solution to production.
  This also shows how multiple teams collaborate together for
  an enterprise-wide to build such solutions. 


## <a name = "algo"> </a> Topic modeling algorithms

• an unsupervised machine learning problem.
• does not aim to find similarities in documents, 
unlike text classification or clustering.
• makes clusters of three types of words – co-occurring words, 
distribution of words, and histogram of words topic-wise.
• Conventional and well-known approaches to topic modeling are: 
    § Latent Semantic Analysis (LSA) (Deerwester et al. 1990)
    § Probabilistic Latent Semantic Analysis (pLSA) (Hofmann, 1999)
    § Latent Dirichlet Allocation (LDA) (Blei et al., 2003)
• basic principle behind the search of latent topics is the 
**decomposition of the Document-Term Matrix (DTM) into a document-topic**
**and a topic-term matrix**. The three methods differ in how
they define and reach this goal.

### LDA

## <a name = "data"> </a>  Dataset used

source, EDA, data cleaning/processing

### <a name = "pandas"> </a> Data processing/transformation using pandas
pandas notebook

### <a name = "spark"> </a> Data processing/transformation using spark
Spark notebook link

## <a name = "results"> </a> Example results
Depending on the requirements of the end-users, the output format of the topics obtained
may change. Usually, when topic modeling is done, the topic and the distribution of
terms/words in the topic is obtained and presented as result. 
| Topic | Topic terms | Topic weights/probablities | 
| --- | --- | --- | 
| 1 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
| 2 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
| 3 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |

However it is also possible to map these output topics back to the
original dataset, with each row showing which set of topic(s) it might belong to.
This can be used if the requirement is that the end users want to drill down 
into each of the comments details. 

| ID | Title | Abstract | Topic | Topic terms | Topic weights/probablities | 
| --- | --- | --- | --- | --- | --- |
| 1 | title 1 | abstract text | 1 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
| 2 | title 2 | abstract text | 3 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |
...
| 500 | title 100 | abstract text | 5 | \[term1, term2, term3\] | \[prob1, prob2, prob3 \] |

Both these output formats can be easily fed into one of the data 
visualization platforms like MS Power BI or Tableau to provide 
the end users with usable dashboards.

### <a name = "pandasresults"> </a> Pandas results
pandas notebook link, explain the pandas lda module

### <a name = "sparkresults"> </a> Spark results
spark notebook link, explain spark nlp modules

## <a name = "end"> </a> End-to-end implementation
There are a lot of pieces involved while designing an end-to-end solution. 
![image](https://github.com/madhurima-nath/topicModeling/blob/main/dataArchitecture.jpg)

### Data architecture
insert sample data architecture.

# References:
1. Deerwester, Scott, et al. "Indexing by latent semantic analysis." Journal of the American society for information science 41.6 (1990): 391-407.
2. Hofmann, Thomas. "Probabilistic latent semantic indexing." In Proceedings of the 22nd annual international ACM SIGIR conference on Research and development in information retrieval. 1999.
3. Blei, David M., Andrew Y. Ng, and Michael I. Jordan. "Latent dirichlet allocation." Journal of machine Learning research 3, no. Jan (2003): 993-1022.
