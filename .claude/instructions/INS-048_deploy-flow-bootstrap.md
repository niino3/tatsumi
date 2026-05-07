# 指示書 INS-048: tatsumi デプロイフロー導入＆初回デプロイ

**発行:** 秘書Claude 2026-05-07
**優先度:** 高
**対象:** tatsumi

## 概要

shimotaka準拠の小規模サイト向け3段デプロイフロー（`feature/* → master → deployment/production`）を導入し、初回デプロイを実行する。FTP接続情報は社長がGitHub Secretsに登録済み。

## 前提状況

- INS-047（5月の更新分1回目）コミット `f1b07f2` が `20260507_5月の更新分1回目` ブランチに存在、master未マージ
- 秘書Claude が以下のファイルを作業ツリーに配置済み（**未コミット**）
  - `.github/workflows/deploy-production.yml`
  - `.github/workflows/validate-branch-flow.yml`
  - `.claude/commands/deploy.md`, `merge-flow.md`
  - `.claude/deploy-checklist.md`, `emergency-hotfix-procedure.md`, `visual-deploy-flow.md`
  - `CLAUDE.md`（Git運用・デプロイ節を追記）
  - `.claude/instructions/INS-047_*.md`, `INS-048_*.md`

## 手順

### 1. INS-047 を master にマージ

```bash
git status                                 # クリーンか確認（.claudeなどは未追跡で残ってOK）
git checkout master
git pull tatsumi master
git merge 20260507_5月の更新分1回目 --no-edit
git push tatsumi master
```

### 2. デプロイ関連ファイルを master に commit

master ブランチに居る状態で：

```bash
git add .github/ .claude/ CLAUDE.md
git status                                 # 上記の新規ファイルのみ含まれることを確認
git commit -m "デプロイフロー導入（GitHub Actions FTP・3段ブランチ運用）"
git push tatsumi master
```

### 3. deployment/production ブランチを作成・push

```bash
git checkout -b deployment/production
git push -u tatsumi deployment/production
```

→ この push をトリガーに `Deploy Tatsumi Site to Production` ワークフローが走り、FTPで本番に配信される。

### 4. デプロイ結果確認

- GitHub Actions: https://github.com/niino3/tatsumi/actions
- ジョブが緑✅で完了することを確認
- 本番サイトの index.html / enkai.html / gentei2605.html を目視確認
- index→gentei2605、enkai→gentei2605 のリンク遷移確認

### 5. master に戻る

```bash
git checkout master
```

## 失敗時の対応

### FTP認証エラー
GitHub Secrets `FTP_SERVER` `FTP_USERNAME` `FTP_PASSWORD` `FTP_SERVER_DIR` を社長に確認。

### exclude ルールで意図せず反映されないファイルがある
`.github/workflows/deploy-production.yml` の `exclude:` 節を確認・調整して再デプロイ。

### `validate-branch-flow.yml` がエラーを出した
`.claude/emergency-hotfix-procedure.md` を参照。

## 完了条件

- [ ] master に INS-047 反映済み
- [ ] master にデプロイインフラ反映済み
- [ ] `deployment/production` ブランチが remote に存在
- [ ] GitHub Actions deploy ジョブ成功
- [ ] 本番サイトに 5月の更新分が反映されていることを目視確認
- [ ] `.secretary/reports/INS-048_tatsumi_完了.md` に完了報告
