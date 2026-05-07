# Emergency Hotfix Procedure

本番障害・誤公開発生時の手順。

## 1. ホットフィックス手順

### 修正リリース
```bash
git checkout master
git pull tatsumi master
git checkout -b hotfix/YYYYMMDD_概要

# 修正作業
git add <files>
git commit -m "hotfix: <概要>"
git push -u tatsumi hotfix/YYYYMMDD_概要

# master へマージ
git checkout master
git merge hotfix/YYYYMMDD_概要 --no-edit
git push tatsumi master

# 本番デプロイ
git checkout deployment/production
git merge master --no-edit
git push tatsumi deployment/production
```

## 2. ロールバック（直前のデプロイを取り消し）

```bash
git checkout deployment/production
git log --oneline -5     # 戻したいコミットのハッシュを確認
git reset --hard <一つ前のコミットhash>
git push --force-with-lease tatsumi deployment/production
```

⚠️ `--force-with-lease` を使う（誰かが先行 push してたら止まる）。  
⚠️ master が先行している場合は、master 側にも同じ取消コミットを反映させること（divergence 検知に引っかかる）。

## 3. deployment/production が master より先行してしまった場合

`validate-branch-flow.yml` がエラーを出したら、選択肢2つ：

### A. cherry-pick で master に取り込む（推奨）
```bash
git checkout master
git cherry-pick <deployment/production の先行コミットhash>
git push tatsumi master
```

### B. deployment/production を master 基準にリセット
```bash
git checkout deployment/production
git reset --hard master
git push --force-with-lease tatsumi deployment/production
```

## 鉄則

- 本番反映は必ず `master → deployment/production` 経由
- `deployment/production` への直接コミット・直接マージは厳禁
- force push は `--force-with-lease` 必須
