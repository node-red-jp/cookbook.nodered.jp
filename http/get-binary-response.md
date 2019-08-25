---
layout: default
title: バイナリレスポンスを得る
slug:
  - label: http
    url: /#http-requests
  - binary
---

### 課題

HTTPリクエストからバイナリのHTTPレスポンスを得たい。

### 解決

<code class="node">HTTP Request</code> ノードのデフォルトの動作は、レスポンスボディを文字列として `msg.payload` で返します。
ノードの設定の `出力形式` を `バイナリバッファ` に変更して、バイナリバッファとして `msg.payload` を返すようにします。

#### 例

![](/images/http/get-binary-response.png){:width="556px"}

{% raw %}
~~~json
[{"id":"871ee927.0d69c8","type":"inject","z":"c9a81b70.8abed8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":240,"y":660,"wires":[["8ea4e52a.03d678"]]},{"id":"8ea4e52a.03d678","type":"http request","z":"c9a81b70.8abed8","name":"binary http request","method":"GET","ret":"bin","url":"http://localhost:1880/binary","tls":"","x":410,"y":660,"wires":[["70309d0c.4dc504"]]},{"id":"70309d0c.4dc504","type":"debug","z":"c9a81b70.8abed8","name":"","active":true,"console":"false","complete":"false","x":590,"y":660,"wires":[]}]
~~~
{: .flow}
{% endraw %}

上記の例は[リクエスト先URLをセットするレシピ](set-request-url.html)から
<code class="node">HTTP Request</code> ノードの `出力形式` 設定を `バイナリバッファ` に変更したものです。
<code class="node">Debug</code> ノードも、下記のようにpayloadをバイナリバッファで表示します:

{% raw %}
~~~text
[ 80, 75, 3, 4, 20, 0, 6, 0, 8, 0 … ]
~~~
{% endraw %}
