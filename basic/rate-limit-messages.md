---
layout: default
title: メッセージの通過を遅らせる
---

### 課題

メッセージがフローを通過する量を制限したい。
たとえば、[メッセージをストリームに分割](/basic/operate-on-array) した配列をもつメッセージがあるとして、
そのストリーム内の各メッセージを毎秒1回処理したいとします。

### 解決

<code class="node">Delay</code> ノードを使用して、
メッセージ通過の流量制限を設定します。

#### 例

![](/images/basic/rate-limit-messages.png){:width="615px"}

{% raw %}
~~~json
[{"id":"1fccc223.7ba87e","type":"inject","z":"ac14500e.2c57d","name":"Inject Array","topic":"","payload":"[0,1,2,3,4,5,6,7,8,9]","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":110,"y":1280,"wires":[["b2837466.e02a38"]]},{"id":"b2837466.e02a38","type":"split","z":"ac14500e.2c57d","name":"","splt":"\\n","spltType":"str","arraySplt":1,"arraySpltType":"len","stream":false,"addname":"","x":250,"y":1280,"wires":[["bd97c8ed.a5c8d8"]]},{"id":"bd97c8ed.a5c8d8","type":"delay","z":"ac14500e.2c57d","name":"","pauseType":"rate","timeout":"5","timeoutUnits":"seconds","rate":"1","nbRateUnits":"1","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":false,"x":390,"y":1280,"wires":[["bd66f03e.bdf0c"]]},{"id":"bd66f03e.bdf0c","type":"debug","z":"ac14500e.2c57d","name":"Debug","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":530,"y":1280,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

メッセージの流量制限を動作に設定した <code class="node">Delay</code> ノードは
通過するメッセージの流量を変更することに使われます。
ノードには、時間間隔ごとにノードを通過するメッセージの数を設定します。
これはメッセージの通過を等間隔に広げます。
