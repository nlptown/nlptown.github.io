---
layout: post
title:  Off-the-shelf methods for sentiment analysis
date:   2016-11-23 14:00:00
tags: NLP api software sentiment analysis natural language processing
comments: true
author: Yves Peirsman
---

<p class="first">
Sentiment analysis is one of the most popular applications of Natural Language Processing. Many companies run software that automatically classifies a text as positive, negative or neutral to monitor how their products are received online. 
Other people use sentiment analysis to conduct political analyses: they <a href="http://tarsier.monkeylearn.com/">track the average sentiment in tweets that mention the US presidential candidates</a>, or show that <a href="http://varianceexplained.org/r/trump-tweets/">Donald Trump’s tweets are much more negative than those posted by his staff</a>. There are even companies that <a href="http://www.stockfluence.com/">rely on sentiment analysis to try and predict the stock markets</a>. But how good is sentiment analysis exactly? And what approach should you take when you’re faced with a sentiment analysis task yourself?</p>

<p>
In line with the rising popularity of machine learning and artificial intelligence in recent years, the landscape of NLP software has grown increasingly complex. On the one hand, there are cloud APIs that offer off-the-shelf models for many common NLP tasks, such as sentiment analysis, named entity recognition, keyword extraction, etc. Some good examples are <a href="https://indico.io/">Indico</a> and <a href="https://cloud.google.com/natural-language/">Google’s Natural Language API</a>. On the other hand, there are software libraries for machine learning that allow you to build your own custom model, such as <a href="http://scikit-learn.org/">Scikit-learn</a> for Python. In between those two options, you have cloud APIs that allow you to train your own model (<a href="http://monkeylearn.com/">Monkeylearn</a>, for example), and there are software libraries that ship with pre-trained models for some popular tasks (<a href="http://textblob.readthedocs.io/en/dev/">Textblob</a>, or <a href="http://stanfordnlp.github.io/CoreNLP/">Stanford CoreNLP</a>). In this first blog post of two I’ll evaluate a wide range of available off-the-shelf methods for sentiment analysis. I’d like to find out if we can just take an existing API, apply it to a data set, and trust its results. In other words: is it OK to be lazy?
</p>

<img class="img-fluid padded" src="https://www.dropbox.com/s/rgg7gesfc7iorci/Screenshot%202016-11-17%2022.09.08.png?raw=1">

<p>
This is the full list of systems I tested:<sup><a href="#fn1" id="ref1">1</a></sup> 
<ul class="nomargin">
    <li>The <a href="http://textblob.readthedocs.io/en/dev/advanced_usage.html#sentiment-analyzers">Naive Bayes</a> and <a href="http://textblob.readthedocs.io/en/dev/advanced_usage.html#sentiment-analyzers">Pattern-based</a> sentiment analyzers in <a href="https://textblob.readthedocs.io/en/dev/">TextBlob</a></li>
    <li>The <a href="http://nlp.stanford.edu/sentiment/">neural sentiment classifier</a> in <a href="http://stanfordnlp.github.io/CoreNLP/">Stanford CoreNLP</a></li>
    <li>The sentiment API at <a href="http://sentiment.vivekn.com">sentiment.vivekn.com</a></li>
    <li>The <a href="https://cloud.google.com/natural-language/docs/sentiment-tutorial">sentiment analyzer</a> of the <a href="https://cloud.google.com/natural-language/">Google Cloud Natural Language API</a></li>
    <li>The <a href="https://developer.aylien.com/text-api-demo?text=&language=en&tab=sentiment&mode=document">document-level sentiment analysis</a> from <a href="http://aylien.com/">Aylien</a></li>
    <li>The <a href="https://indico.io/product">Sentiment Analysis Text Api</a> from <a href="https://indico.io/">Indico</a></li>
    <li>The Sentiment Analysis API from <a href="http://sentigem.com">Sentigem</a></li>
    <li>The <a href="https://developer.rosette.com/features-and-functions#introduction">Sentiment endpoint</a> of <a href="https://developer.rosette.com/">Rosette API</a></li>
    <li>The <a href="https://dev.havenondemand.com/apis/analyzesentiment#overview">Sentiment Analysis API</a> from <a href="https://www.havenondemand.com/">Haven OnDemand</a></li>
    <li>The <a href="http://text-processing.com/docs/sentiment.html">Sentiment Analysis API</a> from <a href="http://text-processing.com/">text-processing.com</a></li>
    <li>The pre-trained hotel, restaurant and product sentiment classifiers from <a href="http://monkeylearn.com/">MonkeyLearn</a></li>
</ul>
</p>


<p>It’s a well-known fact that the performance of an NLP model depends on the similarity between its training data and the data you’re testing it on. To get an idea of this variation in performance, I evaluated the available models on user reviews from four very different domains: movies, baby products, Android apps and tourism. In each of these domains, I compared the performance of the model with a <em>baseline</em> that always labels a text as positive. Obviously, we’d like to do better than that baseline.</p> 

<img class="img-fluid padded" src="https://www.dropbox.com/s/6kj5kd9vwwjm9qj/Screenshot 2016-11-17 21.51.47.png?raw=1">

<p>Three examples will show you what the test data looks like. The following texts should get the labels positive, neutral and negative, respectively: 
<ul class="nomargin">
  <li><em>If you’re looking for something scary, this is the first great horror film of the spooky season.</em></li>
  <li><em>Avernum is a series of demoware role-playing video games by Jeff Vogel of Spiderweb Software available for Macintosh and Windows-based computers. Several are available for iPad and Android tablet.</em></li>
  <li><em>It’s Starbucks only with bad customer service. Baristas with attitude that don’t know their own product. If I'm paying $7.00 for a coffee at least drop the ‘tude.</em></li>
</ul>
</p>

<p>
There’s no more classic application of sentiment analysis than movie reviews. In order to make sure none of the models was trained on our data, I collected a balanced set of 500 positive and 500 negative sentences from <a href="https://www.rottentomatoes.com/">www.rottentomatoes.com</a>. As its baseline of 50% is pretty low, it’s no surprise that most of the pre-trained models outperform this threshold. Still, there’s a huge difference in performance between the best model (76.8%) and the worst (45.6%). The best three off-the-shelf systems are Indico (76.8%), IBM AlchemyAPI (73.4%) and Stanford CoreNLP (71.9%). I won’t name and shame the worst ones, since their low performance may say more about the appropriateness of their models for this data set than about their inherent quality.

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=567243840&format=interactive"></iframe>
</p>

<p>
When we move to baby product reviews, the picture becomes much less positive. This 
time only two off-the-shelf models beat the baseline comfortably: Indico (92.6%) and the pre-trained product model in Monkeylearn (87.5%). Textblob’s Pattern-based classifier comes in third, with 82.5% accuracy. Granted, the baseline of 81.5% positive reviews for this data set is much higher than before, but these disappointing figures put the performance of the off-the-shelf methods in perspective.

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=1616059214&format=interactive"></iframe>
</p>

<p>
Clearly, the pre-trained models are more at home with the hotel and restaurant reviews from Yelp: most systems beat the baseline on this data set. The best API is again Indico (92.9%), followed closely by Google’s Natural Language API (91.0%), and IBM’s AlchemyAPI (90.4%). Monkeylearn’s pre-trained restaurant and hotel classifiers are in fourth and fifth position, which highlights the importance of choosing the right training data for your application.

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=1047884806&format=interactive"></iframe>
</p>

<p>Finally, the Android apps do not bring any big surprises. When we look at the data set with only positive and negative examples, three models clearly outperform the baseline: Indico (90.6%), Google (90.5%) and Monkeylearn’s pre-trained product model (87.1%). When we replace 300 random texts by a neutral paragraph from Wikipedia that contains the word Android, and test how well the systems recognize neutral examples, the top three changes: it’s now Indico (80.0%), Haven On Demand (79.1%) and Google (77.8%).
<sup><a href="#fn2" id="ref2">2</a></sup>

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=722567687&format=interactive"></iframe>

<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/1c7x9AbxqYblyerp5vC7tMsdzTvDyYbNIJ5h3Oduuduc/pubchart?oid=844180096&format=interactive"></iframe>
</p>

<p>In short, the results of our experiments show enormous variation <em>between</em> and <em>within</em> the available off-the-shelf solutions for sentiment analysis. There is huge variation <em>between</em> the models: some of them perform really well, while others tend to struggle. The names that most often returned among the best systems are Indico, which came out top for all five data sets, and Google and IBM, the big players in this field. In addition, we see considerable variation <em>within</em> the models: some of them do very well on one data set, but poorly on another. High quality is possible, but it is far from guaranteed.</p>

<p>
It’s definitely not OK to be lazy when you need to attack your own sentiment analysis task. To make sure you’re getting the best results possible, it’s crucial you evaluate a variety of models on your specific data.
Given our mixed results, and the effort of comparing the available models, you’d be right to ask yourself whether you’d be better off ignoring the available off-the-shelf solutions, and building your own custom model. That’s a question I’ll answer in my next blog post.
</p>
<hr>

<sup id="fn1">1. Some of these APIs offer more than one model for sentiment analysis, for example one in the form of a classifier with discrete labels (positive, negative, neutral) and another in the form of a regression model with continuous sentiment values.<a href="#ref1" title="Jump back to footnote 1 in the text.">↩</a></sup>

<sup id="fn2">2. Note that I tuned the models with continuous variables on 500 different examples, in order to determine the most appropriate cut-off between negative, neutral and positive values.<a href="#ref2" title="Jump back to footnote 2 in the text.">↩</a></sup>
