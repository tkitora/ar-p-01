# sdk/

GUIDE01 SDK をここに配置する。

## 取得元

https://git.happylifecreators.com/guide-series-sdk/guide01

(リポジトリは Apache License 2.0。ビルド済みライブラリの扱いは別途確認。)

## 配置手順

```bash
cd p-01/sdk
git clone https://git.happylifecreators.com/guide-series-sdk/guide01.git
```

確認すること:

- `guide01/esp32/examples/` に `hello`, `scan_connect_battery`, `full_demo` がある
- `guide01/libs/esp32/<arch>/<target>/libguide01_protocol.a` が、使うボード用に存在する
  - 無い場合は配布元に連絡する(SDK の README に記載あり)

## 注意

- この `sdk/` 以下はこちらで改変しない。改変が必要なコードは `firmware/` にコピーして行う。
- SDK 本体は再配布条件を確認するまで、外部に公開しない。
