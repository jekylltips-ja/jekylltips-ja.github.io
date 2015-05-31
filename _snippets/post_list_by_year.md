---
source: https://github.com/mdo/jekyll-snippets/blob/master/posts-archive-by-year.html
author:
  name: MDO
  link: https://github.com/mdo
title: Post List by Year
category: Posts
---

年だけに分解して、記事一覧に表示します。

{% highlight html %}
{% raw %}
{% for post in site.posts %}
  {% capture currentyear %}{{ post.date | date: "%Y" }}{% endcapture %}
  {% if currentyear != year %}
    {% unless forloop.first %}</ul>{% endunless %}
    <h1>{{ currentyear }}</h1>
    <ul>
    {% capture year %}{{ currentyear }}{% endcapture %}
  {% endif %}
  <li><a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
{% endraw %}
{% endhighlight %}
