# 指示書 INS-156: tatsumi 9月の更新分 1回目

**発行:** 秘書Claude 2026-09-06
**優先度:** 通常
**対象:** tatsumi（居酒屋たつみ・GoLive製 静的HTML）
**担当:** 秘書Claude（作成〜本番反映まで一貫実施）
**種別:** ✏️ HP更新（定例）
**関連:** INS-047（5月分）／INS-119（7月分）— 同一パターン

---

## 依頼内容

デザイナーからの入稿 `docs/9月の更新分 1回目/` を本番へ反映する。

## 入稿内容（3ファイル）

| ファイル | 扱い | 備考 |
|---|---|---|
| `gentei2609.html` | **新規追加** | 2026年 9・10月 特別宴会プラン |
| `index.html` | 上書き | お知らせブロックが `gentei2609.html` を参照 |
| `enkai.html` | 上書き | 宴会ページ |

`指示.txt` は同梱なし。**5月分・7月分と同一パターン**のため、3ファイル適用と判断する（前例どおり）。

## 適用前の検証結果（2026-09-06 実施済）

- `index.html` の参照が `gentei2607.html` → **`gentei2609.html`** に更新されている
- 季節コピーが `index.html` / `gentei2609.html` とも **「2026年 9・10月特別宴会プラン!!」** で一致
- `gentei2609.html` はルートに未存在＝新規追加で正しい
- `enkai.html` は差分あり（上書き対象）

## 作業方針（CLAUDE.md 準拠）

- **デザイナー版をそのまま適用。ハンド編集は行わない**（デザイナーがGoLiveで再度開くため）
- GoLive由来の `<csscriptdict>` / `<csactiondict>` / `<csobj>` / `CSInit` 等は**一切触らない**
- **旧 `gentei*.html` は削除しない**（過去分は保持する運用）
- `docs/9月の更新分 1回目/` は納品記録としてそのまま残す
- `.DS_Store` はDropbox由来のノイズ。ブランチ切替を阻む場合は破棄してよい

## ブランチフロー（⚠️厳守）

```
202609月の更新分1回目 → master → deployment/production
```

- `deployment/production` への直接コミット禁止
- `master` を経由しない直マージ禁止
- `validate-branch-flow.yml` が違反を検知してデプロイを停止する

## デプロイ

`deployment/production` への push で GitHub Actions のFTPデプロイが起動する。

## 完了条件

- [ ] 3ファイルを適用（gentei2609 新規追加／index・enkai 上書き）
- [ ] ルートの `index.html` が `gentei2609.html` を参照していることを確認
- [ ] ブランチフローを遵守して master へマージ
- [ ] `deployment/production` へ push
- [ ] **GitHub Actions のFTPデプロイが success**（Validate Branch Flow も通過）
- [ ] 完了報告を `.secretary/reports/INS-156_tatsumi_完了.md` に配置
