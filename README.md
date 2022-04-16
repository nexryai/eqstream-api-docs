## サービス終了のお知らせ
サーバー維持や安定性の問題からサービスを終了します。短い間でしたがありがとうございました。

# これはなんだ
緊急地震速報や津波情報、震度の情報、PGAアラートなどをWebsocketでほぼ無遅延で配信する無償のAPIです。<br>
国外のVPSで実行されているため災害に強いです。強震モニターとP2P地震情報の情報を利用しています。

## EqStream APIの基本的な仕様
プロトコル: Websocket <br>
データ形式: json

## 利用時の注意
このAPIは正確性、速度に最大限気を配って開発されています。しかし人間は誰でも間違いを起こすものです。<br>
**このAPIを利用したことで発生したいかなる種類の損害も補償できません。** またAPI運営者は、サーバーに過剰な不可をかける等の行為が認められた場合、特定のIPアドレス、もしくはユーザーエージェントからのアクセスを遮断する権利を有します。

## プライバシーについて
セキュリティの事情により、接続に使用したIPアドレスは記録されます。それ以外は一切記録されません。 <br>
トラッキングなども一切行っておらず、記録されたIPアドレスも厳重に保管され法的機関からの要請がない限り第3者に開示することはありません。

## 詳細な仕様
[配信されるjsonデータについて](json.md) <br>
[サンプルコード](example.md)
