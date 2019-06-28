---
layout: default
title: 設定した時間内でメッセージの受信がなければフローを実行
slug:
  - label: flow control
    url: /#flow-control
  - trigger timeout
---

### 課題

設定した時間内にメッセージを受信しなかったらフローを実行したい。
たとえば、5秒ごとに読み取っているセンサー値の受信に失敗したら、それを知りたい場合などです。

### 解決

<code class="node">Trigger</code> ノードを使用して、
定義した時間の間隔にメッセージを受信していないことを検出します。

#### 例

![](/images/basic/trigger-timeout.png){:width="555px"}

{% raw %}
~~~json
[{"id":"6ea53ad8.2362a4","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":450,"y":1160,"wires":[]},{"id":"3da6946e.184a5c","type":"inject","z":"ac14500e.2c57d","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":1160,"wires":[["38caaff4.03f6d","6ea53ad8.2362a4"]]},{"id":"38caaff4.03f6d","type":"trigger","z":"ac14500e.2c57d","op1":"","op2":"timeout","op1type":"nul","op2type":"str","duration":"5","extend":true,"units":"s","reset":"","bytopic":"all","name":"Watchdog","x":270,"y":1200,"wires":[["ae477709.016088"]]},{"id":"ae477709.016088","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":450,"y":1200,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

例のフローでは、上部のブランチで通常のメッセージのフローを示しています。
また2つめのブランチとして、<code class="node">Trigger</code> ノードにも渡されています。

<code class="node">Trigger</code> は、最初は何も送信せず、
5秒待って `"timeout"` メッセージを送信するよう設定されています。
新しいメッセージを受信した場合、この遅延を延長するオプションも選択されています。
つまり、メッセージを継続的に受信し続ける限り、ノードは何もしないということです。
最後のメッセージ受信から5秒を経過すると、一度だけ `"timeout"` メッセージを送信します。
