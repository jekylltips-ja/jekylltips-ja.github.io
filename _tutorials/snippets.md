---
title: Snippets
heading: Jekyll Snippets
---

便利なJekyllのスニペットコードです：

## Dates

### Month, day, year
{% highlight liquid %}
{% raw %}
{{ page.date | date: "%B %-d, %Y" }}
{% endraw %}
{% endhighlight %}

### Current year
{% highlight liquid %}
{% raw %}
{{ site.time | date: '%Y' }}
{% endraw %}
{% endhighlight %}

## Posts

### Next Post

{% highlight html %}
{% raw %}
{% if page.next %}
  <a href="{{ page.next.url }}">
    {{ page.next.title }}
  </a>
{% endif %}
{% endraw %}
{% endhighlight %}

### List Posts

{% highlight html %}
{% raw %}
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <time>{{ post.date | date: "%B %-d, %Y" }}</time>
    </li>
  {% endfor %}
</ul>
{% endraw %}
{% endhighlight %}

### List Posts by year

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

### Posts Pagination

ページにリンクしていない`<span>`要素か、リンクが有効な`<a>`要素を表示します。
`page1`が無いというJekyllのページネーションの問題を、特別な`if`文（`page == 2`のとき）を使って自動で解決しています。

{% highlight html %}
{% raw %}
<div class="pagination">
  {% if paginator.next_page %}
    <a class="pagination-item older" href="{{ site.baseurl }}page{{ paginator.next_page }}">Older</a>
  {% else %}
    <span class="pagination-item older">Older</span>
  {% endif %}
  {% if paginator.previous_page %}
    {% if paginator.page == 2 %}
      <a class="pagination-item newer" href="{{ site.baseurl }}">Newer</a>
    {% else %}
      <a class="pagination-item newer" href="{{ site.baseurl }}page{{ paginator.previous_page }}">Newer</a>
    {% endif %}
  {% else %}
    <span class="pagination-item newer">Newer</span>
  {% endif %}
</div>
{% endraw %}
{% endhighlight %}

### Post List with Content

{% highlight html %}
{% raw %}
{% for post in site.posts %}
  <article>
    <h1>
      <a href="{{ post.url }}">
        {{ post.title }}
      </a>
    </h1>
    <time>{{ post.date | date: "%B %-d, %Y" }}</time>
    {{ post.content }}
  </article>
{% endfor %}
{% endraw %}
{% endhighlight %}

### Reading Time

postレイアウトに貼り付けてください。レイアウトファイル内で使う場合は`content`を`page.content`に変えてください。

{% highlight liquid %}
{% raw %}
{% capture words %}
  {{ content | number_of_words | minus: 180 }}
{% endcapture %}
{% unless words contains "-" %}
  {{ words | plus: 180 | divided_by: 180 | append: " minutes to read" }}
{% endunless %}
{% endraw %}
{% endhighlight %}

### Related Posts

{% highlight html %}
{% raw %}
<ul>
  {% for post in site.related_posts limit:3 %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <time>{{ post.date | date: "%B %-d, %Y" }}</time>
    </li>
  {% endfor %}
</ul>
{% endraw %}
{% endhighlight %}
