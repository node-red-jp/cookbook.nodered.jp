---
layout: default
title: エラー発生後に自動的にリトライ
slug:
  - label: エラーハンドリング
    url: /#error-handling
  - リトライ
---

### 課題

エラーがthrowされたあとに、リトライしたい。

### 解決

<code class="node">Catch</code> ノードを使用してエラー情報を受け取り、
リトライしたい動作を行うノードに接続し直します。

#### 例

![](/images/basic/retry-on-error.png){:width="610px"}

{% raw %}
~~~json
[{"id":"27e61f12.c1a15","type":"inject","z":"fc046f99.4be08","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":320,"wires":[["d7d08440.31b678"]]},{"id":"d7d08440.31b678","type":"function","z":"fc046f99.4be08","name":"Random error","func":"// Randomly throw an error rather than\n// pass on message.\nif (Math.random() < 0.5) {\n   node.error(\"a random error\", msg);\n} else {\n    return msg;\n}","outputs":1,"noerr":0,"x":320,"y":320,"wires":[["f22b1e9a.5d89b"]]},{"id":"f22b1e9a.5d89b","type":"debug","z":"fc046f99.4be08","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":510,"y":320,"wires":[]},{"id":"2166290d.98d736","type":"delay","z":"fc046f99.4be08","name":"","pauseType":"delay","timeout":"2","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":240,"y":380,"wires":[["d7d08440.31b678"]]},{"id":"139b836e.7950ed","type":"catch","z":"fc046f99.4be08","name":"","scope":["d7d08440.31b678"],"uncaught":false,"x":90,"y":380,"wires":[["2166290d.98d736","9c8ab214.0ecaa"]]},{"id":"9c8ab214.0ecaa","type":"debug","z":"fc046f99.4be08","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"error","targetType":"msg","x":240,"y":440,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

一部のエラーは一時的なものであり、
単にリトライすると成功するようなものがあります。
もしくは、リトライする前に何かしらの対処が必要になることもあります。

例のフローでは <code class="node">Function</code> ノードが
50%の確率でメッセージを通過させずランダムにエラーをthrowします。

<code class="node">Catch</code> ノードはエラーを受け取り、
<code class="node">Function</code> ノードへリトライします。
これに <code class="node">Delay</code> ノードを含んでいるのは、
状況によっては少し短い間隔をあけてリトライすることが良い場合もあるためです。
