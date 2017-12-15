---
layout: page
titles:
  en: Résumé
  zh: 简历
  zh-Hans: 简历
key: page-about
lans: C#:5,Java:5,JavaScript:4,PHP:2,Matlab:2,Ada:1
tools: Git:5,Agile Scrum:5,Docker:4,Tableau:4,Relational databases:3,Travis/TeamCity:3
vis: D3.js:5,LeafletJS:3,Voreen:2,VolView:2,Mapbox:2
os: Unix-based:5,Android:2
services: Tiny Tiny RSS:4,Huginn:4,Plex:3,aria2:3,WebDAV:3,Hubot:2,Squid:2
---

Hi, I'm Henry, a **Programmer** :computer:, a **Data Visualiser** :bar_chart:, a **Photographer** :camera: and an **Entrepreneur** :department_store:. 


I feel comfortable collaborating in multicultural and diverse-background 🌏 environments, with plethora of skills I'm able to complete everything that's being tasked to me.

## Work

Company |Country |Title |Year 
[Swansea University](http://www.swansea.ac.uk/){:target="_blank"} |UK 🇬🇧 |Software Developer |Nov 2017 - **Present** 
[Swansea University](http://www.swansea.ac.uk/){:target="_blank"} |UK 🇬🇧 |Software Developer Intern |Jun 2017 - Sep 2017 
[LeEco](https://www.letv.com/){:target="_blank"} |China 🇨🇳 |Java Developer Intern |Apr 2015 – July 2015
[TiViCloud](http://www.tivicloud.cn/){:target="_blank"} |China 🇨🇳 |Java Developer Intern |Sep 2014 – Oct 2014
[Singapore Land Authority](https://www.sla.gov.sg/){:target="_blank"} |Singapore 🇸🇬 |Geospatial Intern| June 2014 – Sep 2014

## Skill

I know many skills across different proficiency levels.
<div class="m-tags">
<ul class="inline-list">
{%- assign skills = page.lans | split: ',' -%}
{%- assign i = skills | size -%}
  {% for skill in skills %}
  {%- assign pair = skill | split: ':' -%}
    <li>
     <button type="button" class="js-article-tag skill-{{ pair[1] }} round-rect-button"
       onclick="javascript:void(0)">
         <span>&nbsp; {{ pair[0] }} &nbsp;</span>
      </button>
    </li>
  {% endfor %}
</ul>
</div>

<div class="m-tags">
<ul class="inline-list">
{%- assign skills = page.tools | split: ',' -%}
{%- assign i = skills | size -%}
  {% for skill in skills %}
  {%- assign pair = skill | split: ':' -%}
    <li>
     <button type="button" class="js-article-tag skill-{{ pair[1] }} round-rect-button"
       onclick="javascript:void(0)">
         <span>&nbsp; {{ pair[0] }} &nbsp;</span>
      </button>
    </li>
  {% endfor %}
</ul>
</div>

<div class="m-tags">
<ul class="inline-list">
{%- assign skills = page.vis | split: ',' -%}
{%- assign i = skills | size -%}
  {% for skill in skills %}
  {%- assign pair = skill | split: ':' -%}
    <li>
     <button type="button" class="js-article-tag skill-{{ pair[1] }} round-rect-button"
       onclick="javascript:void(0)">
         <span>&nbsp; {{ pair[0] }} &nbsp;</span>
      </button>
    </li>
  {% endfor %}
</ul>
</div>

<div class="m-tags">
<ul class="inline-list">
{%- assign skills = page.os | split: ',' -%}
{%- assign i = skills | size -%}
  {% for skill in skills %}
  {%- assign pair = skill | split: ':' -%}
    <li>
     <button type="button" class="js-article-tag skill-{{ pair[1] }} round-rect-button"
       onclick="javascript:void(0)">
         <span>&nbsp; {{ pair[0] }} &nbsp;</span>
      </button>
    </li>
  {% endfor %}
</ul>
</div>

I host services on my debian cloud server that simplify and automate my life, you can [request for access](mailto:henry@wangqiru.com).
<div class="m-tags">
<ul class="inline-list">
{%- assign skills = page.services | split: ',' -%}
{%- assign i = skills | size -%}
  {% for skill in skills %}
  {%- assign pair = skill | split: ':' -%}
    <li>
     <button type="button" class="js-article-tag tag-{{ pair[1] }} round-rect-button"
       onclick="javascript:void(0)">
         <span>&nbsp; {{ pair[0] }} &nbsp;</span>
      </button>
    </li>
  {% endfor %}
</ul>
</div>

## Education

Institution |Country |Major |Year |Result
[Swansea University](http://www.swansea.ac.uk/){:target="_blank"} |UK 🇬🇧 |Msc. Advanced Computer Science |2015 - 2017 |***Distinction***
[Nanyang Polytechnic](http://www.nyp.edu.sg/){:target="_blank"} |Singapore 🇸🇬 |Dip. Information Technology |2012 - 2015 |GPA 3.69/4.00 
[Stanford University](https://www.coursera.org/learn/machine-learning){:target="_blank"}[^1]|US 🇺🇸 |Machine Learning|2016 |[Certificate](https://www.coursera.org/account/accomplishments/records/QE9LANFCQYVE){:target="_blank"}
[Wharton School](https://www.coursera.org/specializations/wharton-entrepreneurship){:target="_blank"}[^1]|US 🇺🇸 |Entrepreneurship|2016 |[Certificate 1](https://www.coursera.org/account/accomplishments/records/GCCLDS6M5JAS){:target="_blank"} [Certificate 2](https://www.coursera.org/account/accomplishments/records/N99U8UYLY44T){:target="_blank"}
[Princeton University](https://www.coursera.org/learn/algorithms-part1){:target="_blank"}[^1]|US 🇺🇸 |Algorithms |2016 | -null pointer-[^2]

## Testimonial

- For my masters dissertation, my supervisors said:
> #### This is perhaps the best taught MSc project I have ever seen.  It's excellent work.
Dr. Robert S. Laramee, Associate Professor, Swansea University
> #### Of a very high standard is the handling of the special situations, with its many details, and the integration into visualisation methods. A complete and useful software tool has been produced.
Dr. Oliver Kullmann, Associate Professor, Swansea University

- For my academic performance, my lecturers said:
> #### He was very driven and strived hard in his study. He worked well with team and was able to achieve goals set out by the project.
Mr. Law Chee Yong, Senior Lecturer, Nanyang Polytechnic
> #### He is hard working and has a positive attitude towards his studies. He has always scored good grades.
Mr. Loh Kuan Pang, Senior Lecturer, Nanyang Polytechnic

- For my internship performance, my supervisors said:
> #### He has demonstrated excellent learning attitude, vast knowledge and skills on programming. He also displayed initiative by constantly seeking feedback.
Mr. Justin Chua, Principal GeoSpatial Consultant, Singapore Land Authority

## Volunteer

Organisation |Country |Contribution |Year
[Food from the Heart](https://www.foodheart.org/){:target="_blank"} |Singapore 🇸🇬|A [web portal](https://www.foodheart.org/vportal/){:target="_blank"}  for Food Stock, Delivery Schedule and Volunteer Schedule Management [^3]| Apr 2014 – Sep 2014


### For more details, please [email me](mailto:henry@wangqiru.com) or visit my [LinkedIn profile](https://www.linkedin.com/in/wangqiru).


##### Footnote
[^1]: Course was taken on [Coursera](https://www.coursera.org/), a learning platform offers online courses from universities and organisations. As of Oct 2017, Coursera has more than 28 million registered users and more than 2,000 courses.

[^2]: This course does not offer a certificate upon completion.

[^3]: The project is actively maintained by students from Nanyang Polytechnic.