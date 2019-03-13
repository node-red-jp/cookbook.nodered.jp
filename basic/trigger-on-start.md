---
layout: default
title: Node-RED起動の度にフローを実行
---

### 課題

Node-REDが起動の度にフローを実行したい。

これは、コンテキスト変数を初期化したり、Node-REDが再起動されたという通知を送るためにも使われます。

### 解決

起動時に1回実行するよう設定された <code class="node">Inject</code> ノードを使用します。

#### 例

![](/images/basic/trigger-on-start.png){:width="530px"}

{% raw %}
~~~json
[{"id":"e60b12c1.93bb3","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"Started!","payloadType":"str","repeat":"","crontab":"","once":true,"x":140,"y":540,"wires":[["9b1d7727.56d0f8"]]},{"id":"9b1d7727.56d0f8","type":"debug","z":"535331d8.55c1f","name":"","active":true,"console":"false","complete":"false","x":410,"y":540,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

起動時に実行されるよう設定すると、<code class="node">Inject</code> ノードは
デプロイ後、数百ミリ秒後に自動的に実行されます。
この数百ミリ秒の遅延は、この時点までに残りのフローが作成され開始されるために使用されます。

このノードはデプロイされるたびに実行されます。
