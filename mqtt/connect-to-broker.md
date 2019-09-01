---
layout: default
title: MQTTブローカーに接続
slug:
  - label: mqtt
    url: /#mqtt
  - connect
---

### 課題

ローカルで実行されているMQTTブローカーに接続したい。

### 解決

<code class="node">MQTT Input</code> または <code class="node">MQTT Output</code> ノードを使用し、
関連付けた <code class="node">MQTT Config</code> ノードでMQTTブローカーに接続します。

#### Example

![](/images/mqtt/connect-to-broker.png)

{% raw %}
~~~json
[{"id":"2c6873d2.992abc","type":"mqtt out","z":"eda2a949.74ea98","name":"","topic":"sensors/livingroom/temp","qos":"","retain":"","broker":"407a01e4.6b637","x":330,"y":80,"wires":[]},{"id":"d9beed59.94155","type":"inject","z":"eda2a949.74ea98","name":"","topic":"","payload":"22","payloadType":"num","repeat":"","crontab":"","once":false,"x":150,"y":80,"wires":[["2c6873d2.992abc"]]},{"id":"be80048.8f232f8","type":"mqtt in","z":"eda2a949.74ea98","name":"","topic":"sensors/livingroom/temp","qos":"2","broker":"407a01e4.6b637","x":170,"y":160,"wires":[["8640b8ff.f82ff8"]]},{"id":"8640b8ff.f82ff8","type":"debug","z":"eda2a949.74ea98","name":"","active":true,"console":"false","complete":"false","x":370,"y":160,"wires":[]},{"id":"407a01e4.6b637","type":"mqtt-broker","z":"","broker":"localhost","port":"1883","clientid":"","usetls":false,"compatmode":true,"keepalive":"60","cleansession":true,"willTopic":"","willQos":"0","willPayload":"","birthTopic":"","birthQos":"0","birthPayload":""}]
~~~
{: .flow}
{% endraw %}

### 議論

多くのユーザが、[mosquitto](http://mosquitto.org) のように
同一のRaspberry PiやPC上でMQTTブローカーを動かしています。
<code class="node">MQTT</code> の入力や出力のノードがフロー上にあれば、
`新規に mqtt-broker を追加...` を選択して、`サーバ` 設定ポップアップをクリックして
<code class="node">MQTT Config</code> ノードが作成できます。
ブローカーがオープンになっているとすれば、サーバに `localhost` を設定し、ポートには `1883` が入力されたままにします。

ローカルでないセキュアなブローカーへ接続を行う場合は、
接続先のMQTT接続要件を満たしている他の設定が <code class="node">MQTT Config</code> ノードに必要です。
