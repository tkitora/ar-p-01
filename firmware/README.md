# firmware/

検証用ファームウェア(PlatformIO プロジェクト)。Claude Code が作成する。

方針:

- SDK の `full_demo` をコピーして改造する(BLE は Arduino + NimBLE 構成)
- Wi-Fi・MQTT の認証情報は `include/secrets.h` に置き、git に入れない
- 雛形として `include/secrets.example.h` を作る
- 検証ごとにコミットまたはブランチを分ける

想定構成:

```
firmware/
├── platformio.ini
├── include/
│   ├── secrets.example.h
│   └── secrets.h         (git 管理外)
└── src/
    └── main.cpp
```
