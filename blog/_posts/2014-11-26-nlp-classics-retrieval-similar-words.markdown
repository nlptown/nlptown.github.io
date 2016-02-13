---
layout: post
title:  "NLP Classics (1): Automatic Retrieval and Clustering of Similar Words"
date:   2014-11-29 9:00:00
tags: NLP semantics meaning words
comments: true
---

<p class="first">In this series of blog posts, I present some of the most influential papers from the recent history of Natural Language Processing. First up is Dekang Lin's <a href="https://webdocs.cs.ualberta.ca/~lindek/papers/acl98.pdf">Automatic Retrieval and Clustering of Similar Words</a>, published in 1998. Lin’s paper, which has been cited more than 1500 times according to Google, explains how computers can automatically find words with a similar meaning. This similarity can be exploited by search engines like Google, for example, in order to find more relevant web pages for a given search term.</p>

<p class="nomargin">Last month I learnt a new word: <i>echidna</i>. Unless you know Greek, the word itself gives you no clue to its meaning. For all you know, its meaning might be similar to <i>influenza</i>, <i>geyser</i>, <i>hedgehog</i> or <i>oak</i>. Now suppose I gave you these four sentences, which describe my first encounter with an echidna:</p>

<blockquote>
I looked up at the sound, and suddenly I saw an echidna next to the trail. <br />
The echidna was eating ants with its long nose. <br />
I tried to take a picture, but the echidna hid in the bushes. <br />
Eventually the echidna crawled out and looked me in the eye. <br />
</blockquote>

<p>Although none of these sentences defines <i>echidna</i>, together they give you a vague idea of what the word means: it must be some kind of small, ant-eating animal with a long nose. Confronted with the list of words above, you can easily pick the most similar one: it must be <i>hedgehog</i>. Now imagine having hundreds of sentences like these. Your understanding of <i>echidna</i> would become very precise indeed.</p>

<p>Just like you did, computers guess the similarity in meaning between two words on the basis of its use in example sentences. One of their strategies is described in Lin’s classic paper <a href="https://webdocs.cs.ualberta.ca/~lindek/papers/acl98.pdf">Automatic Retrieval and Clustering of Similar Words</a>. Its central idea is to exploit the syntactic behaviour of the words in a text to determine their similarity in meaning. For example, in the sentences above, <i>echidna</i> occurs as the subject of <i>eat</i>, <i>hide</i> and <i>crawl</i>, and as the object of <i>see</i>. This information can be automatically extracted by a so-called <i>parser</i>, a piece of software that uncovers the syntactic structure of a sentence. The general idea behind Lin’s approach is that the more often two words occur in the same syntactic relationship (for example, as the subject of <i>eat</i>), the more similar their meanings must be. </p>

<p class="nomargin">Lin tested this hypothesis on a large collection of newspaper articles, totalling 64 million words. For all nouns, verbs and adjectives/adverbs in that collection, he found the words from the same word class with the most similar syntactic behaviour. This proved to be a very reliable technique of extracting words with a similar meaning. Here are some of the word pairs that Lin identified:
</p>

<blockquote>
Nouns: earnings-profit, plan-proposal, employee-worker, battle-fight, actor-actress, oil-petroleum, gallery-museum, pub-tavern, waiter-waitress, … <br />
Verbs: fall-rise, injure-kill, concern-worry, convict-sentence, limit-restrict, narrow-widen, hit-strike, … <br />
Adjectives/Adverbs: high-low, bad-good, extremely-very, alleged-suspected, stormy-turbulent, communist-leftist, sad-tragic, enormously-tremendously, … <br />
</blockquote>

<p>Although the words in these pairs tend to have a very similar meaning, they are not always substitutable. In fact, many word pairs contain polar opposites, such as <i>fall-rise</i>, <i>narrow-widen</i>, <i>high-low</i>, <i>bad-good</i>, etc. Other words differ in one specific semantic dimension, such as gender (<i>actor-actress</i>, <i>waiter-waitress</i>). Similarity in meaning can take on many forms. </p>

<p>In general, however, Lin’s results indicate that the syntactic similarity between two words gives a pretty good idea of their semantic similarity. It’s no surprise that Dekang Lin works at Google these days: search engines often use strategies like the one he developed to expand people’s queries. For example, if you Google <i>lawyers in Belgium</i>, Google also looks for websites with the word <i>attorney</i>, in order to give you more relevant hits. Lin’s technique offers one way to do this automatically. </p> <br />

<p>P.S.: By the way, if you still want to know what an echidna looks like, here’s my picture:</p>

<img class="centered" src="https://movingdots.files.wordpress.com/2014/11/dsc00479a.jpg" height="300" width="450" />

<p class="noindent">However close your guess was, I bet you didn’t expect it to be so cute.</p>








