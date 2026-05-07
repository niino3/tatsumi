# Deploy Checklist

`/deploy` 実行前に必ず確認。

## 事前チェック

- [ ] 現在のブランチが `master`
- [ ] `git status` がクリーン（未コミット変更なし）
- [ ] `git pull tatsumi master` で最新状態
- [ ] `git log --oneline deployment/production..master` で未デプロイコミット確認
- [ ] 差し替えファイル名・リンク先（gentei*.html）に typo なし
- [ ] index.html / enkai.html のリンク先 gentei ファイルが存在する

## デプロイ後

- [ ] GitHub Actions の deploy ジョブ成功（緑✅）
- [ ] 本番サイト（https://tatsumi-sushi.com/ 等）でトップページ表示確認
- [ ] 更新したページ（index, enkai, gentei*）の表示確認
- [ ] index→gentei、enkai→gentei のリンク遷移確認

## トラブル時

- FTP認証失敗 → GitHub Secrets の `FTP_SERVER` `FTP_USERNAME` `FTP_PASSWORD` `FTP_SERVER_DIR` を確認
- 一部ファイル未反映 → exclude ルール（`.github/workflows/deploy-production.yml`）確認
- ロールバック → `emergency-hotfix-procedure.md` の「ロールバック」節参照
