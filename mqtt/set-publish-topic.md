---
layout: default
title: publishされるメッセージのtopicを設定
slug:
  - label: mqtt
    url: /#mqtt
  - publish topic
---

### 課題

publishされるMQTTメッセージのtopicを動的に設定したい。

### 解決

<code class="node">MQTT Output</code> ノードでメッセージを送信する前に、`トピック` プロパティを設定しておきます。

#### 例

![](/images/mqtt/set-publish-topic.png)

{% raw %}
~~~json
[{"id":"73abc692.bb3838","type":"mqtt out","z":"eda2a949.74ea98","name":"","topic":"","qos":"","retain":"","broker":"61de5090.0f5d9","x":410,"y":300,"wires":[]},{"id":"ef5a01ee.a940d","type":"inject","z":"eda2a949.74ea98","name":"kitchen temperature","topic":"sensors/kitchen/temperature","payload":"22","payloadType":"num","repeat":"","crontab":"","once":false,"x":250,"y":300,"wires":[["73abc692.bb3838"]]},{"id":"61de5090.0f5d9","type":"mqtt-broker","z":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]
~~~
{: .flow}
{% endraw %}

この例では、<code class="node">Inject</code> ノードが `msg.topic` をセットしていますが、
必ずしもInjectノードを使用しなければならないわけではありません。

### 議論

<code class="node">MQTT Output</code> ノードの設定ダイアログの、
`トピック` フィールドが空のままになっていることを確認します。
