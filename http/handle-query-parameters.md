---
layout: default
title: HTTPエンドポイントに渡されたクエリパラメータのハンドル
slug:
  - label: HTTP
    url: /#http-endpoints
  - クエリパラメータ
---

### 課題

次に示すような、HTTPエンドポイントに渡されたクエリパラメータにアクセスしたい。

    http://example.com/hello-query?name=Nick

### 解決

<code class="node">HTTP In</code> ノードに送信されたメッセージの `msg.req.query` プロパティを使用して、
パラメータにアクセスします。

#### 例

![](/images/http/handle-query-parameters.png)

{% raw %}
~~~json
[{"id":"b34dd1af.4cb23","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Hello {{req.query.name}}!</h1>\n    </body>\n</html>","x":290,"y":180,"wires":[["b828f6a6.47d708"]]},{"id":"1052941d.efad6c","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-query","method":"get","swaggerDoc":"","x":120,"y":180,"wires":[["b34dd1af.4cb23"]]},{"id":"b828f6a6.47d708","type":"http response","z":"3045204d.cfbae","name":"","x":430,"y":180,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl http://localhost:1880/hello-query?name=Nick
<html>
    <head></head>
    <body>
        <h1>Hello Nick!</h1>
    </body>
</html>
~~~
{: .shell}

### 議論

`msg.req.query` プロパティは各クエリパラメータのキーと値のペアで成り立つオブジェクトです。

上記の例での `/hello-query?name=Nick&colour=blue` へのリクエストの結果は次に示すものを含みます。

~~~json
{
    "name": "Nick",
    "colour": "blue"
}
~~~

同じ名前の複数のクエリパラメータがある場合は、配列になります。
たとえば、`/hello-query?colour=blue&colour=red` は次のとおりです。

~~~json
{
    "colour": ["blue","red"]
}
~~~
