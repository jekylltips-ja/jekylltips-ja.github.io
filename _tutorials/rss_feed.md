---
source: https://github.com/snaptortoise/jekyll-rss-feeds/blob/master/feed.articles.xml
author:
  name: snaptortoise
  link: https://github.com/snaptortoise
title: RSS Feed
title_ja: RSSフィード
heading: RSS Feed in Jekyll
heading_ja: JekyllでのRSSフィード
---

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

RSSフィードへのリンクを、`_layouts/default.html`の`<head>`に追加する必要があります。

{% highlight html %}
...
<link rel="alternate" type="application/rss+xml" title="My Site RSS" href="/feed.xml" />
...
{% endhighlight %}
