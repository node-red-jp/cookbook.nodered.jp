---
layout: default
title: フローにフォームデータをPOST
slug:
  - label: http
    url: /#http-endpoints
  - post form data
---

### 課題

フォームのデータをフローにPOSTしたい。

### 解決

<code class="node">HTTP In</code> ノードを使用して
`Content-Type` に `application/x-www-form-urlencoded` を持ったPOSTリクエストを待ち受けます。
そして、`msg.payload` のプロパティとしてフォームのデータにアクセスします。

#### 例

![](/images/http/post-form-data-to-a-flow.png)

{% raw %}
~~~json
[{"id":"5b98a8ac.a46758","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-form","method":"post","swaggerDoc":"","x":120,"y":820,"wires":[["bba61009.4459f"]]},{"id":"bba61009.4459f","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Hello {{ payload.name }}!</h1>\n    </body>\n</html>","x":290,"y":820,"wires":[["6ceb930a.93146c"]]},{"id":"6ceb930a.93146c","type":"http response","z":"3045204d.cfbae","name":"","x":430,"y":820,"wires":[]}]
~~~
{: .flow}
{% endraw %}

~~~text
[~]$ curl -X POST -d "name=Nick" http://localhost:1880/hello-form
<html>
    <head></head>
    <body>
        <h1>Hello Nick!</h1>
    </body>
</html>
~~~
{: .shell}

### 議論

HTMLのフォームはブラウザのデータをサーバーに送信するよう使用されます。
もしデータが `POST` するよう設定されている場合、
ブラウザは `<form>` が保持するデータを `application/x-www-form-urlencoded` という `content-type` となるようエンコードします。

たとえば、次のようなフォームが送信されたとします。

~~~html
<form action="http://localhost:1880/hello-form" method="post">
  <input name="name" value="Nick">
  <button>Say hello</button>
</form>
~~~

この場合のリクエストは次のようになります。

~~~text
POST / HTTP/1.1
Host: localhost:1880
Content-Type: application/x-www-form-urlencoded
Content-Length: 9

name=Nick
~~~

<code class="node">HTTP In</code> ノードはこのようなリクエストを受けると、
リクエストボディを変換し、フォームのデータを `msg.payload` を通じて利用できるようにします。

~~~javascript
var name = msg.payload.name;
~~~
