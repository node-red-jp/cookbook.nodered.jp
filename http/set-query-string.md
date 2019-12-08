---
layout: default
title: URLにクエリストリングパラメータを設定
slug:
  - label: http
    url: /#http-requests
  - クエリストリングを設定
---

### 課題

HTTPリクエストに利用するURLのクエリストリングパラメータを設定したい場合。

### 解決

URLにクエリパラメータ文字列を直接代入するため、<code class="node">HTTP Request</code>ノードの[mustache](http://mustache.github.io/mustache.5.html)サポートを利用します。

#### 例

![](/images/http/set-query-string.png)

{% raw %}
~~~json
[{"id":"e95c6faa.ab2e1","type":"http request","z":"c9a81b70.8abed8","name":"","method":"GET","ret":"txt","url":"https://query.yahooapis.com/v1/public/yql?q={{{query}}}&format=json","tls":"","x":470,"y":420,"wires":[["7cf30700.5bc978"]]},{"id":"7cf30700.5bc978","type":"debug","z":"c9a81b70.8abed8","name":"","active":true,"console":"false","complete":"payload","x":630,"y":420,"wires":[]},{"id":"637d3c55.eb3084","type":"inject","z":"c9a81b70.8abed8","name":"query parameter","topic":"","payload":"select astronomy.sunset from weather.forecast where woeid in (select woeid from geo.places(1) where text=\"maui, hi\")","payloadType":"str","repeat":"","crontab":"","once":false,"x":120,"y":420,"wires":[["b001d489.d8f818"]]},{"id":"b001d489.d8f818","type":"change","z":"c9a81b70.8abed8","name":"","rules":[{"t":"set","p":"query","pt":"msg","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":300,"y":420,"wires":[["e95c6faa.ab2e1"]]}]
~~~
{: .flow}
{% endraw %}

<code class="node">Inject</code>ノードはURLとして送られるクエリストリングを生成します。<code class="node">Change</code>ノードはこれを、<code class="node">HTTP Request</code>ノードのURLプロパティに以下のようにmustacheテンプレートとして代入する`msg.query`に移動します:

{% raw %}
~~~text
https://query.yahooapis.com/v1/public/yql?q={{{query}}}&format=json
~~~
{% endraw %}

返却されたJSONコンテンツはハワイの日没時刻です:

{% raw %}
~~~text
"{"query":{"count":1,"created":"2017-01-22T01:31:07Z","lang":"en-US","results":{"channel":{"astronomy":{"sunset":"6:9 pm"}}}}}"
~~~
{% endraw %}


#### 議論

デフォルトでは、mustacheは代入する値の中のHTMLエンティティをエスケープします。URL中にHTMLエスケープがおこなわれないゆおにするには、`{% raw %}{{{triple}}}{% endraw %}`のように3重の波括弧を利用します。
