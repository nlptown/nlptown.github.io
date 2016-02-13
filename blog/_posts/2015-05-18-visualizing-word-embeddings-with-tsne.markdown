---
layout: post
title:  "Visualizing Word Embeddings with t-SNE"
date:   2015-05-18 22:00:00
tags: NLP word embeddings visualization t-SNE GloVe
comments: true
---

<p class="first">When you work with high-dimensional datasets such as the word embeddings produced by <a href="https://code.google.com/p/word2vec/">word2vec</a> or <a href="http://nlp.stanford.edu/projects/glove/">GloVe</a>, it’s important to get a good idea of what the data look like. One option is to create a visualization of the dataset in two dimensions, and simply eyeball the clusters of data points that appear. However, creating a useful low-dimensional visualization is a non-trivial matter. The best solution I’ve found so far is <a href="http://lvdmaaten.github.io/tsne/">t-SNE</a>.</p> 

<p>t-SNE was developed by Laurens van der Maaten at the University of Delft. It won Maarten the <a href="https://www.kaggle.com/c/MerckActivity/prospector#186">Merck Visualization Challenge on Kaggle</a>, and has been applied to images, word embeddings, movies, and similar high-dimensional datasets. Maarten’s website contains links to implementations of the technique in a variety of programming languages, in addition to the research papers that go into the gory details of the underlying maths.</p>

<p>I tested out t-SNE by visualizing the 300-dimensional word embeddings produced by GloVe, a deep learning method developed at Stanford University. Word embeddings model the contexts in which words appear, and are often used as a way of measuring the semantic similarity between two words. You can download a large number of example word embeddings directly from the <a href="http://nlp.stanford.edu/projects/glove/">GloVe website</a>. As you’ll see, t-SNE really makes it easy to visualize these 300-dimensional embeddings in two dimensions, and to get a rough idea of their quality.</p>

<p>Let’s go through the process step by step. First we read in the word embeddings and their labels from two text files. The embeddings file should contain one vector per line, with the values for all dimensions separated by tabs. The label file should list the labels for these vectors &mdash; one label per line, in the same order as the vectors in the embeddings file.</p>

<div class="highlight"><pre><code>import numpy as Math
import pylab as Plot

glove_matrix = Math.loadtxt("glove.6B.300d.embeddings.txt");
glove_words = [line.strip() for line in open("glove.6B.300d.labels.txt")]
</code></pre></div>

<p>Obviously, visualizing all 400,000 words in the GloVe download would give a plot that is rather hard to read.  Instead we will focus on the <a href="http://www.rupert.id.au/resources/1000-words.php">2,000 most frequent words in English</a>, according to the <a href="http://www.wordfrequency.info/">American National Corpus</a>. This means we need to select from the GloVe matrix the rows that correspond to these words. We do this by compiling a list of all the row indices for the target words and using that list to assemble our target matrix:</p>

<div class="highlight"><pre><code>target_words = [line.strip().lower() for line in open("4000-most-common-english-words-csv.csv")][:2000]

rows = [glove_words.index(word) for word in target_words if word in glove_words]
target_matrix = glove_matrix[rows,:]
</code></pre></div>

<p>One line of code then puts t-SNE to work. The first argument to the function is the matrix with our word embeddings, the second is the number of dimensions we want to reduce the matrix to:</p>

<div class="highlight"><pre><code>reduced_matrix = tsne(target_matrix, 2);
</code></pre></div>

<p>Finally, we use a scatter plot to visualize the results of the dimensionality reduction:</p>

<div class="highlight"><pre><code>Plot.figure(figsize=(200, 200), dpi=100)
max_x = Math.amax(reduced_matrix, axis=0)[0]
max_y = Math.amax(reduced_matrix, axis=0)[1]
Plot.xlim((-max_x,max_x))
Plot.ylim((-max_y,max_y))

Plot.scatter(reduced_matrix[:, 0], reduced_matrix[:, 1], 20);

for row_id in range(0, len(rows)):
    target_word = glove_words[rows[row_id]]
    x = reduced_matrix[row_id, 0]
    y = reduced_matrix[row_id, 1]
    Plot.annotate(target_word, (x,y))

Plot.savefig("glove_2000.png");
</code></pre></div>

<p>A quick inspection immediately shows the quality of both the GloVe word embeddings and the t-SNE visualization. For example, in the two-dimensional plot all frequent numbers are grouped together. “One” is likely missing because its contextual distribution is so different from the other numbers &mdash; it fulfils more linguistic functions than just counting.</p>

<img src="/images/glove-word-embeddings-numbers.png" class="centered" width="300"><br />

<p>Similarly, there is a separate group of words related to education:</p> 

<img src="/images/glove-word-embeddings-education.png" class="centered" width="500"><br />

<p>And here’s another one about crime and punishment:</p>

<img src="/images/glove-word-embeddings-crime-and-punishment.png" class="centered" width="500"><br />

<p>If you take a look at <a href="/images/glove-word-embeddings.png" class="centered">the complete figure</a>, you’ll find similar clusters for body parts, family members, media, food, politics, sports, and many other concepts.</p>

<p>This simple exercise shows t-SNE is a great tool for visualizing word embeddings. It’s easy to use, produces high-quality visualizations, and will certainly contribute to our understanding of high-dimensional datasets.</p>







