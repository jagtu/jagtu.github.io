---
layout: page
title: 编码改变世界
description: 程序员的世界就是编码
keywords: Jagtu, iOS, Swift, Flutter
comments: true
menu: 关于
permalink: /about/
---



<div>
  <div style="float: left;padding: 10px 20px 10px 1px;">
  <img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ site.url }}/assets/images/bio-photo.jpg" alt="我是程序员" />
  </div>
  <div style="padding: 10px 1px;">
    <p>我是Jagtu，曾经有一份优雅的代码摆在我面前，但是我没有珍惜，等我失去后才后悔莫及，尘世间最痛苦的事情莫过于此。如果上天能够给我一个再来一次的机会，我一定要对这段代码加上注释。如果非要在这份注释上加一个要求，我希望是不要改需求！</p>
		<p>优雅的人生，编码的艺术，熟能生巧，努力改变人生。 </p>
  </div>
</div>



## 联系

<ul>
{% for website in site.data.social %}
<li><a href="{{ website.url }}" target="_blank">{{website.sitename }}</a></li>
{% endfor %}
</ul>




## Skill Keywords

{% for skill in site.data.skills %}
### {{ skill.name }}
<div class="btn-inline">
{% for keyword in skill.keywords %}
<button class="btn btn-outline" type="button">{{ keyword }}</button>
{% endfor %}
</div>
{% endfor %}
