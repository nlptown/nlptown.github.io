---
layout: post
title: Why computers don’t yet read better than us
shorttitle: Why computers don’t yet read better than us
date:   2018-01-31 12:00:00
tags: NLP AI deep learning reading natural language processing
comments: true
author: Yves Peirsman
image: robot.png
---

<p class="first">
Artificial Intelligence is on a roll these days. It feels like the media report a new breakthrough every day. 
In 2017, <a href="https://newatlas.com/ai-2017-beating-humans-games/52741/">computer and board games</a> were at 
the center of public attention, but this year things look different. In the early days of 2018, both Microsoft 
and Alibaba claimed to have developed software that can read as well as humans do. Sensational headlines followed 
suit. CNN wrote that “<a href="http://money.cnn.com/2018/01/15/technology/reading-robot-alibaba-microsoft-stanford/index.html">Computers are getting better than humans at reading</a>”, while Newsweek feared “<a href="http://money.cnn.com/2018/01/15/technology/reading-robot-alibaba-microsoft-stanford/index.html">Computers can now read better than humans, putting millions 
of jobs at risk</a>”. Reality is much less spectacular, however.
</p>

## SQuAD

<p>Every AI challenge needs a fancy title. The catalyst behind the new breakthrough in Question Answering (QA) is 
SQuAD, the <a href="https://rajpurkar.github.io/SQuAD-explorer/">Stanford Question Answering Dataset</a>. SQuAD brings together over 100,000 questions and answers about hundreds of Wikipedia articles. For example, there is a series of questions about the Oil Crisis, such as <em>When did the 1973 oil crisis begin?</em> (October 1973) or <em>What was the price of oil in March of 1974?</em> (&dollar;12). The idea is that researchers use 80&#37; of these questions to train their QA models. During this training process, their software can learn how to find the answers in the texts and pick up the difference between various question words such as <em>where</em> and <em>when</em>. Afterwards, it is evaluated on the remaining 20&#37; of the questions it has never seen before.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/88iftblw9rwv5ja/Screenshot%202018-01-17%2014.04.55.png?raw=1" />
<figcaption>An example Wikipedia paragraph from SQuAD, with the corresponding questions and answers.</figcaption>
</figure>

<p>SQuAD has quickly become an influential dataset in Natural Language Processing, 
as it allows AI researchers to evaluate their software objectively and to compare their systems with one another. It’s no coincidence that since its release in 2016, NLP
has seen increasing interest in the development of QA systems. Still, we shouldn’t overestimate the progress that has been made. It is actually much easier for a piece of software to score well on the Wikipedia questions in SQuAD than you would think.</p>

<p>First of all, the SQuAD challenge is really not that difficult. Together with each question, participating
QA systems are presented with the Wikipedia paragraph that is guaranteed to
contain the answer. This simplifies the challenge considerably: instead of fully interpreting a paragraph, the task boils down
to identifying the words that most likely form the answer to the question. In many cases, this is pretty straightforward.
When the question starts with <em>where</em> and the paragraph contains only one location, for example, 
little can go wrong. Then there’s the fact that the participating QA systems don’t have to search Wikipedia for the relevant paragraphs. There are <a href="https://arxiv.org/pdf/1704.00051.pdf">systems that do this</a>, but they score much worse on the SQuAD test.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/sads42mxjiqsof8/Screenshot%202018-01-17%2022.11.07.png?raw=1" />
<figcaption>Right now, two QA systems share the first place on the SQuAD scoreboard.</figcaption>
</figure>

<p>Second, the human score on SQuAD &mdash; 82.3&#37; correct answers &mdash; is undoubtedly an underestimation of reality. 
The human answers were collected via <a href="https://www.mturk.com/">Amazon Mechanical Turk</a> &mdash; a site where people solve simple tasks against payment. 
Because they earn very little for these tasks, so-called “Turkers” often work quickly and sloppily. What’s more, 
most of their “mistakes” are not really incorrect answers. Usually these are cases where one Turker’s answer happens to contain 
one or two words more or fewer than those of his colleagues. If one person chooses <em>nearly &dollar;12</em> to answer the oil 
price question above, and everyone else selects <em>&dollar;12</em>, the first answer is considered incorrect. In 
contrast to an uninformed Turker, the competing QA 
software knows how Turkers typically select their answers: it has seen thousands 
of examples during its training process.</p>

## Question Answering

<p>Admittedly, even if the current QA systems do not outperform people, they still score an impressive 83&#37; on a reading test &mdash; a feat that initially may look like a proof of intelligence. However, we shouldn’t attribute too much intelligence to the software. Although there are of course individual differences, modern QA systems such as Microsoft’s and Alibaba’s are hardly able to really interpret a text. Instead they rely on pattern matching &mdash; very sophisticated pattern matching, but that’s all there is to it.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/gd7ebu1phpxufxi/Screenshot%202018-01-17%2014.10.50.png?raw=1" />
<figcaption>The Wikipedia paragraph about the oil crisis from SQuAD, with the answers from the Alibaba model.</figcaption>
</figure>

<p>As they look for the answer to a question, modern QA systems first try to find a mapping between the question and the Wikipedia paragraph they are presented with. For a question like <em>When did the oil crisis of 1973 begin?</em> this is not very difficult: by searching for the words <em>1973</em>, <em>oil crisis</em> and <em>begin</em>, the software quickly finds the correct sentence in the paragraph above. The question word <em>when</em> provides the final piece of the puzzle: from the training data, the software has been able to learn that <em>when</em>-questions always take a point in time as an answer. It therefore selects the only time in the relevant sentence (October 1973), and answers the question correctly.</p>

<p>The second question in the example above (What was the price of oil in March 1974?) shows the limitations of this method. 
By looking for the words <em>price</em>, <em>oil</em>, <em>March</em> and <em>1974</em> the software still finds the right sentence, but interestingly 
enough, this sentence contains two possible prices: <em>US&dollar;3 per barrel</em> and <em>&dollar;12</em>. Because it doesn’t 
interpret the sentence but only matches patterns to each other, the QA system now selects the first price it sees: <em>US&dollar;3 per barrel</em>. 
The SQuAD page shows that both the Microsoft and the Alibaba systems come up with this incorrect answer. Indeed, this lack of interpretation is a fundamental shortcoming of all current QA systems.</p>

# One-trick ponies

<p>Most existing AI systems are one-trick ponies. If they have been trained on one particular task and one particular text type, they usually cannot handle different domains or problems. QA software that has been trained on Wikipedia will mostly fail to answer questions about other texts, such as legal documents or scientific articles. To do this, it would need to see tens of thousands of questions and answers from that particular domain. Collecting such training data is pretty expensive at best, and often it is an insurmountable task.</p>

<p>The tunnel vision of the current QA systems is even worse than that. Last summer, two researchers from Stanford University demonstrated <a href="https://arxiv.org/pdf/1707.07328.pdf">how easy it is to fool QA software</a> trained on SQuAD: by changing just a few details in the Wikipedia texts, they managed to bring down the quality of the best systems enormously.</p>

<figure class="padded2">
<img class="img-fluid" src="https://www.dropbox.com/s/crqzr3oxft2n9go/Screenshot%202018-01-17%2021.59.52.png?raw=1" />
<figcaption>Add an additional sentence with a possible answer, and the QA software starts to guess.</figcaption>
</figure>

<p>The oil price example already demonstrated the main weakness of current QA software: if the Wikipedia paragraph contains several possible answers, even the best QA systems begin to guess. The same problem is illustrated by the Super Bowl paragraph above: if you simply add an extra quarterback to the text, the systems can’t tell anymore which player was exactly 38 years old during the 33rd Super Bowl. Worse still, if you add to the article an ungrammatical sequence of words that are vaguely related to the answer, even the best systems fall below 10&#37; correct answers. People deal with such misleading situations much better.</p>

## Conclusion

<p>One thing is clear: we’re still a long way from computers that read as well as humans. Still, the recent evolution in QA systems is quite promising &mdash; or frightening, if you fear job loss in the long term. After all, how often are we not confronted with unstructured collections of text? Legal documents, scientific literature, or even the mails of Hillary Clinton &mdash; all hundreds or thousands of pages with interesting content that no one can read from A to Z. Wouldn’t it be great if QA software could answer all our questions about them?</p>

<p>However, the real breakthrough will only come when QA systems lose their dependence on expensive training data. When they can answer a question about a new domain without first seeing tens of thousands of similar examples. The success of such so-called “unsupervised” methods will undoubtedly herald a new revolution in Artificial Intelligence. But we’re not quite there yet.</p>
