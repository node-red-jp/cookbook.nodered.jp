---
layout: default
title: リクエスト先URLをセット
slug:
  - label: HTTP
    url: /#http-requests
  - URLをセット
---

### 課題

HTTPリクエストノードのURLを動的に設定したい。

### 解決

<code class="node">HTTP Request</code> ノードのURLプロパティをセットします。

#### 例

![](/images/http/set-request-url.png)

{% raw %}
~~~json
[{"id":"b36aa30.3a7276","type":"http request","z":"c9a81b70.8abed8","name":"","method":"GET","ret":"txt","url":"","x":470,"y":300,"wires":[["1ef9987c.956c78"]]},{"id":"11167f67.5d5031","type":"inject","z":"c9a81b70.8abed8","name":"cars on craigslist","topic":"","payload":"http://vancouver.craigslist.org/search/sss?format=rss&query=cars","payloadType":"str","repeat":"","crontab":"","once":false,"x":140,"y":300,"wires":[["70154cd4.de1444"]]},{"id":"70154cd4.de1444","type":"change","z":"c9a81b70.8abed8","name":"","rules":[{"t":"set","p":"url","pt":"msg","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":310,"y":300,"wires":[["b36aa30.3a7276"]]},{"id":"1ef9987c.956c78","type":"debug","z":"c9a81b70.8abed8","name":"","active":true,"console":"false","complete":"false","x":630,"y":300,"wires":[]}]
~~~
{: .flow}
{% endraw %}

<code class="node">Inject</code> ノードでURLとなる文字列を生成し、
<code class="node">Change</code> ノードで `msg.URL` プロパティにセットしています。
このフローではURLは次のようにセットされます:

{% raw %}
~~~text
http://vancouver.craigslist.org/search/sss?format=rss&query=cars
~~~
{% endraw %}

Craigslistというサイトの、バンクーバーで売りに出されている車の情報をRSSで返します。
次のようなXMLの内容がデバッグウィンドウに表示されます:

{% raw %}
~~~text
<?xml version="1.0" encoding="UTF-8"?>

<rdf:RDF
 xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
 xmlns="http://purl.org/rss/1.0/"
 xmlns:enc="http://purl.oclc.org/net/rss_2.0/enc#"
 xmlns:ev="http://purl.org/rss/1.0/modules/event/"
 xmlns:content="http://purl.org/rss/1.0/modules/content/"
 xmlns:dcterms="http://purl.org/dc/terms/"
 xmlns:syn="http://purl.org/rss/1.0/modules/syndication/"
 xmlns:dc="http://purl.org/dc/elements/1.1/"
 xmlns:taxo="http://purl.org/rss/1.0/modules/taxonomy/"
 xmlns:admin="http://webns.net/mvcb/"
>

<channel rdf:about="https://vancouver.craigslist.ca/search/sss?format=rss&#x26;query=cars">
<title>craigslist vancouver, BC | for sale search "cars"</title>
<link>https://vancouver.craigslist.ca/search/sss?query=cars</link>
<description></description>
<dc:language>en-us</dc:language>
<dc:rights>copyright 2017 craiglist</dc:rights>
<dc:publisher>robot@craigslist.org</dc:publisher>
<dc:creator>robot@craigslist.org</dc:creator>
<dc:source>https://vancouver...
~~~
{% endraw %}

### 議論

RSSの内容をXMLに変換するために、<code class="node">HTTP Request</code> の後に
<code class="node">XML</code> も追加できます。
RSSの内容はアクセスしやすいようにJavaScriptオブジェクトで返されます。
