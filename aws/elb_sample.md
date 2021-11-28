ALBをまず使ってみるサンプル(ECSでALBの使い方が分からずうまくできなかった。。。)

expressでアプリケーションを作成

```
$ mkdir sample_express
$ cd sample_express
```

nodeをインストール  
https://nodejs.org/en/download/package-manager/#macos
```
$ brew install node
```

expressをインストール  
https://expressjs.com/
```
$ npm install express
```

```
$ touch server.js
```

```js
// server.jsの内容
const express = require('express');
const app = express();
const port = 3000;

app.get('/', function(req, res) {
  res.send('Hello World!')
});

app.post('/', function (req, res) {
    res.send("This is POST method")
});

app.listen(port, function() {
  console.log(`Example app listening on port ${port}!`)
});
```


```
$ cd express_sample
$ node server.js
// これでlocalhost:3000にアクセスしてHello World!が表示されたらアプリは完成
```
リモートリポジトリにソースコード保存しておく





AWSリソース作成

EC2インスタンスを作りアプリを動かす

1 EC2用のSGに カスタムTCPポート3000、ソース0.0.0.0/0 を設定する

2 EC2インスタンスにアクセスしnodejsをインストール
https://docs.aws.amazon.com/ja_jp/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html#setting-up-node-on-ec2-instance-procedure

```
// nvmインストール
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
// nvm有効化
. ~/.nvm/nvm.sh
// nodejsインストール
nvm install node
// インストールされていればバージョン確認できる
node --version
v17.1.0
```
3 gitをインストールしソースコードをpullし実行
```
$ sudo yum update
$ sudo yum install git
$ git clone {expressのリポジトリurl}
```
4 アプリを起動
```
$ npm install 
$ node {expressのサーバーファイル(server.js)}
// アプリの起動
$ curl {EC2 パブリック IPv4 アドレス}:3000
// 接続できたらOK
// サービス化 https://github.com/Unitech/pm2#installing-pm2
$ npm install pm2 -g
$ pm2 start {expressのサーバーファイル(server.js)}
```

5 ロードバランサーを追加してアプリを実行  

1. ALBにアクセスするためのSGを作成  
   インバウンド：ポート80,リソース0.0.0.0/0で設定,アウトバウンド:ポート3000, リソース0.0.0.0/0)
3. ALBからEC2につなぐターゲットグループ作成(接続先をEC２インスタンス)
4. ALBからEC2に渡すためのリスナー設定(ポート80, ターゲットグループ)
5. ALBにターゲットグループをアタッチ



