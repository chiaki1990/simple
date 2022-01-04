# nuxtjs と firebase で画像のアップロードとダウンロード

### 前提

@nuxtjs/firebase を使う

### 手順概要

1. プロジェクト作成
1. firebase 等のモジュールをプロジェクトにインストール
1. firebase を作成
1. cloud storage バケット を firebase 内で作成
1. プロジェクトの nuxt.config.js に firebase のキー等を登録
1. ダウンロード、アップロードのコードを記述

### 手順

1 プロジェクト作成

```
// プロジェクト作成 (選択肢はデフォルトで)
yarn create nuxt-app test_firebase_storage
```

2 firebase 等のモジュールをプロジェクトにインストール

https://firebase.nuxtjs.org/guide/getting-started#requirements

```
yarn add firebase
yarn add @nuxtjs/firebase
```

3 firebase プロジェクト を作成

割愛。ここは firebase のコンソールから作成したけど、（firebase cli でも作成できる？）

4 cloud storage バケット を firebase 内で作成
![1](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.48.22.png)

![2](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.51.43.png)
![3](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.54.27.png)

![4](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.59.54.png)
