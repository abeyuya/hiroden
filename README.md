# hiroden

ひろでんのウェブサイト。Firebase Hosting で https://hiroden.web.app に公開する。

## リリースフロー

1. PR を `main` にマージする
2. `main` と `release/production` の差分からリリース PR が自動で作成・更新される ([create-release-pr.yml](.github/workflows/create-release-pr.yml))
3. リリース PR を merge commit でマージすると本番に deploy される ([deploy-production.yml](.github/workflows/deploy-production.yml))

リリース PR を squash / rebase でマージすると `release/production` が `main` から分岐し、次回以降のリリース PR が壊れる。

## ローカル確認

```bash
yarn install
yarn serve
```

`public/` 配下がそのまま配信される。

## Firebase / GCP 構成

| 項目 | 値 |
| --- | --- |
| GCP プロジェクト | `hiroden-web` (project ID `hiroden` は取得済みだったため) |
| Hosting サイト | `hiroden` (`hiroden.web.app`) |
| deploy 用サービスアカウント | `github-actions@hiroden-web.iam.gserviceaccount.com` (Firebase Hosting 管理者のみ) |
| 認証 | Workload Identity Federation。pool `github-actions` / provider `github`、`abeyuya/hiroden` の `release/production` ブランチで動くワークフローのみ許可 |

サービスアカウント鍵は発行していない。GitHub の secrets 設定は不要。
