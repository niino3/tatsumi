# Deploy to Production

本番環境にtatsumiサイトをデプロイします。master → deployment/production にマージしてGitHub Actions FTPデプロイを起動します。

## 手順

### 1. 事前チェック
- `git status` で作業ツリーがクリーンか確認
- `git branch --show-current` で現在のブランチを確認
- masterブランチでなければ、masterに切り替えるよう案内

### 2. デプロイ対象の確認
- `git log --oneline deployment/production..master` で未デプロイのコミットを表示
- コミットが無ければ「デプロイ対象がありません」と案内して終了

### 3. ユーザーに確認
- 表示されたコミット一覧を見せて、デプロイしてよいか確認を取る

### 4. デプロイ実行
```bash
git checkout deployment/production
git merge master --no-edit
git push tatsumi deployment/production
git checkout master
```

### 5. 完了報告
- 「デプロイがトリガーされました」と報告
- GitHub Actionsの確認URL: https://github.com/niino3/tatsumi/actions
