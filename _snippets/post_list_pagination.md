---
source: https://github.com/mdo/jekyll-snippets/blob/master/posts-pagination.html
author:
  name: MDO
  link: https://github.com/mdo
title: Post List Pagination
category: Posts
---

ページにリンクしていない`<span>`要素か、リンクが有効な`<a>`要素を表示します。
`page1`が無いというJekyllのページネーションの問題を、特別な`if`文（`page == 2`のとき）を使って自動で解決しています。

{% highlight html %}
{% raw %}
<div class="pagination">
  {% if paginator.next_page %}
    <a class="pagination-item older" href="{{ site.baseurl }}page{{ paginator.next_page }}">Older</a>
  {% else %}
    <span class="pagination-item older">Older</span>
  {% endif %}
  {% if paginator.previous_page %}
    {% if paginator.page == 2 %}
      <a class="pagination-item newer" href="{{ site.baseurl }}">Newer</a>
    {% else %}
      <a class="pagination-item newer" href="{{ site.baseurl }}page{{ paginator.previous_page }}">Newer</a>
    {% endif %}
  {% else %}
    <span class="pagination-item newer">Newer</span>
  {% endif %}
</div>
{% endraw %}
{% endhighlight %}
