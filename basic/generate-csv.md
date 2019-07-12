---
layout: default
title: CSVを生成して出力
slug:
  - label: formats
    url: /#working-with-data-formats
  - csv
---

### 課題

キーと値のペアであるデータを含むメッセージから、CSV出力に適した文字列を生成します。

### 解決

<code class="node">CSV</code> ノードを使用して、フォーマットされたCSV文字列を出力します。

#### 例

![](/images/basic/generate-csv.png){:width="688px"}

{% raw %}
~~~json
[{"id":"457d9ad6.b737b4","type":"inject","z":"64133d39.bb0394","name":"single","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":90,"y":640,"wires":[["1e05fafd.887b05"]]},{"id":"1e05fafd.887b05","type":"change","z":"64133d39.bb0394","name":"Generate single payload","rules":[{"t":"set","p":"payload","pt":"msg","to":"{ \"a\":$floor(100*$random()),\"b\":$floor(100*$random()),\"c\":$floor(100*$random())}","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":270,"y":640,"wires":[["e9546682.b39898"]]},{"id":"e9546682.b39898","type":"csv","z":"64133d39.bb0394","name":"","sep":",","hdrin":"","hdrout":false,"multi":"one","ret":"\\n","temp":"a,b,c","skip":"0","x":450,"y":640,"wires":[["f83ad3b0.78d32"]]},{"id":"f83ad3b0.78d32","type":"debug","z":"64133d39.bb0394","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":590,"y":640,"wires":[]},{"id":"ae242f2c.d1c8a","type":"inject","z":"64133d39.bb0394","name":"array","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":90,"y":700,"wires":[["7535f521.4a88bc"]]},{"id":"7535f521.4a88bc","type":"change","z":"64133d39.bb0394","name":"Generate array payload","rules":[{"t":"set","p":"payload","pt":"msg","to":"[\t    { \"a\":$floor(100*$random()),\"b\":$floor(100*$random()),\"c\":$floor(100*$random())},\t    { \"a\":$floor(100*$random()),\"b\":$floor(100*$random()),\"c\":$floor(100*$random())},\t    { \"a\":$floor(100*$random()),\"b\":$floor(100*$random()),\"c\":$floor(100*$random())},\t    { \"a\":$floor(100*$random()),\"b\":$floor(100*$random()),\"c\":$floor(100*$random())}\t]","tot":"jsonata"}],"action":"","property":"","from":"","to":"","reg":false,"x":270,"y":700,"wires":[["f4e0465f.ef0338"]]},{"id":"f4e0465f.ef0338","type":"csv","z":"64133d39.bb0394","name":"","sep":",","hdrin":"","hdrout":true,"multi":"one","ret":"\\n","temp":"a,b,c","skip":"0","x":450,"y":700,"wires":[["6eb67fdf.58626"]]},{"id":"6eb67fdf.58626","type":"debug","z":"64133d39.bb0394","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":590,"y":700,"wires":[]}]
~~~
{: .flow}
{% endraw %}

### 議論

例では、最初のフローがランダムに生成された3つのプロパティを持つ単一のオブジェクトを含んだpayloadを生成しています。

```javascript
{
    a: 10,
    b: 20,
    c: 30
}
```

<code class="node">CSV</code> ノードは、列名として抽出するための列名を設定し、
それらを埋めるように対応するオブジェクトのプロパティが利用されます。

結果としてメッセージは、フォーマットされた単一の行データと末尾に改行を含んだ
CSV用の文字列を含みます。

```javascript
"10,20,30\n"
```

これは <code class="node">File Out</code> ノードへそのまま渡して、
既存のCSVファイルへ追記する用途に適しています。

2つめのフローはランダムに生成された値を含む配列オブジェクトをinjectしています:

```javascript
[
    { a: 80, b: 18, c: 2},
    { a: 52, b: 36, c: 10},
    { a: 91, b: 18, c: 61},
    { a: 32, b: 47, c: 65},
]
```

ここでも、CSVノードは抽出するための列名を設定しています。
また、1行目を列名として含めて出力するよう設定されています。

```javascript
a,b,c
80,18,2
52,36,10
91,18,61
32,47,65
```
