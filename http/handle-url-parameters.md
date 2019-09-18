---
layout: default
title: HTTPエンドポイントのURLパラメータのハンドル
slug:
  - label: HTTP
    url: /#http-endpoints
  - URLパラメータ
---

### 課題

リクエストごとにセットされるパスパラメータを
ハンドルできる単一のHTTPエンドポイントを作成したい。

たとえば、単一のエンドポイントで次の両方がハンドルできます:

    http://example.com/hello-param/Nick
    http://example.com/hello-param/Dave

### 解決

<code class="node">HTTP In</code> ノードの `URL` プロパティで、
名前付きのパスパラメータを使用します。
そして、メッセージの `msg.req.params` プロパティで
リクエスト内で使用している値へアクセスできます。

#### フロー

![](/images/http/handle-url-parameters.png)

{% raw %}
~~~json
[{"id":"ce53954b.31ac68","type":"http response","z":"3045204d.cfbae","name":"","x":490,"y":280,"wires":[]},{"id":"288a7c0.fd77584","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Hello {{req.params.name}}!</h1>\n    </body>\n</html>","x":350,"y":280,"wires":[["ce53954b.31ac68"]]},{"id":"7665c67d.899a38","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-param/:name","method":"get","swaggerDoc":"","x":150,"y":280,"wires":[["288a7c0.fd77584"]]}]
~~~
{: .flow}
{% endraw %}

#### 例

~~~text
[~]$ curl http://localhost:1880/hello-param/Nick
<html>
    <head></head>
    <body>
        <h1>Hello Nick!</h1>
    </body>
</html>
~~~
{: .shell}

### 議論

`URL` プロパティの名前付きパスパラメータは、
さまざまなリクエストのパスの一部を識別するのに使用されます。

`msg.req.params` プロパティは、各パスパラメータのキーと値のペアで構成されたオブジェクトです。

上記の例では、ノードにはURL `/hello-params/:name` が設定されているため、
`/hello-param/Nick` へのリクエスト結果である `msg.req.params` には次を含みます:

~~~json
{
    "name": "Nick"
}
~~~
