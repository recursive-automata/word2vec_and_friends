A place to put my explorations of and ruminations about vector embedding
methods, starting with Doc2Vec.

# Introduction

As you're probably aware, unsupervised algorithms for learning vector
representations of words are interesting and useful.
The maybe-still-most-popular is Google's Word2Vec, which works by training a 
shallow neural network on the task of predicting the presence of
a word from the context window of nearby words [1]. It turns out that
optimizing Word2Vec's prediction task is equivalent to factoring
a specific construction of the pointwise mutual information matrix [2].
So it's not surprising that Word2Vec is just one of a handful of algorithms
for learning so-called "word embeddings" into vector spaces, including
Stanford's GloVe and Facebook's FastText.

# Data sets

SNAP Datasets: Stanford Large Network Dataset Collection,
Jure Leskovec and Andrej Krevl, http://snap.stanford.edu/data, June 2014

Stack Exchange Data Dump, Stack Exchange, Inc.,
https://archive.org/details/stackexchange, December 2016

# Investigations

## Facebook friendships

### Overview

Word2Vec simply treats an individual word as a unique token, which means there's nothing
stopping us from applying the algorithm to non-NLP problems. The underlying task is to predict
occurence of a token given its context, so any problem domain in which the context
can be encoded in a sequence of tokens is a candidate for throwing Word2Vec at.

Word2Vec can learn meaningful vector representations of words essentially
because analogous words tend to occur in analogous contexts. Words that occur
in the same contexts tend to have proximal representative vectors, and differences 
between vectors can carry semantic meaning. These embedded vectors are well suited
for machine learning tasks such as clustering, classification, or similarity 
detection.

A modication to Word2Vec, dubbed Doc2Vec, learns vector representations of 
whole documents, simply by appending a document-specific tag to the context windows of
the prediction task [3]. One (very mathematical) way to think about this: a
document's vector defines a bias over the word vectors that increases the
probability of predicting that document's words given their context windows.
We're also free to stack such biases additively, by attaching multiple
(and non-document-specific) tags to a document.

Let's see if Word2Vec can identify major clusters in a social network. We have good reason to be
optimistic: both the problem and the algorithm can be recast in terms of factoring a co-occurance matrix.
Let's also see if we can display the network in an easy-for-human-comprehension kind of way.

### Munge

One of SNAP's datasets includes a small text file containing the friendships between 4.3K anonymized 
facebook users. There are 88K friendships, or for a mean of about 20 friendships per user. This one's
easy: let's read the file, build the network graph, and store it in memory.

Word2Vec expects "sentences", so what do those look like in our problem? Try random walks of the social
graph. After minimal experimentation, the lengths of these random walks was set to 25.
The size of the "context window" has a clear interpretation here: it places an upper bound on how
many degrees of seperation we're using to define a user's social context.

4.3K is a much smaller vocabulary than Word2Vec usually sees. To compensate, use small
dimensionality for the embedding vector spaces, and concatenate two different embeddings:
those with degrees of speration < 3 and < 7 in users' social contexts.

There's another cool trick we can use: append each random walk with a document tag denoting which
user started the random walk, and use Doc2Vec to learn both a word and a document vector for each user.
While the word vectors will be sensitive only to friendships within users' social contexts, document
vectors will be sensitive to any friendships along the random walks. As we'll see, the long-range sensitivity
of the document vectors will lead to good coarse-grained cluster detection, at the expense of obsuring some 
of the details of the network.

Once training's done, apply PCA and t-SNE to get a 2D representation of the for use in plotting. 
Cluster on the user vectors, hierarchically and with k-means.

### Discussion

I think this example is fascinating for a few reasons. First, it's a dataset with < 100K rows and ~4.3K
distinct tokens. Although generating random walks through the network is a cool sampling trick, the
training data is still less than a million examples -- the big data regime, this is not. But the structure
of the problem is a good match for what Word2Vec does (again: matrix factorization), and I was able to get
a good result quickly.

Second, it illustrates how Doc2Vec trains word vectors and document vectors. Recall that word vectors only
get trained on context windows that contain that word, whereas document vectors get trained on every context
window in every document with a matching tag. Here, the document tags are who the random walks started at, which
means that the document vectors encode more information about who knows who *outside of* a user's immediate
friend group than the word vectors do. Two users will have proximal document vectors if they "run in similar crowds",
largely irrespective of whether they have any mutual friends. In contrast, the word vectors for two users will be
closer if they have more friends in common.

And third, it's a great opportunity to make some eye candy. The information in these embedded vectors can be rendered
visually with t-SNE diagrams [4], on which the network's social graph is plotted. The first diagram was made mostly from
the document vectors, with small contribution from the short-range word vectors to provide cleaner seperation between
the clusters. Six large clusters clearly turn up, and hierarchical clustering captures them very well. The second
t-SNE diagram takes the concatenated long- and short-range word and document vectors as input, and the result
captures much more of the structure of individual clusters while retaining distinction and not getting too noisy.

## Stackoverflow posts

### Munge

Stack Exchange, Inc. is very generous and does data dumps of the stackoverflows'
site posts (find the link below). There's a good spectrum of dataset sizes from
their distinct subdomains, and if you're fearless, you can do the whole shabang,
probably more than a TB uncompressed.

I settled on a few 10s of GB, but that's a bit much to sit comfortably in my box's
memory (maybe yours, too), so I setup a streaming xml parser to do the preproccessing
and write the results to a few tsv's.

Applications to follow....

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

[4] L.J.P. van der Maaten, G.E. Hinton. Visualizing High-Dimensional Data Using
t-SNE. 2008

# License

Copyright (c) 2017 Simon Schneider

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
