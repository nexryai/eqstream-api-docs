# 配信されるjson

## 共通部分
#### `type`

|  値  |  内容  |
| ---- | ---- |
|  eew  |  緊急地震速報  |
|  tsunami  |  津波速報  |
|  intensity_report  |  強震モニターの震度情報  |
|  pga_alert  |  PGAアラート  | 


#### `time`
イベントが発生した時間です。UNIX時間で表します。


## 緊急地震速報

|  キー |  内容  |
| ---- | ---- |
|  report  |  第何報か。最終報の場合`final`になります。 |
|  epicenter  |  震源地の地名  |
|  depth  |  震源の深さ  |
|  magnitude  |  マグニチュード  |
|  latitude  |  震源の緯度  |
|  longitude  |  震源の軽度  |
|  intensity  |  予想最大震度（string）  |
|  index  |  予想最大震度（数値）  |


#### `intensity`と`index`の違い
どちらも震度を表しますが、5弱、5強、6弱、6強の際に`intensity`には文字列が入ります。`index`はプログラム内部で震度を判定するのに、`intensity`は人間向けにメッセージを出力するときに使われるべきです。<br>
間違えるとプログラムが異常終了したり、震度9などの化け物が爆誕してパニックに陥ったりします。

|  震度  |  `intensity`  |  `index`  |
| ---- | ---- | ---- |
|  1 - 4  |  1 - 4 |  1 - 4  |
|  5弱  |  5弱 | 5 |
|  5強  |  5強 | 6 |
|  6弱  |  6弱 | 7 |
|  6強  |  6強 | 8 |
|  7  |  7  | 9 |


#### 例: 沖縄本島北西で最大予想震度3の地震が予報されたとき
##### 第1報
`{"type":"eew","time":"1647553950000","report":"1","epicenter":"沖縄本島北西沖","depth":"10km","magnitude":4.5,"latitude":26.6,"longitude":126.6,"intensity":"3","index":3}`

##### 最終報
`{"type":"eew","time":"1647553950000","report":"final","epicenter":"沖縄本島北西沖","depth":"10km","magnitude":5,"latitude":26.6,"longitude":126.6,"intensity":"3","index":3}`

## 津波情報

津波の情報は`areas`という配列に格納されます。以下の表は`areas`内のオブジェクト一覧です。

|  キー |  内容  |
| ---- | ---- |
|  grade  |  警報の種類。詳細は後述。  |
|  immediate  |  既に到達・もしくは直ちに到達するか  |
|  name  |  地域名  |

#### 警報の種類
|  `grade`の値  |  内容  |
| ---- | ---- |
|  Watch  |  津波注意報  |
|  Warning  |  津波警報  |
|  MajorWarning  |  大津波警報  |

#### 例: 宮城県と福島県に津波注意報が発表されたとき
```
{
   "type":"tsunami",
   "areas": [
   	{ "grade": 'Watch', "immediate": true, "name": '宮城県' },
   	{ "grade": 'Watch', "immediate": true, "name": '福島県' }
   ],
   "time": 1647611887107
}
```

## 強震モニターの震度情報

|  キー  |  内容  |
| ---- | ---- |
|  max_index  |  最大震度（数値）  |
|  intensity_list  |  各地域の震度情報（配列）  |

|  `intensity_list`内のキー  |  内容  |
| ---- | ---- |
|  intensity  |  震度（string）  |
|  index  |  震度（数値）  |
|  region_list  |  その震度を観測した地域一覧  |

`intensity`と`index`の違いは[ここ](#intensity%E3%81%A8index%E3%81%AE%E9%81%95%E3%81%84)を参照 <br>
<br>

#### 例: 岩手、山形、宮城、福島で震度1が観測されたとき
`{"type":"intensity_report","time":"1647637944472","max_index":1,"intensity_list":[{"intensity":"1","index":1,"region_list":["岩手","山形","宮城","福島"]}]}}`

## PGAアラート

|  キー  |  内容  |
| ---- | ---- |
|  max_pga  |  観測されたPGA  |
|  new  |  新しいか  |
|  estimated_intensity  |  推定される震度（数値）  |
|  region_list  |  地域一覧（配列）  |

#### 例: PGA1.156、推定震度1の地震が岩手で観測されたとき
`{"type":"pga_alert","time":"1647645924074","max_pga":1.156,"new":true,"estimated_intensity":1,"region_list":["岩手"]}`

## 実際の例
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
