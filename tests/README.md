# ブラウザ回帰テスト

Python 3 と Playwright、Chromium を使用します。アプリ自体にビルドは不要です。

```sh
python -m pip install playwright
python -m playwright install chromium
python tests/browser_regression.py
```

システムの `chromium` があれば使用します。別の実行ファイルは `CHROMIUM_PATH` で指定できます。
テスト用 HTTP サーバーは空きポートで自動起動し、終了時に停止します。
各テストは独立したブラウザコンテキストを使い、実際の利用者の保存データには触れません。

画像保存の検証には html2canvas 1.4.1 を使用します。初回に公式 CDN から TLS 検証付きで取得し、SHA-256 も検証します。
保存済みのライブラリは `HTML2CANVAS_PATH` で指定できます。CDN へのブラウザリクエストだけをこのファイルで応答し、アプリの画像は実際の HTTP 配信で確認します。
読み込み失敗・容量不足・タッチキャンセルは意図的に再現しています。

```sh
python tests/browser_regression.py --report /tmp/zzz-results.json
python tests/browser_regression.py --source /tmp/before.html --report /tmp/zzz-before.json
```

42 シナリオを確認し、失敗があれば終了コード 1 を返します。
編成・移動・入れ替え・重複制限・削除・検索・凸設定・保存復元・旧名と旧画像の移行・全103画像・グループ増減・リセット・PNG出力を含みます。
コスト計算は62エージェント × 7段階の凸 × 5段階の音動機凸 × 3種類の保存表記、計6,510通りを検証します。
モバイル操作は Chromium のタッチ入力による検証であり、iPhone / Android 実機の検証ではありません。
