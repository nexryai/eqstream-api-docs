# eqstream-api-docs
EqStream APIのドキュメント（令和最新版 : 公式）

## これはなんだ
緊急地震速報や津波情報、震度の情報、PGAアラートなどをWebsocketでほぼ無遅延で配信する無償のAPIです。<br>
国外のVPSで実行されているため災害に強いです。強震モニターとP2P地震情報の情報を利用しています。

## EqStream APIの基本的な仕様
プロトコル: Websocket <br>
アドレス: `wss://eqstream.xyz:3001` <br>
データ形式: json
