---
layout: post
title:  Understanding Deep Learning Models in NLP
date:   2017-01-26 12:00:00
tags: NLP AI deep learning understanding decisions
comments: true
author: Yves Peirsman
image: understanding-dl.png
---

<p class="first">
Deep learning methods have taken Artificial Intelligence by storm. As their dominance grows, one inconvenient truth
 is slowly emerging: we don’t actually understand these complex models very well. Our lack of understanding leads to some uncomfortable questions.
Do we want to travel in self-driving cars whose inner workings no one really comprehends? Can we base important decisions
in business or healthcare on models whose reasoning we don’t grasp? It’s problematic, to say the least.</p>

<p>In line with the general evolutions in AI, applications of deep learning in Natural Language Processing suffer from this lack of understanding as well. 
It already starts with the word embeddings that often form the first hidden layer of the
network: we typically don’t know what their dimensions stand for. The more hidden layers a model has, the more serious this problem becomes. How do neural networks combine the meanings of
single words? How do they bring together information from different parts of a sentence or text? 
And how do they use all these various sources of information to arrive at their final decision? The complex network
architectures often leave us groping in the dark. Luckily more and more researchers are starting to address exactly these questions,
and a number of recent articles have put forward methods
for improving our understanding of the decisions deep learning models make.</p>
 
<p>Of course, neural networks aren’t the only machine learning models that are difficult to comprehend. Explaining a complex
decision by a Support Vector Machine (SVM), for example, is not exactly child’s play. For precisely this reason, 
<a href="https://arxiv.org/abs/1602.04938">Riberi, Singh and Guestrin</a> have developed <a href="https://github.com/marcotcr/lime">LIME</a>,
an explanatory technique that can be applied to a variety of machine learning methods. As it explains how a classifier has come
to a given decision, LIME aims to combine interpretability
with local fidelity. Its explanations are interpretable because they account for the model’s decision with a small number of single words that
have influenced it. They are locally faithful because they correspond well to how the model behaves in the vicinity
of the instance under investigation. This is achieved by basing the explanation on sampled instances that are weighted by their similarity to the target example.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/x1seyfxawrta7id/Screenshot%202017-01-24%2023.27.46.png?raw=1">
<figcaption>Ribeiro et al. use a small set of words to explain a classifier’s decision.</figcaption>
</figure>

<p>Ribeiro and colleagues show that LIME can help expose models whose high accuracy on a test set is due 
to peculiarities of the data rather than their ability to generalize well. This can happen, for example, when a text classifier bases
its decision on irrelevant words (such as <em>posting</em> or <em>re</em>) that happen to occur predominantly in one text class,
but that are not actually related to that class. Explanations like the one above can also help people discover noisy features,
and improve the model by removing them. LIME clearly demonstrates how a deeper understanding of the models can lead to a better
application of machine learning.</p>

<p>Although neural networks are often viewed as black boxes, <a href="https://arxiv.org/abs/1612.07843">Leila Arras and colleagues</a> argue
 that their decisions may actually be easier to interpret than those by some competing approaches. 
To underpin their argument, they apply a technique called layer-wise relevance propagation (LRP). LRP measures how much each individual word in a text
contributes to the decision of a neural network classifier by backward-propagating its output. Step by step, LRP allocates
relevance values for a specific class to all cells in the intermediate
layers. When it reaches the embedding layer, it pools the relevances over all dimensions to obtain word-level relevance scores.
</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/3kyr078p4s1j8bh/Screenshot%202017-01-22%2012.29.16.png?raw=1">
<figcaption width="500">Arras et al. show that a CNN classifier focuses on fewer, more meaningful words than an SVM.</figcaption>
</figure>

<p>
A comparison of these word-level relevance scores between a convolutional neural 
network (CNN) and a support vector machine (SVM) in a document classification task shows that the decisions of the CNN 
are based on fewer, more semantically meaningful
words. 
Thanks to its word embeddings, the neural network deals much better with words that are not present in the
training data, and it is less affected by the peculiarities of word frequency. Moreover, its relevance
scores can be used to compute document-level summary vectors that make much more semantic sense
than those obtained with other weighting schemes, such as tf-idf. In short, neural networks may not only have the quantitative,
but also the qualitative edge over their competitors.
</p>

<p>Like Arras et al, <a href="https://arxiv.org/abs/1506.01066">Li, Chen, Hovy and Jurafsky</a> try to demystify neural
networks by taking their cue from back-propagation. To find out which words in a sentence make the most significant contribution to the network’s choice
of class label, they look at the first-order derivatives of the loss function with respect to the word
embeddings of the words in the sentence. The higher the absolute values of this derivative, the more a word is responsible
for the decision. With their method, Li and colleagues show that the gating structure of Long Short-Term Memory (LSTM)
networks, and bidirectional LSTMs in particular, allows the network to focus better on the important keywords than standard Recurrent
Neural Networks (RNNs).
 In the sentence “I hate the movie”, 
all three models correctly recognize the negative sentiment mostly because of the word “hate”, but the LSTMs give much less
attention to the other words than the RNN. Other examples, however, indicate that the obtained first-order derivatives are
only a rough approximation of the individual contributions of the words, so they should be interpreted with caution.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/l0esdr7ouqdo0pz/Screenshot%202017-01-24%2022.18.01.png?raw=1">
<figcaption>Li et al. visualize the more focused attention of LSTM networks.</figcaption>
</figure>

<p>Luckily, there are more direct ways of measuring word salience. 
<a href="https://arxiv.org/abs/1602.08952">Kádár, Chrupała and Alishahi</a> explore an erasure technique for RNNs: they 
compare the activation vector produced by the neural network after reading a full sentence with the activation
vector for the sentence with the word removed. The more these two vectors differ, the more
importance the network assigns to the word in question. For example, in the phrase “a pizza sitting
next to a bowl of salad”, an RNN typically assigns a lot of importance to the words “pizza” and “salad”, but 
not to “a” or “of”.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/kr4y1g9euh43y58/Screenshot%202017-01-22%2011.44.41.png?raw=1">
<figcaption>Kádár et al. identify important words by erasing them from the input.</figcaption>
</figure>

<p>To demonstrate the full potential of this method, Kádár et al. apply it to one specific RNN architecture: 
the <a href="https://arxiv.org/abs/1506.03694">Imaginet</a> model they introduced in 2015. 
ImageNet consists of two separate Gated Recurrent Unit (GRU) pathways. 
One of these predicts image vectors given image descriptions; the other is a straightforward language
model. They show that the visual pathway focuses most of its attention on a small
number of elements (mostly nouns), while the textual pathway distributes its attention more equally across
all words and assigns more importance to purely grammatical words (such as prepositions).
The visual pathway focuses more on the first part of the sentence (where the central entity is usually
introduced), whereas the textual pathway is more sensitive to the end of the sentence. Some of these observations
confirm well-known intuitions, while others are more surprising.
</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/2b79rjo9klun4vy/Screenshot%202017-01-22%2011.45.05.png?raw=1">
<figcaption>Kádár et al. show how visual and textual tasks make neural networks focus on different parts of the sentence.</figcaption>
</figure>

<p><a href="https://arxiv.org/abs/1612.08220">Li, Monroe and Jurafsky</a> also
investigate the behaviour of a neural network by feeding two inputs to the model: first the original input, and then the input without
the word or dimension of interest. Instead of looking at the full activation vector, however, they focus on the 
final (log-)likelihoods that the model assigns to the correct label for the two inputs. The larger the difference
between the two output values, the more important the word or dimension is for that particular decision.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/ish18zzaqdhinys/Screenshot%202017-01-21%2020.43.00.png?raw=1">
<figcaption>Li et al. investigate the influence of individual word embedding dimensions on a variety of NLP tasks.</figcaption>
</figure>

<p>With this erasure technique, Li and colleagues discover that some dimensions in <a href="https://arxiv.org/pdf/1301.3781.pdf">word2vec</a> embeddings correlate
with specific NLP tasks, such as part-of-speech tagging and named entity recognition. <a href="http://www-nlp.stanford.edu/pubs/glove.pdf">Glove embeddings</a>, 
by contrast, have one dimension that dominates for almost all tasks they investigate. This dimension
likely contains information about word frequency. When dropout is applied in training, information gets spread out more equally
across the dimensions. The higher layers of the neural model, too, are characterized by more scattered
information, and the final decision of the network is more robust to changes at these levels.
</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/0szthtqa87rbaj9/Screenshot%202017-01-21%2020.43.27.png?raw=1">
<figcaption>Li et al. show that LSTM networks focus more on sentiment words than RNNs in sentiment analysis.</figcaption>
</figure>

<p>
Finally, an application of Li et al.’s technique to sentiment analysis confirms some of their earlier findings.
It again shows that LSTM-based models attach a higher
importance to sentiment words (such as <em>masterpiece</em> or <em>dreadful</em>) than standard RNNs. 
Moreover, it illustrates how data scientists can perform error analyses on their models by looking at the words with negative importance
scores for the correct class: the negative scores of these words indicate that their removal improves the decision of the model.</p>

<p>It’s clear neural networks don’t have to be black boxes. All the studies above show that with the right techniques, we can gain a better
understanding of how deep learning works, and what factors motivate its decisions. This blog post has only scratched
the surface of the many interpretative methods researchers are developing. I for one hope that 
in future applications their efforts will lead to model choices that are not only motivated by higher accuracies, but
also by a deeper grasp of what neural models actually do.</p>
