とりあえず nuxt/firebase を使って認証を実装する
page にログイン用の page 作成して認証させる

```
yarn create nuxt-app test_auth
# https://github.com/nuxt-community/firebase-module
yarn add firebase
yarn add @nuxtjs/firebase
# ログインコンポーネントを利用 https://github.com/firebase/firebaseui-web
yarn add firebaseui
```

nuxt.config.js を編集

```
# nuxt.config.js

modules: [
    [
      '@nuxtjs/firebase',
      {
        config: {
          apiKey: '<apiKey>',
          authDomain: '<authDomain>',
          projectId: '<projectId>',
          storageBucket: '<storageBucket>',
          messagingSenderId: '<messagingSenderId>',
          appId: '<appId>',
          measurementId: '<measurementId>'
        },
        services: {
          auth: true // Just as example. Can be any other service.
        }
      }
    ]
  ],
```

```
$ touch pages/login.vue
```

```
# login.vue

<template>
  <v-container>
    <div>
      <div id="firebaseui-auth-container"></div>
    </div>
    <v-btn to="/"> home </v-btn>
  </v-container>
</template>
<script>
export default {
  mounted() {
    const firebaseui = require('firebaseui')
    require('firebaseui/dist/firebaseui.css')
    const ui =
      firebaseui.auth.AuthUI.getInstance() ||
      new firebaseui.auth.AuthUI(this.$fire.auth)
    const tmp = this

    const config = {
      signInOptions: [
        {
          provider: this.$fireModule.auth.EmailAuthProvider.PROVIDER_ID,
          requireDisplayName: false,
        },
        //firebase.auth.EmailAuthProvider.PROVIDER_ID,
        // this.$fireModule.auth.EmailAuthProvider.PROVIDER_ID,
        this.$fireModule.auth.GoogleAuthProvider.PROVIDER_ID,
        //this.$fireModule.auth.GoogleAuthProvider.PROVIDER_ID
      ],
      signInSuccessUrl: '/',
      callbacks: {
        signInSuccessWithAuthResult(authResult, redirectUrl) {
          console.log('ok')
          console.log(authResult, redirectUrl)
          return true
        },
        uiShown: function () {
          console.log('ui')
        },
      },
    }

    ui.start('#firebaseui-auth-container', config)
  },
}
</script>



```

##### 補足 1

認証した user のデータを取りたいときは以下の流れで取得する

```js
import { getAuth, onAuthStateChanged } from "firebase/auth";

const auth = getAuth();
onAuthStateChanged(auth, (user) => {
  if (user) {
    // User is signed in, see docs for a list of available properties
    // https://firebase.google.com/docs/reference/js/firebase.User
    const uid = user.uid;
    console.log(uid);
    // ...
  } else {
    // User is signed out
    // ...
  }
});
```

##### 補足 2

https://firebase.nuxtjs.org/guide/usage/

this.$fireやthis.$fireModule を使うことで firebase のモジュールを使うことができる
