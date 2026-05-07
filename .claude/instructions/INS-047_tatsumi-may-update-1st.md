# 指示書 INS-047: tatsumi 5月の更新分 1回目

**発行:** 秘書Claude 2026-05-07
**優先度:** 中
**対象:** tatsumi

## 内容

`docs/5月の更新分 1回目/` の3ファイルをそのまま置き換え。

| ファイル | 種別 |
|---|---|
| `gentei2605.html` | 新規 |
| `index.html` | 差し替え |
| `enkai.html` | 差し替え |

## 手順

```bash
git checkout master && git pull
git checkout -b 20260507_5月の更新分1回目

cp "docs/5月の更新分 1回目/gentei2605.html" ./
cp "docs/5月の更新分 1回目/index.html" ./
cp "docs/5月の更新分 1回目/enkai.html" ./

git add gentei2605.html index.html enkai.html
git commit -m "5月の更新分1回目"
git push -u tatsumi 20260507_5月の更新分1回目
```

master マージは社長の指示待ち。

## 完了報告

`.secretary/reports/INS-047_tatsumi_完了.md` に書くこと（書式は CLAUDE.md 末尾参照）。
