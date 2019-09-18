---
layout: default
title: CSV入力を変換
slug:
  - label: フォーマット
    url: /#working-with-data-formats
  - CSV
---

### 課題

CSVデータを変換したい。

### 解決

<code class="node">CSV</code> ノードを使用して、CSVからJavaScriptオブジェクトへ変換します。

#### 例

![](/images/basic/parse-csv.png){:width="660px"}

{% raw %}
~~~json
[{"id":"73e4e16.4d9742","type":"inject","z":"64133d39.bb0394","name":"Inject","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":110,"y":780,"wires":[["2bef78fd.ae70f8"]]},{"id":"90ed51dc.dcc71","type":"csv","z":"64133d39.bb0394","name":"","sep":",","hdrin":true,"hdrout":false,"multi":"mult","ret":"\\n","temp":"","skip":"1","x":410,"y":780,"wires":[["9aace6e7.adc538"]]},{"id":"9aace6e7.adc538","type":"debug","z":"64133d39.bb0394","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":570,"y":780,"wires":[]},{"id":"2bef78fd.ae70f8","type":"template","z":"64133d39.bb0394","name":"CSV data","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"# This is some random data\na,b,c\n80,18,2\n52,36,10\n91,18,61\n32,47,65","output":"str","x":260,"y":780,"wires":[["90ed51dc.dcc71"]]}]
~~~
{: .flow}
{% endraw %}

### 議論

例では、最初のフローでCSVデータを含むpayloadを生成しています:

```javascript
# This is some random data
a,b,c
80,18,2
52,36,10
91,18,61
32,47,65
```

<code class="node">CSV</code> ノードは最初の1行を無視するように設定されており、
これにより最初のコメント行を読み飛ばします。
そして、次の行で列名を取得して、残りをデータ行として処理します。

またこの例では、ノードは配列化した1つのメッセージを送信するよう設定されています。
結果となるメッセージのpayloadは次のとおりです:

```javascript
[
    { a: 80, b: 18, c: 2},
    { a: 52, b: 36, c: 10},
    { a: 91, b: 18, c: 61},
    { a: 32, b: 47, c: 65},
]
```

このノードは、行毎にメッセージを分割して出力するようにも設定できます。
このモードでは、メッセージは `msg.parts` プロパティも含まれ、
<code class="node">Join</code> ノードで単一の配列に再構成することもできます。
