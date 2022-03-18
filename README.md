# eqstream-api-docs
EqStream APIのドキュメント（令和最新版 : 公式）

## これはなんだ
緊急地震速報や津波情報、震度の情報、PGAアラートなどをWebsocketでほぼ無遅延で配信する無償のAPIです。<br>
国外のVPSで実行されているため災害に強いです。強震モニターとP2P地震情報の情報を利用しています。

## EqStream APIの基本的な仕様
プロトコル: Websocket <br>
アドレス: `wss://eqstream.xyz:3001` <br>
データ形式: json

## 配信されるjsonファイル

### 共通部分



### 例1 (緊急地震速報 予報)
沖縄本島北西で最大予想震度3の地震が予報されたとき

#### 第1報
`{"type":"eew","time":"1647553950000","report":"1","epicenter":"沖縄本島北西沖","depth":"10km","magnitude":4.5,"latitude":26.6,"longitude":126.6,"intensity":"3","index":3}`

#### 最終報
`{"type":"eew","time":"1647553950000","report":"final","epicenter":"沖縄本島北西沖","depth":"10km","magnitude":5,"latitude":26.6,"longitude":126.6,"intensity":"3","index":3}`

### 例2 (強震モニターの震度情報)
岩手、山形、宮城、福島で震度1が観測されたとき <br>
`{"type":"intensity_report","time":"1647637944472","max_index":1,"intensity_list":[{"intensity":"1","index":1,"region_list":["岩手","山形","宮城","福島"]}]}}`

## お前は誰
作成者: [nexryai](https://twitter.com/nexryai)
