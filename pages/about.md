---
layout: page
title: About
description: 
keywords: 
comments: true
menu: 关于
permalink: /about/
---

你好，我是蜜柑。

一个名不见经传的小开发者，曾经在某宠物医疗类软件公司做PM，做过一段时间的开发，技术层次仅限于「增删改查」。

但是我坚信 Steve Jobs 的一句话：「Stay hungry, stay foolish.」。

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
{% if site.url contains 'mazhuang.org' %}
<li>
微信公众号：<br />
<img style="height:192px;width:192px;border:1px solid lightgrey;" src="{{ site.url }}/assets/images/qrcode.jpg" alt="闷骚的程序员" />
</li>
{% endif %}
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
