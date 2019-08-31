---
layout: default
title: リクエスト先URLにテンプレートを使用
slug:
  - label: http
    url: /#http-requests
  - set url
---

### 課題

リクエストによってURLの一部のみが変更になるような、HTTPリクエストのURLを動的に設定したい。

### 課題

[mustache](http://mustache.github.io/mustache.5.html) URLテンプレートを使用して
URLを動的に設定するよう <code class="node">HTTP Request</code> ノードを設定します。

#### 例

![](/images/http/set-request-url-template.png)

{% raw %}
~~~json
[{"id":"41747a17.54ffd4","type":"http request","z":"c9a81b70.8abed8","name":"","method":"GET","ret":"txt","url":"https://jsonplaceholder.typicode.com/posts/{{post}}","tls":"","x":550,"y":480,"wires":[["d682318c.36823"]]},{"id":"d682318c.36823","type":"debug","z":"c9a81b70.8abed8","name":"","active":true,"console":"false","complete":"payload","x":710,"y":480,"wires":[]},{"id":"90bfea22.dd2b98","type":"inject","z":"c9a81b70.8abed8","name":"post id","topic":"","payload":"2","payloadType":"str","repeat":"","crontab":"","once":false,"x":250,"y":480,"wires":[["e67a0cc.596d4f"]]},{"id":"e67a0cc.596d4f","type":"change","z":"c9a81b70.8abed8","name":"","rules":[{"t":"set","p":"post","pt":"msg","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":390,"y":480,"wires":[["41747a17.54ffd4"]]}]
~~~
{: .flow}
{% endraw %}

このフローでは <code class="node">Inject</code> ノードがリクエストするAPIに送信するpostのidを送ります。
<code class="node">Change</code> ノードはこれを `msg.post` に代入します。
<code class="node">HTTP Request</code> ノードは、次に示すように設定されたURLプロパティを `msg.post` で置換することにより送信先のURLを生成します。

{% raw %}
~~~text
https://jsonplaceholder.typicode.com/posts/{{post}}
~~~
{% endraw %}

The JSON output from this API in the debug panel will look as follows:

{% raw %}
~~~text
{
  "userId": 1,
  "id": 2,
  "title": "qui est esse",
  "body": "est rerum tempore vitae\nsequi sint nihil reprehenderit dolor beatae ea dolores neque\nfugiat blanditiis voluptate porro vel nihil molestiae ut reiciendis\nqui aperiam non debitis possimus qui neque nisi nulla"
}
~~~
{% endraw %}

#### 議論

デフォルトでは、mustacheは値の中のHTMLエンティティをエスケープします。
URLエスケープが効かないようにするには `{% raw %}{{{triple}}}{% endraw %}` のように中括弧を使います。
