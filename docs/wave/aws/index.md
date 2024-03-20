# Wave for AWS 概要

Waveを利用することで日次、月次のコストデータをほぼリアルタイムで確認できます。更に予算設定を行うことで予算を超えている場合に通知を受け取るなど、コスト最適化にご活用いただけます。

[Wave ログインURL](https://app.alphaus.cloud/wave/login)

![](../../assets/wave/2021-01-03_19.41.14.gif)

## 日次、月次グラフ

日次コストは過去31日分がグラフで表示されます。サービスのフィルタリングや日次、月次データの切り替えなどを行うことで日々のコスト状況を確認することができます。

**仕様上の注意点**

1. EC2(Amazon Elastic Compute Cloud) 、RDS(Amazon Relational Database Service)などの転送量はAWS Data Transferとして別項目で表示されます。
2. EBS(Amazon Elastic Block Store) の料金はEC2に含まれて表示されています。
