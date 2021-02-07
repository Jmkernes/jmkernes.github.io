---
layout: page
title: Adaptive Softmax
description: A TF2.0+ implementation of the Adaptive Softmax layer.
img: /assets/img/AdaptiveSoftmax/Projections.png
importance: 3
github: https://github.com/Jmkernes/PAR-Transformer-XL/blob/main/adaptive_softmax.py
---
For a full description of the project, please see my TowardsDataScience post: <a href="https://towardsdatascience.com/how-to-overcome-the-large-vocabulary-bottleneck-using-an-adaptive-softmax-layer-e965a534493d">https://towardsdatascience.com/how-to-overcome-the-large-vocabulary-bottleneck-using-an-adaptive-softmax-layer-e965a534493d</a>

For the code, please follow the link to my github: <a href="https://github.com/Jmkernes/PAR-Transformer-XL/blob/main/adaptive_softmax.py"> https://github.com/Jmkernes/PAR-Transformer-XL/blob/main/adaptive_softmax.py</a>


<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/AdaptiveSoftmax/HiddenToClusters.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    The adaptive softmax is a type of class-based hierarchical softmax intended to reduce he cost of computing word probabilities over large vocabularies.
</div>

The goal of this project is to create a TensorFlow 2.0+ implementation of the adaptive softmax, outlined in the paper <a href="https://arxiv.org/abs/1609.04309">"Efficient softmax approximation for GPUs" </a>.

For large vocabularies, the final dense layer can become prohibitively expensive, making alternative layers a must. You can easily achieve anywhere from 2x-10x speedups in both training and inference.

The layer consists of two stages
1. A segmentation into a *head* cluster, and $$J$$ *tail* clusters. We perform the usual softmax over the head cluster, and add $$J$$ additional slots for the class probabilities that a word resides in the tail clusters.
2. A dimensional reduction of the final layer hidden state via linear projections, for the inputs feeding into the tail cluster softmaxes. The softmax over tail clusters is only performed for labels that fall in those clusters. When we do have to compute these softmaxes, they are a) much less frequent by defintion, and b) much less computationally intensive due to the dimensional reduction.

A schematic of the projection procedure is shown below:

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <img class="img-fluid rounded z-depth-1" src="{{ '/assets/img/AdaptiveSoftmax/Projections.png' | relative_url }}" alt="" title="example image"/>
    </div>
</div>
<div class="caption">
    Dimensional reduction helps alleviate the burden of computing the probabilties of rare words.
</div>

Again, for the full project description and code, please see my <a href="https://towardsdatascience.com/how-to-overcome-the-large-vocabulary-bottleneck-using-an-adaptive-softmax-layer-e965a534493d">TDS article</a> and <a href="https://github.com/Jmkernes/PAR-Transformer-XL/blob/main/adaptive_softmax.py">repository</a> respectively.
