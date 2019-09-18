---
layout: default
title: 定期的にメッセージを処理
slug:
  - label: フロー制御
    url: /#flow-control
  - メッセージの流量制限
---

### 課題

大量のメッセージをそのまま流さず、定期的に処理したい。
たとえば、1秒ごとにデータを送信するセンサーがあるが、
値の更新は5秒ごとで、操作するメッセージがいちばん最新のものでよい場合など。

### 解決

中間メッセージを削除するオプションを有効に設定した <code class="node">Delay</code> ノードを使用して、
メッセージ通過の流量制限を行います。

#### 例

![](/images/basic/rate-limit-message-stream.png){:width="601px"}

{% raw %}
~~~json
[{"id":"8a1bcd7d.f6b67","type":"inject","z":"ac14500e.2c57d","name":"Inject Array","topic":"","payload":"[0,1,2,3,4,5,6,7,8,9]","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":110,"y":1380,"wires":[["bd4bdd42.bd1b"]]},{"id":"bd4bdd42.bd1b","type":"delay","z":"ac14500e.2c57d","name":"","pauseType":"rate","timeout":"5","timeoutUnits":"seconds","rate":"1","nbRateUnits":"5","rateUnits":"second","randomFirst":"1","randomLast":"5","randomUnits":"seconds","drop":true,"x":320,"y":1380,"wires":[["be20c513.237c78"]]},{"id":"be20c513.237c78","type":"debug","z":"ac14500e.2c57d","name":"Debug","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":510,"y":1380,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

メッセージの流量制限を動作に設定した <code class="node">Delay</code> ノードは
通過するメッセージの流量を変更することに使われます。
中間メッセージを削除するオプションを有効にすると、
流量に設定した時間間隔内に受信したメッセージはすべて破棄されます。
