---
layout: default
title: Cookieを使用
slug:
  - label: HTTP
    url: /#http-endpoints
  - Cookie
---

### 課題

Cookieを使うHTTPフローを作成したい。

### 解決

<code class="node">HTTP In</code> ノードに送信されたメッセージは
`msg.req.cookies` プロパティを含んでおり、これが送信されたリクエストのCookieのリストになっています。

<code class="node">HTTP Response</code> ノードは
Cookieをセットやクリアするために `msg.cookies` を使用します。

#### 例

![](/images/http/work-with-cookies.png)

{% raw %}
~~~json
[{"id":"c362b989.954ae8","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-cookie","method":"get","swaggerDoc":"","x":130,"y":1020,"wires":[["21ddf71f.d00518"]]},{"id":"21ddf71f.d00518","type":"function","z":"3045204d.cfbae","name":"Format cookies","func":"msg.payload = JSON.stringify(msg.req.cookies,null,4);\nreturn msg;","outputs":1,"noerr":0,"x":340,"y":1020,"wires":[["f3aa98c1.befc18"]]},{"id":"f3aa98c1.befc18","type":"template","z":"3045204d.cfbae","name":"page","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":"<html>\n    <head></head>\n    <body>\n        <h1>Cookies</h1>\n        <p></p><a href=\"hello-cookie/add\">Add a cookie</a> &bull; <a href=\"hello-cookie/clear\">Clear cookies</a></p>\n        <pre>{{ payload }}</pre>\n    </body>\n</html>","x":530,"y":1020,"wires":[["f52e2880.180968"]]},{"id":"f52e2880.180968","type":"http response","z":"3045204d.cfbae","name":"","x":750,"y":1020,"wires":[]},{"id":"9a2a9a4.0fc0768","type":"change","z":"3045204d.cfbae","name":"Redirect to /hello-cookie","rules":[{"t":"set","p":"statusCode","pt":"msg","to":"302","tot":"num"},{"t":"set","p":"headers","pt":"msg","to":"{}","tot":"json"},{"t":"set","p":"headers.location","pt":"msg","to":"/hello-cookie","tot":"str"}],"action":"","property":"","from":"","to":"","reg":false,"x":550,"y":1080,"wires":[["f52e2880.180968"]]},{"id":"afefb90.53dcf48","type":"function","z":"3045204d.cfbae","name":"Add a cookie","func":"msg.cookies = { };\nmsg.cookies[\"demo-\"+(Math.floor(Math.random()*1000))] = Date.now();\nreturn msg;","outputs":1,"noerr":0,"x":330,"y":1060,"wires":[["9a2a9a4.0fc0768"]]},{"id":"d5205a2c.db9018","type":"function","z":"3045204d.cfbae","name":"Clear cookies","func":"// Find demo cookies and clear them\nvar cookieNames = Object.keys(msg.req.cookies).filter(function(cookieName) { return /^demo-/.test(cookieName);});\nmsg.cookies = {};\n\ncookieNames.forEach(function(cookieName) {\n    msg.cookies[cookieName] = null;\n});\n\nreturn msg;","outputs":1,"noerr":0,"x":340,"y":1100,"wires":[["9a2a9a4.0fc0768"]]},{"id":"fda60c66.04975","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-cookie/add","method":"get","swaggerDoc":"","x":140,"y":1060,"wires":[["afefb90.53dcf48"]]},{"id":"35285a76.1f8636","type":"http in","z":"3045204d.cfbae","name":"","url":"/hello-cookie/clear","method":"get","swaggerDoc":"","x":140,"y":1100,"wires":[["d5205a2c.db9018"]]}]
~~~
{: .flow}
{% endraw %}

この例では次の3つのエンドポイントを提供しています。

 - `/hello-cookie` は、現在セットされているCookie一覧のページを返します。
 - `/hello-cookie/add` は、新しいCookieを追加し、`/hello-cookie` へリダイレクトします。
 - `/hello-cookie/clear` は、サンプルとして作られたすべてのCookieをクリアしし、`/hello-cookie` へリダイレクトします。

### 議論

`msg.req.cookies` プロパティは、現在のリクエストのCookieセットを含んだ
キーと値のペアで構成されたオブジェクトです。

~~~javascript
var mySessionId = msg.req.cookies['sessionId'];
~~~

レスポンスでCookieをセットするためには、キーと値のオブジェクトのような形で
`msg.cookies` プロパティにセットされていなければなりません。

値は、デフォルトとして文字列を設定することもできますし、
オプションでオブジェクトにすることもできます。

次の例は2つのCookieをセットしています。
ひとつは `name` をキーとして、値が `Nick` です。
もうひとつは `session` をキーとして、値が `1234` であり、15分で有効期限が切れます。

~~~javascript
msg.cookies = {
    name: 'nick',
    session: {
        value: '1234',
        maxAge: 900000
    }
}
~~~

オプションは次のとおりです:

- `domain` - (String) Cookieのドメイン名。
- `expires` - (Date) GMTでの有効期限。設定されていない場合や0が設定されている場合は、セッションCookieとなります。
- `maxAge` - (String) Cookieを発行した時点からミリ秒で相対的に指定する有効期限。
- `path` - (String) Cookieのパス。デフォルトは / です。
- `value` - (String) Cookieに登録される値。

Cookieを削除するには、そのキーの値にnullをセットします。
