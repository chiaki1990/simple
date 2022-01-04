# nuxtjs と firebase で画像のアップロードとダウンロード

## 前提

- @nuxtjs/firebase を使う
- CSS フレームワークは Vuetify を選ぶ(重要)

## 手順概要

1. プロジェクト作成
1. firebase 等のモジュールをプロジェクトにインストール
1. firebase を作成
1. cloud storage バケット を firebase 内で作成
1. プロジェクトの nuxt.config.js に firebase のキー等を登録
1. ダウンロード、アップロードのコードを記述

## 手順

### 1 プロジェクト作成

```
// プロジェクト作成 (選択肢はデフォルトで)
yarn create nuxt-app test_firebase_storage
```

### 2 firebase 等のモジュールをプロジェクトにインストール

https://firebase.nuxtjs.org/guide/getting-started#requirements

```
yarn add firebase
yarn add @nuxtjs/firebase
```

@nuxtjs/firebase は nuxt.config.js を編集するだけで firebase を統合する事ができるモジュールで、素早く firebase をプロジェクトで使うことができる。また firebase 認証用の追加的な機能も提供している　https://firebase.nuxtjs.org/#what-is-this

### 3 firebase プロジェクト を作成

割愛。ここは firebase のコンソールから作成したけど、（firebase cli でも作成できる？）

### 4 cloud storage バケット を firebase 内で作成

![1](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.48.22.png)

![2](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.51.43.png)
![3](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.54.27.png)

![4](https://chiaki1990.github.io/simple/images/firebase_storage_2022-01-04 21.59.54.png)

### 5 プロジェクトの nuxt.config.js に firebase のキー等を登録

以下修正箇所

```javascript
 // Modules: https://go.nuxtjs.dev/config-modules
  modules: [
    // https://go.nuxtjs.dev/axios
    '@nuxtjs/axios',
    // @nuxt/firebaseをインストールしたから以下を追加
    // https://firebase.nuxtjs.org/guide/getting-started#example-configuration
    [
      '@nuxtjs/firebase',
      {
        config: {
          apiKey: process.env.apiKey,
          authDomain: process.env.authDomain,
          projectId: process.env.projectId,
          storageBucket: process.env.storageBucket,
          messagingSenderId: process.env.messagingSenderId,
          appId: process.env.appId,
          measurementId: process.env.measurementId,
        },
        services: {
          storage: true, // Just as example. Can be any other service.
        },
      },
    ],
  ],
```

### 6 ダウンロード、アップロードのコードを記述

this.$fire.storage でストレージモジュールにアクセスできる。
ユーザーから受け取った画像を push メソッドを使ってアップロードする。

https://firebase.google.com/docs/storage/web/upload-files#upload_files

```vue
<template>
  <div>
    <v-row>
      <v-col>
        <v-file-input
          v-model="file"
          accept="image/png, image/jpeg"
          label="画像をアップロード"
        ></v-file-input>
        <v-btn @click="uploadImage">アップロードする</v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn @click="getImageUrl">画像を表示させる</v-btn>
        <v-img
          v-if="imageUrl"
          max-height="150"
          max-width="250"
          :src="imageUrl"
        ></v-img>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  data() {
    return {
      file: null,
      imageUrl: null,
    };
  },
  methods: {
    uploadImage() {
      // firebase storage取得
      console.warn(this.file);
      const storage = this.$fire.storage;
      const ref = storage.ref();
      const imageRef = ref.child(this.file.name);
      imageRef.put(this.file).then((snapshot) => {
        console.log("Uploaded a blob or file!");
      });
    },
    async getImageUrl() {
      const storage = this.$fire.storage;
      // ストレージのルート
      const ref = storage.ref();
      // ストレージに入っている画像を表示するために画像一覧を取得
      // https://firebase.google.com/docs/storage/web/list-files
      const res = await ref.listAll();
      // 一番最初の画像URLを取得 画像のファイルパスを使って画像のrefを取得。refからダウンロードURLを取得
      // https://firebase.google.com/docs/storage/web/download-files#download_data_via_url
      const imageUrl = await ref.child(res.items[0].fullPath).getDownloadURL();
      this.imageUrl = imageUrl;
    },
  },
};
</script>
```

##### ソースコード

https://github.com/chiaki1990/firebase_cloud_storage
