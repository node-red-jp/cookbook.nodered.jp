---
layout: default
title: 定期的にフローを実行
slug:
  - label: flow control
    url: /#flow-control
  - trigger at interval
---

### 課題

定期的にフローを実行したい。
たとえば、定期的にAPIを呼び出して現在の状態を取得するなどです。

### 解決

繰り返し間隔を設定した <code class="node">Inject</code> ノードを使用します。

#### 例

![](/images/basic/trigger-at-interval.png){:width="530px"}

{% raw %}
~~~json
[{"id":"372cfc32.bcd244","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"","payloadType":"date","repeat":"5","crontab":"","once":false,"x":150,"y":600,"wires":[["6c63c499.ce3adc"]]},{"id":"6c63c499.ce3adc","type":"debug","z":"535331d8.55c1f","name":"","active":true,"console":"false","complete":"false","x":410,"y":600,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### Discussion

<code class="node">Inject</code> ノードは固定の間隔で繰り返すよう設定されています。
必要に応じて、特定の日の特定の時間の範囲内で実行するよう制限することもできます。
