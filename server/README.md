# server/

検証用の MQTT ブローカー設定。

```bash
mosquitto -c server/mosquitto.conf -v
```

## トピック案(検証用)

| トピック | 方向 | 内容 |
|---|---|---|
| `p01/display/text` | PC → ESP32 | GUIDE01 に表示する文字列 |
| `p01/touch` | ESP32 → PC | タッチイベント(種類と時刻) |
| `p01/status` | ESP32 → PC | 接続状態、バッテリー、空きヒープ |

動作確認:

```bash
mosquitto_sub -h localhost -t 'p01/#' -v
mosquitto_pub -h localhost -t 'p01/display/text' -m 'テスト'
```

注意: `allow_anonymous true` は検証専用。製品では認証と TLS を使う。
