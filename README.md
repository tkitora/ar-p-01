# p-01 — GUIDE01 × ESP32 実現性検証(公式アプリなし)

## 目的

**公式アプリを使わず、ESP32 から GUIDE01 を直接制御できるか**を確認する。
これは製品化の前の「できる/できない」の判定用プロトタイプであり、製品コードではない。

最終構想(参考):

```
[GUIDE01] <--BLE-- [ESP32] --Wi-Fi/MQTT-- [サーバー] -- [管理サイト]
```

p-01 で確認するのは、上の図のうち **GUIDE01 <-BLE-> ESP32** と、
**BLE 接続中に Wi-Fi + MQTT が同時に動くか**の2点だけ。
音声取り込み・文字起こし・管理サイトは対象外(p-02 以降)。

## 検証したいこと(判定項目)

| ID | 項目 | 重要度 |
|---|---|---|
| Q1 | ESP32 から GUIDE01 に BLE 接続できる(公式アプリなし) | 必須 |
| Q2 | ESP32 から GUIDE01 に**テキスト**を表示できる | 必須 |
| Q3 | GUIDE01 の**タッチ(タップ・スライド)**を ESP32 で受け取れる | 重要(不可なら ESP 側ボタンで代替) |
| Q4 | 複数行のログ表示・スクロールができる | 重要 |
| Q5 | BLE 接続を保ったまま Wi-Fi + MQTT が安定して動く | 必須 |
| Q6 | MQTT → ESP32 → GUIDE01 表示、GUIDE01 タッチ → ESP32 → MQTT が通る | 必須 |

詳細な手順と合否基準は `docs/TEST_PLAN.md` を参照。

## 前提・材料

- GUIDE01 本体 1台
- ESP32 系ボード(第一候補: M5Stick-C。SDK サンプルの動作確認機。メモリに余裕が要る場合は ESP32-S3)
- PC(PlatformIO、MQTT ブローカー Mosquitto を動かす)
- GUIDE01 SDK(GitLab: https://git.happylifecreators.com/guide-series-sdk/guide01)
  - `esp32/examples/` に `hello` / `scan_connect_battery` / `full_demo` がある
  - ビルド済み `libguide01_protocol.a` が同梱される(無い場合は配布元に連絡)
  - ライセンス: リポジトリは Apache 2.0(同梱ライブラリの扱いは未確認)

SDK は `sdk/` に置く(配置方法は `sdk/README.md`)。

## フォルダ構成

```
p-01/
├── README.md            このファイル
├── CLAUDE.md            Claude Code への作業指示
├── docs/
│   ├── TEST_PLAN.md     テスト手順と合否基準
│   └── FINDINGS.md      結果記録(テスト中に埋める)
├── sdk/                 GUIDE01 SDK を置く場所
├── firmware/            ESP32 検証コード(Claude Code が作る)
├── server/              Mosquitto 設定など
└── logs/                シリアルログ・測定ログ
```

## 結果の使い方

`docs/FINDINGS.md` の判定表が埋まったら、次を決める。

- Q1, Q2, Q5, Q6 が OK → 構想どおり進める(次は p-02: 音声取り込み)
- Q3 が NG → 操作は ESP 側のボタンで代替し、継続
- Q1 または Q2 が NG → 原因を特定し、グラスの変更も含めて再検討
