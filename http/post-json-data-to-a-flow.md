---
layout: default
title: フローにJSONをPOST
slug:
  - label: HTTP
    url: /#http-endpoints
  - JSONをPOST
---

### 課題

フローにJSONをPOSTしたい。

### 解決

<code class="node">HTTP In</code> ノードを使用して
`Content-Type` に `application/json` を持ったPOSTリクエストを待ち受けます。
そして、`msg.payload` のプロパティとして変換されたJSONにアクセスします。

#### 例

![](/images/http/post-form-data-to-a-flow.png)

{% raw %}
~~~json
[{"id":"5b98a8ac.a46758","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-form","method":"post","swaggerDoc":"","x":120,"y":820,"wires":[["bba61009.4459f"]]},{"id":"bba61009.4459f","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Hello {{ payload.name }}!</h1>\n    </body>\n</html>","x":290,"y":820,"wires":[["6ceb930a.93146c"]]},{"id":"6ceb930a.93146c","type":"http response","z":"3045204d.cfbae","name":"","x":430,"y":820,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl -X POST -d '{"name":"Nick"}' -H "Content-type: application/json" http://localhost:1880/hello-form
<html>
    <head></head>
    <body>
        <h1>Hello Nick!</h1>
    </body>
</html>
~~~
{: .shell}

### 議論

<code class="node">HTTP In</code> ノードは、
`Content-Type` ヘッダに `application/json` がセットされているリクエストを受け取ると、
リクエストボディを変換し、データを `msg.payload` を通じて利用できるようにします。

~~~javascript
var name = msg.payload.name;
~~~
