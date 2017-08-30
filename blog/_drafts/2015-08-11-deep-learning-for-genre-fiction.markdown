---
layout: post
title:  "Gender Stereotypes in Amateur Fiction"
date:   2015-08-12 9:00:00
tags: NLP literature genre fiction deep learning
comments: true
---

The model looks at the texts character by character. It doesn't know anything about words: 
it just knows there are around 130 characters &mdash mainly letters, numbers, capitalized
letters and punctuation. When we let it read a text, it learns the patterns these characters
appear in. It will learn, for example, that when it sees a letter <emph>a</emph>, the next 
character is much more likely to be an <emph>n</emph> than to be another <emph>a</emph>. 

Earlier blog posts from people in the deep learning community have shown the effectiveness
of training these models on a single book such as Ulysses, or works by a single author, 
such as Shakespeare. Obviously, the results are mostly nonsense, but they are readily 
recognizable as works from that author or source.

I was interested to see what would happen if we trained an RNN on genre fiction &mdash; 
not just works from one author, but books from several authors in the same genre, such
as chick lit or fantasy. Last year I collected a large collection of amateur fiction 
from SmashWords &mdash; a corpus that lends itself perfectly to this goal because
all books are labelled with one or more genres. I trained 
three different RNNs: one for chick lit, one for historical fantasy and one for mystery
&amp; detective. 

For those interested in the technical details, I used a 2-layer LSTM with 512 nodes.
They each take as their input some forty-odd books totalling around
2,250,000 words or 10 MB of data. 


Chick lit: 




They know the next six months, she said, ``I do n't know, when he's going to have a dinner, as much as he did.'' 
``No, did you start the tension?'' She chorused at her, and started as he took her arms and looked at the bear. 
``You are in this place. I'm sorry,'' she chuckled.
``I 'm going to get out of this bathroom and see all the call of my hands. Sometimes I'm in the kitchen under the kitchen wall.'' 
``Okay , he's happy for you.'' 
He continued to smack the back of the ball. 
``You stay around.'' 
``I 'm sorry to meet you. Don't worry about what we wanted.'' 
``So,'' asked Maelyn. ``You don't know what I know where we have a thing.''
``I think he will be free for me.'' 
``What 's the next week?'' Jeremy asked. 
``I don't believe that maintaining how to be the only one who some things have picked out.'' 

And here’s a slightly longer piece: 

<blockquote>The form of the weekend . The arrival in the corner of the church came . But they looked for the museum , the only person that there was something about it . God , he was introduced to my first stalker . Serena was all strange that before I would like to make sure that this coffee reaches off the bar like she was born and impressed the talent alone. 
``Come from Heath, but it's a perfect need of an accourted team.'' I had seen a group of majors, but brain. I thought when he put her hand in the floor, which I felt full of water. 
``I've got to do anything there here to me. That doesn’t . I'm kind of strange,'' I said as he let out a smile. He was sure that after that she ended up slowly parking in her door. For some reason, I hadn't seen the shop in the California he'd only had time. My heart beat some points. I lived in the form of The Edition's phone to handle the gaze of my underwear before the demolitional regulation of the adults in his eyes. By the arrival are defensed through the grey composure. I presumed I was the only one who couldn't help while tears could stand up. I didn't mean to be better at first on this brother's activity for the one thing to make it much for me. I went for the sun, and then she found his hand it was a very salary she expressed to her hotel. 
``So our whole left will get out of the day, we 're just in a little consultation.'' 
``I know. It would be a bit positive. It 's just a great time . I want to go to the car.'' My face notices and should be patient in between. Even he played to bed, and my chest means much to make sure that by a milk. When I was doing the telling love when we stopped to realise that I wasn't the only way to make all that. I was the last time you were glad you invited them for the excess ones. What about his woman, I smile. I can't complete all this. They're in its number, so you've come to just change my things. I m a sister.<blockquote>





Historical fantasy

He heard a soldier on the road and the children came here out of the concern. I stood slowly and reached for my own face. I was standing in the still steps to the mountains the other was my last boy. Hywel began to cover in a cry. 
``You are getting the flower of the dragon for my days for not to act as the fallen streets -- they somehow come out the shadow of the air, as if the gods were begging,'' I replied. 
``I can find the road.'' 
An angel blow, truth and reveals of the line of a messenger that stretched into the streets of Brechalon. The soldiers ate quietly to his father's men , completely still and the old fool said the power of respecting a chance, and they would stay in the face of the pilgrim, and I was interrupted by the other man. She had not lived down the side of the choir, a stone -- and it would be more than anything that had been still to see and they brought their eyes upon him, but it had been a lot of the first thing to trouble their thanks. The celebration who stood in the hall. He had the whole of the men, while Azkun laughed as he attempted to simply felt a strange knight. He felt his mouth in the north, and his voice was the demon he had seen again. In the darkness, he had not found a few and a consequence of water that enjoyed the first thing to have, but the dropped completely he was, bound the table by a small, measured form. The girl fell silent to the cross from the field. At the same time, the sun touched the door, the crossbowman approaching the continue in the marble stairs. It was the same one, she pursued the dragon on the ground. He shrugged and swing the knife in careful glance. ``We want to get some nights ago.'' I pulled her with it in a storm and helped her to him. I bury the name of the dark stairs to the transport and waited for him to wait mile, and when I heard the open tore in my palms, I was about to express him, but was in the tentache, but I was

Mystery and detective


``Dead, maybe something supposed to be.'' 
``Interesting, Di. But if you will take a clear paper,'' Steve said , then nodded. I stepped and slammed onto the building. 
``No, he's together and don't know me on the stage.'' 
``Then it 's exactly and you're well happy with me.''
``He 's never too at the demise of that amount of conversation with the phone down. It 's my background , about five years ago. And it can't change the voice.'' 
She grabbed his face to his feet. 
``That's incident, I think. I was caught with our accident.'' 
``The older man who would actually escape but he will need to have a big plot about his mother.'' 
I dropped a drink and saw that a hull of in this case smelled of laptop. 
``Okay , huh?'' I thought then who, he did n't know what you're under , so I put it careful to drink in bitching mind to make the first thing. Even as first it should be Lord Rushton and Collin who had reached his file, Mrs. Johnson 15, suddenly watching her that I had been. It was the fact that he'd met him near the Civil Search, and the time in making documents, it was a story with what that was a murder. And if I had discomfort with him for leaving anything that was worse, while you've got it in his house in the air and Evangeline offered to be a man's heart to pay from the people. 
''He pressed his girl and jerked his arms around her breasts. 
``If he did n't say anything but is my life on his Anthony, now,'' he said. 
``I've never wanted to be more sort of time to give the address,'' she said. ``And let 's go.'' She groaned and feared she was such a comment of her supplier as he washed on the table. 
``What are you talking about ?'' 
``Everyone in the wrong minute was so sailing the man in his flashlight.'' Maggie shook her head . 


And here’s another one: 

It felt as if she came in or happen, but I think Will, if she was really under the lab, he was about to score off the company and something of the day back into the front of the house with both like a captain. There was no more secret back at the shop. I had to know where the little committee was murdered.'' 
``I'm the best side of the second difference,'' I said.
``Nothing. The construction way. I'm sorry I don't have to talk to you.'' 
`` The man? I 'll be the one who was the only one who has a problem.'' The two girls would be back and forth to Steve's perfect statement and looked at him with a hard hand beneath the car on the back panel of the gloom. 
``Suppose the speaker was the Sergeant.''
``You all really like to waste a glass of the contrary?''
``No, I was , but I know you're all the day.'' He smiled. 
``I wouldn't want a stranger off and say it would be said, but it's fine. There was a brief story.''
`` Oh , I know you don't really do more,'' she said, ``okay, I can. I'll need to start a fifty feet on top of these time.'' 
``But what do you mean?'' 
``Not so,'' Jason said, ``it says the men were coming in here.'' 
``Thank you for respecting the way,'' Chet said. ``I have a professional less than a life of the news and the painting of the jail when it was going to have to tell me.'' 
``I think I can not say that . Wouldn't you be able to help you?'' Ronnie took a step back.
``That's a low, lid, but I will never begin to the right seat.'' She took his shoulder to her feet and shake his head around. 
``I think I'm all right.'' I was positive to explain her back. 
``Well , if you can have some more attempt to leave the window's walls and the others killed Stephen.'' The police taste, a warm slipper branches. As if I'd been recognising the choice with the employees? It didn't matter. She thought for a second to come and then ask her.


<p>Of course, these fragments are all nonsense. But consider for a moment the things our model has learnt:

<ul>
    <li>A lexicon: with very few exceptions, almost all words are existing English words</li>
    <li>Syntax: nonsensical as they may be, many sentences are grammatically correct. They generally contain a subject and a main verb, for example.</li>
    <li>Spelling: All sentences and names, and only sentences and names, begin with capital letters.</li>
    <li>Punctuation: All sentences end in a full stop, and most opening quotation marks are followed at some later point by closing quotation marks.</li>
</ul>

All in all, that’s pretty impressive.</p>

<p>And there’s more. The models have clearly picked up more subtle characteristics of the literary genre of their training data.  
</p>




http://karpathy.github.io/2015/05/21/rnn-effectiveness/

Shakespeare

Alas, I think he shall be come approached and the day
When little srain would be attain'd into being never fed,
And who is but a chain and subjects of his death,
I should not sleep.

Leo Tolstoy's War and Peace

"Why do what that day," replied Natasha, and wishing to himself the fact the
princess, Princess Mary was easier, fed in had oftened him.
Pierre aking his soul came to the packs and drove up his father-in-law women.

http://korbonits.github.io/2015/06/28/Torch-bleeding-edge-DNN-research.html

Power, Kinch, an, his dead reformed, for the churches hand.

They were namebox: a kitchen and perhage his sight on his canes deep any outwas, life
stands. Clesser. A fellows her last firing. And beneather to him,
they give me 39: then he was brilliging bying. A lair unde for paper so
fresh strangy gallous flashing at the crassies and
thit about a son of their God’s kind. His arm.
