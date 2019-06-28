---
layout: default
title: messageプロパティを削除
slug:
  - label: messages
    url: /#messages
  - delete property
---

### 課題

messageプロパティを削除したい。

### 解決

<code class="node">Change</code> ノードを使用してプロパティを削除します。

#### 例

![](/images/basic/delete-message-property.png){:width="616px"}

{% raw %}
~~~json
[{"id":"91cd2fa9.e0a96","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":140,"y":180,"wires":[["54ec03e4.5714bc"]]},{"id":"54ec03e4.5714bc","type":"change","z":"535331d8.55c1f","name":"","rules":[{"t":"delete","p":"payload","pt":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":350,"y":180,"wires":[["321900de.3cbea"]]},{"id":"321900de.3cbea","type":"debug","z":"535331d8.55c1f","name":"","active":true,"console":"false","complete":"false","x":550,"y":180,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">Change</code> ノードはmessageプロパティの削除にも使用できます。
