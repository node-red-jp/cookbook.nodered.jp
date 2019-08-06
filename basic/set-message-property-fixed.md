---
layout: default
title: 固定値をmesssageプロパティにセット
slug:
  - label: messages
    url: /#messages
  - set property
---

### 課題

固定値をmesssageプロパティにセットしたい。

### 解決

<code class="node">Change</code> ノードを使用してmesssageのプロパティにセットする。

#### 例

![](/images/basic/set-message-property-fixed.png){:width="616px"}

{% raw %}
~~~json
[{"id":"d72dc4ce.89b368","type":"inject","z":"535331d8.55c1f","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":140,"y":80,"wires":[["78075f19.e0174"]]},{"id":"78075f19.e0174","type":"change","z":"535331d8.55c1f","name":"","rules":[{"t":"set","p":"payload","pt":"msg","to":"Hello World!","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":340,"y":80,"wires":[["78dc7c25.b90d54"]]},{"id":"78dc7c25.b90d54","type":"debug","z":"535331d8.55c1f","name":"","active":true,"console":"false","complete":"false","x":550,"y":80,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

<code class="node">Change</code> ノードは、messageのプロパティをセットするために使用します。

このノードは、さまざまなJavaScriptの型と、いくつかのNode-RED固有の型へのセットをサポートしています。

 - string: `"hello world"`
 - numbers: `42`
 - boolean: `true`/`false`
 - timestamp: エポック(1970年1月1日)から現在日時までのミリ秒
 - JSON: オブジェクトとして変換可能なJSON文字列
 - Buffer: Node.jsのBufferオブジェクト

コンテキストのプロパティ値や、他のmesssageプロパティや、JSONata式でのセットもサポートしています。
