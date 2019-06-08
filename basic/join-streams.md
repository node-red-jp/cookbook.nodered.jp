---
layout: default
title: 複数のストリームメッセージから単一のメッセージへまとめる
---

### 課題

異なるソースからメッセージを受信しており、単一のメッセージにまとめたい。
たとえば、3つの異なるセンサーが発する値を、単一のデータベースのエントリとして登録したい場合。

### 解決

各ストリームに一意の `msg.topic` を指定して、
<code class="node">Join</code> ノードを使用して単一のメッセージにまとめます。

#### 例

![](/images/basic/join-streams.png){:width="571px"}

{% raw %}
~~~json
[{"id":"8ccddb9a.a55f38","type":"inject","z":"ac14500e.2c57d","name":"temperature","topic":"temperature","payload":"10","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":110,"y":1760,"wires":[["47b769c5.cb0e28"]]},{"id":"47b769c5.cb0e28","type":"join","z":"ac14500e.2c57d","name":"","mode":"custom","build":"object","property":"payload","propertyType":"msg","key":"topic","joiner":"\\n","joinerType":"str","accumulate":false,"timeout":"","count":"3","reduceRight":false,"reduceExp":"","reduceInit":"","reduceInitType":"","reduceFixup":"","x":310,"y":1800,"wires":[["f9afb265.b11b7"]]},{"id":"f9afb265.b11b7","type":"debug","z":"ac14500e.2c57d","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","x":470,"y":1800,"wires":[]},{"id":"2d269127.4f04ce","type":"inject","z":"ac14500e.2c57d","name":"humidity","topic":"humidity","payload":"","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":1800,"wires":[["47b769c5.cb0e28"]]},{"id":"d6fbe805.0e4628","type":"inject","z":"ac14500e.2c57d","name":"pressure","topic":"pressure","payload":"999","payloadType":"num","repeat":"","crontab":"","once":false,"onceDelay":0.1,"x":100,"y":1840,"wires":[["47b769c5.cb0e28"]]}]
~~~
{: .flow}
{% endraw %}

### 議論

例のフローでは、それぞれの <code class="node">Inject</code> ノードは異なるメッセージ送信元を表しています。
それらは、別々の `msg.topic` に値をセットしており、後続のフローで識別されます。

<code class="node">Join</code> ノードは `msg.topic` をキー名として、
key/valueオブジェクトを生成するよう手動で設定されています。
すでにご存知のとおり、結合すべき3つの独立したメッセージのストリームが存在し、
ノードは指定数のメッセージパーツを受信後にメッセージを送信します。

各3つの異なるtopicに少なくとも1つのメッセージを受信するたびに、
各topicの最新の値を使用してメッセージを送信することを意味します。

```json
{
    "temperature":10,
    "humidity":0,
    "pressure":999
}
```

このノードにはさらに、レシピ内で使用されていない場合の挙動を変更するオプションを持っています。
たとえば、タイムアウトを設定し、センサーのひとつが値送信を停止した場合でも、
*何かしらのメッセージ*が送信されることを担保するする場合などです。
これが問題ある場合は、[このレシピ](/basic/trigger-timeout)を参考にプレースホルダーとなる値を送信することを検討してみてください。
