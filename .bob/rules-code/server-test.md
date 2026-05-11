# サーバー起動中のAPIテスト方法

## 短時間起動による動作確認（PowerShell）

APIの疎通確認など、サーバを一時的に起動して即停止したい場合は `Start-Process` を使う。

```powershell
$proc = Start-Process -FilePath ".\venv\Scripts\python.exe" -ArgumentList "app.py" -PassThru -WindowStyle Hidden
Start-Sleep -Seconds 3          # Flask 起動完了を待つ
Invoke-WebRequest -Uri "http://localhost:<ポート>/<確認したいパス>" -UseBasicParsing
$proc.Kill()                    # 確認後に停止
```

- `-PassThru` でプロセスオブジェクトを受け取り、最後に `Kill()` で確実に終了させる。
- `activate` は使わず、`venv\Scripts\python.exe` をフルパス指定する。

## curl 実行ルール（Windows PowerShell）

PowerShell では `curl` が `Invoke-WebRequest` のエイリアスなので、必ず `curl.exe` を使う。加えて以下に注意：

- 日本語を含む JSON は別ファイルに保存し `-d '@data.json'` で渡す（インラインの `\"` エスケープと日本語の混在は壊れる）。
- `@` から始まる引数はシングルクォートで囲む（PowerShell のスプラッティング回避）。
- URL クエリの日本語は事前に URL エンコードして渡す（自動エンコードされない）。
