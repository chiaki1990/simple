#### ecrにpushがうまく行かない
エラー：no basic auth credentials

```
// 以下コマンドはAWS CLI Version 2では削除されている
$ aws ecr get-login
```

```
// 代わりにこれを使う
$ aws ecr get-login-password | docker login --username AWS --password-stdin https://<aws_account_id>.dkr.ecr.<region>.amazonaws.com

```

