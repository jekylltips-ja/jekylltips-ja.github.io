---
source: https://github.com/mdo/jekyll-snippets/blob/master/pages-nav.html
author:
  name: MDO
  link: https://github.com/mdo
title: Site Navigation
category: Pages
---

Front Matterに`layout: page`が書かれているページを使って、動的にリンクを生成します。URL名からアルファベット順に並びます。

{% highlight liquid %}
{% raw %}
{% assign pages_list = site.pages | sort:"url" %}
{% for node in pages_list %}
  {% if node.title != null %}
    {% if node.layout == "page" %}
      <a class="{% if page.url == node.url %}active{% endif %}" href="{{ node.url }}">
        {{ node.title }}
      </a>
    {% endif %}
  {% endif %}
{% endfor %}
{% endraw %}
{% endhighlight %}
