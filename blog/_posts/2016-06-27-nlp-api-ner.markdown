---
layout: post
title:  "NLP in the Cloud: Measuring the Quality of NLP APIs"
date:   2016-06-27 12:00:00
tags: NLP api named entity recognition extraction   
comments: true
author: Yves Peirsman
---

<p class="first">
Natural Language Processing seems to have become somewhat of a commodity in recent years. More than a few companies have sprung 
up that offer basic NLP capabilities through a cloud API. If you’d like to know whether a text carries a positive or negative 
message, or what people or companies it mentions, you can just send it to one of these black boxes, and receive the answer 
in less than a second. Superficially, all these NLP APIs look more or less the same. 
<a href="https://www.textrazor.com/">Textrazor</a>,
<a href="http://www.alchemyapi.com/">AlchemyAPI</a>,
<a href="http://aylien.com/">Aylien</a>,
<a href="https://www.meaningcloud.com/">MeaningCloud</a> and
<a href="https://www.lexalytics.com/">Lexalytics</a> all offer similar services (named entity recognition, 
sentiment analysis, keyword extraction, topic identification, etc.), and do so through similar interfaces. However, this surface 
conceals huge differences in quality.   
</p>

<p>
In this article, I evaluate five NLP APIs on one specific, seemingly simple task: 
<a href="https://en.wikipedia.org/wiki/Named-entity_recognition">named entity recognition</a> (NER) for English. 
In named entity recognition, the goal is to identify so-called named entities, such as locations, people and organizations. 
This is useful for many applications in information extraction, such as search engines or question answering software. 
Most APIs offer more entity types than the three main categories 
(for example, dates or events), but since locations, people and organizations are the most widely accepted and available, 
they are the ones I will focus on. 
</p>

<p>
To evaluate the output of these APIs, I collected one hundred sentences from a range of news websites, including 
<a href="http://www.theguardian.com/international">the Guardian</a>, 
<a href="http://www.bbc.co.uk/news">BBC</a>, 
<a href="http://edition.cnn.com/">CNN</a>, etc. 
All of these sentences contain at least one entity. They are well-written sentences in grammatical English, so they 
should pose fewer problems than, say, tweets. They cover news facts from different corners of the world and various 
domains (politics, sports, science, etc.), 
so the entities in them are pretty varied. To give an example of the output I expect, take the sentence
<blockquote class="nomargin">
A diplomatic fight, usually kept behind closed doors, exploded in public Thursday as U.N. Secretary-General Ban Ki-Moon accused Saudi Arabia and its military allies of placing "undue pressure" on the international organization.
</blockquote>
</p>
<p class="noindent">Good NER software should identify <em>U.N.</em> as an organization, <em>Ban Ki-Moon</em> as a person, 
and <em>Saudi Arabia</em> as a location. 
</p>

<h3>Technical details</h3>

<p>
I used this test set of sentences to evaluate the five APIs that are discussed in Robert Dale’s article 
<a href="http://web.science.mq.edu.au/~rdale/publications/industrywatch/2015-V21-4.pdf">NLP meets the cloud</a>: 
<a href="https://www.textrazor.com/">Textrazor</a>, 
<a href="http://www.alchemyapi.com/">AlchemyAPI</a> (which is now part of 
<a href="http://www.ibm.com/cloud-computing/bluemix/watson/">IBM Watson</a>), 
<a href="http://aylien.com/">Aylien</a>, 
<a href="https://www.meaningcloud.com/">MeaningCloud</a> and 
<a href="https://www.lexalytics.com/">Lexalytics</a>. These all offer a free account with a limited number 
of calls that more than suffices for a thorough evaluation of their output.
Interfacing with the API happens through manual REST calls or a custom library in your favorite programming language. 
Some interfaces are more confusing than others (looking at you, Lexalytics), but in general, setting this up is simple enough. 
</p>

<p>
Per entity type, I use three standard metrics to measure the quality of the API:
<ul class="nomargin">
<li>Precision: If an API identifies a word or word sequence as an entity, how likely is this to be correct?</li>
<li>Recall: What percentage of entities in the text does the API identify correctly?</li>
<li>F-score: the (harmonic) mean of precision and recall captures the quality of the API for each entity type 
in one single figure.</li>
</ul>
</p>

<p class="noindent">
Here are some additional rules of the game:
<ul class="nomargin">
<li>I use the available APIs as off-the-shelf services, without tweaking the results. Some APIs give a confidence score for every entity, which would allow users to filter out entities with a low confidence. This is potentially interesting, but it also takes quite some effort, so I just evaluate the output as-is.</li>
<li>When an API misclassifies a location as an organization, or vice versa, I don’t penalize it and just follow the 
classification of the API. Place names are often ambiguous (as in <em>Iran said …</em>), so let’s not be too strict.</li>
<li>When an API identifies an entity but does not classify it as a person, location or organization, I ignore it.</li>
<li>Some APIs are able to identify adjectives (such as <em>Chinese</em>) as locations (<em>China</em>), others are not. If they do so, I count these entities as correct, but if they don’t, I don’t penalize them. Again, let’s not be too strict.</li>
<li>Some APIs not only identify entities in a sentence, but also try to determine the real-world entity that it refers to, 
and give a link to the relevant Wikipedia page. Because not all APIs provide it, I ignore this reference, whether
it is correct or not.</li>
</ul>
</p>

<p>
TextRazor’s offering is pretty complex, because it classifies entities as both <a href="http://wiki.dbpedia.org/">DBPedia</a>
 categories and <a href="http://wiki.freebase.com/wiki/Main_Page">Freebase</a> types. 
Not only do these two sources have completely different lists of categories, sometimes the Textrazor classifications do not agree. 
In a sentence such as <em>Iraqi troops begin operation to seize Falluja from Isis</em>, <em>Isis</em> is classified 
as a Freebase Organization (correct), but a DBPedia Place (incorrect). In this exercise, I chose to work with the Freebase types.
Additionally, Textrazor tends to give long lists of 
categories for every entity, most of which are really specific and frankly, irrelevant. Are you interested in the fact that 
<em>the United States</em> is not only a <em>/location/location</em>, but also a 
<em>/meteorology/cyclone_affected_area</em>, <em>/fictional_universe/fictional_setting</em> or 
<em>/travel/travel_destination</em>, wherever it occurs? I know I’m not.
</p>

<p>Aylien’s API, too, is rather confusing, but in a different way: the API has two endpoints that can be used for entity extraction 
(Entity and Concept extraction), and their results can be very different. Aylien’s Concept extraction makes use of an external knowledge
base (e.g. DBPedia), whereas its Entity extraction is purely self-contained. Concept extraction makes use of rather specific DBPedia entity types (e.g. Scientist,
OfficeHolder, TennisPlayer, etc. are all types of Person), whereas Entity extraction has just the three main categories (Location, 
Person, Organization). Concept extraction can also return several entity types per entity, whereas Entity extraction sticks to one.</p>

<p>
The remaining APIs are simpler, and return just one category per identified entity. The table below shows how I mapped them to the three
main entity types.

 <table style="width:100%">
  <tr>
    <th></th>
    <th>Location</th>
    <th>Person</th>
    <th>Organization</th>
  </tr>
  <tr>
    <td>Textrazor</td>
    <td><em>/location/location</em></td>
    <td><em>/people/person</em>, <em>/royalty/noble_title</em></td>
    <td><em>organization/organization</em>, <em>internet/website</em>, <em>sports/sports_team</em></td>
  </tr>
  <tr>
    <td>AlchemyAPI</td>
    <td><em>City</em>, <em>Country</em>, <em>StateOrCountry</em>,
<em>Region</em>, <em>Facility</em>, <em>GeographicFeature</em></td>
    <td><em>Person</em></td>
    <td><em>Company</em>, <em>Organization</em></td>
  </tr>
  <tr>
    <td>Aylien</td>
    <td><em>Location</em></td>
    <td><em>Person</em></td>
    <td><em>Organization</em></td>
  </tr>
  <tr>
    <td>Lexalytics</td>
    <td><em>Place</em></td>
    <td><em>Person</em></td>
    <td><em>Company</em></td>
  </tr>
  <tr>
    <td>MeaningCloud</td>
    <td><em>Top>Location</em></td>
    <td><em>Top>Person</em></td>
    <td><em>Top>Organization</em></td>
  </tr>
</table> 

</p>

<h3>Results</h3>

<p>
Let’s now take a look at the results, starting with locations. Locations are generally the easiest type of entity to identify. 
Four of the five APIs in this evaluation exercise are best at identifying locations, when compared to organizations or people. 
Still, the differences in quality between the APIs are enormous. Textrazor does a great job, with an F-score of 95.9%.
It finds 99% of all locations in the test sentences (recall), and when it identifies a word or word sequence as a location,
this is correct in 93% of the cases (precision). AlchemyAPI is in second position, with an F-score of 90.8%. Its entities
are also correct in 93% of the cases (precision), but it only finds 89% of all locations.
Likewise, the other three APIs all achieve a precision of 93% and above, but their recall suffers greatly: 
MeaningCloud finds 76% of all locations, Aylien's Concept extraction 72%, its Entity extraction 68%, and Lexalytics a meagre 53%. 
</p>

<p>Let me give two examples to
show what this means in practice. In the following sentence, Aylien’s Concept Extraction 
finds <em>Belgium</em>, but fails to classify it as a person, location or organization.
Its Entity extraction does not identify it at all. Both endpoints miss all four people:
<blockquote class="nomargin">
Belgium created the game’s first opening when Lukaku and Fellaini
combined to tee up Nainggolan for a 25-yard drive that Buffon pushed away
</blockquote>
<p class="noindent">And in</p>
<blockquote class="nomargin">
Indian president Pranab Mukherjee arrived in Ivory Coast capital Abidjan on Tuesday for a two-day state visit, the first visit of an Indian president since the establishment of diplomatic relations between the two countries in 1960.
</blockquote>
<p class="noindent">Lexalytics misses both locations (<em>Ivory Coast</em> and <em>Abidjan</em>), as well as <em>Pranab Mukherjee</em> (person).
That’s pretty disappointing.</p>
<iframe width="600" height="371" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/16EIiMRHZHqy6mE9kbMtK4nGSMa-XbqtC7A_nRd2TOJ4/pubchart?oid=2010346884&amp;format=interactive"></iframe>

</p>
<p>
The results for organizations and people follow a similar pattern. Again, Textrazor and AlchemyAPI are in first and second 
position, respectively. Textrazor is particularly strong at identifying many entities (recall), AlchemyAPI wants to get them right (precision). 
MeaningCloud is a solid third place, while the performance of Aylien depends on the endpoint you use: the Concepts endpoint is better at identifying
well-known people, places and organizations, while for lesser known entities and surnames without a first name, the Entities endpoint mostly does
a better job. Lexalytics achieves a very high precision, but its recall is really low. It’s possible its recall and F-score 
can be improved by setting a lower confidence threshold for the entities.
<iframe width="599.4557377049182" height="370.61469858156033" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/16EIiMRHZHqy6mE9kbMtK4nGSMa-XbqtC7A_nRd2TOJ4/pubchart?oid=494995784&amp;format=interactive"></iframe>
<iframe width="602" height="372.10226424361497" seamless frameborder="0" scrolling="no" src="https://docs.google.com/spreadsheets/d/16EIiMRHZHqy6mE9kbMtK4nGSMa-XbqtC7A_nRd2TOJ4/pubchart?oid=1492503240&amp;format=interactive"></iframe>
</p>

<p>
Some of the more striking results can help illustrate the differences between the APIs. In the sentence 
<blockquote class="nomargin">
Jenson Button was an encouraging seventh for McLaren, matching the time of Williams' Valtteri Bottas in sixth.
</blockquote>
</p>
<p class="noindent">both Lexalytics and Aylien’s Entity extraction fail to identify a single entity, although there are two people (<em>Jenson Button</em> and 
<em>Valtteri Bottas</em>) and two organizations (<em>McLaren</em> and <em>Williams</em>). Textrazor and MeaningCloud find all four 
entities correctly. Aylien’s Concept extraction finds three entities, but fails to identify Williams as an organization. 
AlchemyAPI finds <em>Valtteri Bottas</em>, but it misses <em>Jenson Button</em> and misclassifies 
<em>McLaren</em> and <em>Williams</em> as people. 
</p>
<p>
In the sentence
<blockquote class="nomargin">
This statement by the vice president of the NFF, Seyi Akinwunmi, is a repeat of homophobic statements made by football officials in the country in the past which were strongly condemned by FIFA.
</blockquote>
</p>
<p class="noindent">Aylien’s endpoints do not classify a single entity as person, location or organization. MeaningCloud and Lexalytics both find <em>FIFA</em> (organization), but miss 
<em>NFF</em> (organization) and <em>Seyi Akinwunmi</em> (person). Textrazor and AlchemyAPI find all three entities, 
although they both map <em>NFF</em> to the Norwegian Football Federation, and not the Nigerian one. 
</p>

<p>
Named entity recognition is a harder task than it might seem at first glance. Even the best APIs occasionally make mistakes. 
In many ways, I’ve tested only the most basic features of named entity recognition here. Things get considerably 
harder when the identified entities have to be linked to their correct real-world counterpart (such as <em>NFF</em> 
in the sentence above), or when several mentions of the same entity (<em>Donald Trump … Trump … he</em>) have to be 
mapped to one and the same referent. Similarly, even the best APIs struggle when they have to disambiguate an entity 
and infer, for example, that <em>Southwest</em> in a context such as <em>drier than normal conditions in the Southwest</em> 
refers to a location rather than a company. Kudos to AlchemyAPI, which was the only one in this test to get this example right. 
There’s no way this can be done without the basic entity recognition 
that I’ve tested here. 
</p>

<p>
It is certainly true that the NLP APIs in this article have made basic NLP tasks available
to the masses. However, while the offerings may look similar on the outside, their differences become very clear 
when we measure their performance. In my NER test, Textrazor outperformed all others, but it did so with long 
and often confusing lists of possible entity types. AlchemyAPI’s results were much more straightforward, but 
it found considerably fewer entities. MeaningCloud performed reasonably well on simple examples, but failed to perform
on the more complex ones. Aylien has two endpoints that both struggle at times, and its plans to merge them
should really pay off. Lexalytics has its work cut out. Of course, it is important 
to bear in mind that my evaluation
exercise is rather informal and only looked at 100 sentences typical of one 
linguistic style and domain. It’s always possible that other domains may give different results.
Still, it clearly pays off to compare the various APIs available, so buyer beware!
</p>
