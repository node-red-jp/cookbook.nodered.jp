---
layout: default
title: topicにメッセージをpublishする
slug:
  - label: MQTT
    url: /#mqtt
  - publish
---

### 課題

MQTTブローカーのtopicにメッセージをpublishしたい。

### 解決

<code class="node">MQTT Output</code> ノードを使用してtopicにメッセージをpublishする。

#### 例

![](/images/mqtt/publish-to-topic.png)

{% raw %}
~~~json
[{"id":"9c138886.116928","type":"mqtt out","z":"eda2a949.74ea98","name":"","topic":"sensors/livingroom/temp","qos":"","retain":"","broker":"61de5090.0f5d9","x":430,"y":100,"wires":[]},{"id":"ff654e7f.32e9e","type":"inject","z":"eda2a949.74ea98","name":"temperature","topic":"","payload":"22","payloadType":"num","repeat":"","crontab":"","once":false,"x":230,"y":100,"wires":[["9c138886.116928"]]},{"id":"61de5090.0f5d9","type":"mqtt-broker","z":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]
~~~
{: .flow}
{% endraw %}

### 議論

MQTTブローカーに接続済の <code class="node">MQTT Config</code> ノードが関連付けられている <code class="node">MQTT Output</code> ノードが、
あらかじめ設定されたtopicにメッセージをpublishします。
