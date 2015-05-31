---
title : CMS and Hosting
title_ja: CMSとホスティング
order: 8
---

[CloudCannon](http://cloudcannon.com)はJekyllサイトのためにCMS機能を提供するサービスです。
彼らはホスティングも提供しています。クライアントがウェブサイトを更新できるよう、CloudCannonを使っていきましょう。

**大切なこと**: CloudCannonは`/_config.yml`ファイルがあるかどうかで、Jekyllサイトがどうか判断しています。
`_config.yml`をウェブサイトルートに作成して（現在は空でも大丈夫です）、GitHubにpushしましょう。

[CloudCannon](http://cloudcannon.com)にアクセスし、無料アカウントを作成し、ウェブサイトを作成しましょう。

![サイトを作成](/img/guide/cms/create_site.png)

これでサイトのファイルブラウザが表示されます。現在はファイルが無いので追加していきましょう！
**connect storage provider**ボタンをクリックしてください。

![ダッシュボード](/img/guide/cms/dashboard.png)

先ほどのリポジトリを同期したいので、GitHubという文字の横の**Connect**をクリックして、CloudCannonがあなたのGitHubアカウントにアクセスできるよう許可してください。

![Connect](/img/guide/cms/connect.png)

ウェブサイトのリポジトリと連携します。

![Repo](/img/guide/cms/repo.png)

CloudCannonはあなたのファイルをpullし、ファイルブラウザに表示するでしょう。
CloudCannonで行った変更はすべてGitHubに反映され、またGitHubにpushしたすべての変更もCloudCannonに反映されます。

![Files](/img/guide/cms/files.png)

今、CloudCannonにファイルがあります。いよいよ更新できるエリアを作成しましょう。
`index.html`をクリックすると、サイトのプレビューが表示されます。このプレビューでウェブサイト中を移動できます。
**Code Editor**をクリックし、サイトのソースコードを表示させましょう。

![Preview](/img/guide/cms/preview.png)

HTMLの要素に**editable**暮らすを追加することで、編集可能なエリアを作成することができます。
index pageの見出しにeditableを付けてみましょう:

{% highlight html %}
<header>
  <div class="header-content">
    <div class="header-content-inner">
      <h1 class="editable">Your Favorite Source of Free Bootstrap Themes</h1>
      <hr>
      <p class="editable">Start Bootstrap can help you build better websites using the Bootstrap CSS framework! Just download your template and start going, no strings attached!</p>
      <a href="#about" class="btn btn-primary btn-xl page-scroll">Find Out More</a>
    </div>
  </div>
</header>
{% endhighlight %}

editableクラスを追加する場所は重要です。もし、クライアントにもう少しコントロール権を与えたければ`div`にクラスを付けます。
すると、クライアントはさらに見出しや、箇条書き、画像などを追加できます。

**Visual Editor**に切り替えましょう。editableクラスをつけた要素は、編集できることを示すように、黄色い箱に囲まれています。
編集可能なエリアをクリックし、変更を加えてみましょう。

![Visual Editor](/img/guide/cms/visual.png)

CloudCannonはサイトを閲覧できるよう`*.cloudvent.net`というテストドメインを提供します。
無料プランでは公開状態にできず、閲覧するにはパスワードの設定が必要です。
**Site Setting**に移動し、**Site Password**にてサイトのパスワードを設定しましょう。

![Password](/img/guide/cms/password.png)

ページ上部のcloudventドメインをクリックし、パスワードを入力しましょう。ウェブサイトはインターネットで動いています！

![Cloudvent](/img/guide/cms/cloudvent.png)
