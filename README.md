Explorations with Doc2Vec, dimensionality reduction, and clustering
on text and social data for fun and learning.

# Introduction

As you're probably aware, unsupervised algorithms for learning vector
representations of words are interesting and useful.
The maybe-still-most-popular is Google's Word2Vec, which works by training a 
shallow neural network on the task of predicting the presence of
words from the "context windows" of nearby words [1]. Iterestingly, 
optimizing Word2Vec's prediction task is equivalent to factoring
a specific construction of the word co-occurrence matrix,  
and Word2Vec's improved performance over older, more traditional
methods in NLP has mostly to do with hyperparameter selection [2].
Accordingly, Word2Vec is just one of a handful of algorithms for
learning "word embeddings", including Stanford's GloVe and Facebook's
FastText, as well as older players such as Latent Semantic Indexing
and TextRank.

I'll start with Word2Vec simply because I know it best. As free-time permits,
I'll probably start playing with FastText and post the code.

# Data sets

SNAP Datasets: Stanford Large Network Dataset Collection,
Jure Leskovec and Andrej Krevl, http://snap.stanford.edu/data, June 2014

Stack Exchange Data Dump, Stack Exchange, Inc.,
https://archive.org/details/stackexchange, December 2016

# Investigations

## Stackoverflow posts

Word2Vec can learn meaningful vector representations of words essentially
because analogous words tend to occur in analogous contexts. Words that occur
in the same contexts tend to have proximal representative vectors, and since
there's an additive model of occurance probability under the hood, differences 
between vectors can carry semantic meaning. These word vectors are well suited
for machine learning tasks such as clustering or similarity identification.

A modication to Word2Vec, dubbed Doc2Vec, learns vector representations of whole
documents, simply by appending a document-specific label to the context windows of
the prediction task [3].

TODO ...

## Social networks

Word2Vec simply treats an individual word as a unique token, which means there's nothing
stopping us from applying the algorithm to non-NLP problems. The underlying task is to predict
occurence of a token given its context, so any problem domain in which the context
can be encoded in a sequence of tokens is a candidate for throwing Word2Vec at.

TODO ...

# Acknowledgements

Like all (data-)scientists, I stand on the shoulders of giants. The inspiration
for this project was largely a talk Chris Moody gave at *Text by the Bay* in 2015.
And the excellent, open-source implementation of the algorithm was written and
maintained by Radim Řehůřek.

# References

[1] Tomas Mikolov, Kai Chen, Greg Corrado, and Jeffrey Dean.
Efficient Estimation of Word Representations in Vector Space.
2013

[2] Omer Levy, Yoav Goldberg.
Neural Word Embedding as Implicit Matrix Factorization. 2014

[3] QV Le and T Mikolov. Distributed Representations of Sentences and Documents. 2014
