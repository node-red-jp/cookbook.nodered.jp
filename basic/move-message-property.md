---
layout: default
title: messsageプロパティの移動
slug:
  - label: messages
    url: /#messages
  - move property
---

### 課題

messsageプロパティを別のプロパティに移動させたい。

### 解決

<code class="node">Change</code> ノードを使用してプロパティを移動させます。

#### 例

![](/images/basic/move-message-property.png){:width="616px"}

{% raw %}
~~~json
[{"id":"d11f7311.77c15","type":"inject","z":"535331d8.55c1f","name":"","topic":"Hello","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":160,"y":280,"wires":[["13c01487.eb13cb"]]},{"id":"13c01487.eb13cb","type":"change","z":"535331d8.55c1f","name":"","rules":[{"t":"move","p":"topic","pt":"msg","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":360,"y":280,"wires":[["89cc4fb1.9b208"]]},{"id":"89cc4fb1.9b208","type":"debug","z":"535331d8.55c1f","name":"","active":true,"console":"false","complete":"false","x":550,"y":280,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">Change</code> ノードはmessageプロパティを移動させるために使用できます。

Changeノード内では別々の2つのアクションとしても実行できます。
まず、セットの動作を使用し新しい場所にプロパティをコピーします。
そして、削除の動作で元の値を削除します。

あるいは、このノードは1ステップで移動をサポートしています。
