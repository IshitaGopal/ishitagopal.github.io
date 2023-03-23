---
layout: archive
title: "Research"
permalink: /research/
author_profile: true
---
<h2> Published </h2>

Kevin Munger, **Ishita Gopal**, Jonathan Nagler, and Joshua Tucker. 2020. “Accessibility and Generalizability: Are Digital Media Effects Moderated by Age or Digital Literacy?” (Research & Politics)

Published in [*Research & Politics*](https://journals.sagepub.com/doi/full/10.1177/20531680211016968), 2021
&nbsp;&nbsp;&nbsp;<details>
    <summary> Abstract </summary>
    An emerging empirical regularity suggests that older people use and respond to social media very differently than younger people. Older people are the fastest-growing population of Internet and social media users in the US, and this heterogeneity will soon become central to online politics. However, many important experiments in this field have been conducted on online samples that do not contain enough older people to be useful to generalize to the current population of Internet users; this issue is more pronounced for studies that are even a few years old. In this paper, we report the results of replicating two experiments involving social media (specifically, Facebook) conducted on one such sample lacking older users (Amazon’s Mechanical Turk) using a source of online subjects which does contain sufficient variation in subject age. We add a standard battery of questions designed to explicitly measure digital literacy. We find evidence of significant treatment effect heterogeneity in subject age and digital literacy in the replication of one of the two experiments. This result is an example of limitations to generalizability of research conducted on samples where selection is related to treatment effect heterogeneity; specifically, this result indicates that Mechanical Turk should not be used to recruit subjects when researchers suspect treatment effect heterogeneity in age or digital literacy, as we argue should be the case for research on digital media effects.

</details>

Taegyoon Kim, Nitheesha Nakka, **Ishita Gopal**, Bruce Desmarias, Abigail Mancinelli, Jeffrey J. Harden, Hyein Ko, and Frederick Boehmke. 2021. “Attention to the COVID-19 pandemic on Twitter: Partisan differences among U.S. state legislators” (Legislative Studies Quaterly) <br/> 
Published in [*Legislative Studies Quaterly*](https://onlinelibrary.wiley.com/doi/10.1111/lsq.12367)
<details>
<summary> Abstract </summary>
  Subnational governments in the United States have taken the lead on many aspects of the response to the COVID-19 pandemic. Variation in government activity across states offers the opportunity to analyze responses in comparable settings. We study a common and informative activity among state officials—state legislators’ attention to the pandemic on Twitter. We find that legislators’ attention to the pandemic strongly correlates with the number of cases in the legislator’s state, the national count of new deaths, and the number of pandemic-related public policies passed within the legislator’s state. Furthermore, we find that the degree of responsiveness to pandemic indicators differs significantly across political parties, with Republicans exhibiting weaker responses, on average. Lastly, we find significant differences in the content of tweets about the pandemic by Democratic and Republican legislators, with Democrats focused on health indicators and impacts, and Republicans focused on business impacts and opening the economy.
</details>

<h2> Under Review </h2>

<span style="color:SteelBlue">Ishita Gopal</span>, Taegyoon Kim, Nitheesha Nakka and Bruce Desmarias. 2020. “Modeling State Legislator Networks on Twitter” <br/> 

<details>
<summary> Abstract </summary>

<i>Abstract</i>: A lot of attention has been paid to studying the online activity of the members of the United States congress. This scrutiny has not been extended to state legislators. Very few studies exist which catalogue why state legislators connect and communicate with one another online in the ways they do. Inspired by this question and building on studies which have analysed online communication of members of the national legislatures, this paper aims to systematically analyse state legislator relationships in the online environment. We collect original data for 4000+ legislators and study patterns of connection and communication of state legislators on Twitter. The results from this study will help better understand what motivates tie formation in the online environment and if these patterns of connection conform to or can predict offline relationships. We test the impact of variables such as party affiliation, state, chamber, cohort, race, gender, professionalism and policy area focus in the organisation of these online networks. We look at three main types of networks that can arise due to participation on Twitter - follower, retweets and mentions. We also aggregate the ties to infer dynamics between states.    
  
</details>


[\[Slides\]]({{ BASE_PATH}}/files/ppt/PolNet_2021.pdf) 


<img src="{{ishitagopal.github.io}}/images/follower_net.png" style="display: block; margin: auto;" />
<img src="{{ishitagopal.github.io}}/images/mentions_net.png" style="display: block; margin: auto;" />
<img src="{{ishitagopal.github.io}}/images/rt_net.png" style="display: block; margin: auto;" />

<h2> Working Papers </h2>
<span style="color:SteelBlue">Ishita Gopal</span>. 2021 “Targeting and the Timing of Online Censorship: The Case of Venezuela.” <br/> 
[\[Slides\]]({{ BASE_PATH}}/pages/ppt/PolMeth_2020.pdf)  

{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{[author.googlescholar](https://scholar.google.com/citations?hl=en&user=7QhrrSYAAAAJ)}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
