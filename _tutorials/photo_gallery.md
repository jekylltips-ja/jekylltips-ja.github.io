---
title: Photo Gallery
title_ja: フォトギャラリー
heading: Photo Galleries in Jekyll
heading_ja: Jekyllでフォトギャラリー
---

フォトギャラリーは基本的に多くの重複したコードでできています。
ウェブサイトをメンテナンスしやすくするために、重複したコードを無くす良いアイデアをご紹介します。

この内容を通して、技術に詳しくないユーザーでもスライドショーのコンテンツを簡単に更新できるようにもしていきます。

このチュートリアルでは、[ShapeBootstrap](http://shapebootstrap.net/)から[Corlate](http://shapebootstrap.net/download?id=439)テンプレートを使います。

コードエディタで`index.html`を開き、スライドショーのHTMLコードを探してください。

はじめは、各スライドのための要素を持った`carousel-indicators`というクラス付きの`ol`要素です。
これはいくつスライドがあるか、またどれがactiveか、を表示するドットを表示します。今はこの部分をコメントアウトして、後で戻ってきましょう。

{% highlight html %}
{% raw %}
...
<!--<ol class="carousel-indicators">
    <li data-target="#main-slider" data-slide-to="0" class="active"></li>
    <li data-target="#main-slider" data-slide-to="1"></li>
    <li data-target="#main-slider" data-slide-to="2"></li>
</ol>-->
...
{% endraw %}
{% endhighlight %}

We need to pull out the content for each slide and put it in Front Matter. So what content does each slide have? I can see:
各スライドのコンテンツを抜き出し、Front Matterに記述する必要があります。では、各スライドにはどんなコンテンツがあるでしょうか？以下があります：

* 背景画像
* メイン見出し
* サブ見出し
* Read Moreのリンク
* ポップアップ画像

こちらに1つめのスライドのコンテンツを書き出しました：

{% highlight html %}
{% raw %}
...
<div class="item active" style="background-image: url(images/slider/bg1.jpg)"> <!-- Background Image -->
  <div class="container">
    <div class="row slide-margin">
      <div class="col-sm-6">
        <div class="carousel-content">
          <h1 class="animation animated-item-1">Lorem ipsum dolor sit amet consectetur adipisicing elit</h1> <!-- Main Heading -->
          <h2 class="animation animated-item-2">Accusantium doloremque laudantium totam rem aperiam, eaque ipsa...</h2> <!-- Sub Heading -->
          <a class="btn-slide animation animated-item-3" href="#">Read More</a> <!-- Read More Link -->
        </div>
      </div>

      <div class="col-sm-6 hidden-xs animation animated-item-4">
        <div class="slider-img">
            <img src="images/slider/img1.png" class="img-responsive"> <!-- Pop up Image -->
        </div>
      </div>

    </div>
  </div>
</div>
...
{% endraw %}
{% endhighlight %}

次にFront Matterに各スライドのコンテンツを記述します。

YAMLで配列を指定するには`-`を使うことでできます：

{% highlight yaml %}
{% raw %}
my_array:
  - item 1
  - item 2
  - item 3
  - item 4
{% endraw %}
{% endhighlight %}

以下のようにすることで、各要素を持つ配列のハッシュの作成もできます：

{% highlight yaml %}
{% raw %}
my_array:
  - name: Product 1
    price: $20
    weight: 40kg
  - name: Product 2
    price: $78
    weight: 34kg
  - name: Product 3
    price: $12
    weight: 14kg
  - name: Product 4
    price: $99
    weight: 90kg
{% endraw %}
{% endhighlight %}

では、私たちのスライドショーに適用しましょう。

以下を`index.html`の一番上に追加します：

{% highlight liquid %}
{% raw %}
---
slideshows:
  - background_image_path: /images/slider/bg1.jpg
    main_heading: First Slide
    sub_heading: This is the sub heading for the first slide
    read_more_link: http://google.com
    pop_up_image_path: /images/slider/img1.png
  - background_image_path: /images/slider/bg2.jpg
    main_heading: Second Slide
    sub_heading: This is the sub heading for the second slide
    read_more_link: /blog.html
    pop_up_image_path: /images/slider/img2.png
  - background_image_path: /images/slider/bg3.jpg
    main_heading: Third Heading
    sub_heading: This is the sub heading for the third slide
    read_more_link: /portfolio.html
    pop_up_image_path: /images/slider/img4.png
---
{% endraw %}
{% endhighlight %}

そして`<div class="carousel-inner">`を以下のように置き換えます：

{% highlight liquid %}
{% raw %}
...
<div class="carousel-inner">
  {% for item in page.slideshow %}
    <div class="item {% if forloop.index == 1 %}active{% endif %}" style="background-image: url({{ item.background_image_path }})">
      <div class="container">
        <div class="row slide-margin">
          <div class="col-sm-6">
            <div class="carousel-content">
              <h1 class="animation animated-item-1">{{ item.main_heading }}</h1>
              <h2 class="animation animated-item-2">{{ item.sub_heading }}</h2>
              <a class="btn-slide animation animated-item-3" href="{{ item.read_more_link }}">Read More</a>
            </div>
          </div>

          <div class="col-sm-6 hidden-xs animation animated-item-4">
            <div class="slider-img">
              <img src="{{ item.pop_up_image_path }}" class="img-responsive">
            </div>
          </div>

        </div>
      </div>
    </div><!--/.item-->
  {% endfor %}
</div><!--/.carousel-inner-->
...
{% endraw %}
{% endhighlight %}

これはfront matterに設定した配列に対して繰り返し処理をしており、前と同じかたちでデータを出力しています。

ひとつトリッキーな部分は、forloop変数を使って最初のスライドにactive classを設定する必要があるところです：

{% highlight liquid %}
{% raw %}
...
{% if forloop.index == 1 %}active{% endif %}
...
{% endraw %}
{% endhighlight %}

もし、サイトを[CloudCannon](http://cloudcannon.com)にアップロードしている場合（`_config.yml`を作成するのを忘れないでくださいね）、
技術に詳しくないユーザーでも簡単に内容を変更でき、スライドショーを並び替え、追加/削除ができます。

![CloudCannon Front Matter](/img/tutorials/slideshow/cloudcannon.png)

すべてが動作しているので、carousel indicatorsに戻りましょう。各スライドに`<li>`を追加して、ひとつめのスライドにactive classを追加します。

`<ol class="carousel-indicators">`のコメントアウトを解除し、以下のものに置き換えます：

{% highlight html %}
{% raw %}
...
<ol class="carousel-indicators">
    {% for item in page.slideshow %}
      <li data-target="#main-slider" data-slide-to="{{ forloop.index0 }}" {% if forloop.index == 1 %}class="active"{% endif %}></li>
    {% endfor %}
</ol>
...
{% endraw %}
{% endhighlight %}
