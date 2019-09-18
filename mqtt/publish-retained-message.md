---
layout: default
title: retainされたメッセージをtopicにpublish
slug:
  - label: MQTT
    url: /#mqtt
  - retain
---

### 課題

retainされたメッセージをブローカーのMQTTのtopicにpublishしたい。

### 解決

<code class="node">MQTT Output</code> ノードの設定ダイアログで、`保持` オプションを `する` にセットするか、
ノードに送信されるメッセージ内の `msg.retain` プロパティを `true` にセットします。

#### 例

![](/images/mqtt/publish-retained-message.png)

{% raw %}
~~~json
[{"id":"4a7dc819.3aa6f8","type":"mqtt out","z":"eda2a949.74ea98","name":"","topic":"sensors/livingroom/temp","qos":"","retain":"true","broker":"61de5090.0f5d9","x":430,"y":420,"wires":[]},{"id":"fb7b873.c391878","type":"inject","z":"eda2a949.74ea98","name":"temperature","topic":"","payload":"22","payloadType":"num","repeat":"","crontab":"","once":false,"x":230,"y":420,"wires":[["4a7dc819.3aa6f8"]]},{"id":"61de5090.0f5d9","type":"mqtt-broker","z":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]
~~~
{: .flow}
{% endraw %}

### 議論

retainされたメッセージをtopicに送信すると、
すべてのsubscriberはsubscribe時にメッセージを受け取ることになります。

ブローカーにある直前にretainされたtopicをクリアするには、
retainフラグがセットされているブランクのメッセージをそのtopicに送信します。
