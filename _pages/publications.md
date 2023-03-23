---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---
<h3> Published </h4>

<u>Kevin Munger, <span style="color:SteelBlue">Ishita Gopal</span>, Jonathan Nagler, and Joshua Tucker. 2020. “Accessibility and Generalizability: Are Digital Media Effects Moderated by Age or Digital Literacy?”</u> (Research & Politics)<br/>
<a href="https://journals.sagepub.com/doi/full/10.1177/20531680211016968"> [Paper] </a> <br/> <br/>    

<u>Taegyoon Kim, Nitheesha Nakka, <span style="color:SteelBlue">Ishita Gopal</span>, Bruce Desmarias, Abigail Mancinelli, Jeffrey J. Harden, Hyein Ko, and Frederick Boehmke. 2021. “Attention to the COVID-19 pandemic on Twitter: Partisan differences among U.S. state legislators” </u>(Legislative Studies Quaterly) <br/> 
<a href="https://onlinelibrary.wiley.com/doi/10.1111/lsq.12367"> [Paper] </a> <br/> <br/>   

<h3> Under Review </h4>

<u><span style="color:SteelBlue">Ishita Gopal</span>. 2021 “Targeting and the Timing of Online Censorship: The Case of Venezuela.”</u> <br/> 
[\[Slides\]]({{ BASE_PATH}}/pages/ppt/PolMeth_2020.pdf)  

<u><span style="color:SteelBlue">Ishita Gopal</span>, Taegyoon Kim, Nitheesha Nakka and Bruce Desmarias. 2020. “Modeling State Legislator Networks on Twitter”</u><br/> 

<i>Abstract</i>: A lot of attention has been paid to studying the online activity of the members of the United States congress. This scrutiny has not been extended to state legislators. Very few studies exist which catalogue why state legislators connect and communicate with one another online in the ways they do. Inspired by this question and building on studies which have analysed online communication of members of the national legislatures, this paper aims to systematically analyse state legislator relationships in the online environment. We collect original data for 4000+ legislators and study patterns of connection and communication of state legislators on Twitter. The results from this study will help better understand what motivates tie formation in the online environment and if these patterns of connection conform to or can predict offline relationships. We test the impact of variables such as party affiliation, state, chamber, cohort, race, gender, professionalism and policy area focus in the organisation of these online networks. We look at three main types of networks that can arise due to participation on Twitter - follower, retweets and mentions. We also aggregate the ties to infer dynamics between states.    
[\[Slides\]]({{ BASE_PATH}}/pages/ppt/PolNet_2021.pdf) 


<img src="{{ishitagopal.github.io}}/images/follower_net.png" style="display: block; margin: auto;" />
<img src="{{ishitagopal.github.io}}/images/mentions_net.png" style="display: block; margin: auto;" />
<img src="{{ishitagopal.github.io}}/images/rt_net.png" style="display: block; margin: auto;" />

<img src="../pages/research_img/follower_net.png" style="float: left; width: 30%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="../pages/research_img/mentions_net.png" style="float: left; width: 30%; margin-right: 1%; margin-bottom: 0.5em;">
<img src="../pages/research_img/rt_net.png" style="float: left; width: 30%; margin-right: 1%; margin-bottom: 0.5em;">



{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
