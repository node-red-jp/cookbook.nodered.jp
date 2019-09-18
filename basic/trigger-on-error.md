---
layout: default
title: ノードがエラーをthrowした場合にフローを実行
slug:
  - label: エラーハンドリング
    url: /#error-handling
  - エラー時に実行
---

### 課題

ノードがエラーをthrowした場合にフローを実行したい。

### 解決

<code class="node">Catch</code> ノードを使って、エラー情報を受け取り、フローを実行します。

#### 例

![](/images/basic/trigger-on-error.png){:width="401px"}

{% raw %}
~~~json
[{"id":"2bd6810d.e22ece","type":"catch","z":"fc046f99.4be08","name":"","scope":["2c94a22c.91012e"],"uncaught":false,"x":130,"y":160,"wires":[["d16b9fac.8212a"]]},{"id":"2c94a22c.91012e","type":"function","z":"fc046f99.4be08","name":"Throw Error","func":"node.error(\"an example error\", msg);   ","outputs":1,"noerr":0,"x":310,"y":100,"wires":[[]]},{"id":"d16b9fac.8212a","type":"debug","z":"fc046f99.4be08","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"error","targetType":"msg","x":300,"y":160,"wires":[]},{"id":"c5ee9670.5dbbd8","type":"inject","z":"fc046f99.4be08","name":"Trigger error","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":110,"y":100,"wires":[["2c94a22c.91012e"]]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">Catch</code> ノードは、フロー内の特定のノードや任意のノードで発生したエラーをcatchするよう設定できます。
これにより、異なるノードに対して異なるエラーハンドリングを設定できます。

<code class="node">Catch</code> ノードは、エラーとしてロギングされた情報をメッセージとして送信します。
さらに、`msg.error` にエラーの詳細情報とどのノードで発生したかがセットされています。

これらのエラーハンドリングが行われることも考慮して、ノードはエラーを正しくロギングする必要があります。
