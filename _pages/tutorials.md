---
layout: archive
title: Tutorials
permalink: /tutorials/
subtitle: This is what keeps my mind busy
---

&ndash; [Collecting messages from Telegram using Telegram’s API and Python](https://medium.com/@ishitagopal/collecting-messages-from-telegram-using-telegrams-api-and-python-5d7e4a9286b2) 

This tutorial illustrates how to use the `Telethon` library in Python to collect messages from any public channel or group chat on Telegram. It covers basics of concurrent and asynchronous programming, required for utilizing the Telethon package effectively. The tutorial also shows how to extract and save the retrieved responses and employs k-means clustering to analyze topics discussed in the posts collected from the official channel of the [New York Times](https://telegram.me/nytimes).

<div style="display: inline-block; width: 40%; margin-bottom: 0.5em;">
    <img src="{{ishitagopal.github.io}}/images/telegramapi_func.png" style="width: 100%;">
</div>

<div style="clear: both;"></div>

<div style="page-break-after: always; visibility: hidden;"> \pagebreak </div>

&ndash; <a href="../files/network_visualization.html" target="_blank">Creating Network Visualizations Using tidygraph And ggraph</a>

This tutorial shows how to use `tidygraph`, and `ggraph` libraries in R to create informative and 'pretty' network visualizations. I visualize the Twitter follower-followee connections among 4000+ state legislators in the US, comprising ~160,000 ties. 

<div style="display: inline-block; width: 40%; margin-bottom: 0.5em;">
    <img src="{{ishitagopal.github.io}}/images/net_tut_party.png" style="width: 100%;">
</div>

&ndash; [Evaluating Zero-Shot Learning for Political Advertisement Classification](https://colab.research.google.com/drive/1wz0btgzCYdXPzwVcuxCpRFiiJV13u7AZ?usp=sharing)

In this tutorial, I evaluate the performance of zero-shot learning for classification tasks using Facebook's BART model to identify "Attack" and "Promote" ads. Unlike traditional classification models, zero-shot models do not require any training data. I implement these models with the `transformers` library, experiment with prompts to improve performance and evaluate the model on an expert labeled dataset of 10,000 advertisements. 

<div style="display: inline-block; width: 33%; margin-bottom: 0.5em;">
    <img src="{{ishitagopal.github.io}}/images/balanced_acc.png" style="width: 100%;">
</div> 

&ndash; [Few Shot Text Classification with SetFit](https://colab.research.google.com/drive/1DEX3-CxxoZi-2j6N54sUjF_BNxqk3ICq#scrollTo=n4FhH0b-y2wB)
In this post, I evaluated Few Shot Learning using [SetFit](https://huggingface.co/docs/setfit/en/index) to classify political advertisements into: "promote," "attack," and "contrast" ads. I fine-tuned sentence embedding model and the classifier with just 100 samples per class, achieving an impressive balanced accuracy of 85%. The findings highlight the effectiveness of Few Shot Learning in delivering high classification performance with limited labeled data!

<div style="display: inline-block; width: 33%; margin-bottom: 0.5em;">
    <img src="{{ishitagopal.github.io}}/images/Setfit_fewShot.png" style="width: 100%;">
</div> 


&ndash; [Zero-shot Text Classification with SetFit](https://colab.research.google.com/drive/1-tO8BRHArDWqZwe5hviqWfzxzyDBbDBs#scrollTo=W-cv1hCioeFe)
Although SetFit is primarily designed for few-shot learning, it can be adapted for situations where no labeled data is available by generating "synthetic examples" that mimic the target classification task. This approach enables training the model on artificially created data. In this post, I evaluate SetFit's performance on an advertisement classification task. I walk through the steps, which involve creating synthetic examples using 20+ templates, training the model, and evaluating the results.



&ndash; [Text Classification in Python](https://github.com/IshitaGopal/TRIADS_workshops/blob/main/Introduction_to_TextAnalysis/TextAnalysis_session_4.ipynb)

<div style="display: inline-block; width: 33%; margin-bottom: 0.5em;">
    <img src="{{ishitagopal.github.io}}/images/textclass_svm.png" style="width: 100%;">
</div>
<div style="display: inline-block; width: 33%; margin-bottom: 0.5em;">
    <img src="{{ishitagopal.github.io}}/images/textclass_rf.png" style="width: 100%;">
</div>
<div style="display: inline-block; width: 33%; margin-bottom: 0.5em;">
    <img src="{{ishitagopal.github.io}}/images/textclass_roc.png" style="width: 100%;">
</div>

<div style="clear: both;"></div>

<div style="page-break-after: always; visibility: hidden;"> \pagebreak </div>

&ndash; [Some notes on Computer Assisted Keyword Search](https://fast-zenith-918.notion.site/Some-notes-on-Computer-assisted-keyword-search-9e787f82ae0f43259cf24052a431cb2c)




