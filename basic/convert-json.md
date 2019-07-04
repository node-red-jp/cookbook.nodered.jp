---
layout: default
title: JSONの相互変換
slug:
  - label: formats
    url: /#working-with-data-formats
  - json

---

### 課題

メッセージプロパティをJSON文字列に変換したり、JavaScriptのオブジェクトに変換したりしたい。

### 解決

<code class="node">JSON</code> ノードを使用して、それぞれ2つのフォーマットを変換します。

#### 例

![](/images/basic/convert-json.png){:width="534px"}

{% raw %}
~~~json
[{"id":"634256b7.2d6818","type":"inject","z":"64133d39.bb0394","name":"JSON String","topic":"","payload":"{\"a\":1}","payloadType":"str","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":110,"y":80,"wires":[["a2fe0fc8.095e1"]]},{"id":"a2fe0fc8.095e1","type":"json","z":"64133d39.bb0394","name":"","property":"payload","action":"","pretty":false,"x":270,"y":80,"wires":[["9a4ce2b8.47698"]]},{"id":"9a4ce2b8.47698","type":"debug","z":"64133d39.bb0394","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":430,"y":80,"wires":[]},{"id":"80032e2.7c92cd","type":"inject","z":"64133d39.bb0394","name":"Object","topic":"","payload":"{\"a\":1}","payloadType":"json","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":90,"y":120,"wires":[["cd40a0f4.4f5ac"]]},{"id":"cd40a0f4.4f5ac","type":"json","z":"64133d39.bb0394","name":"","property":"payload","action":"","pretty":false,"x":270,"y":120,"wires":[["478b4106.4fd7c"]]},{"id":"478b4106.4fd7c","type":"debug","z":"64133d39.bb0394","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":430,"y":120,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

例では、最初のフローが `'{"a":1}'` というJSON文字列を生成しており、
<code class="node">JSON</code> ノードが等価のJavaScriptオブジェクトへ変換しています。

2つめのフローでは逆をやっており、`{ a: 1 }` というオブジェクトを生成し、JSONに変換しています。

<code class="node">JSON</code> はデフォルトでは、何がどう変換されるかを検出します。
プロパティとして与えられる型を設定することもできます。
たとえば、フローがJSONとオブジェクトを受け取っていても、
<code class="node">JSON</code> ノードは、プロパティがオブジェクトになるよう設定できます。
