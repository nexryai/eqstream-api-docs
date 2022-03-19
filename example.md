# サンプルコード
ここにあるコードはコピペしても構いませんが、正確性は保証しません。

## node.js
```
var WebSocket = require('ws');
process.env['NODE_TLS_REJECT_UNAUTHORIZED'] = 0;
let ws = new WebSocket('wss://eqstream.xyz:3001');
 
function ws_connect(){
    // 接続時に通知
    ws.addEventListener('open', e => {
        console.log('connected!');
    })

    // サーバからのデータ受信時の処理
    ws.addEventListener('message', e => {
        const obj = JSON.parse(e.data);

        console.log('=======new event=======');
        console.log(obj.time);
        console.log('コード：' + obj.type);

        switch (obj.type) {
            case 'eew':
                console.log('!@@@@@@緊急地震速報（EEW）@@@@@@!');
                console.log(obj.report + '報')
                console.log(obj.epicenter + ' 震度' + obj.intensity);
                break;


            case 'pga_alert':
                console.log('地震検知情報')
                console.log(obj.region_list.toString() + ' 震度' + obj.estimated_intensity + ' 最大PGA' + obj.max_pga)
                break;
            
            case 'intensity_report':
                console.log('震度情報')
                console.log('最大震度' + obj.max_index + 'の地震が発生しました。')

                var intensity_list = obj.intensity_list; 

                intensity_list.forEach(function(value, index, array){
                    console.log('=====');
                    Object.keys(array[index]).forEach(function(key) {

                        switch (key) {
                            case 'intensity':
                            console.log('震度' + array[index][key]);  
                            break;

                        case 'region_list':
                            console.log(array[index][key].toString());  
                            break;

                        default:  
                        } 
                        
                    })
                })
                console.log('=====');

                break;

            default:
                console.log('不明な情報');
        }
    })

    // 切断時に再接続
    ws.addEventListener('close', e => {
        console.log('APIサーバーから切断されました。接続を再試行します。');
        ws_connect();      
    })

}

ws_connect();
```
