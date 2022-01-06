### ドキュメント

https://firebase.google.com/docs/rules/get-started?authuser=0

### 要点

- Cloud Firestore、Realtime Database、Cloud Storage のデータセキュリティを確保する

- firebase セキュリティルール言語と呼ばれるものでルールを定める。注意点は Realtime Database と Cloud Firestore と Cloud Storage では設定方法が異なる。

- セキュリティルールを定めたら、それを firebase コンソールまたは firebase cli で反映させる

### Cloud Storage のルール定め方

ルールの基本要素はいかのもの。

- service 宣言
- match ブロック
- allow ステートメント

### 理解の仕方

「何のデータ」に対し「誰が」「どのように」アクセスするかを定めるのが趣旨なので、どこで「何のデータ」を定め、どこで「誰が」を定めるか対応させて考えると良いかも。

service 宣言：何のデータ（広義）
match ブロック：何のデータ（狭義）
allow ステートメント：どのように、誰が

### Cloud Storage セキュリティルールサンプル

firebase のストレージバケットが一つしかなくても
まず、/b/{bucket}/o の match ブロックを作ること。これを作らないとセキュリティルールができない。

```
rules_version = '2';
service firebase.storage {
	match /b/{bucket}/o {
  	match /users/{userId}/{document=**} {
    	allow read, write: if request.auth != null && request.auth.uid == userId;
  	}
  }
}
```
