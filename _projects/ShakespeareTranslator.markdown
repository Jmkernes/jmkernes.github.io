---
layout: page
title: Shakespeare Translator
description: An attention based translator to rewrite modern day english into Shakespearian english.
img: /assets/img/ShakespeareTranslator/where_art_thou.png
importance: 2
github: https://github.com/jmkernes/ShakespeareTranslator
---

For the full project, please follow the link to my github repository:
<a href="https://github.com/Jmkernes/Shakespeare-Translator">https://github.com/Jmkernes/Shakespeare-Translator</a>

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ShakespeareTranslator/stock_shakes_photo.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ShakespeareTranslator/stock2.jpg' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Sources: left) matt riches, unsplash.com. right) tabitha turner, unsplash.com.
</div>

As the title suggests, the goal of this project is to translate a sentence from english to shakespearian english. More specifically, we provide two types of inference:

1. __Faithful translation__. This utilizes beam search to try to construct the most probable shakespearian translation given the input.

2. __Generative translation__. This uses a temperature parameter to sample possible translations. It may not be as accurate, but certainly more fun to give the model some freedom. We also implement a basic Minimum Bayes Risk inference on these translations to try to pick the "best" translation.

## Non-trivial aspects

* The translation dataset was created from scratch, by **web scraping** from NoFearShakespeare's translations.
* The data was totally raw, so it had to be cleaned and preprocessed.
* An **encoder-decoder transformer**
* A **Masked Language Model pretraining** cloze task, to learn bidirectional transformer encoder embeddings, similar to BERT but without next-sentence prediction.
* A homeade **Beam search** implementation, for both graph and eager mode.
* An alternative **Minimum Bayes Risk** translator, built on python's `difflib` library for basic sequence matching

One of the fun things you can do is use this as an insult generator. Below is a screenshot of what it looks like in the terminal.


<div class="row justify-content-sm-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ShakespeareTranslator/prog_screenshot.png' | relative_url }}" alt="" title="example image"/>
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/ShakespeareTranslator/insult.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
  Insult someone, with sophistication.
</div>


The Minimum Bayes Risk generator will generate 16 random translations, and order them based on similarity. The similarity score is computed as follows: Each sequence is compared to every other sequence using python's difflib module method SequenceMatcher, resulting in a similarity score. A given entry is scored based on how similar it is to every other entry. We then rank sequences in order from most similar (meaning the sequence resembles the "mean" sequence) to least similar. An alternative similarity test using a count vectorizer is at the end of the inference notebook. It doesn't work as well; it loses temporal alignment and favors repetitive nonsensical translations.

## Using the data
Please use it! I ran out of GPU power, so I couldn't fine tune more. I strongly recommend using the data however to try to do better! Any comments, questions, concerns please email me or open a pull request. Thanks!
