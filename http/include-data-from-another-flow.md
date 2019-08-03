---
layout: default
title: 他のフローで取得したデータの埋め込み
slug:
  - label: http
    url: /#http-endpoints
  - context
---

### 課題

HTTPリクエストに対して、他のフローから取得したデータを使用してレスポンスを返したい。

### 解決

`flow context` を使用してデータを格納し、HTTPのフロー内で取得する。

#### 例

![](/images/http/include-data-from-another-flow.png)

{% raw %}
~~~json
[{"id":"92eaf6c0.6d1508","type":"inject","z":"3045204d.cfbae","name":"","topic":"","payload":"","payloadType":"date","repeat":"","crontab":"","once":false,"x":100,"y":480,"wires":[["8055b557.7faa48"]]},{"id":"8055b557.7faa48","type":"change","z":"3045204d.cfbae","name":"Store time","rules":[{"t":"set","p":"timestamp","pt":"flow","to":"payload","tot":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":270,"y":480,"wires":[[]]},{"id":"93bf2335.6c40e","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-data","method":"get","swaggerDoc":"","x":120,"y":520,"wires":[["9e3aa25e.61c56"]]},{"id":"9e3aa25e.61c56","type":"change","z":"3045204d.cfbae","name":"Copy time","rules":[{"t":"set","p":"timestamp","pt":"msg","to":"timestamp","tot":"flow"}],"action":"","property":"","from":"","to":"","reg":false,"x":310,"y":520,"wires":[["f2c385a.f0d3c78"]]},{"id":"f2c385a.f0d3c78","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Time: {{ timestamp }}</h1>\n    </body>\n</html>","x":470,"y":520,"wires":[["def756a1.2108a8"]]},{"id":"def756a1.2108a8","type":"http response","z":"3045204d.cfbae","name":"","x":610,"y":520,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl http://localhost:1880/hello-data
<html>
    <head></head>
    <body>
        <h1>Time: 1480201022517</h1>
    </body>
</html>
~~~
{: .shell}

### 議論

フロー内でデータを格納や取得するには、たとえば外部のデータベースを使うなど、
異なる方法がたくさんあります。

Node-REDはキーと値をシンプルに格納する方法として `flow context` を提供しており、
同じタブ上ではすべてのノードからアクセスできます。

上記の例では、<code class="node">Inject</code> ノードで生成されたタイムスタンプを、
<code class="node">Change</code> ノードで `flow context` に格納しています。
フローはHTTPリクエストをハンドルし、そして別の <code class="node">Change</code> ノードで値を取得します。
そして、取得した値は <code class="node">Template</code> ノードでメッセージに値を埋め込み、レスポンスを生成します。
