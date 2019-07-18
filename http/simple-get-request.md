---
layout: default
title: シンプルなGETリクエスト
slug:
  - label: http
    url: /#http-requests
  - get
---

### 課題

WebサイトへのシンプルなGETリクエストを行い、必要な情報を抽出したい。

### 解決

<code class="node">HTTP Request</code> ノードでHTTPリクエストを行い、
取得したHTMLドキュメントから <code class="node">HTML</code> ノードで要素を抽出します。

#### 例

![](/images/http/simple-get-request.png)

{% raw %}
~~~json
[{"id":"d88dd470.0ac7b8","type":"inject","z":"18c99b30.cf9d35","name":"make request","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":130,"y":180,"wires":[["874a3d4e.9b666"]]},{"id":"874a3d4e.9b666","type":"http request","z":"18c99b30.cf9d35","name":"","method":"GET","ret":"txt","url":"https://nodered.org","tls":"","x":294.5,"y":180,"wires":[["90243cc1.87edc"]]},{"id":"7403c68f.21d7c8","type":"debug","z":"18c99b30.cf9d35","name":"","active":true,"console":"false","complete":"false","x":650,"y":180,"wires":[]},{"id":"90243cc1.87edc","type":"html","z":"18c99b30.cf9d35","name":"","property":"","tag":".node-red-latest-version","ret":"text","as":"single","x":471.5,"y":180,"wires":[["7403c68f.21d7c8"]]}]
~~~
{: .flow}
{% endraw %}

### 議論

Webページ内のコンテンツを見つけるには、Google Chromeブラウザの'検証'が有用なツールとなります。
ブラウザを使用して、ページの要素上で右クリックをすると、要素に適用されているタグ・id・classが表示されます。

この例では、[https://nodered.org]() からNode-REDの最新のバージョンを取得しています。
インスペクタを使用すると、最新バージョンが `<span>` タグの `node-red-latest-version` クラスにあることが判ります。

<code class="node">HTML</code> ノードは、CSSセレクタで
`.node-red-latest-version` にマッチする各要素のメッセージを取得するよう設定してあります。

![](/images/http/simple-get-request-example-page.png)
