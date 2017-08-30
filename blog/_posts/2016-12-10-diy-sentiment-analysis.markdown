---
layout: post
title:  DIY methods for sentiment analysis
date:   2016-12-10 12:00:00
tags: NLP api software sentiment analysis
comments: true
author: Yves Peirsman
---

<p class="first">
In <a href="http://nlp.yvespeirsman.be/blog/off-the-shelf-sentiment-analysis/">my previous blog post</a>, I explored the wide variety of off-the-shelf solutions that are available for sentiment analysis. The variation in accuracy, both within and between models, led to the question whether you’re better off building your own model instead of trusting a pre-trained solution. I’ll address that dilemma in this blog post.
</p>

<p>Training your own sentiment analysis model obviously brings with it its own challenges. First, 
you need a large number of relevant texts that have been labelled with one of the target categories: positive, negative or neutral. If you don’t have such a labelled training set, there may be quick and cheap ways of collecting one &mdash; by scraping it from an online source or crowdsourcing annotations from sites such as Amazon’s <a href="https://www.mturk.com/mturk/welcome">Mechanical Turk</a>. Second, you need to pick a machine learning library to implement your solution. Most generic machine learning libraries usually fit the bill, unless you’re aiming to train a non-standard model. I’m a big fan of <a href="http://scikit-learn.org/">Scikit-learn</a>, which I’ve used for all my experiments here. Third, you need to pick the model that is most appropriate for your task and tune its parameter settings. I’ll evaluate two popular models for sentiment analysis, a Naive Bayes Classifier and a Support Vector Machine. Like in <a href="http://nlp.yvespeirsman.be/blog/simple-text-classification/">my blog post about text classification</a>, I’ll use both unigrams and bigrams as features, and set the maximum number of features to 10,000. I’ll evaluate the models on the data sets from <a href="http://nlp.yvespeirsman.be/blog/off-the-shelf-sentiment-analysis/">my previous blog post</a> for which I have sufficient training data: Amazon reviews about baby products, Amazon reviews about Android apps, and Yelp reviews about hotels, restaurants and other tourist attractions. 
</p>

<p>
The results on the set of baby product reviews show a mixed picture. The newly trained SVM (the green line in the graph below) beats the best off-the-shelf model, but only by 0.8%. The Naive Bayes Classifier (the orange line) gives comparable performance to the second-best off-the-shelf model, around 6% behind the best one.  

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=1783830508&format=interactive"></iframe>
</p>

<p>
The results for the Android apps are a bit more outspoken. Our new SVM again outperforms the best off-the-shelf model, and this time it does so by a healthy margin of 2.5%. The Naive Bayes model scores 90.3%, similar to the best two off-the-shelf APIs.

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=1100154712&format=interactive"></iframe>
</p>

<p>
The results on the Yelp reviews confirm these trends. At 95.6%, the SVM performs 2.7% better than Indico; at 89.3%, the Naive Bayes Classifier is outshone by the top three off-the-shelf solutions.

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=1397300606&format=interactive"></iframe>
</p>

<p>
These figures look pretty convincing: if you have your own data, the right model will typically outperform even the best off-the-shelf method. However, to put this conclusion into perspective, we need to determine how much labelled data we need to achieve these results. It turns out we need a considerable amount. The blue line in the figures below shows the average accuracy across nine training runs as the training set grows. Obviously, our model gets better as we give it more and more labelled data to learn from. On the Android app reviews, the SVM catches up with the best off-the-shelf method at around 20,000 examples. On the baby product reviews, however, the SVM only starts beating the best off-the-shelf method after it has seen around 80,000 labelled examples. In many applications, that amount of data might be really hard to come by. Whether or not the increased accuracy compared to pre-trained solutions is worth the effort will depend on your specific challenge and needs.

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=223762019&format=interactive"></iframe>

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=1382234680&format=interactive"></iframe>
</p>

<p>In conclusion, there’s often no easy way to choose between an off-the-shelf solution or a custom-built model for your particular NLP task. Off-the-shelf solutions require no data and little effort. They can give you good quality, but this is far from guaranteed. Moreover, you have no control over the model: you cannot improve it through time, or tweak its parameters to perform better on your data. Custom-built models require considerably more data and effort, but if you have those resources available, they will typically give you a superior model over which you have full control. It’s important to remember that no model is perfect. In fact, in George Box’s words, “all models are wrong, but some are useful”. You just have to figure out which ones are which.
</p>
