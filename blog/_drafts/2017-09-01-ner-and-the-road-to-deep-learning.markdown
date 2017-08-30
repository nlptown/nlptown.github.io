---
layout: post
title: Named Entity Recognition and the Road to Deep Learning
shorttitle: NER and the Road to Deep Learning
date:   2017-09-01 12:00:00
tags: NLP AI deep learning word embeddings named entity recognition
comments: true
author: Yves Peirsman
---

<p class="first">
</p>

<p>
</p> 

<p>
One of the most well-known weaknesses of this type of model is that it is unable to model semantic similarity between two words. For all it knows,
<em>Paris</em> and <em>London</em> are as different meaning-wise as <em>Paris</em> and <em>cat</em>. To overcome this problem, most CRFs for 
Named Entity Recognition rely on gazetteers &mdash; word lists with names of people, locations and organizations that are known in advance. 
This is suboptimal solution, however: gazetteers are inherently limited and can be costly to develop. Also, they’re usually only present for the
target named entities themselves, so that semantic similarity between other words in their context generally goes unnoticed. This is 
where it can pay off to take the first step on the road to deep learning.</p>
 
<p>
As I’ve <a href="http://nlp.yvespeirsman.be/blog/anything2vec/">written before</a>, word embeddings are 
one of the main drivers behind the success of deep learning in NLP. The first layer in a neural network
for text processing is often an embedding layer where one-hot word vectors are mapped to their dense 
embeddings. Still, this does not mean that the semantic knowledge in word embeddings can only be leveraged 
in neural networks; it’s also possible to add them to CRFs. One popular way of doing this is to group the 
word embeddings in a number of clusters, and feed these cluster IDs as features to the CRF.<p>

<p>
To illustrate this procedure, let’s train a set of 64-dimensional word embeddings on the English Wikipedia for all words
that appear at least 1000 times in that corpus. Let’s then use K-means to group these embeddings in 500 
clusters. While it’s easy enough to write the code to do this with the help of libraries such as <a href="https://radimrehurek.com/gensim/">Gensim</a>
or <a href="http://scikit-learn.org/">Scikit-learn</a>, <a href="https://github.com/dav/word2vec">Google’s original word2vec code</a> actually has
a command that lets us perform all the necessary steps in one go: 

<div class="highlight">
> word2vec -train our_input -output our_output -cbow 0 -size 64 -window 3 -negative 0 -hs 1 -sample 1e-3 -threads 12 -classes 500 -min-count 1000
</div>

You can find it in the bash script <code>demo-word.sh</code>.
</p>

<p>The clusters that we obtain, are a treasure trove for Named Entity Recognition.
For example, cluster 437 contains many location names, such as <em>München</em>, <em>Paris</em>
and <em>Brussels</em>. The presence of a target word in this cluster clearly increases the 
probability that it refers to a location. Similarly, cluster 254 contains verbs such as 
<em>say</em>, <em>think</em> or <em>pray</em>. Our model should therefore be able to learn
that a target word that is followed by one of the words from this cluster is pretty likely
the name of a person. This is backed up by our own experiments: when we add the cluster
IDs of these 500 clusters as features to our simple CRF, its F-score on the named
entities increases from 81.15% to 84.86%. Adding additional features, such as
gazetteers, or informative feature conjuctions, is certain to take up the F-score
further still.</p>

<p>
Useful as these embedding clusters are, they only give a first indication of what
deep learning can do. 
</p>

<p>
</p>

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRpybpcEGwT54HBbG9uhWKHGOTDy64LZF1edzChqMRev8AvFisyMsDGHxz1hY7TvnI4Sd0i15Fb0wsp/pubchart?oid=284160483&amp;format=interactive"></iframe>


