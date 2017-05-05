A place to put my explorations of and ruminations about vector embedding
and dimensionality reduction, starting with Doc2Vec and t-SNE.

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

# Acknowledgements

Like all (data-)scientists, I stand on the shoulders of giants. The inspiration
for this project began awhile ago when I watched a 2015 talk Chris Moody gave at
*Text by the Bay* [3], and the flame's been fanned by all the subsequent progress
in the field. Thanks to Radim Řehůřek et al for an excellent, open-source
implementation of Doc2Vec [4].

# References

[1] Tomas Mikolov, Kai Chen, Greg Corrado, and Jeffrey Dean.
Efficient Estimation of Word Representations in Vector Space.
2013

[2] Omer Levy, Yoav Goldberg.
Neural Word Embedding as Implicit Matrix Factorization. 2014

[3] Chris Moody. "A Word is Worth a Thousand Vectors". 2015. https://youtu.be/vkfXBGnDplQ

[4] RaRe Technologies, Gensim. https://github.com/RaRe-Technologies/gensim

# License

Copyright (c) 2017 Simon Schneider

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
