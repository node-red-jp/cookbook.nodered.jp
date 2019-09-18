---
layout: default
title: ローカルファイルを返す
slug:
  - label: HTTP
    url: /#http-endpoints
  - ファイルを返す
---

### 課題

GETリクエストに対してpngイメージなどのローカルファイルをを返す
HTTPエンドポイントを作成したい。

### 解決

<code class="node">File In</code> ノードを使用し必要なコンテンツをロードして、
`Content-Type` に適切なファイルの形式となる値をセットします。

#### 例

![](/images/http/serve-a-local-file.png)

{% raw %}
~~~json
[{"id":"c7e341a0.381cc","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-file","method":"get","swaggerDoc":"","x":110,"y":720,"wires":[["2fb1c354.d04e3c"]]},{"id":"2fb1c354.d04e3c","type":"file in","z":"3045204d.cfbae","name":"","filename":"/tmp/node-red.png","format":"","x":290,"y":720,"wires":[["c9e28681.361d78"]]},{"id":"c9e28681.361d78","type":"change","z":"3045204d.cfbae","name":"Set Headers","rules":[{"t":"set","p":"headers","pt":"msg","to":"{}","tot":"json"},{"t":"set","p":"headers.content-type","pt":"msg","to":"image/png","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":470,"y":720,"wires":[["88974243.7768c"]]},{"id":"88974243.7768c","type":"http response","z":"3045204d.cfbae","name":"","x":610,"y":720,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl  http://localhost:1880/hello-file > file.png
~~~
{: .shell}

### 議論

画像のようにテキストでないファイルをロードする場合、
<code class="node">File In</code> ノードには `Buffer` オブジェクトを返すように設定されているべきです。

受信者がファイルを処理できるように、`Content-Type` ヘッダには適切なMIMEタイプがセットされているべきです。
上記の例では `.png` ファイルを返すため、`Content-Type` ヘッダに `image/png` をセットしています。
