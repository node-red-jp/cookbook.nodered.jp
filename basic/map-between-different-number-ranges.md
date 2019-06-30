---
layout: default
title: 異なる範囲の数値をマッピングする
slug:
  - label: messages
    url: /#messages
  - map range
---

### 課題

ある範囲の数値をスケールさせたい。
たとえば、センサー値が0〜1023の範囲で取得できるものに対して、電圧として0〜5にマッピングするなど。

### 解決

<code class="node">Range</code> ノードを使用して、定義した範囲にマッピングします。

#### Example

![](/images/basic/map-between-different-number-ranges.png){:width="616px"}

{% raw %}
~~~json
[{"id":"80dae67d.b4d8f8","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"0","payloadType":"num","repeat":"","crontab":"","once":false,"x":130,"y":380,"wires":[["81f13534.456348"]]},{"id":"81f13534.456348","type":"range","z":"535331d8.55c1f","minin":"0","maxin":"1023","minout":"0","maxout":"5","action":"clamp","round":false,"name":"","x":350,"y":420,"wires":[["e80b61d7.4b399"]]},{"id":"cb21de23.75a2f","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"512","payloadType":"num","repeat":"","crontab":"","once":false,"x":130,"y":420,"wires":[["81f13534.456348"]]},{"id":"342552de.255a1e","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"1023","payloadType":"num","repeat":"","crontab":"","once":false,"x":130,"y":460,"wires":[["81f13534.456348"]]},{"id":"e80b61d7.4b399","type":"debug","z":"535331d8.55c1f","name":"","active":true,"console":"false","complete":"false","x":550,"y":420,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">Range</code> ノードは指定した2つの値の範囲内を直線的にスケールさせるために使用します。

デフォルトでは、結果はノードで定義された範囲に制限されません。
上記の電圧の例でいうと、2046は10にマッピングされます。

このノードはターゲットとする値の範囲を設定することもできますし、
シンプルな合同算術を適用して範囲におさめることもできます。
