
### 要素
1. クラスター
2. タスク
3. サービス


タスクで定めること
family
containerDefinitions
ここで、ECRリポジトリのイメージやコンテナ情報を定める

サービスで定めること
cluster
serviceName
taskDefinition
loadBalancers
desiredCount
role
どのタスクを使うか、ロードバランサー使うならロードバランサーの設定、どのクラスタを使うか




