---
layout: default
title: HTTPエンドポイントを作成
slug:
  - label: HTTP
    url: /#http-endpoints
  - エンドポイント作成
---

### 課題

HTMLページやCSSのような静的コンテンツを返す
GETリクエストに対応したHTTPエンドポイントを作成したい。

### 解決

<code class="node">HTTP In</code> ノードを使用してリクエストを待ち受けます。
<code class="node">Template</code> ノードで静的コンテンツを含めます。
そして、<code class="node">HTTP Response</code> ノードでリクエストに対してレスポンスを返します。

#### 例

![](/images/http/create-an-http-endpoint.png)

{% raw %}
~~~json
[{"id":"59ff2a1.fa600d4","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello","method":"get","swaggerDoc":"","x":100,"y":80,"wires":[["54c1e70d.ab3e18"]]},{"id":"54c1e70d.ab3e18","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Hello World!</h1>\n    </body>\n</html>","x":250,"y":80,"wires":[["266c286f.d993d8"]]},{"id":"266c286f.d993d8","type":"http response","z":"3045204d.cfbae","name":"","x":390,"y":80,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl http://localhost:1880/hello
<html>
    <head></head>
    <body>
        <h1>Hello World!</h1>
    </body>
</html>
~~~
{: .shell}

### 議論

<code class="node">HTTP In</code> と <code class="node">HTTP Response</code> のノードのペアは
作成するすべてのHTTPエンドポイントの出発点となります。

<code class="node">HTTP In</code> で始まるいかなるフローも、タイムアウト以外では
<code class="node">HTTP Response</code> ノードで終わらなければなりません。

<code class="node">HTTP Response</code> ノードはメッセージの `payload` プロパティを
レスポンスボディとして使用します。
他のプロパティはカスタマイズされたレスポンスとして使用できます、これについては他のレシピで説明します。


<code class="node">Template</code> ノードは
ボディコンテンツをフローに埋め込むための便利な方法を提供します。
ただ、このような静的コンテンツはフローの外側でメンテナンスすることが望ましいかもしれません。

HTTP認証を起動した場合、curlコマンドにユーザIDとパスワードを追加する必要があるでしょう。
例

~~~text
[~]$ curl -u userid:password  http://localhost:1880/hello 
~~~