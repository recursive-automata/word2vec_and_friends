Explorations with Doc2Vec, dimensionality reduction, and clustering
on text and social data for fun and learning.

# Introduction

As you're probably aware, Word2Vec is an unsupervised algorithm for
learning vector representations of words. It works by training a 
shallow neural network on the task of predicting the presence of
words from the "context windows" of nearby words [1]. Word2Vec also
well undersood theoretically -- optimizing for the prediction task is
equivalent to a word cooccurence matrix factorization problem [2]. And
modication of the algorithm, dubbed alternately Doc2Vec and Paragraph2Vec,
learns vector representations of whole documents, simply by appending a
document-specific label to the context windows of the prediction task [3]. 

Word2Vec can learn meaningful vector representations of words essentially
because analogous words tend to occur in analogous contexts. Words that occur
in the same contexts tend to have proximal representative vectors, and since
there's an additive model of occurance probability under the hood, differences 
between vectors can carry semantic meaning. These word vectors are well suited
for machine learning tasks such as clustering or classification.

Word2Vec simply treats a word as a token, which means there's nothing stopping us 
from applying the algorithm to non-NLP problems. The underlying task is to predict
occurence of a token given its context, so any problem domain in which the context
can be encoded in a sequence of tokens is a candidate for throwing Word2Vec at.

# Data sets

SNAP Datasets: Stanford Large Network Dataset Collection, Jure Leskovec and Andrej Krevl,
http://snap.stanford.edu/data, June, 2014

Stack Exchange Data Dump, Stack Exchange, Inc.,
https://archive.org/details/stackexchange, December 15, 2016

# References

[1] Tomas Mikolov, Kai Chen, Greg Corrado, and Jeffrey Dean.
Efficient Estimation of Word Representations in Vector Space.
2013

[2] Omer Levy, Yoav Goldberg.
Neural Word Embedding as Implicit Matrix Factorization. 2014

[3] QV Le and T Mikolov. Distributed Representations of Sentences and Documents. 2014
