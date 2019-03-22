---
layout: default
title: メッセージをプロパティによってルーティングする
---

### 課題

`msg.topic` プロパティ値によって、メッセージを異なるフローにルーティングしたい。
たとえば、複数のセンサーをsubscribeしている <code class="node">MQTT</code> ノードが存在し、
異なるダッシュボードの <code class="node">ui_gauge</code> ノードに渡したい場合など。

### 解決

<code class="node">Switch</code> ノードを使用して
さまざまな入力を異なる出力に対応させるようプロパティの値をチェックします。

#### 例

![](/images/basic/route-on-property.png){:width="601px"}

{% raw %}
~~~json
[{"id":"3bc8e1d2.744abe","type":"switch","z":"ac14500e.2c57d","name":"Route ","property":"topic","propertyType":"msg","rules":[{"t":"eq","v":"temperature","vt":"str"},{"t":"eq","v":"humidity","vt":"str"},{"t":"eq","v":"pressure","vt":"str"}],"checkall":"true","repair":false,"outputs":3,"x":330,"y":420,"wires":[["907bf3b8.def45"],["fe425938.926838"],["ec261304.52f73"]]},{"id":"be3da36c.1c142","type":"inject","z":"ac14500e.2c57d","name":"","topic":"temperature","payload":"27","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":140,"y":380,"wires":[["3bc8e1d2.744abe"]]},{"id":"f271ceef.172b3","type":"inject","z":"ac14500e.2c57d","name":"","topic":"humidity","payload":"45","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":130,"y":420,"wires":[["3bc8e1d2.744abe"]]},{"id":"907bf3b8.def45","type":"debug","z":"ac14500e.2c57d","name":"Temperature","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":510,"y":380,"wires":[]},{"id":"fe425938.926838","type":"debug","z":"ac14500e.2c57d","name":"Humidity","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":500,"y":420,"wires":[]},{"id":"ec261304.52f73","type":"debug","z":"ac14500e.2c57d","name":"Pressure","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":500,"y":460,"wires":[]},{"id":"fca957dd.9d8078","type":"inject","z":"ac14500e.2c57d","name":"","topic":"pressure","payload":"1001","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":130,"y":460,"wires":[["3bc8e1d2.744abe"]]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">Switch</code> ノードは、受信したメッセージを一致したルールへ対応する出力に送信します。

一致するすべてのルール、または、最初に一致したルールのみへ送信するかを設定できます。
