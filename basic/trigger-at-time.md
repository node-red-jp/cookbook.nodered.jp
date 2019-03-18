---
layout: default
title: 指定の時間にフローを実行
---

### 課題

たとえば、毎週平日の午後4時、のような指定の時間にフローを実行したい。

### 解決

指定した日時を設定した <code class="node">Inject</code> ノードを使用します。

#### 例

![](/images/basic/trigger-at-time.png){:width="530px"}

{% raw %}
~~~json
[{"id":"24579bcb.5c9814","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"It is 4pm on a weekday!","payloadType":"str","repeat":"","crontab":"00 16 * * 1,2,3,4,5","once":false,"x":190,"y":660,"wires":[["145b508a.f3325f"]]},{"id":"145b508a.f3325f","type":"debug","z":"535331d8.55c1f","name":"","active":true,"console":"false","complete":"false","x":410,"y":660,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">Inject</code> ノードは指定の時間の指定の曜日に実行するよう設定できます。
複数の時間指定が必要な場合は、複数の <code class="node">Inject</code> ノードを使用します。
