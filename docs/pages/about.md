---
layout: page
title: About
description: 编码改变世界
keywords: Jagtu, iOS, Swift, Flutter
comments: true
menu: 关于
permalink: /about/
---

我是Jagtu，曾经有一份优雅的代码摆在我面前，但是我没有珍惜，等我失去后才后悔莫及，尘世间最痛苦的事情莫过于此。如果上天能够给我一个再来一次的机会，我一定要对这段代码加上注释。如果非要在这份注释上加一个要求，我希望是。。。。。。再花个流程图！

优雅的人生，编码的艺术，熟能生巧，努力改变人生。

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
<li>
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ site.url }}/assets/images/bio-photo.jpg" alt="闷骚的程序员" />
</li>
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
