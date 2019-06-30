---
layout: default
title: 値が変わっていないメッセージの削除
slug:
  - label: flow control
    url: /#flow-control
  - rbe
---

### 課題

最後にメッセージを受信したときから値が変更されていない場合、そのメッセージを削除したい。
たとえば、スイッチの状態を定期的に送信するセンサーがあり、
値が変わった時だけ知りたい場合など。

### 解決

<code class="node">RBE</code> (Report By Exception) ノードを使用して
値が変更されていない限りメッセージをブロックします。

#### 例

![](/images/basic/report-by-exception.png){:width="618px"}

{% raw %}
~~~json
[{"id":"6079638d.df403c","type":"inject","z":"ac14500e.2c57d","name":"","topic":"","payload":"0","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":90,"y":1500,"wires":[["87129503.c7b358"]]},{"id":"87129503.c7b358","type":"rbe","z":"ac14500e.2c57d","name":"report-by-exception","func":"deadband","gap":"","start":"","inout":"out","property":"payload","x":300,"y":1520,"wires":[["5e2ffc27.c61dd4"]]},{"id":"5e2ffc27.c61dd4","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":510,"y":1520,"wires":[]},{"id":"2dc49f96.3070c","type":"inject","z":"ac14500e.2c57d","name":"","topic":"","payload":"1","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":90,"y":1540,"wires":[["87129503.c7b358"]]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">RBE</code> は、値が変更されていない場合にメッセージを削除すために利用されます。
これは値の変更検知に有効です。

比較するプロパティ値が数値だった場合は、どれくらい値が変わったときにメッセージを通過させるかのしきい値を
ノードに設定することもできます。
