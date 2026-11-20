# Discord スタンプ画像リポジトリ

`/stamp` コマンドから参照する、公開用のスタンプ画像集です。画像ファイルと`stamps/manifest.json`を同じリポジトリで管理し、GitHubのraw URLをWorkerの`STAMP_MANIFEST_URL`に設定します。

## ディレクトリ形式

```text
discord-stamps/
├── README.md
├── LICENSE
└── stamps/
    ├── manifest.json
    └── <stamp-id>/
        └── result.gif
```

`<stamp-id>`は小文字英数字、ハイフン、アンダースコアのみで、先頭は英数字、最大32文字とします。1スタンプにつきGIFを1つ置き、ファイル名は`result.gif`に統一します。

## `stamps/manifest.json` フォーマット

```json
{
  "version": 1,
  "stamps": [
    {
      "id": "sample",
      "label": "サンプル",
      "emoji": { "name": "✨" },
      "gif": "https://raw.githubusercontent.com/OWNER/discord-stamps/main/stamps/sample/result.gif"
    }
  ]
}
```

- `version`：必ず`1`
- `stamps`：最大25件。配列順がボタン順になる
- `id`：一意なスタンプID
- `label`：ボタンのラベル（80文字以内）
- `emoji`：Unicode絵文字なら`name`、カスタム絵文字なら`name`・`id`・必要に応じて`animated: true`
- `gif`：HTTPSで取得できるGIFの公開URL。GitHubではraw URLを使う

画像は無断転載を避け、リポジトリの`LICENSE`で利用条件を明記してください。GIFはDiscordの制限を超えないよう、実際の利用環境で表示確認してください。

## Workerへの設定

```text
STAMP_MANIFEST_URL=https://raw.githubusercontent.com/OWNER/discord-stamps/main/stamps/manifest.json
```

変更後はWorkerを再デプロイし、コマンド定義変更時はBotトークンを使って`npm run register`を実行します。
