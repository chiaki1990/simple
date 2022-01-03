認証の実装方法は 2 種類ある。

- FirebaseUI の利用(推奨)
- Firebase Authentication SDK の利用

## FirebaseUI

[ドキュメント](https://firebase.google.com/docs/auth/web/firebaseui?authuser=0)

## Firebase の使い方 Nuxt.js

まず firebase を plugin として組み合わせる。
nuxt には plugins ディレクトリがあるので、そこで firebase を初期化する。
初期化するコードは、プロジェクトの設定で表示されるコード

またプラグインを使わない方法として、@nuxtjs/firebase がある

## plugin の使い方・手順

プラグインには種類があるよう

- Vue プラグイン
- ES6 プラグイン

## 実装方法

Vue プラグイン：
1 pluins ディレクトリに使うプラグインの js ファイル作成
2 nuxt.config.js に上記のファイルパスを plugins キーの値として記述
(firebase の初期化は Vue プラグインを使用する)

また、プラグインを使わないで実装する方法もあるみたい。その際は

```
// こちらを使わず
npm install firebase firebaseui

// 以下をつかう

npm install firebase
npm install @nuxtjs/firebase
```

```
// 以下のimportは使えない
// import firebase from 'firebase'
import firebase from 'firebase/compat/app'
```

とりあえず nuxt/firebase を使って認証を実装する
page にログイン用の page 作成して認証させる

### signInSuccessWithAuthResult について

signInSuccessWithAuthResult は認証が成功したときに実行される。
だからこのメソッド実行中に認証データをストアに保存すると思っていた。しかしここで保存するのではなく onAuthStateChanged で使う

```

signInSuccessWithAuthResult(authResult, redirectUrl) {
          console.log('ok')
          console.log(authResult, redirectUrl)
          // 以下は出来ない
          // this.$store.dispatch('setAuthResult', authResult)
          return true
        }
```

### onAuthStateChanged について

onAuthStateChanged を使えばユーザーが認証されているか判定する

```
  const auth = getAuth()
    onAuthStateChanged(auth, (user) => {
      if (user) {
        // User is signed in, see docs for a list of available properties
        // https://firebase.google.com/docs/reference/js/firebase.User
        const uid = user.uid
        console.log(uid)
        // ...
      } else {
        // User is signed out
        // ...
      }
    })
```
