---
layout: archive
title: "Publications"
permalink: /publications/
author_profile: true
---
<h4> Published </h4>

<u>Kevin Munger, <span style="color:SteelBlue">Ishita Gopal</span>, Jonathan Nagler, and Joshua Tucker. 2020. “Accessibility and Generalizability: Are Digital Media Effects Moderated by Age or Digital Literacy?”</u> (Research & Politics)<br/>
<a href="https://journals.sagepub.com/doi/full/10.1177/20531680211016968"> [Paper] </a> <br/> <br/>    

<u>Taegyoon Kim, Nitheesha Nakka, <span style="color:SteelBlue">Ishita Gopal</span>, Bruce Desmarias, Abigail Mancinelli, Jeffrey J. Harden, Hyein Ko, and Frederick Boehmke. 2021. “Attention to the COVID-19 pandemic on Twitter: Partisan differences among U.S. state legislators” </u>(Legislative Studies Quaterly) <br/> 
<a href="https://onlinelibrary.wiley.com/doi/10.1111/lsq.12367"> [Paper] </a> <br/> <br/>   

<h4> Under Review </h4>




{% if author.googlescholar %}
  You can also find my articles on <u><a href="{{author.googlescholar}}">my Google Scholar profile</a>.</u>
{% endif %}

{% include base_path %}

{% for post in site.publications reversed %}
  {% include archive-single.html %}
{% endfor %}
