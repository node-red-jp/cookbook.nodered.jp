---
layout: default
title: HTTPリクエストヘッダへアクセス
slug:
  - label: HTTP
    url: /#http-endpoints
  - ヘッダ
---

### 課題

送信されたリクエストのHTTPヘッダへアクセスしたい。

### 解決

<code class="node">HTTP In</code> ノードに送信されたメッセージの
`msg.req.headers` プロパティを使用して、ヘッダにアクセスします。

#### 例

![](/images/http/access-http-request-headers.png){:width="800px"}

{% raw %}
~~~json
[{"id":"c1460268.3eba","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-headers","method":"get","swaggerDoc":"","x":130,"y":380,"wires":[["24199456.dbe66c"]]},{"id":"24199456.dbe66c","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>User agent: {{req.headers.user-agent}}</h1>\n    </body>\n</html>","x":310,"y":380,"wires":[["b3531892.4cace8"]]},{"id":"b3531892.4cace8","type":"http response","z":"3045204d.cfbae","name":"","x":450,"y":380,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl http://localhost:1880/hello-headers
<html>
    <head></head>
    <body>
        <h1>User agent: curl&#x2F;7.49.1</h1>
    </body>
</html>
~~~
{: .shell}

### 議論

`msg.req.headers` プロパティは、各リクエストパラメータのキーと値のペアで構成されたオブジェクトです。
ヘッダ名は、リクエストの内容にかかわらず、すべて小文字です。
