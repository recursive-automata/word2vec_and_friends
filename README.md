Explorations with Doc2Vec, dimensionality reduction, and clustering
on text and social data for fun and learning.

# Introduction

As you're probably aware, unsupervised algorithms for learning vector
representations of words are interesting and useful.
The maybe-still-most-popular is Google's Word2Vec, which works by training a 
shallow neural network on the task of predicting the presence of
a word from the context window of nearby words [1]. It turns out that
optimizing Word2Vec's prediction task is equivalent to factoring
a specific construction of the pointwise mutual information matrix [2].
Accordingly, Word2Vec is just one of a handful of algorithms for
learning so-called "word embeddings" into vector spaces, including
Stanford's GloVe and Facebook's FastText.

# Data sets

SNAP Datasets: Stanford Large Network Dataset Collection,
Jure Leskovec and Andrej Krevl, http://snap.stanford.edu/data, June 2014

Stack Exchange Data Dump, Stack Exchange, Inc.,
https://archive.org/details/stackexchange, December 2016

# Investigations

## Stackoverflow posts

### Overview

Word2Vec can learn meaningful vector representations of words essentially
because analogous words tend to occur in analogous contexts. Words that occur
in the same contexts tend to have proximal representative vectors, and since
there's an additive model of occurance probability under the hood, differences 
between vectors can carry semantic meaning. These word vectors are well suited
for machine learning tasks such as clustering, classification, or similarity 
detection.

A modication to Word2Vec, since dubbed Doc2Vec, learns vector representations of 
whole documents, simply by appending a document-specific tag to the context windows of
the prediction task [3]. One (very mathematical) way to think about this: a
document's vector defines a bias over the word vectors that increases the
probability of predicting that document's words given their context windows.
We're also free to stack such biases additively, by attaching multiple
(and non-document-specific) tags to a document. Furthermore, we get the nice 
property that words and tags that tend to co-occur will end up with proximal
vectors. Very interesting....

Applications abound -- why not a Stackoverflow search engine?

### Munge

Stack Exchange, Inc. is very generous and does data dumps of the stackoverflows'
site posts (find the link below). There's a good spectrum of dataset sizes from
their distinct subdomains, and if you're fearless, you can do the whole shabang,
probably more than a TB uncompressed.

I settled on a few 10s of GB, but that's a bit much to sit comfortably in my box's
memory (maybe yours, too), so I setup a streaming xml parser to do the preproccessing
and write the results to a few tsv's. Gensim's implementation is perfectly content
with a streaming corpus, so we can just read those tsv's and feed them to Word2Vec as 
we go.

TODO ...

### Discussion

TODO ...

## Social networks

### Overview

Word2Vec simply treats an individual word as a unique token, which means there's nothing
stopping us from applying the algorithm to non-NLP problems. The underlying task is to predict
occurence of a token given its context, so any problem domain in which the context
can be encoded in a sequence of tokens is a candidate for throwing Word2Vec at.

Let's see if Word2Vec can identify major clusters in a social network. We have good reason to be
optimistic: both the problem and the algorithm can be recast in terms of factoring a co-occurance matrix.
Let's also see if we can display the network in an easy-for-human-comprehension kind of way.

### Munge

One of SNAP's datasets includes a small text file containing the friendships between 4.3K anonymized 
facebook users. There are 88K friendships, or for a mean of about 20 friendships per user. This one's
easy: let's read the file, build the network graph, and store it in memory.

Word2Vec expects "sentences", so what do those look like in our problem? Try random walks of the social
graph. The size of the context window has a clear interpretation here: it places an upper bound on how
many degrees of seperation we're using to define a user's social context.

4.3K is a much smaller vocabulary than Word2Vec usually sees. To compensate, use small
dimensionality for the embedding vector spaces, and concatenate three different embeddings:
those with degrees of speration < 2, < 4, and < 7 in users' social contexts.

Once training's done, apply PCA and t-SNE to get a 2D representation of the for use in plotting. 
Cluster on the user vectors, agglomerative and k-means

### Discussion

I think this example is interesting in that it clearly illustrates what Word2Vec isn't. With
a dataset of < 100K training examples, it's hard to construe this as deep learning. And a
vocabulary of 4.3K is unnaturally small, so it's not strictly an NLP algorithm.

TODO ...

# Acknowledgements

Like all (data-)scientists, I stand on the shoulders of giants. The inspiration
for this project began awhile ago when I watched a 2015 talk Chris Moody gave at
*Text by the Bay*, and the flame's been fanned since by the blogs, talks, and github
repos of many who've worked with word embeddings. And thanks to Radim Řehůřek for an 
excellent, open-source implementation of Word2Vec.

# References

[1] Tomas Mikolov, Kai Chen, Greg Corrado, and Jeffrey Dean.
Efficient Estimation of Word Representations in Vector Space.
2013

[2] Omer Levy, Yoav Goldberg.
Neural Word Embedding as Implicit Matrix Factorization. 2014

[3] QV Le and T Mikolov. Distributed Representations of Sentences and Documents. 2014
