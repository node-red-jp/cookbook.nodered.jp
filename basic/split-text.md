---
layout: default
title: テキストを1行1メッセージに分割
slug:
  - label: formats
    url: /#working-with-data-formats
  - text
---

### 課題

ブロックテキストの各行に対して処理を行いたい。
たとえば、各行に対して行番号を付け加えたい場合。

### 解決

<code class="node">Split</code> ノードはメッセージの1行ごとを1メッセージに分割するため使用できます。

その各行のテキストを処理するためのノードを接続し、
そして、<code class="node">Join</code> で単一のブロックテキストに再結合して戻すことができます。

#### 例

![](/images/basic/split-text.png){:width="717px"}

{% raw %}
~~~json
[{"id":"df6514f0.029748","type":"inject","z":"64133d39.bb0394","name":"inject","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":"","x":110,"y":900,"wires":[["11f53f61.2f7be1"]]},{"id":"11f53f61.2f7be1","type":"template","z":"64133d39.bb0394","name":"","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"one\ntwo\nthree\nfour\nfive","x":240,"y":900,"wires":[["760c1d71.c29744"]]},{"id":"760c1d71.c29744","type":"split","z":"64133d39.bb0394","name":"","splt":"\\n","x":190,"y":960,"wires":[["3e427aac.9b9596"]]},{"id":"3e427aac.9b9596","type":"change","z":"64133d39.bb0394","name":"Prepend line number","rules":[{"t":"set","p":"payload","pt":"msg","to":"(parts.index+1) & \": \" & payload","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":360,"y":960,"wires":[["d44d4767.945fd8"]]},{"id":"d44d4767.945fd8","type":"join","z":"64133d39.bb0394","name":"","mode":"auto","build":"string","property":"payload","propertyType":"msg","key":"topic","joiner":"\\n","timeout":"","count":"","x":530,"y":960,"wires":[["bfe3e43b.85fa88"]]},{"id":"bfe3e43b.85fa88","type":"debug","z":"64133d39.bb0394","name":"debug","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","x":650,"y":960,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

例では、複数行のテキストブロックを生成するために
<code class="node">Inject</code> と <code class="node">Template</code> ノードを使用しています。

~~~text
one
two
three
four
five
~~~

<code class="node">Split</code> ノードのデフォルトの挙動は
メッセージを通過させる際に、1行ごとに1メッセージへ分割します。

<code class="node">Change</code> ノードは分割された各メッセージを
JSONata式 `(parts.index+1) & ": " & payload` で変換します。
これは、`msg.parts.index` で、行番号を取得し、
それを `msg.payload` の先頭に追加しています。

最後に、<code class="node">Join</code> ノードで
メッセージを単一のブロックテキストに再構築します。

~~~text
1: one
2: two
3: three
4: four
5: five
~~~
