---
title: Photo Gallery
title_ja: フォトギャラリー
order: 11
---
Collectionsを使って、特定の順番でコンテンツを表示させることは、そう簡単ではありません。
もちろん、アルファベット順に並べたい場合ならシンプルですが、順番を決めたいときはどうすればよいでしょうか？

コレクションのFront Matterで順序や重み付けをした変数を使い、並べ替える、という方法も1つあるでしょう。しかし、別の方法を使いましょう: YAMLの配列です。

Portfolioページは、この方法を使うよい機会です。クライアントにPortfolioページ内で項目や順序を管理させたいですね。

`portfolio.html`を開くと、ポートフォリオ内の各項目のためのHTML要素が繰り返されているのがわかるでしょう。
お気づきのように、メンテナンスしやすいよう、サイトから重複を削除していきましょう。

まず、Front Matterにポートフォリオのデータを追加します:

{% highlight yaml %}
---
layout: default
title: Portfolio
portfolio:
  - image_path: /img/portfolio/1.jpg
    category: Web Design
    project: Apple
    url: http://apple.com
  - image_path: /img/portfolio/2.jpg
    category: SEO
    project: Tesla
    url: http://www.teslamotors.com/
  - image_path: /img/portfolio/3.jpg
    category: SEO
    project: Optimizely
    url: https://www.optimizely.com/
  - image_path: /img/portfolio/4.jpg
    category: Content Writing
    project: GitHub
    url: https://github.com/
  - image_path: /img/portfolio/5.jpg
    category: Web Design
    project: Dropbox
    url: https://www.dropbox.com
  - image_path: /img/portfolio/6.jpg
    category: Social Media Marketing
    project: Uber
    url: https://www.uber.com/
---
...
{% endhighlight %}

This sets an array called `portfolio`. Each item in the array has a hash which has four keys: `image_path`, `category`, `project` and `url`.
この配列のセットを`portfolio`と名付けます。
配列内のそれぞれの項目は、このような4つのキーを持つハッシュで構成されています: `image_path`、`category`、`project`そして`url`です。

Front Matterの項目に対して、Liquidの繰り返し処理を使い、HTMLの重複を置き換えていきましょう:

{% highlight html %}
{% raw %}
...
{% for item in page.portfolio %}
  <div class="col-lg-4 col-sm-6">
    <a href="{{ item.url }}" class="portfolio-box">
      <img src="{{ item.image_path }}" class="img-responsive" alt="{{ item.project }}">
      <div class="portfolio-box-caption">
        <div class="portfolio-box-caption-content">
          <div class="project-category text-faded">
            {{ item.category }}
          </div>
          <div class="project-name">
            {{ item.project }}
          </div>
        </div>
      </div>
    </a>
  </div>
{% endfor %}
...
{% endraw %}
{% endhighlight %}

現在、`portfolio.html`はこのような感じです:
{% highlight html %}
{% raw %}
---
layout: default
title: Portfolio
portfolio:
  - image_path: /img/portfolio/1.jpg
    category: Web Design
    project: Apple
    url: http://apple.com
  - image_path: /img/portfolio/2.jpg
    category: SEO
    project: Tesla
    url: http://www.teslamotors.com/
  - image_path: /img/portfolio/3.jpg
    category: SEO
    project: Optimizely
    url: https://www.optimizely.com/
  - image_path: /img/portfolio/4.jpg
    category: Content Writing
    project: GitHub
    url: https://github.com/
  - image_path: /img/portfolio/5.jpg
    category: Web Design
    project: Dropbox
    url: https://www.dropbox.com
  - image_path: /img/portfolio/6.jpg
    category: Social Media Marketing
    project: Uber
    url: https://www.uber.com/
---
<section class="bg-dark">
  <div class="text-center">
    <h1>Portfolio</h1>
  </div>
</section>

<section class="no-padding" id="portfolio">
  <div class="container-fluid">
    <div class="row no-gutter">
      {% for item in page.portfolio %}
      <div class="col-lg-4 col-sm-6">
        <a href="{{ item.url }}" class="portfolio-box">
          <img src="{{ item.image_path }}" class="img-responsive" alt="{{ item.project }}">
          <div class="portfolio-box-caption">
            <div class="portfolio-box-caption-content">
              <div class="project-category text-faded">
                {{ item.category }}
              </div>
              <div class="project-name">
                {{ item.project }}
              </div>
            </div>
          </div>
        </a>
      </div>
      {% endfor %}
    </div>
  </div>
</section>
{% endraw %}
{% endhighlight %}

**Visual Editor**に移動すると、クライアントは以下のものを確認するでしょう。**Settings**サイドバーの**Update Portfolios**をクリックです。

順番を入れ替えたり、削除したり、更新、そしてここから新しいポートフォリオ項目を追加できます。

![Settings](/img/guide/photogallery/settings.png)
