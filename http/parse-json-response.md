---
layout: default
title: レスポンスをJSONにパース
slug:
  - label: http
    url: /#http-requests
  - parse json
---

### 課題

HTTPリクエストに対して、パースされたJavaScriptオブジェクトとしてのJSONレスポンスを返したい。

### 解決

<code class="node">HTTP Request</code> ノードはデフォルトで、`msg.payload`に文字列としてJSONレスポンスを返します。
このノードの `出力形式` 設定を `JSON オブジェクト` に変更して、
後続のノードがアクセスしやすいよう `msg.payload` をJSONのレスポンスにします。

#### 例

![](/images/http/parse-json-response.png)

{% raw %}
~~~json
[{"id":"14c60a10.794df6","type":"http request","z":"c9a81b70.8abed8","name":"","method":"GET","ret":"obj","url":"https://jsonplaceholder.typicode.com/posts/{{post}}","tls":"","x":390,"y":500,"wires":[["b4ea8dd4.61a05"]]},{"id":"b4ea8dd4.61a05","type":"debug","z":"c9a81b70.8abed8","name":"","active":true,"console":"false","complete":"payload.title","x":570,"y":500,"wires":[]},{"id":"3479192a.04f016","type":"inject","z":"c9a81b70.8abed8","name":"post id","topic":"","payload":"2","payloadType":"str","repeat":"","crontab":"","once":false,"x":90,"y":500,"wires":[["e69250cf.368fd"]]},{"id":"e69250cf.368fd","type":"change","z":"c9a81b70.8abed8","name":"","rules":[{"t":"set","p":"post","pt":"msg","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":230,"y":500,"wires":[["14c60a10.794df6"]]}]
~~~
{: .flow}
{% endraw %}

上記の例は[リクエスト先URLをセットするレシピ](set-request-url.html)から
<code class="node">HTTP Request</code> ノードの設定を変更したものです。
<code class="node">Debug</code> ノードも、JSONレスポンスの `title` プロパティだけを表示するよう変更されています。

{% raw %}
~~~text
"qui est esse"
~~~
{% endraw %}

### 議論

XMLでレスポンスがほしい場合は、<code class="node">XML</code> ノードを使用して、JSONからXMLに変換します。
