---
layout: default
title: フローに生データをPOST
slug:
  - label: HTTP
    url: /#http-endpoints
  - データをPOST
---

### 課題

生のテキストデータをフローにPOSTしたい。

### 解決

<code class="node">HTTP In</code> ノードを使用して
`Content-Type` に `text/plain` を持ったPOSTリクエストを待ち受けます。
そして、`msg.payload` としてPOSTされたデータにアクセスします。

#### 例

![](/images/http/post-raw-data-to-a-flow.png)

{% raw %}
~~~json
[{"id":"3e1c5107.c1e3ae","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-raw","method":"post","swaggerDoc":"","x":120,"y":920,"wires":[["cf679478.309868"]]},{"id":"cf679478.309868","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Hello {{ payload }}!</h1>\n    </body>\n</html>","x":290,"y":920,"wires":[["f3c1a3f0.0c3e6"]]},{"id":"f3c1a3f0.0c3e6","type":"http response","z":"3045204d.cfbae","name":"","x":430,"y":920,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl -X POST -d 'Nick' -H "Content-type: text/plain" http://localhost:1880/hello-raw
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
`Content-Type` ヘッダに `text/plain` がセットされているリクエストを受け取ると、
リクエストボディを `msg.payload` として利用できるようにします。

~~~javascript
var name = msg.payload;
~~~
