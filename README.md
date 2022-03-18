# eqstream-api-docs
EqStream APIのドキュメント（令和最新版 : 公式）

# これはなんだ
緊急地震速報や津波情報、震度の情報、PGAアラートなどをWebsocketでほぼ無遅延で配信する無償のAPIです。<br>
国外のVPSで実行されているため災害に強いです。強震モニターとP2P地震情報の情報を利用しています。

# EqStream APIの基本的な仕様
プロトコル: Websocket <br>
アドレス: `wss://eqstream.xyz:3001` <br>
データ形式: json

# 配信されるjsonファイル

## 共通部分
#### `type`
eew（緊急地震速報）
tsunami（津波速報）
intensity_report（強震モニターの震度情報）、pga_alert（PGAアラート） <br>
のいずれかの値です。


## 緊急地震速報 予報
沖縄本島北西で最大予想震度3の地震が予報されたとき

#### 第1報
`{"type":"eew","time":"1647553950000","report":"1","epicenter":"沖縄本島北西沖","depth":"10km","magnitude":4.5,"latitude":26.6,"longitude":126.6,"intensity":"3","index":3}`

#### 最終報
`{"type":"eew","time":"1647553950000","report":"final","epicenter":"沖縄本島北西沖","depth":"10km","magnitude":5,"latitude":26.6,"longitude":126.6,"intensity":"3","index":3}`

## 津波情報
```
{
   "type":"tsunami",
   "areas": [
   	{ grade: 'Watch', immediate: true, name: '宮城県' },
   	{ grade: 'Watch', immediate: true, name: '福島県' }
   ],
   "time": 1647611887107
}
```

## 強震モニターの震度情報
岩手、山形、宮城、福島で震度1が観測されたとき <br>
`{"type":"intensity_report","time":"1647637944472","max_index":1,"intensity_list":[{"intensity":"1","index":1,"region_list":["岩手","山形","宮城","福島"]}]}}`

## PGAアラート
`{"type":"pga_alert","time":"1647638187472","max_pga":0.033,"new":true,"estimated_intensity":0,"region_list":["福島"]}`

# 実際の例
[この地震](https://typhoon.yahoo.co.jp/weather/jp/earthquake/20220318232520.html)発生時の配信データ<br>

```
{"type":"pga_alert","time":"1647645924074","max_pga":1.156,"new":true,"estimated_intensity":1,"region_list":["岩手"]}
{"type":"eew","time":"1647645917000","report":"1","epicenter":"岩手県沖","depth":"10km","magnitude":5.6,"latitude":40,"longitude":142,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"2","epicenter":"岩手県沖","depth":"10km","magnitude":5.6,"latitude":40,"longitude":142,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"3","epicenter":"岩手県沖","depth":"10km","magnitude":5.6,"latitude":40,"longitude":142,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"4","epicenter":"岩手県沖","depth":"10km","magnitude":5.8,"latitude":40,"longitude":142,"intensity":"5強","index":6}
{"type":"eew","time":"1647645917000","report":"5","epicenter":"岩手県沖","depth":"20km","magnitude":5.8,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"6","epicenter":"岩手県沖","depth":"20km","magnitude":5.8,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"7","epicenter":"岩手県沖","depth":"20km","magnitude":5.8,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"intensity_report","time":"1647645924074","max_index":4,"intensity_list":[{"intensity":"4","index":4,"region_list":["岩手"]},{"intensity":"3","index":3,"region_list":["青森"]}]}
{"type":"eew","time":"1647645917000","report":"9","epicenter":"岩手県沖","depth":"20km","magnitude":4.9,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"7","epicenter":"岩手県沖","depth":"20km","magnitude":5.8,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"8","epicenter":"岩手県沖","depth":"20km","magnitude":5.4,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"9","epicenter":"岩手県沖","depth":"20km","magnitude":4.9,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"10","epicenter":"岩手県沖","depth":"20km","magnitude":5.2,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"final","epicenter":"岩手県沖","depth":"10km","magnitude":5.3,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"12","epicenter":"岩手県沖","depth":"10km","magnitude":5.3,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
{"type":"eew","time":"1647645917000","report":"final","epicenter":"岩手県沖","depth":"10km","magnitude":5.3,"latitude":40,"longitude":142.1,"intensity":"5弱","index":5}
```

## お前は誰
作成者: [nexryai](https://twitter.com/nexryai)
