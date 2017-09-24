---
layout: post
title: Named Entity Recognition and the Road to Deep Learning
shorttitle: NER and the Road to Deep Learning
date:   2017-09-12 12:00:00
tags: NLP AI deep learning word embeddings named entity recognition
comments: true
author: Yves Peirsman
image: ner_model.png
---

<p class="first">
Not so very long ago, Natural Language Processing looked very different. In sequence labelling tasks such as 
Named Entity Recognition, Conditional Random Fields were the go-to model. The main challenge for NLP
engineers consisted in finding good features that captured their data well. Today, deep learning has replaced
 CRFs at the forefront of sequence labelling, and the focus has shifted from feature engineering to designing and implementing effective neural network
architectures. Still, the old and the new-style NLP are not diametrically opposed: just as it is possible
(and useful!) to incorporate neural-network features into a CRF, CRFs have influenced some of the best 
deep learning models for sequence labelling.
</p>

<p>
One of the most popular sequence labelling tasks is <a href="http://nlp.town/blog/nlp-api-ner/">Named Entity Recognition</a>, 
where the goal is to identify the names of entities in a sentence.
Named entities can be generic proper nouns that refer to locations, people or organizations,
but they can also be much more domain-specific, such as diseases or genes in biomedical NLP. A seminal task for Named Entity
Recognition was the <a href="https://www.clips.uantwerpen.be/conll2003/ner/">CoNLL-2003 shared task</a>, 
whose training, development and testing data are still often used
to compare the performance of different NER systems. In this blog post, we’ll rely on this data to help us answer a few questions
about how the standard approach to NER has evolved in the past few years: are neural networks really better than CRFs?
What components make up an effective deep learning architecture for NER? And in what way can CRFs and neural networks be combined?
</p> 

<p>
For a long time, Conditional Random Fields were the standard model for sequence labelling tasks. CRFs take as their
input a set of features for each token in a sentence, and learn to predict an optimal sequence
of labels for the full sentence. When you develop a CRF, a lot of time and effort typically goes into finding those
feature functions that help the model most at predicting the likelihood of every label for a given word.
Several software libraries exist for developing CRFs: <a href="http://mallet.cs.umass.edu/">Mallet</a>, 
<a href="http://www.chokkan.org/software/crfsuite/">CRFSuite</a> and <a href="https://taku910.github.io/crfpp/">CRF++</a> are some
of the most popular ones.
For this article, I used CRF++ to train a vanilla CRF with some common features for Named Entity Recognition: the token itself, its bigram and trigram prefix and suffix, its part of 
speech, its chunk type, several pieces of information about the word’s form (Does it start with a capital? Is it uppercase? Is it a digit?),
and a subset of those features for the surrounding context words. Out of the box, this baseline CRF obtains an F1-score of 81.15% on the b test set
of the CONLL-2003 data. 
This puts it in thirteenth place in the original CONLL ranking from 2003. That’s not great, but given
how quickly we can get this result, it’s not too shabby either. 
</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/jrx7a906e9ysyek/Screenshot%202017-09-11%2014.46.47.png?raw=1" />
<figcaption>The relationship between CRFs, HMMs, Logistic Regression and Naive Bayes. From Sutton & McCallum’s <a href="http://www.research.ed.ac.uk/portal/files/10482724/crftut_fnt.pdf">An Introduction to Conditional Random Fields</a>.</figcaption>
</figure>

<p>
How can we improve this performance? One of the most well-known weaknesses of the previous generation of NLP models is that 
they are unable to model semantic similarity between two words. For all our CRF knows,
<em>Paris</em> and <em>London</em> differ as much in meaning as <em>Paris</em> and <em>cat</em>. To overcome this problem, many CRFs for 
Named Entity Recognition rely on gazetteers &mdash; lists with names of people, locations and organizations that are known in advance. 
This is a suboptimal solution, however: gazetteers are inherently limited and can be costly to develop. Also, they’re usually only available for the 
named entities themselves, so that the semantic similarity between other words in their context goes unnoticed. This is 
where it can pay off to take the first step on the path to deep learning.</p>
 
<p>
As I’ve <a href="http://nlp.yvespeirsman.be/blog/anything2vec/">written before</a>, word embeddings are 
one of the main drivers behind the success of deep learning in NLP. The first layer in a neural network
for text processing is mostly an embedding layer that maps one-hot word vectors to their dense 
embeddings. Still, this does not mean that the distributional information in word embeddings can only be leveraged 
in neural networks &mdash; it’s also possible to feed this information to a CRF. One popular way of doing this is to 
cluster a set of word embeddings by distributional similarity, and provide the CRF with the cluster IDs of a token and its context words. 
Following the recommendations of <a href="https://arxiv.org/pdf/1705.01265.pdf">Balikas et al. (2017)</a>, I 
trained a set of 64-dimensional word embeddings on the English Wikipedia for all words that
appear at least 1000 times in that corpus. I then used K-means to group these embeddings in 500 clusters.
While it’s easy enough to write the code for this with the help of libraries such as <a href="https://radimrehurek.com/gensim/">Gensim</a>
or <a href="http://scikit-learn.org/">Scikit-learn</a>, <a href="https://github.com/dav/word2vec">Google’s original word2vec code</a> actually has
a command that lets us perform all the necessary steps in one go. You can find it in the bash script <code>demo-word.sh</code>.
<p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/umrhocphinsh518/Screenshot%202017-09-11%2015.04.12.png?raw=1">
<figcaption>Word embeddings contain useful information for Named Entity Recognition. From <a href="https://colah.github.io/posts/2015-01-Visualizing-Representations/">Christopher Olah's blog</a>.</figcaption>
</figure>

<p>
The clusters we obtain are a treasure trove for Named Entity Recognition.
For example, cluster 437 contains many location names, such as <em>München</em>, <em>Paris</em>
and <em>Brussels</em>. The presence of a target word in this cluster clearly increases the 
probability that it refers to a location. Similarly, cluster 254 contains verbs such as 
<em>say</em>, <em>think</em> or <em>pray</em>. When a target word precedes one of the words
from this cluster, this may indicate that it refers to a person.
This intuition is backed up by the results of my simple experiment: when we add the cluster
IDs of these 500 clusters as features to our simple CRF, its F1-score on the named
entities increases from 81.15% to 84.86%. That puts it somewhere in the middle of the 2003 CONLL 
rankings. Adding additional features, such as
gazetteers, or informative feature conjuctions, is certain to take up the F1-score
further still.</p>

<p>
Useful as these embedding clusters may be, there is of course much more to neural networks than shallow word embedding
models. Let’s therefore leave our CRF behind and build a completely neural model to see how much improvement deep learning can bring.
As so often in NLP, our model starts with a simple <a href="http://colah.github.io/posts/2015-08-Understanding-LSTMs/">LSTM</a>. 
LSTMs are a type of <a href="http://karpathy.github.io/2015/05/21/rnn-effectiveness/">Recurrent Neural Network</a>, which means that they are 
ideal for dealing with sequential data. The great thing about LSTMs is that they are able to learn long-term dependencies, as
their structure allows the network to pass or block information from one time step to the other.
Thanks to software libraries such as <a href="https://www.tensorflow.org/">Tensorflow</a>,
building an LSTM has become pretty straightforward. First we need a so-called embedding layer, which maps the input words to dense word embeddings.
Then we add the LSTM layer that reads these word embeddings and outputs a new vector at every step. Finally, a dense layer maps these output
vectors to a n-dimensional vector of logits, where <em>n</em> is the number of output labels, and a softmax function turns the logits
into label probabilities. This vanilla neural network is a great introduction to deep learning, but unfortunately it is far from effective. Where our CRF above easily obtained 
F1-scores of above 80%, you’ll be hard pressed to get above 70% with this simple LSTM.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/o6d7uncab5s5aec/Screenshot%202017-09-12%2011.16.36.png?raw=1">
<figcaption>A simple LSTM-based neural network for sequence labelling.</figcaption>
</figure>

<p>
The most glaring problem is that our standard LSTM only has access to the left context of a word when it assigns a label.
Our CRF, by contrast, took both left and right context into account. Luckily there’s a simple solution to this problem:
bidirectional LSTMs. BiLSTMs process the data with two separate LSTM layers. One of them reads the 
sentence from left to right, while the other one receives a reversed copy of the sentence to read it from
right to left. Both layers produce an output vector for every single word, and these two output vectors are concatenated
to a single vector that now models both the right and the left context. Replacing the simple LSTM layer in
our model by a biLSTM has an enormous impact on its performance. With a few standard settings &mdash; a dimensionality of 300 for the
word embeddings and the LSTM, a dropout of 50% after the embedding and the LSTM layer, a batch size of 32, optimization with Adam,
 an initial learning rate of 0.001 and learning rate decay of 95% &mdash; 
 its F1-score goes up from 64.9% to 76.1%. This is a
big leap forward, but we’re still far below the performance of our CRF.
</p>

<p>
Apart from the surrounding context, there’s another area where our current neural network is at a disadvantage to
our earlier model. Whereas the word embeddings that we clustered for our CRF were trained on all of
Wikipedia, the CoNLL training data contains only around 200,000 words. This is not nearly enough to train reliable
word embeddings. To combat this data sparsity, it is common to initialize the embedding layer in a neural network with word embeddings
that were trained on another, much
larger corpus. In our experiments, we’ll use the 300-dimensional <a href="https://nlp.stanford.edu/projects/glove/">GloVe</a>
vectors. Relying on these pre-trained embeddings rather than letting our network learn its own embeddings
finally makes it competitive with our earlier CRF: we now obtain an F1-score of 85.9%.
</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/qf1hislhi8lt0d1/Screenshot%202017-09-11%2014.39.41.png?raw=1">
<figcaption>The Tensorboard graph for our final model.</figcaption>
</figure>

<p>
Despite this leap in performance, our current neural network is still handicapped compared to our CRF.
Remember that we informed the CRF about the prefixes and suffixes of the tokens in the sentence. Many entities
share informative suffixes, such as the frequent <em>-land</em> or <em>-stan</em> for country names. Because 
it treats all tokens as atomic entities, our neural network currently has no access to this type of information.
This blindness has given rise to the recent popularity of character-based models in NLP. As these models look
at the individual characters of a word, they can collect important information present in the character sequences. Character-based networks tend 
to take one of two forms: Recurrent Neural Networks such as LSTMs or GRUs, or 
<a href="http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/">Convolutional Neural Networks</a>. 
The former build a vector representation for every token by reading it character by character and “remembering”
important information. The latter have a number of filters that treat tokens as chunks of character n-grams, and
build a character-based representation by combining the outputs of these filters in one vector.
These character-based vectors can then be concatenated to our earlier word embeddings to obtain two complementary views of the tokens. 
It is these concatenated vectors that are now fed to the BiLSTM we built earlier. In our
experiments, both methods achieved fairly similar results, but the character-based biLSTM edged out the CNN by a small
margin. As a result of this new ability to draw information from the inner structure of a word,
our neural network now outperforms the CRF quite clearly: its F1-score goes up to 89.3%. This means we’ve left the 2003
CONLL rankings behind us.</p>

<p>
But who can stop when an F1-score of 90% is so tantalizingly close? A final disadvantage of the current method that we’ll discuss here, is that it predicts 
all labels independently of each other: as it labels a word in a sentence, it does not 
take into account the labels it predicts for the surrounding words. This
can be useful information, however: a word is much more likely to carry the label I-PER when 
the previous word had the label B-PER, for example. As <a href="https://arxiv.org/pdf/1508.01991.pdf">Huang et al. (2016)</a> suggest,
we can solve this problem by adding a Conditional Random Field layer to our network. Like
the CRF model we started out with, this CRF layer 
outputs a matrix of transition scores between two states, and dynamic programming
can help us find the optimal tag sequence for the sentence. This final addition leaves us with an F1-score
of 90.3%, about 1% below the current state of the art for this task, and a general feeling of satisfaction .
If you'd like to experiment around
with this final model yourself, I recommend taking a look at the code by <a href="https://github.com/UKPLab/emnlp2017-bilstm-cnn-crf">
the Ubiquitous Knowledge Processing Lab</a> or <a href="https://github.com/guillaumegenthial/sequence_tagging">Guillaume Genthial</a>, who
also has an excellent 
<a href="https://guillaumegenthial.github.io/sequence-tagging-with-tensorflow.html">blog post</a> on the subject.
</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/qht64zx5fyi1mun/Screenshot%202017-09-24%2023.29.44.png?raw=1">
<figcaption>As our models gradually become more complex, their F1-score also goes up.</figcaption>
</figure>

<p>
Countless experiments have shown that neural networks tend to outperform 
CRFs for the task of Named Entity Recognition on a variety of data. Still, you shouldn’t expect 
this improvement to come without effort. As you
move from a CRF or similar model to a deep learning-based solution, the time you used to spend devising good features
will now typically go to designing an effective network and tuning its many 
parameters. Before you take the plunge, it can therefore be worthwhile to investigate whether
word embeddings can improve your CRF. In the end, however, the investment in building a deep learning
approach is likely to pay off, as you’ll have a superior model that can also be applied more easily
to related tasks.
</p>

