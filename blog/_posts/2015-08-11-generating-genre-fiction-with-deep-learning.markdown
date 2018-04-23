---
layout: post
title:  Generating Genre Fiction with Deep Learning
date:   2015-08-09 15:00:00
tags: NLP literature genre fiction deep learning
comments: true
author: Yves Peirsman
---

<p class="first"> These days Deep Learning is everywhere. Neural networks are used for just about every task in Natural Language Processing &mdash; from named entity recognition to sentiment analysis and machine translation. A few months ago, <a href="http://karpathy.github.io">Andrej Karpathy</a>, PhD student at Stanford University, released a small software package for automatically generating texts with a recurrent neural network. I wanted to find out how it performs when it is asked to generate genre fiction, such as fantasy or chick lit.</p>

<p>Andrej’s model is a character-level language model based on multi-layer LSTMs. Let’s break this mouthful down into its most important components. First, we’re dealing with a language model: this means we need to train it on a text, and through analyzing that text, it will learn to generate language all by itself. Second, it operates on the level of characters. In other words, it analyzes and generates texts character by character. Initially, it doesn’t know anything about words: it just knows there are around 130 characters &mdash; mainly letters, numbers, capitalized letters, spaces and punctuation. During its training, it learns the patterns these characters appear in. It will learn, for example, that when it sees the sequence <em>altho</em>, the next character is much more likely to be a <em>u</em> than another <em>o</em>. In this way, it will learn to build words, and eventually, sentences. Third, the language model is based on multi-layer LSTMs. LSTMs, or Long Short-Term Memories, help it learn longer dependencies than the two or three characters it has just seen. We will see an example of that behaviour below. For the interested reader, there’s more information in <a href="http://karpathy.github.io/2015/05/21/rnn-effectiveness/">Andrej’s blog post</a>.</p> 

<p>Earlier blog posts from people in the deep learning community have shown the effectiveness of training this type of model on a single book such as <a href="http://korbonits.github.io/2015/06/28/Torch-bleeding-edge-DNN-research.html">Ulysses</a> or <a href="http://cpury.github.io/learning-holiness/">the Bible</a>, or on works by a single author, such as <a href="http://karpathy.github.io/2015/05/21/rnn-effectiveness/
">Shakespeare</a> or <a href="https://medium.com/@samim/obama-rnn-machine-generated-political-speeches-c8abd18a2ea0">President Obama’s speeches</a>. After a few training cycles, the model is able to generate text all by itself that clearly displays the same characteristics as the text it was trained on. After analyzing Obama’s speeches, it produces sentences such as <em>The United States will step up to the cost of a new challenges of the American people that will share the fact that we created the problem</em>. Training it on Shakespeare results in fragments such as <em>They are away this miseries, produced upon my soul,/ Breaking and strongly should be buried, when I perish/ The earth and thoughts of many states</em>. Obviously, the results are mostly nonsense, but they bear strong similarities to the texts from that particular author or source.</p>

<p>I was interested to see what would happen if I trained a similar language model on genre fiction &mdash; 
not just works from one author, but books from several authors in the same genre, such
as chick lit or fantasy. Last year I collected a large corpus of amateur fiction 
from <a href="https://www.smashwords.com/">Smashwords</a>. As all of its books are labelled with one or more genres, this corpus lends itself perfectly to that experiment. I trained 
three different models: one for chick lit, one for historical fantasy and one for mystery
&amp; detective. For those interested in the technical details, I used three 2-layer LSTMs with 512 nodes. For all other settings, I kept the default configuration in Andrej’s code. Each network took as its input some forty-odd books from one particular genre, totalling around
2,250,000 words or 10 MB of data. The fragments below were generated with a so-called <em>temperature</em> of 0.7 or 0.8, which struck a good balance between confidence and diversity.</p>

<p>It’s time to dive into the results. Here’s a first fragment of automatically generated chick lit. It takes on the form of a short dialogue:</p> 

<blockquote>
They know the next six months, she said, “I don’t know, when he’s going to have a dinner, as much as he did.”<br>
“No, did you start the tension?” She chorused at her, and started as he took her arms and looked at the bear. <br>
“You are in this place. I’m sorry,” she chuckled.<br>
“I’m going to get out of this bathroom and see all the call of my hands. Sometimes I’m in the kitchen under the kitchen wall.”<br>
“Okay , he’s happy for you.” <br>
He continued to smack the back of the ball. <br>
“You stay around.”<br>
“I’m sorry to meet you. Don’t worry about what we wanted.” <br>
“So,” asked Maelyn. “You don't know what I know where we have a thing.”<br>
“I think he will be free for me.” <br>
“What’s the next week?” Jeremy asked. <br>
“I don’t believe that maintaining how to be the only one who some things have picked out.”</blockquote>

<p>While this is far from a realistic conversation, the typical elements of chick lit are present. The model has produced meaningful phrases such as <em>he’s happy for you</em>, <em>he took her arms</em>, <em>we have a thing</em> or <em>I think he will be free for me</em>. Male and female characters are involved, and the topic is never far from the relationship between them. Note also that most sentences are perfectly grammatical, even
though they rarely make sense. My favourite must be <em>Sometimes I’m in the kitchen under the kitchen wall.</em></p> 

<p>Here’s another, slightly longer example:</p> 

<blockquote>The form of the weekend. The arrival in the corner of the church came. But they looked for the museum, the only person that there was something about it. God, he was introduced to my first stalker. Serena was all strange that before I would like to make sure that this coffee reaches off the bar like she was born and impressed the talent alone. <br>
“Come from Heath, but it’s a perfect need of an accourted team.” I had seen a group of majors, but brain. I thought when he put her hand in the floor, which I felt full of water. <br>
“I’ve got to do anything there here to me. That doesn’t . I’m kind of strange,” I said as he let out a smile. He was sure that after that she ended up slowly parking in her door. For some reason, I hadn’t seen the shop in the California he’d only had time. My heart beat some points. I lived in the form of The Edition’s phone to handle the gaze of my underwear before the demolitional regulation of the adults in his eyes. By the arrival are defensed through the grey composure. I presumed I was the only one who couldn’t help while tears could stand up. I didn’t mean to be better at first on this brother’s activity for the one thing to make it much for me. I went for the sun, and then she found his hand it was a very salary she expressed to her hotel. <br>
“So our whole left will get out of the day, we’re just in a little consultation.” <br>
“I know. It would be a bit positive. It’s just a great time . I want to go to the car.” My face notices and should be patient in between. Even he played to bed, and my chest means much to make sure that by a milk. When I was doing the telling love when we stopped to realise that I wasn’t the only way to make all that. I was the last time you were glad you invited them for the excess ones. What about his woman, I smile. I can’t complete all this. They’re in its number, so you’ve come to just change my things. I’m a sister.
</blockquote>

<p>Again, romance is never far off (see <em>I’m kind of strange, I said as he let out a smile</em> or <em>then she found his hand</em>). There is talk of rivalry (<em>What about his woman, I smile</em>) and stalking (<em>he was introduced to my first stalker</em>). And while there are many phrases I don’t know what to make of (<em>the gaze of my underwear</em> or <em>I was doing the telling love</em>), but I’m sure people with a more lively imagination won’t have any problem dreaming up a fine explanation.</p>

<p>Next, I trained the model on forty-four books of historical fantasy. This made it produces passages such as this one:</p>

<blockquote>
He heard a soldier on the road and the children came here out of the concern. I stood slowly and reached for my own face. I was standing in the still steps to the mountains the other was my last boy. Hywel began to cover in a cry. <br>
“You are getting the flower of the dragon for my days for not to act as the fallen streets -- they somehow come out the shadow of the air, as if the gods were begging,” I replied. <br>
“I can find the road.” <br>
An angel blow, truth and reveals of the line of a messenger that stretched into the streets of Brechalon. The soldiers ate quietly to his father’s men, completely still and the old fool said the power of respecting a chance, and they would stay in the face of the pilgrim, and I was interrupted by the other man. She had not lived down the side of the choir, a stone -- and it would be more than anything that had been still to see and they brought their eyes upon him, but it had been a lot of the first thing to trouble their thanks. The celebration who stood in the hall. He had the whole of the men, while Azkun laughed as he attempted to simply felt a strange knight. He felt his mouth in the north, and his voice was the demon he had seen again. In the darkness, he had not found a few and a consequence of water that enjoyed the first thing to have, but the dropped completely he was, bound the table by a small, measured form. The girl fell silent to the cross from the field. At the same time, the sun touched the door, the crossbowman approaching the continue in the marble stairs. It was the same one, she pursued the dragon on the ground. He shrugged and swing the knife in careful glance. “We want to get some nights ago.” I pulled her with it in a storm and helped her to him. 
</blockquote>

<p>This fragment couldn’t be further removed from the previous ones. Instead of stalkers and sisters, it features soldiers, dragons, knights and demons. The scene conjures up an image of beleaguered villages, violent sword fights and a general atmosphere of fear and dread. I would argue it captures the genre of historical fantasy pretty well.</p>

<p>The third and final genre I played with was mystery and detective. Here’s what the model came up with:</p>

<blockquote>
“Dead, maybe something supposed to be.”<br>
“Interesting, Di. But if you will take a clear paper,” Steve said , then nodded. I stepped and slammed onto the building. <br>
“No, he’s together and don’t know me on the stage.” <br>
“Then it’s exactly and you’re well happy with me.”<br>
“He’s never too at the demise of that amount of conversation with the phone down. It’s my background, about five years ago. And it can’t change the voice.” <br>
She grabbed his face to his feet. <br>
“That’s incident, I think. I was caught with our accident.” <br>
“The older man who would actually escape but he will need to have a big plot about his mother.” <br>
I dropped a drink and saw that a hull of in this case smelled of laptop. <br>
“Okay , huh?” I thought then who, he didn’t know what you’re under, so I put it careful to drink in bitching mind to make the first thing. Even as first it should be Lord Rushton and Collin who had reached his file, Mrs. Johnson 15, suddenly watching her that I had been. It was the fact that he’d met him near the Civil Search, and the time in making documents, it was a story with what that was a murder. And if I had discomfort with him for leaving anything that was worse, while you’ve got it in his house in the air and Evangeline offered to be a man’s heart to pay from the people. <br>
“He pressed his girl and jerked his arms around her breasts. <br>
“If he didn’t say anything but is my life on his Anthony, now,” he said.<br> 
“I’ve never wanted to be more sort of time to give the address,” she said. “And let’s go.” She groaned and feared she was such a comment of her supplier as he washed on the table. <br>
“What are you talking about ?” <br>
“Everyone in the wrong minute was so sailing the man in his flashlight.” Maggie shook her head. <br>
</blockquote>

<p>Like many detective stories, this fragment starts with a body. It later mentions an <em>accident</em> and a <em>murder</em>, often the competing explanations offered for someone’s <em>demise</em>. Its general topic is less focused than in the historical fantasy fragment above, but these lexical clues nevertheless link it to mystery and detective.</p>

<p>Here’s a final example, again by the mystery and detective model:</p>

<blockquote>
It felt as if she came in or happen, but I think Will, if she was really under the lab, he was about to score off the company and something of the day back into the front of the house with both like a captain. There was no more secret back at the shop. I had to know where the little committee was murdered.”<br> 
“I’m the best side of the second difference,” I said.<br> 
“Nothing. The construction way. I’m sorry I don’t have to talk to you.”<br>  
“The man? I’ll be the one who was the only one who has a problem.” The two girls would be back and forth to Steve’s perfect statement and looked at him with a hard hand beneath the car on the back panel of the gloom.<br>  
“Suppose the speaker was the Sergeant.”<br> 
“You all really like to waste a glass of the contrary?”<br> 
“No, I was, but I know you’re all the day.” He smiled.<br>  
“I wouldn’t want a stranger off and say it would be said, but it’s fine. There was a brief story.”<br> 
“Oh , I know you don’t really do more,” she said, “okay, I can. I’ll need to start a fifty feet on top of these time.”<br>  
“But what do you mean?”<br>  
“Not so,” Jason said, “it says the men were coming in here.”<br>  
“Thank you for respecting the way,” Chet said. “I have a professional less than a life of the news and the painting of the jail when it was going to have to tell me.”<br>  
“I think I can not say that. Wouldn’t you be able to help you?” Ronnie took a step back.<br> 
“That’s a low, lid, but I will never begin to the right seat.” She took his shoulder to her feet and shake his head around.<br>  
“I think I’m all right.” I was positive to explain her back.<br>  
“Well, if you can have some more attempt to leave the window’s walls and the others killed Stephen.” The police taste, a warm slipper branches. As if I’d been recognising the choice with the employees? It didn’t matter. She thought for a second to come and then ask her.<br> 
</blockquote>

<p>Again, the influence of the detective genre can be recognized in a number of lexical clues, such as <em>killed</em>, <em>police</em>, <em>jail</em> and <em>statement</em>. Some sentences could even come directly from a detective novel, such as <em>I had to know where the secret committee was murdered</em> or <em>I’m sorry I don’t have to talk to you</em>. In short, the model has reliably picked up some of the most typical characteristics of all three genres I have explored. 

<p>Although it’s good to see the neural networks produce genre-specific words, what I find even more impressive is their ability to imitate language at a lower level. Remember that these are character-level language models that produce one character (letter, symbol, space, etc.) at a time, given the preceding context. Now consider for a moment some of the things these LSTMs have learnt:

<ul>
    <li>A lexicon: with very few exceptions, almost all words in the samples are existing English words.</li>
    <li>Syntax: nonsensical as they may be, many sentences are grammatically correct. They generally contain a subject and a main verb, transitive verbs mostly have a direct object, singular nouns are preceded by articles, and so on.</li>
    <li>Spelling: All sentences and names, and only sentences and names, begin with capital letters.</li>
    <li>Punctuation: All sentences end in a full stop, and most opening quotation marks are followed at some later point by closing quotation marks. This is probably due to the long-term memory capabilities of LSTMs, which are able to remember that there are opening quotation marks in the preceding context, and only forget about them when these have been closed again.</li>
</ul>

That’s pretty impressive.</p>

<p>Given the nonsensical nature of all fragments above, deep learning models aren’t going to replace novelists any time soon. Still, they are already able to learn some of the most typical characteristics of genre-specific language. This behaviour could help us classify stories by their genre or help recognise the author of a book. Moreover, the appeal of these models is much wider than language or literature &mdash; the same neural networks can be <a href="https://soundcloud.com/seaandsailor/sets/char-rnn-composes-irish-folk-music">applied to music</a>, for instance. And with deep neural networks becoming better almost by the day, who knows what will become possible in the future.</p>
 
