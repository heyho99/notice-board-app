# サーバー起動中のAPIテスト方法

## 短時間起動による動作確認（bash）

APIの疎通確認など、サーバを一時的に起動して即停止したい場合は、バックグラウンド起動と `curl` を使う。

```bash
venv/bin/python app.py &
PID=$!
sleep 3          # Flask 起動完了を待つ
curl -s "http://localhost:<ポート>/<確認したいパス>"
kill $PID         # 確認後に停止
```

- `$!` で直前にバックグラウンド起動したプロセスのPIDを取得し、最後に `kill` で確実に終了させる。
- `activate` は使わず、`venv/bin/python` をフルパス指定する。
