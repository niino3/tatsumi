# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Static HTML site for 居酒屋たつみ (Izakaya Tatsumi, Shimotakaido). All pages live at the repo root and are deployed by uploading the HTML files and `*_image/` folders as-is. **There is no build step, no tests, no lint, and no package manager.** Do not introduce one.

The pages were authored in **Adobe GoLive**. Every `.html` file contains GoLive-generated boilerplate — `<csscriptdict>`, `<csactiondict>`, `CSInit`, `CSScriptInit()`, `CSILoad`, `CSIShow`, `<csobj …>` wrappers around image-rollover buttons — plus table-based layout, `<font>`, `<center>`, and `<b>`. Preserve this style when editing; do **not** rewrite pages to modern HTML/CSS, do not strip the GoLive scripts, and do not "clean up" `<csobj>` markers — the designer reopens these files in GoLive.

GoLive site-metadata that looks like junk but must be preserved:
- `RESOURCE.FRK/` — GoLive resource fork sidecars (8.3 filenames like `DRINK~1.HTM`).
- `FINDER.DAT` — Mac Finder metadata GoLive uses.
- `tatsumi/` — empty directory kept by GoLive.
- `.DS_Store` — leave it; the project is on Dropbox.

## The update workflow (this is the core of the repo)

The designer delivers each round of updates as a folder dropped under `docs/`, e.g. `docs/5月の更新分 1回目/`. That folder contains:

- A small set of replacement `.html` files (sometimes a brand-new file too).
- `指示.txt` — a few-line plain-text instruction listing which files changed and how to apply them. **Read `指示.txt` first and follow it literally.** Typical content: "更新のファイルは3点（新規1点）です。… すべて、そのまま置き換えてください。" → copy each listed file from the `docs/...` folder over the same-named file at the repo root (and add the new one).

Conventions for handling a delivery:
- Each delivery gets its own branch named in Japanese, e.g. `20250505_5月の更新分1回目`, `202602月の更新分-1回目`. Branches are merged into `master` (this repo's main branch is `master`, not `main`) with commit messages typically just `done` or e.g. `12月の更新分1回目`.
- Apply the swap by overwriting the root-level file with the one from the `docs/.../` folder. Don't hand-edit content unless the user asks — the designer's file is the source of truth.
- After applying, the `docs/.../` folder is normally left in place as a record of the delivery.

## Recurring page family: `gentei*.html`

`gentei{YYMM}.html` (or `gentei{YYMMYY}.html` for two-month spans, e.g. `gentei2603.html` = 2026年3・4月, `gentei2605.html` = 2026年5・6月) is the seasonal banquet-plan page. A new one is added every couple of months. Each update typically:

1. Adds the new `genteiYYMM.html`.
2. Updates `index.html` so the announcement block links to the new file. Search `index.html` for the previous `gentei*.html` href and the season copy (e.g. `2026年 3・4月特別宴会プラン!!`) to find the spot to update.
3. Often updates `enkai.html` to match.

When the user says something like "apply the May update," expect this exact pattern.

## Repository layout cheatsheet

- Root: every user-visible `.html` page. `index.html` is the top page; section pages are `tenpo.html` (店舗), `enkai.html` / `enkai_sp.html` (宴会), `drink.html`, `tsumami.html`, `taiyaki.html`, `ekimae.html` / `honten.html` (店舗マップ), `part-time-job.html`, `coupon.1.html`, plus the `gentei*.html` family.
- `top_image/`, `enkai_image/`, `drink_image/`, `tsuma_image/`, `tai_image/`, `tenpo_image/`, `hdft_image/` — page-scoped GIF/JPG assets. HTML refers to them by relative path (e.g. `top_image/main_image.gif`).
- `20240308/` — an archived earlier version of the site (older `index.html`, `enkai.html`, `gentei2403.html`). Do not edit unless asked.
- `docs/` — staging area for incoming update deliveries (see workflow above).

## Things to leave alone unless explicitly asked

- The GoLive `<csscriptdict>` / `<csactiondict>` JavaScript blocks at the top of every page (identical across pages — it's a runtime, not page logic).
- The `RESOURCE.FRK/`, `FINDER.DAT`, and empty `tatsumi/` artifacts.
- The blog link `http://blog.livedoor.jp/tatsumi_smtk/` and the `meta` keywords/description — these are stable.
- Old archived `gentei*.html` files. New seasons are added; old seasons are not deleted.

## Common commands

There are no project commands. To preview a page locally, open the `.html` file directly in a browser, or serve the repo root with any static file server (e.g. `python3 -m http.server` from the repo root) and visit `http://localhost:8000/`.

## Git運用・デプロイ

### ⚠️ 厳守事項: ブランチフロー（小規模サイト向け3段）

**絶対に守るべきフロー:**
```
feature/* （または YYYYMMDD_案件名） → master → deployment/production
```

**❌ やってはいけないこと:**
- `deployment/production` への直接コミット（**最重要**）
- `master` を経由しない `feature/*` → `deployment/production` 直マージ
- `deployment/production` が `master` より先行している状態の放置

**📋 詳細なガイド:**
- **緊急修正手順:** `.claude/emergency-hotfix-procedure.md`
- **デプロイ前チェックリスト:** `.claude/deploy-checklist.md`
- **視覚的フロー図:** `.claude/visual-deploy-flow.md`

`validate-branch-flow.yml` が `master → deployment/production` 以外の流入と先行コミットを自動検知してデプロイを停止する。

### デプロイ

- **方式:** GitHub Actions + FTP（`SamKirkland/FTP-Deploy-Action@v4.3.5`）
- **トリガー:** `deployment/production` ブランチへの push
- **対象:** リポジトリ全体（`.github/`, `.claude/`, `docs/`, `20240308/`, `CLAUDE.md` 等は除外）
- **コマンド:** `/deploy`（master → deployment/production マージ＆push）、`/merge-flow`（topic → master）
- **GitHub Secrets:** `FTP_SERVER`, `FTP_USERNAME`, `FTP_PASSWORD`, `FTP_SERVER_DIR`

## 指示書・進捗報告ルール

### 指示書
秘書Claudeが `.claude/instructions/` に指示書を配置する。ファイル名は `INS-xxx_{内容}.md` 形式。
ユーザーから「指示書やっといて」と言われたら、このフォルダの未完了の指示書を読んで実行すること。

### 完了報告
指示書の作業が完了（または中断）したら、以下にレポートを書くこと：

**レポート先:** `/Users/tn/フォルテッシモ Dropbox/niino takanori/git/.secretary/reports/`
**ファイル名:** `INS-xxx_{プロジェクト名}_{結果}.md`

**レポート形式:**
```
# 完了報告: INS-xxx {タスク名}
- **プロジェクト:** {プロジェクト名}
- **指示書:** INS-xxx
- **ステータス:** 完了 / 一部完了 / ブロック中

## 実施内容
- {やったこと}

## 未完了・要確認事項
- {残っていること、ユーザー確認が必要なこと}
```

