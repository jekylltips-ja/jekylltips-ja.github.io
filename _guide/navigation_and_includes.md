---
title: Navigation and Includes
title_ja: ナビゲーションとインクルード
order: 6
---

ウェブサイトの残りを作成していきましょう。このテンプレートはシングルページウェブサイトですので、複数ページのサイトに変更していきましょう。
ナビゲーションに各ページへのリンクを追加するために、HTMLページを作成します。
ウェブサイトのルートで`about.html`、`services.html`、`portfolio.html`そして`contact.html`の作成を済ませてください。

今作成した新しいページは空っぽなので、内容を少し追加しましょう。

`index.html`から、aboutセクションを切り取り、`about.html`に貼り付けましょう。

{% highlight html %}
{% raw %}
<section class="bg-primary" id="about">
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-lg-offset-2 text-center">
        <h2 class="section-heading">We've got what you need!</h2>
        <hr class="light">
        <p class="text-faded">Start Bootstrap has everything you need to get your new website up and running in no time! All of the templates and themes on Start Bootstrap are open source, free to download, and easy to use. No strings attached!</p>
        <a href="#" class="btn btn-default btn-xl">Get Started!</a>
      </div>
    </div>
  </div>
</section>
{% endraw %}
{% endhighlight %}

先頭にFront Matterも追加しましょう。`about.html`はこんな感じです:

{% highlight html %}
{% raw %}
---
layout: default
title: About
---
<section class="bg-dark">
  <div class="text-center">
    <h1>About</h1>
  </div>
</section>

<section class="bg-primary" id="about">
  <div class="container">
    <div class="row">
      <div class="col-lg-8 col-lg-offset-2 text-center">
        <h2 class="section-heading">We&#39;ve got what you need!</h2>
        <hr class="light">
        <p class="text-faded">Start Bootstrap has everything you need to get your new website up and running in no time! All of the templates and themes on Start Bootstrap are open source, free to download, and easy to use. No strings attached!</p>
        <a href="#" class="btn btn-default btn-xl">Get Started!</a>
      </div>
    </div>
  </div>
</section>
{% endraw %}
{% endhighlight %}

他のページにも同様のことを行ってください。

次はナビゲーションを機能させる必要があります。正しいページにリンクされており、ナビゲーションで現在のページがハイライトされるようにしたいですね。
これは少しだけ複雑になってしまうので、ナビゲーション専用のページを作って、そのなかに記述しましょう。
まず、`_includes`フォルダをウェブサイトルートに作成してください。そのなかに、`nav.html`を作成しましょう。

`default.html`内の`<nav>`要素を探し、`nav.html`にコピーしてください。

{% highlight html %}
{% raw %}
<nav id="mainNav" class="navbar navbar-default navbar-fixed-top">
    <div class="container-fluid">
        <!-- Brand and toggle get grouped for better mobile display -->
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand page-scroll" href="#page-top">Start Bootstrap</a>
        </div>

        <!-- Collect the nav links, forms, and other content for toggling -->
        <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
            <ul class="nav navbar-nav navbar-right">
                <li>
                    <a class="page-scroll" href="#about">About</a>
                </li>
                <li>
                    <a class="page-scroll" href="#services">Services</a>
                </li>
                <li>
                    <a class="page-scroll" href="#portfolio">Portfolio</a>
                </li>
                <li>
                    <a class="page-scroll" href="#contact">Contact</a>
                </li>
            </ul>
        </div>
        <!-- /.navbar-collapse -->
    </div>
    <!-- /.container-fluid -->
</nav>
{% endraw %}
{% endhighlight %}

`nav.html`のリンクを修正しましょう。hrefの値を正しいページヘのリンク先にする必要があります。
各セクションは、それぞれ別ページになるので`page-scroll`というクラスは削除できます。

`nav.html`のaboutページヘのリンクはこのようになりました。

{% highlight html %}
{% raw %}
...
<li>
    <a href="/about.html">About</a>
</li>
...
{% endraw %}
{% endhighlight %}

全てのリンクに、同様のことを行ってください。

トップページに戻るためのメインロゴリンクも作る必要があります:

{% highlight html %}
{% raw %}
...
<a class="navbar-brand" href="/">Start Bootstrap</a>
...
{% endraw %}
{% endhighlight %}

現在の`nav.html`はこのようになっています:

{% highlight html %}
{% raw %}
<nav id="mainNav" class="navbar navbar-default navbar-fixed-top">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="/">Start Bootstrap</a>
    </div>
    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav navbar-right">
        <li>
          <a href="/about.html">About</a>
        </li>
        <li>
          <a href="/services.html">Services</a>
        </li>
        <li>
          <a href="/portfolio.html">Portfolio</a>
        </li>
        <li>
          <a href="/contact.html">Contact</a>
        </li>
      </ul>
    </div>
    <!-- /.navbar-collapse -->
  </div>
  <!-- /.container-fluid -->
</nav>
{% endraw %}
{% endhighlight %}

`default.html`のnav要素を次の記述と置き換えてください: `{% raw %}{% include nav.html %}{% endraw %}`。

{% highlight html %}
{% raw %}
...
<body id="page-top">
  {% include nav.html %}
  {{content}}
  <!-- jQuery -->
  <script src="/js/jquery.js"></script>
  <!-- Bootstrap Core JavaScript -->
  <script src="/js/bootstrap.min.js"></script>
  <!-- Plugin JavaScript -->
  <script src="/js/jquery.easing.min.js"></script>
  <script src="/js/jquery.fittext.js"></script>
  <script src="/js/wow.min.js"></script>
  <!-- Custom Theme JavaScript -->
  <script src="/js/creative.js"></script>
</body>
...
{% endraw %}
{% endhighlight %}

`nav.html`で現在のページをハイライトさせるには、現在のページが何か、またナビゲーションのリンクに`active`クラスを追加するための作業が必要です。
たくさんの方法がありますが、今はもっともシンプルな方法を使いましょう。

Jekyll has a variable which has the path to the current page you can reference using `page.url`. Let's use that to compare the current page to the item in the navigation. If it matches we'll add the class:

Jekyllでは`page.url`という変数を参照することで、現在のページURLを取得することができます。
これを使ってナビゲーションの要素が現在のページと同じがチェックするようにしましょう。
もし同じなら、クラスを追加します:

{% highlight html %}
{% raw %}
...
<li {% if page.url == '/about.html' %} class="active" {% endif %}>
  <a href="/about.html">About</a>
</li>
...
{% endraw %}
{% endhighlight %}

`nav.html`の全てのリンクに適用すると、このようになります:

{% highlight html %}
{% raw %}
<nav id="mainNav" class="navbar navbar-default navbar-fixed-top">
  <div class="container-fluid">
    <!-- Brand and toggle get grouped for better mobile display -->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
      <span class="sr-only">Toggle navigation</span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="/">Start Bootstrap</a>
    </div>
    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav navbar-right">
        <li {% if page.url == '/about.html' %} class="active" {% endif %}>
          <a href="/about.html">About</a>
        </li>
        <li {% if page.url == '/services.html' %} class="active" {% endif %}>
          <a href="/services.html">Services</a>
        </li>
        <li {% if page.url == '/portfolio.html' %} class="active" {% endif %}>
          <a href="/portfolio.html">Portfolio</a>
        </li>
        <li {% if page.url == '/contact.html' %} class="active" {% endif %}>
          <a href="/contact.html">Contact</a>
        </li>
      </ul>
    </div>
    <!-- /.navbar-collapse -->
  </div>
  <!-- /.container-fluid -->
</nav>
{% endraw %}
{% endhighlight %}

このテンプレートは、すでに`active`のためのデザインがCSSに含まれていたため、すべきことはこれだけです。

ブラウザを開、サイト中を移動してみてください、現在のページは赤い文字になります。
この時点で、基本的な動作をするウェブサイトができ、クライアントに提供できる状態になりました。

ノンデベロッパーユーザーがウェブサイトを更新できるよう、ホスティングやCMSを追加することについて見ていきましょう。
