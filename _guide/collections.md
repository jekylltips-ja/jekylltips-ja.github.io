---
title: Collections
order: 10
---
次は、[Collections](http://jekyllrb.com/docs/collections/)を見ていきましょう。
Collectionsはコンテンツの関係性をまとめるための、すばらしい方法です。Postsと動作は似ていますが、日付を必要としません。

Servicesページ内で、HTMLの繰り返しを減らすために、Collectionを使っていきましょう。

各サービスは同じようなパターンの形式をしています。画像があり、タイトルと説明があります。
現時点ではServiceページのアイコンはフォントですので、クライアントの方が自身で簡単に更新できるよう、画像に変更しましょう。

まず、Jekyllにコレクションについて伝える必要があります。`_config.yml`に以下を追加してください:

{% highlight yaml %}
collections:
  - services
{% endhighlight %}

そして、`_services`というフォルダをウェブサイトルートに作成してください。
（CloudCannonで編集している場合は、まずファイルをつくってからフォルダに移動させるんでしたね。覚えておいてください。）

フォルダのなかに、`web_design.md`というファイルを以下の内容で作成してください:

{% highlight text %}
---
title: Web Design
image_path: ""
---

Beautiful, clean designs tailored to your business
{% endhighlight %}

Front Matterを使って、サービスの情報を定義し、マークダウンファイルに説明を書いていきます。

CloudCannonのCollectionsタブに移動すると、Servecesコレクションがあることがわかるでしょう。

![Collections](/img/guide/collections/collections.png)

Web Designの項目を開きましょう。ビジュアルエディタを使って、簡単に項目を管理できます。
Web Designに画像を追加しましょう。[こちらでダウンロードできる](/flaticons_squidink.zip)無料のフラットアイコンセットを使っていきましょう。

**Add Image Path**をクリックして、適切なアイコンをアップロードしましょう。

![Add Image](/img/guide/collections/add_image.png)

続けてもう3つのサービスも追加しましょう。**Content Writing**、**SEO**、**Social Media Marketing**を追加しました。

次に、`services.html`でこれらのサービスを表示させる必要があります。
Jekyllでは、`site.services`という変数を使うことで、サービスコレクションが使えるようになります。

Liquidと全てのサービスに対する繰り返し処理を使って、現在の静的なHTMLを置き換え、詳細を表示しましょう:

{% highlight html %}
{% raw %}
...
{% for service in site.services %}
  <div class="col-lg-3 col-md-6 text-center">
    <div class="service-box">
      <img src="{{ service.image_path }}" alt="{{ service.title }}"/>
      <h3>{{ service.title }}</h3>
      <p class="text-muted">{{ service.content }}</p>
    </div>
  </div>
{% endfor %}
...
{% endraw %}
{% endhighlight %}

とてもシンプルですね。`site.services`に対して、forループの繰り返し処理を行っただけです。
これによって、さきほど作成したFront Matterの変数を出力することができます。

`services.html`の全体像は、現在以下のようなになります:
{% highlight html %}
{% raw %}
---
layout: default
title: Services
---
<section class="bg-dark">
  <div class="text-center">
    <h1>Services</h1>
  </div>
</section>

<section id="services">
  <div class="container">
    <div class="row">
      <div class="col-lg-12 text-center">
        <h2 class="section-heading">At Your Service</h2>
        <hr class="primary">
      </div>
    </div>
  </div>

  <div class="container">
    <div class="row">
      {% for service in site.services %}
        <div class="col-lg-3 col-md-6 text-center">
          <div class="service-box">
            <img src="{{ service.image_path }}" alt="{{ service.title }}"/>
            <h3>{{ service.title }}</h3>
            <p class="text-muted">{{ service.content }}</p>
          </div>
        </div>
      {% endfor %}
    </div>
  </div>
</section>
{% endraw %}
{% endhighlight %}

これだけです！これで、クライアントはCollectionsを使い、簡単にサイトを管理できるようになります。

![Final](/img/guide/collections/final.png)
