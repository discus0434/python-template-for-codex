---
name: spec-implementation-reviewer
description: 技術仕様書実装後・docs更新前に必ず使うレビュースキル。技術仕様書と現コード/テストの一致を別エージェントに確認させ、乖離を洗い出して修正を実施する。
license: Complete terms in LICENSE.txt
---

# Spec Implementation Reviewer

## 目的
- 技術仕様書と現コード/テストの**一致**を確認し、乖離を洗い出して修正を実施する。
- レビューエージェントに洗い出しのみ行わせ、あなたが修正を実施する。

## トリガ
- 実装が完了し、**docs更新に入る直前**
- 「技術仕様書通りに実装できているか」確認したいとき

## 役割分担

### あなた（メインエージェント）
- `make spec-implementation-review` を実行してレビュー結果を取得する。
- 出力のGAP/FIXをもとに修正する。
- 重要な（Critical/Major）乖離がゼロになるまで **レビュー→修正** を繰り返す。
- 仕様変更の合意が必要なズレはユーザーに確認する。

## 運用フロー（ループ）
1. `make spec-implementation-review PROMPT="<プロンプト>"` で乖離を洗い出す。
2. 出力のGAP/FIXをもとに修正する。
3. 再度 `make spec-implementation-review PROMPT="<プロンプト>"` を実行する。重要な（Critical/Major）乖離（GAP）がゼロになるまで繰り返す。

## 入力
- 対象技術仕様書: `specs/<spec-name>.md`

## レビューの呼び出しコマンド
- レビューはPROMPTを渡して別エージェントで実行する（出力は stdout。`/tmp/spec-implementation-review.txt` にも同じものが保存される）。
  - `make spec-implementation-review PROMPT="<プロンプト>"`

## プロンプトテンプレ
```markdown
You are a read-only spec-implementation-review agent. Do not modify files.
Follow the spec-implementation-review agent procedure in this skill:
1) Extract spec items from ”確定事項/設計/タスク/テスト”.
2) Verify implementation: planned files exist, modules/classes/functions match responsibilities, data models/validation match, tests cover normal/abnormal/boundary cases.
3) List all gaps and mismatches with severity classification: Critical/Major/Minor/Info.
4) Output the Self-Review Summary with OK/GAP/FIX. For GAP/FIX, include severity: Critical/Major/Minor/Info, and cite concrete file paths and spec sections when possible.
Then exit.
```

## 注意
- 技術仕様書自体が誤っていると判明した場合や、技術仕様書と実装が著しく乖離し修正不可能だと判断した場合は、修正前にユーザーへ確認し合意を取る。
- レビューはかなりの時間がかかるので、shell ツールの `timeout_ms` では 18000000 程度のかなり長時間のタイムアウトを設定して実行する必要がある。
