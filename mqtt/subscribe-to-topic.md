---
layout: default
title: topicをsubscribe
slug:
  - label: mqtt
    url: /#mqtt
  - subscribe
---

### 課題

MQTTのtopicをsubscribeしたい。

### 解決

<code class="node">MQTT Input</code> ノードを使用しブローカーをsubscribeし、
topicに一致するメッセージを受信します。

#### 例

![](/images/mqtt/subscribe-to-topic.png)

{% raw %}
~~~json
[{"id":"8024cb4.98c5238","type":"mqtt in","z":"eda2a949.74ea98","name":"","topic":"sensors/#","qos":"2","broker":"61de5090.0f5d9","x":240,"y":180,"wires":[["15d727dd.33e808"]]},{"id":"15d727dd.33e808","type":"debug","z":"eda2a949.74ea98","name":"","active":true,"console":"false","complete":"false","x":390,"y":180,"wires":[]},{"id":"61de5090.0f5d9","type":"mqtt-broker","z":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">MQTT Input</code> ノードのtopicのフィルターはハードコードしなければならず、
動的には変更できません。


これを回避する方法のひとつは、`$(MY_TOPIC)` のような環境変数をtopicにセットすることです。
Node-REDのランタイムが開始したとき、ノードのプロパティがその環境変数に置換されます。
これによりtopicの変更ができますが、
環境変数の変化を反映するにはNode-REDの再起動が必要です。

MQTTワイルドカードを、`+` は単一のtopicレベルに、`#` は複数のtopicレベルに使用できます。
これにより、単一のノードで複数のtopicを受信できます。
ノードから送信されたメッセージの `msg.topic` には、実際に受信したtopicがセットされています。
