---
title: Output as JSON
title_ja: JSONの出力
heading: Output JSON in Jekyll
heading_ja: JekyllでJSONを出力する
---
時には、データファイルやコレクションからJSON形式で出力をしたいことがあるでしょう。

こちらがJekyllでJSONを出力する方法です：

{% highlight liquid %}
{% raw %}
...
[
  {% for item in products %}
    {
      "title": "{{ item.title | xml_escape }}",
      "description": "{{ item.description | xml_escape }}",
      "type": "{{ item.type | xml_escape }}"
    }
    {% unless forloop.last %},{% endunless %}
  {% endfor %}
]
...
{% endraw %}
{% endhighlight %}
