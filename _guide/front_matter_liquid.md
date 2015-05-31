---
title: Front Matter and Liquid
title_ja: Front MatterとLiquid
order: 5
---
Front Matter（フロントマッター）を使うと、ファイルにメタデータをセットすることができます。defaultレイアウトを使うために`index.html`に記述してみましょう。
形式はYAMLで、2つの3連続のダッシュ: `---` の間、そしてファイルの先頭に書きます。

`index.html`の先頭に以下を追加します:

{% highlight yaml %}
---
layout: default
---
{% endhighlight %}

ブラウザを更新すると、サイトの見た目は先ほどと全く同じです。違いは、これからは全てのページで同じレイアウトを使えるようになったということです。

まだ問題があります。全て同じ`<title>`タグはイヤなので、変更する必要があります。Front MatterとLiquid（リキッド）を使って修正することができます。

Liquidは、Jekyllで使われているテンプレート言語です。
すでにレイアウトのセクションにて `{% raw %}{{ content }}{% endraw %}`というものを使いました。

Liquidには2種類の書き方があります:

**Output Markup** - 2つの波括弧を使って、何かしらを出力します。

{% highlight liquid %}
{% raw %}
{{ variable }}
{% endraw %}
{% endhighlight %}


**Tag Markup** - 何も出力しません。基本的にロジックを書くために使われます。Tag markupには波括弧とパーセント記号を使います。

{% highlight liquid %}
{% raw %}
{% if page.variable %}
  Hello
{% endif %}
{% endraw %}
{% endhighlight %}

では、title変数を設定するために、`index.html`にもう1行Front Matterを追加しましょう。
Front Matterは現在このような感じです:

{% highlight yaml %}
---
layout: default
title: Home Page
---
{% endhighlight %}

`default.html`を開き、このように`<title>`タグを置き換えます:

{% highlight liquid %}
{% raw %}
<title>
  {% if page.title %}
    {{ page.title }}
  {% else %}
    Default Page Title
  {% endif %}
</title>
{% endraw %}
{% endhighlight %}

`page`を使うことで、設定したFront Matterを参照できます。ここでは、タイトルがセットされていれば表示され、されていなければ初期表示になるよう確認しましょう。

ページをリロードすると、ページタイトルが _Home Page_となっています。
