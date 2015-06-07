---
source: https://github.com/mdo/jekyll-snippets/blob/master/atom.xml
author:
  name: MDO
  link: https://github.com/mdo
title: Atom Feed
title_ja: Atomフィード
heading: Atom Feed in Jekyll
heading_ja: JekyllでのAtomフィード
---

Atomフィードによって、読者はあなたのブログの最新の記事を購読することができます。AtomフィードをJekyllサイトに追加するのは簡単です。

以下の内容で、ウェブサイトルートに`atom.xml`を作ります：

{% highlight xml %}
{% raw %}
---
layout: null
---

<?xml version="1.0" encoding="utf-8"?>
<feed xmlns="http://www.w3.org/2005/Atom">
  <title>{{ site.name }}</title>
  <link href="{{ site.url }}/atom.xml" rel="self" />
  <link href="{{ site.url }}/"/>
  <updated>{{ site.time | date_to_xmlschema }}</updated>
  <id>{{ site.url }}</id>
  <author>
    <name>{{ site.author.name }}</name>
    <email>{{ site.author.email }}</email>
  </author>
  {% for post in site.posts %}
    <entry>
      <title>{{ post.title }}</title>
      <link href="{{ site.url }}{{ post.url }}" />
      <updated>{{ post.date | date_to_xmlschema }}</updated>
      <id>{{ site.url }}{{ post.id }}</id>
      <content type="html">{{ post.content | xml_escape }}</content>
    </entry>
  {% endfor %}
</feed>
{% endraw %}
{% endhighlight %}

このコードは、すべてのブログ記事に繰り返し処理を行い、記事全てをXML形式で出力しています。次に、`_config.yml`にサイト変数を追加しましょう。

{% highlight yaml %}
...
name: Creative Agency
url: http://creative.com
author:
  name: Mike
  email: mike@creative.com
...
{% endhighlight %}

そして最後に、おそらく`_layouts/default.html`にある`<head>`にリンクを追加します：

{% highlight html %}
...
<link rel="alternate" type="application/atom+xml" title="The Creative Blog" href="/atom_feed.xml" />
...
{% endhighlight %}
