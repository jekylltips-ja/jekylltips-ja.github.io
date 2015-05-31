---
title: GitHub
order: 7
---
この段階で、あなたはGitとGitHubの使い方を知っていると思います。
もしこれらのツールに慣れていなければ、15分で学べる、GitHubのすばらしい[スタートガイド](https://try.github.io/)があります

新しくリポジトリを作成し、ウェブサイトのソースコードをGitHubにpushしましょう。
ひとつ注意することは、`_site`フォルダはリポジトリに含めなくても良いです。これは簡単に生成できますので。

I use this `.gitignore` file for Jekyll websites:
Jekyllサイトでは、私はこのような`.gitignore`ファイルを使っています:

{% highlight text %}
_site/
.sass-cache/
.jekyll-metadata
{% endhighlight %}
