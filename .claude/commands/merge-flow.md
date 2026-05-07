# Merge Flow

トピックブランチ → master のマージフローを実行します。tatsumiは小規模サイトのため develop 中間段なし。

## 引数
$ARGUMENTS: "master" または "all"。省略時は確認して決定。

## 手順

### master へマージ
1. `git status` でクリーンか確認
2. 現在のトピックブランチ名を取得（例: `20260507_5月の更新分1回目` や `feature/xxx`）
3. `git log --oneline master..<topic-branch>` で差分を表示してユーザー確認
4. `git checkout master && git merge <topic-branch> --no-edit && git push tatsumi master`
5. トピックブランチに戻る

### all（master → deploy）
1. master へマージ
2. `/deploy` コマンドを実行するか確認

## 注意
- マージ前に必ず `git log --oneline master..<topic-branch>` で差分を表示
- コンフリクトが発生したらユーザーに報告して手動解決を依頼
- ブランチ名は `feature/*` でも `YYYYMMDD_案件名` でも可（tatsumi慣行）
