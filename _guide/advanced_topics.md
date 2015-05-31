---
title: Advanced Topics
title_ja: 高度な項目
order: 13
---
このガイドをできるだけわかりやすくするために、いくつかの項目については簡単にだけ触れました。
この高度なセクションでは、それらの項目についてより理解を深めるチャンスです。

### Collections vs Front Matter vs Dataファイル

Jekyllにはデータを追加する方法がたくさんあるので、どれを使えばよいの？と混乱したと思います。
いつもはっきりしているわけではないですが、決定のために私たちはいくつかのルールを使っています:

* データがすでにCSV、YML、JSON形式であり、それらのファイル形式のファイルに情報をためている場合、Dataファイルを使います。
* 一般的な順序付けではない方法でデータを並べたい、また特定の1ページでしかそのデータを必要としないなら、Front Matterの配列を使います。
* その他はCollectionを使います。Collectionsは最も柔軟な機能であり、たいてい良い選択です。

### RSS Feed

ブログはふつう、リーダーなどに内容を送るためのRSSフィードを持っています。RSSフィードをJekyllブログに追加する方法は超簡単です。

`feed.xml`ファイルを以下の内容で、ウェブサイトルートに作成します:

{% highlight xml %}
{% raw %}
---
---
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0" xmlns:atom="http://www.w3.org/2005/Atom">
  <channel>
    <title>{{ site.name | xml_escape }} - Articles</title>
    <description>{% if site.description %}{{ site.description | xml_escape }}{% endif %}</description>
    <link>
    {{ site.url }}</link>
    {% for post in site.posts %}
      {% unless post.link %}
      <item>
        <title>{{ post.title | xml_escape }}</title>
        {% if post.excerpt %}
          <description>{{ post.excerpt | xml_escape }}</description>
        {% else %}
          <description>{{ post.content | xml_escape }}</description>
        {% endif %}
        <pubDate>{{ post.date | date: "%a, %d %b %Y %H:%M:%S %z" }}</pubDate>
        <link>
        {{ site.url }}{{ post.url }}</link>
        <guid isPermaLink="true">{{ site.url }}{{ post.url }}</guid>
      </item>
      {% endunless %}
    {% endfor %}
  </channel>
</rss>
{% endraw %}
{% endhighlight %}

ここでは、空のFront Matterが重要です。なぜなら、これはJekyllにLiquidを使うことを伝えるからです。
このスクリプトでは、全てのブログ記事を通して、RSS XML形式で出力します。

`site.name`のような、いくつかの設定すべき変数がありますね。`_config.yml`を開いて以下のYAMLを追加してください:

{% highlight yaml %}
name: Creative Agency
description: We are a creative agency who provides web design, SEO, content and social services.
url: http://creative.com
{% endhighlight %}

We just need to add a link to the RSS feed to `<head>` in `_layouts/default.html`:

{% highlight html %}
...
<link rel="alternate" type="application/rss+xml" title="My Site RSS" href="/feed.xml" />
...
{% endhighlight %}

### Collectionsでページを生成する

このガイドの前半で出てきたサービスの例で、サービスごとのページを生成したいと思ったかもしれません。Jekyllなら簡単にできます。

`_config.yml`を変更します:

{% highlight yaml %}
collections:
  services:
    output: true
defaults:
  -
    scope:
      path: ""
      type: "services"
    values:
      layout: "post"
{% endhighlight %}

それぞれのCollectionの項目にレイアウトを設定する必要もあるでしょう。
それぞれのページに追加するかわりに、Servicesコレクションに対してdefaultレイアウトを設定しました。

`services.html`に作成したページへのリンクを追加するだけです:

{% highlight html %}
{% raw %}
...
{% for service in site.services %}
  <div class="col-lg-3 col-md-6 text-center">
    <div class="service-box">
      <img src="{{ service.image_path }}" alt="{{ service.title }}"/>
      <h3><a href="{{ service.url }}">{{ service.title }}</a></h3>
      <p class="text-muted">{{ service.content | truncatewords: 10 }}</p>
    </div>
  </div>
{% endfor %}
...
{% endraw %}
{% endhighlight %}

フィルター: `{% raw %}{{ service.content | truncatewords: 10 }}{% endraw %}`を使うことで、ページに10単語しか表示しないよう制限しています。
フィルターはコンテンツの出力に、少し手を加える事ができます。[こちら](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)でさらに多くのフィルターが探せます。

### ページネーション

数えるのが大変になってくるほど、ブログの記事数が増えてきたとします。
Jekyllではページネーションを使って、ブログの記事一覧を、複数ページに切り分けることができます。

ページネーションを有効にするには**_config.yml**を開き、paginate変数を設定します。こちらは1ページにいくつ記事を表示するかというものです:

{% highlight yaml %}
...
paginate: 5
paginate_path: "/blog/page:num"
...
{% endhighlight %}

`blog.html`を`/blog/index.html`に変更する必要もあります。
これは、ページネーションのURLがうまく動作するために必要です。URLは`/blog/page2`のようになるでしょう。

次に、先ほどファイルを移動したので、`_includes/nav.html`のリンクを変更する必要があります。

{% highlight html %}
{% raw %}
...
<li {% if page.url == "/blog/index.html" %} class="active" {% endif %}>
  <a href="/blog/">Blog</a>
</li>
...
{% endraw %}
{% endhighlight %}

Finally, we need to update `/blog/index.html` to use pagination:

{% highlight html %}
{% raw %}
---
layout: default
title: Blog
---
<section class="bg-dark">
  <div class="text-center">
    <h1>Blog</h1>
  </div>
</section>

<section>
  <div class="container">
    <div class="row">
      <div class="text-center">
        <ul style="list-style-position: inside">
           {% for post in paginator.posts %}
             <li>
               <a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%d %B %Y" }}
             </li>
           {% endfor %}
        </ul>
      </div>
    </div>
  </div>
</section>

<nav>
  <ul class="pager">
    {% if paginator.previous_page %}
      <li><a href="{{ paginator.previous_page_path }}">Previous</a></li>
    {% endif %}

    {% if paginator.next_page %}
      <li><a href="{{ paginator.next_page_path }}">Next</a></li>
    {% endif %}
  </ul>
</nav>
{% endraw %}
{% endhighlight %}

Jekyllは、複数ページに分割するために`paginator`変数を使えるようにしてくれます。前/次というリンクもページの下部で追加しています。

### よりよいナビゲーション

より少ないコードで重複を無くしたナビゲーションを作る1つの方法は、他のソースからリンクを引っ張ってくることです。。
CSVファイルを使ってやってみましょう。

`_data/nav.csv`を以下の内容で作成します:

{% highlight text %}
name,link
About,/about.html
Services,/services.html
Portfolio,/portfolio.html
Blog,/blog/index.html
Contact,/contact.html
{% endhighlight %}

そして`_includes/nav.html`内で、このCSVに対して繰り返し処理をします:

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

### まとめ

願わくば、このガイドが、よい基礎の獲得に役立てばと思います。これから、美しいJekyllサイトとともに、インターネットに飛び出しましょう。

もし行き詰まったり、助けが必要なときは、Jekyllの公式サイトにとてもすばらしい[ドキュメント](http://jekyllrb.com/docs/home/)があります。
[talk.jekyllrb.com](http://talk.jekyllrb.com)というコミュニティもすばらしい教材です。

Happy Jekylling!
