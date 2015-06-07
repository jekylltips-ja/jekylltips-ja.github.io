---
title: SEO
heading: SEO in Jekyll
heading_ja: JekyllでのSEO
---
Wordpressを使っていたユーザーからの、よくある質問が、JekyllではどのようにSEOをするかということです。
Jekyllでは、Wordpressのプラグインでできていた以上に、もっとSEOをコントロールすることができます。

このチュートリアルの基礎として、Googleがまとめた[Search Engine Optimization
Starter Guide](http://static.googleusercontent.com/media/www.google.com/en/us/webmasters/docs/search-engine-optimization-starter-guide.pdf)を参考にしようと思います。（[日本語版はこちら](http://static.googleusercontent.com/media/www.google.co.jp/ja/jp/intl/ja/webmasters/docs/search-engine-optimization-starter-guide-ja.pdf)）

`<title>`と`<meta>`descriptionタグを設定し、URL構造を改善し、サイトマップとカスタム404ページの追加を行っていきましょう。

### タイトルとサイトの説明

Front Matterを使うことで、簡単に編集できる`<title>`と`<meta>`descriptionタグを作ることができます。

`<title>`には、Front Matterに変数があればタイトルとして出力し、なければデフォルトのものを使います：

{% highlight liquid %}
{% raw %}
...
<title>
  {% if page.title %}
    {{ page.title }}
  {% else %}
    Default Page Title
  {% endif %}
</title>
...
{% endraw %}
{% endhighlight %}

`<meta>`descriptionには、Front Matterに変数があったときだけ出力しましょう：

{% highlight liquid %}
{% raw %}
...
{% if page.description %}
  <meta name="description" content="{{ page.description}}" />
{% endif %}
...
{% endraw %}
{% endhighlight %}

このように各HTMLページのFront Matterにこれらの変数を設定しましょう：

{% highlight liquid %}
{% raw %}
---
title: Creative Hub - Blog
description: A blog with the latest trends and news in Web Design
---
...
{% endraw %}
{% endhighlight %}

もしあなたが[CloudCannon](http://cloudcannon)をお使いなら、技術に詳しくないユーザーでもFront Matterを簡単に変更できます：

![Front Matter on CloudCannon](/img/tutorials/seo/front_matter.png)


### URL構造を改善する

Permalinks（パーマリンク）を使えば、JekyllでどのようにURLを構築するかコントロールできます。
SEOのために、Googleは説明的な単語をURLに使うことを推奨しています。

`/red.html`というHTMLページがあることを宣言しましょう。ですが、`/cups/red/`というURLにしたいとき、Front Matterに書かれたpermalinkはこのようになります：

{% highlight liquid %}
{% raw %}
---
permalink: /cups/red/
---
...
{% endraw %}
{% endhighlight %}

ファイル名を`/red.html`から`/cups/red/index.html`に変更することでも可能ですが、Jekyllはお好きな方法でサイトを構成する力を与えてくれます。

さらに便利な例は、ブログ記事のpermalinkを変更するものです。`_config.yml`に以下の行を追加するだけです：

{% highlight yaml %}
{% raw %}
...
permalink: /blog/:year/:month/:day/:title/
...
{% endraw %}
{% endhighlight %}


コレクション全体のpermalinkも変更することができます：

{% highlight yaml %}
{% raw %}
...
collections:
  products:
    output: true
    permalink: "/:collection/:title/"
...
{% endraw %}
{% endhighlight %}

### サイトマップ

[David Ensinger](http://davidensinger.com/2013/11/building-a-better-sitemap-xml-with-jekyll/)のスクリプトを使って、
サイトマップを生成することができます。

以下の内容で`/sitemap.xml`を作ります：

{% highlight liquid %}
{% raw %}
---
layout: null
sitemap:
  exclude: 'yes'
---
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  {% for post in site.posts %}
    {% unless post.published == false %}
    <url>
      <loc>{{ site.url }}{{ post.url }}</loc>
      {% if post.sitemap.lastmod %}
        <lastmod>{{ post.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
      {% elsif post.date %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
      {% endif %}
      {% if post.sitemap.changefreq %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
      {% else %}
        <changefreq>monthly</changefreq>
      {% endif %}
      {% if post.sitemap.priority %}
        <priority>{{ post.sitemap.priority }}</priority>
      {% else %}
        <priority>0.5</priority>
      {% endif %}
    </url>
    {% endunless %}
  {% endfor %}

  {% for page in site.pages %}
    {% assign split_path = page.path | split: "." %}
    {% assign extension = split_path | last %}

    {% if page.sitemap.exclude != "yes" and extension == "html" %}
    <url>
      <loc>{{ site.url }}{{ page.permalink | remove: "index.html" }}</loc>
      {% if page.sitemap.lastmod %}
        <lastmod>{{ page.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
      {% elsif page.date %}
        <lastmod>{{ page.date | date_to_xmlschema }}</lastmod>
      {% else %}
        <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
      {% endif %}
      {% if page.sitemap.changefreq %}
        <changefreq>{{ page.sitemap.changefreq }}</changefreq>
      {% else %}
        <changefreq>monthly</changefreq>
      {% endif %}
      {% if page.sitemap.priority %}
        <priority>{{ page.sitemap.priority }}</priority>
      {% else %}
        <priority>0.3</priority>
      {% endif %}
    </url>
    {% endif %}
  {% endfor %}

  {% for collection in site.collections %}
    {% if collection[1].output %}
      {% for doc in collection[1].docs %}
        <url>
          <loc>{{ site.url }}{{ doc.url | remove: "index.html" }}</loc>
          {% if doc.sitemap.lastmod %}
            <lastmod>{{ doc.sitemap.lastmod | date: "%Y-%m-%d" }}</lastmod>
          {% elsif doc.date %}
            <lastmod>{{ doc.date | date_to_xmlschema }}</lastmod>
          {% else %}
            <lastmod>{{ site.time | date_to_xmlschema }}</lastmod>
          {% endif %}
          {% if doc.sitemap.changefreq %}
            <changefreq>{{ doc.sitemap.changefreq }}</changefreq>
          {% else %}
            <changefreq>monthly</changefreq>
          {% endif %}
          {% if doc.sitemap.priority %}
            <priority>{{ doc.sitemap.priority }}</priority>
          {% else %}
            <priority>0.3</priority>
          {% endif %}
        </url>
      {% endfor %}
    {% endif %}
  {% endfor %}
</urlset>
{% endraw %}
{% endhighlight %}

これはデフォルトでウェブサイトのすべての記事、ページを含んでいます。ページの除外や、優先度（priority）の設定がFront Matterを使うことでできます：

{% highlight liquid %}
{% raw %}
sitemap:
  lastmod: 2015-01-23
  priority: 0.7
  changefreq: monthly
  exclude: yes
{% endraw %}
{% endhighlight %}

`_config.yml`にウェブサイトのドメインを追加する必要もあります：

{% highlight yaml %}
{% raw %}
...
url: http://mysite.com
...
{% endraw %}
{% endhighlight %}

### カスタム404ページ

これはさらに詳細なベストプラクティスです。Jekyllでのカスタム404ページは、ユーザーが存在しないページにアクセスしてしまったとき、案内をして助けることができます。
404ページの内容とともに`404.html`を作ります。Front Matter、liquidとレイアウトが使えます。

[GitHub Pages](https://pages.github.com), [CloudCannon](http://cloudcannon.com)やJekyll Serverは、ページが見つからない時に404.htmlを表示します。
その他のサービスの場合はカスタム404ページを機能させるために設定が必要でしょう。

### コンテンツでのTips

良いコンテンツはSEOの鍵です。少しのシンプルなルールによって、サイトのコンテンツが高い検索エンジンの順位を実現することを確実にできます。

* 検索エンジンは新しいコンテンツを大変好む
* 検索エンジンは重複したコンテンツを好まない
* 画像には常に[alt属性](http://www.w3schools.com/tags/att_img_alt.asp)をつける
* リンクには説明的なテキストを使う： _悪い例_：Google SEOスターターガイドは[こちらをクリック](http://static.googleusercontent.com/media/www.google.com/en/us/webmasters/docs/search-engine-optimization-starter-guide.pdf) 。 _良い例_：[Google SEOスターターガイド](http://static.googleusercontent.com/media/www.google.com/en/us/webmasters/docs/search-engine-optimization-starter-guide.pdf)をダウンロード。
* もしページを削除した場合は、適切で関連のある場所へリダイレクトを作成する。[CloudCannon](http://cloudcannon)で[301 redirects](http://docs.cloudcannon.com/#common_tasks6_301_redirectshtml)を使うことで可能です。
