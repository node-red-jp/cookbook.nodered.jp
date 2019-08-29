---
layout: default
title: リクエストヘッダをセット
slug:
  - label: http
    url: /#http-requests
  - set header
---

### 課題

HTTPリクエストのヘッダを指定したい。

### 解決

リクエストヘッダに含めたい内容を `msg.headers` フィールドにセットして
<code class="node">HTTP request</code> ノードで送信します。

#### 例

![](/images/http/set-request-header.png)

{% raw %}
~~~json
[{"id":"92272f91.20a43","type":"inject","z":"c9a81b70.8abed8","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":120,"y":760,"wires":[["af92df2f.3032e"]]},{"id":"64da113d.24a75","type":"http request","z":"c9a81b70.8abed8","name":"post to FRED","method":"POST","ret":"txt","url":"http://mike.fred.sensetecnic.com/api/test","tls":"","x":520,"y":760,"wires":[["31ab53be.5111dc"]]},{"id":"af92df2f.3032e","type":"function","z":"c9a81b70.8abed8","name":"set payload and headers","func":"msg.payload = \"data to post\";\nmsg.headers = {};\nmsg.headers['X-Auth-User'] = 'mike';\nmsg.headers['X-Auth-Key'] = 'fred-key';\n\nreturn msg;","outputs":1,"noerr":0,"x":310,"y":760,"wires":[["64da113d.24a75"]]},{"id":"31ab53be.5111dc","type":"debug","z":"c9a81b70.8abed8","name":"","active":true,"console":"false","complete":"false","x":690,"y":760,"wires":[]}]
~~~
{: .flow}
{% endraw %}

この例では、FREDというNode-REDのクラウドサービス内のプライベートなHTTP inputノードを呼び出す際に
リクエストヘッダに `X-Auth-User` と `X-Auth-Key` をセットしています。

次に示す <code class="node">Function</code> ノードのコードでは、
`msg.headers` オブジェクトに追加のメッセージフィールドを追加し、
その中にリクエストヘッダのフィールドと値をセットしています。

{% raw %}
~~~text
msg.payload = "data to post";
msg.headers = {};
msg.headers['X-Auth-User'] = 'mike';
msg.headers['X-Auth-Key'] = 'fred-key';
return msg;
~~~
{: .javascript}
{% endraw %}
