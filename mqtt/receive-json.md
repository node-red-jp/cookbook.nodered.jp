---
layout: default
title: パースされたJSONメッセージを受信
slug:
  - label: MQTT
    url: /#mqtt
  - JSONをsubscribe
---

### 課題

MQTT BrokerからパースされたJSONメッセージを受信したい。

### 解決

<code class="node">MQTT Input</code> ノードと <code class="node">JSON node</code> ノードを使用して、パースされたJSONメッセージを受信します。

#### 例

![](/images/mqtt/receive-json.png)

{% raw %}
~~~json
[{"id":"8024cb4.98c5238","type":"mqtt in","z":"eda2a949.74ea98","name":"","topic":"sensors/#","qos":"2","broker":"61de5090.0f5d9","x":260,"y":580,"wires":[["b5098b7f.2361d8"]]},{"id":"15d727dd.33e808","type":"debug","z":"eda2a949.74ea98","name":"","active":true,"console":"false","complete":"false","x":530,"y":580,"wires":[]},{"id":"2aed678c.3de738","type":"mqtt out","z":"eda2a949.74ea98","name":"","topic":"sensors/livingroom/temp","qos":"","retain":"false","broker":"61de5090.0f5d9","x":310,"y":520,"wires":[]},{"id":"3b613a69.a247c6","type":"inject","z":"eda2a949.74ea98","name":"temp json","topic":"","payload":"{\"sensor_id\":1234,\"temperature\":13}","payloadType":"json","repeat":"","crontab":"","once":false,"x":120,"y":520,"wires":[["2aed678c.3de738"]]},{"id":"b5098b7f.2361d8","type":"json","z":"eda2a949.74ea98","name":"","pretty":false,"x":390,"y":580,"wires":[["15d727dd.33e808"]]},{"id":"61de5090.0f5d9","type":"mqtt-broker","z":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">MQTT Input</code> ノードのpayloadは、バイナリバッファとして検出されなければ文字列となります。JSON文字列にパースしてJavaScript Objectに変換するには、<code class="node">JSON</code> ノードを使用します。

新しいバージョンのMQTT node(Node-REDバージョン0.19以降)は、
出力オプションが必須指定で存在するため、JSONノードは必須ではありません。
