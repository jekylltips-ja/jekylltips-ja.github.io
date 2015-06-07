---
title: Navigation
title_ja: ナビゲーション
heading: Navigation in Jekyll
heading_ja: Jekyllでのナビゲーション
---
少ない繰り返しでJekyllにナビゲーションを作るひとつの方法は、別のソースからリンクを取ってくるものです。CSVファイルを使ってやってみましょう。

以下の内容で`_data/nav.csv`を作ります：

{% highlight text %}
name,link
About,/about.html
Services,/services.html
Portfolio,/portfolio.html
Blog,/blog/index.html
Contact,/contact.html
{% endhighlight %}

では、ナビゲーション内で、このCSVに繰り返し処理を行い、さらに現在のページならactive classを追加します：

{% highlight html %}
{% raw %}
...
<ul class="nav navbar-nav navbar-right">
  {% for nav_item in site.data.nav %}
    <li {% if page.url == nav_item.link %} class="active" {% endif %}>
      <a href="{{ nav_item.link }}">{{ nav_item.name }}</a>
    </li>
  {% endfor %}
</ul>
...
{% endraw %}
{% endhighlight %}
