# はじめに

Voicevox Engine は、エディタの後ろで動作している合成音声エンジンです。
実体は HTTP サーバーで、HTTP API を通じて操作することができます。

この章では、Voicevox Engine の HTTP API について説明します。

## CORS について

Voicevox Engine ではセキュリティ保護のため

- `localhost`
- `127.0.0.1`
- `app://`
- Origin なし

以外の Origin からリクエストを受け入れないようになっています。 そのため、一部のサードパーティアプリからのレスポンスを受け取れない可能性があります。
この設定を変更するには：

- `localhost:50021/setting` にアクセスし、許可したい Origin を追加する
- `--allow_origin [許可したい Origin]` を引数に追加して起動する
- `--cors_policy_mode all` を引数に追加して起動する（非推奨）

のいずれかを行ってください。
