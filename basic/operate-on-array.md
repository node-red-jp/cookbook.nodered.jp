---
layout: default
title: 配列内の各要素に対して操作を実行
slug:
  - label: flow control
    url: /#flow-control
  - operate on array
---

### 課題

配列内の各要素に対して操作を実行したい。
たとえば、与えられた数値の配列を整数の近似値に丸めたい場合などです。

### 解決

<code class="node">Split</code> ノードは配列の各要素を送信するために使用します。
これに個々の要素に対して必要な操作を行うノードが続き、
その後 <code class="node">Join</code> ノードによって、単一の配列に再結合します。

#### 例

![](/images/basic/operate-on-array.png){:width="645px"}

{% raw %}
~~~json
[{"id":"3149f240.c0e25e","type":"inject","z":"ac14500e.2c57d","name":"Array of decimals","topic":"","payload":"[1.67,2.98,3.12,4.99,5.50]","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":120,"y":960,"wires":[["bd57baa6.00f998"]]},{"id":"bd57baa6.00f998","type":"split","z":"ac14500e.2c57d","name":"Split array","splt":"\\n","spltType":"str","arraySplt":"1","arraySpltType":"len","stream":false,"addname":"","x":200,"y":1020,"wires":[["7ab9e9ed.d514b8"]]},{"id":"7ab9e9ed.d514b8","type":"range","z":"ac14500e.2c57d","minin":"0","maxin":"10","minout":"0","maxout":"10","action":"scale","round":true,"property":"payload","name":"Round value","x":350,"y":1020,"wires":[["f26660ab.007b3"]]},{"id":"f26660ab.007b3","type":"join","z":"ac14500e.2c57d","name":"","mode":"auto","build":"string","property":"payload","propertyType":"msg","key":"topic","joiner":"\\n","joinerType":"str","accumulate":"false","timeout":"","count":"","reduceRight":false,"x":490,"y":1020,"wires":[["f9b5abac.f13828"]]},{"id":"f9b5abac.f13828","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":550,"y":1080,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

その他のプログラミング環境では、このタスクは配列の各要素でループすることによって実現されます。

Node-REDで同じようなことをする場合の方法としては、
配列を含む単一のメッセージを個別に処理するメッセージのストリームに変換し、
最後にひとつのメッセージに再結合して返します。

<code class="node">Split</code>/<code class="node">Join</code> ノードのペアは、
一般的にはこれらを行うために一緒に使用されます。
<code class="node">Split</code> ノードは、ストリーム内の各メッセージに `msg.parts` プロパティを追加し、
<code class="node">Join</code> ノードは、元のメッセージを再構築します。
