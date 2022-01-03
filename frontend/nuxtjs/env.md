## 主題：シークレットキーをどう管理するか

## 手段

- @nuxtjs/dotenv
- runtimeConfig

Node.js は標準で環境変数へのアクセス方法を提供しています。

```
// process.envでアクセスできる
console.log(process.env);
```

.env ファイルから変数をロード
.gitignore に追加する。dotenv 使わなくても、RuntimeConfig 使わなくても.env を process.env に追加できてる？

## 参考

https://nuxtjs.org/ja/tutorials/moving-from-nuxtjs-dotenv-to-runtime-config/

https://www.twilio.com/blog/working-with-environment-variables-in-node-js-html-jp

https://qiita.com/taai/items/cbc61b9b4a18aacad57e
