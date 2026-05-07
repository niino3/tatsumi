# Visual Deploy Flow

```
┌────────────────────┐
│  feature/* または   │  ← 通常の作業ブランチ
│  YYYYMMDD_案件名   │     （クライアント更新依頼ごとに切る）
└────────┬───────────┘
         │ /merge-flow （または手動マージ）
         ▼
┌────────────────────┐
│      master        │  ← 検収済みの最新状態
└────────┬───────────┘
         │ /deploy （master → deployment/production マージ＋push）
         ▼
┌────────────────────┐
│ deployment/        │  ← push をトリガーに GitHub Actions が
│ production         │     FTPで本番サーバーへ自動配信
└────────────────────┘
```

## ❌ やってはいけないこと

- `deployment/production` への直接コミット
- `master` を経由しない `feature/*` → `deployment/production` 直マージ
- `deployment/production` が `master` より先行している状態の放置

## ✅ 緊急修正

緊急時も `hotfix/*` → `master` → `deployment/production` の順を守ること。  
詳細は `emergency-hotfix-procedure.md` 参照。
