---
layout: default
title: ストリームの送信が止まった際にプレースホルダーメッセージを送信
slug:
  - label: フロー制御
    url: /#flow-control
  - プレースホルダーの送信
---

### 課題

定期的にセンサー値をストリームで受信しているとします。
センサーからのメッセージ送信が止まった際に、同じ間隔でプレースホルダーとなる値を送信したいとします。

例として、センサーデータをダッシュボードに流しているいる場合、
センサーの値送信が止まった場合、ダッシュボード上のチャートでは値の更新が止まってしまいます。
センサーが止まったことを示すためにも、チャートを更新するためのプレースホルダーのメッセージとなる `0` 値の送信が必要になります。

### 解決

<code class="node">Trigger</code> ノードを使用して、あらかじめ定義した間隔でセンサー値を受信していないことを検知し、
2つめの <code class="node">Trigger</code> ノードでプレースホルダーメッセージを定期的に送信します。

#### 例

![](/images/basic/trigger-placeholder.png){:width="711px"}

{% raw %}
~~~json
[{"id":"9ccdf268.c96ff","type":"inject","z":"ac14500e.2c57d","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":1660,"wires":[["38950a5.28d15f6","2c532f67.0330e"]]},{"id":"38950a5.28d15f6","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":610,"y":1660,"wires":[]},{"id":"2c532f67.0330e","type":"trigger","z":"ac14500e.2c57d","op1":"reset","op2":"true","op1type":"str","op2type":"bool","duration":"2","extend":true,"units":"s","reset":"","bytopic":"all","name":"","x":260,"y":1700,"wires":[["e4e42b96.97a338"]]},{"id":"e4e42b96.97a338","type":"trigger","z":"ac14500e.2c57d","op1":"0","op2":"0","op1type":"num","op2type":"str","duration":"-2","extend":false,"units":"s","reset":"reset","bytopic":"all","name":"","x":420,"y":1700,"wires":[["38950a5.28d15f6"]]}]
~~~
{: .flow}
{% endraw %}

### 議論

例のフローでは、上部のブランチで <code class="node">Inject</code> ノードから
<code class="node">Debug</code> ノードへの通常のメッセージのフローを示しています。

メッセージは、2つめのブランチの先頭の <code class="node">Trigger</code> ノードへも流れています。
ノードは最初に `"reset"` のpayloadを送信し、2秒待って `"timeout"` メッセージを送信するよう設定されています。
新しいメッセージを受信した場合、この遅延を延長するオプションも選択されています。
つまり、メッセージを継続的に受信し続ける限り、ノードは何もしないということです。
最後のメッセージ受信から2秒を経過すると、一度だけ `"timeout"` メッセージを送信します。

タイムアウトメッセージは後続の <code class="node">Trigger</code> ノードに送信されます。
このノードは2秒ごとに `0` を送信するように設定されており、一番上のブランチに合流します。
ノードは `msg.payload` が `"reset"` の場合、送信を停止するようにも設定されています。
これは、先頭の <code class="node">Trigger</code> ノードがセンサーのメッセージを受信することにより、そこから最初のメッセージが送信され、
センサー自身の値送信が再開されたとき、2番目の <code class="node">Trigger</code> ノードが初期化されます。
