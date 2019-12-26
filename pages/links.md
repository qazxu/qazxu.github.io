---
layout: mypost
title: 友情链接
---

欢迎各位朋友与我建立友链，如需友链请发送xuhang5983@163.com邮箱，我收到邮件后会添加上的，本站的友链信息如下

```
名称：{{ site.title }}
描述：{{ site.description }}
地址：{{ site.githubUrl }}
头像：{{ site.githubUrl }}static/img/logo.jpg
```

<ul>
  {% for link in site.links %}
  <li>
    <p><a href="{{ link.url }}" title="{{ link.desc }}" target="_blank" >{{ link.title }}</a></p>
  </li>
  {% endfor %}
</ul>
