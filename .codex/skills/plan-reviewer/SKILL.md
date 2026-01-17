---
name: spec-reviewer
description: プラン作成時に必ず使うレビュースキル。プランがそもそも実現可能か？過不足なくプランを実装したときに問題なく動作するか？を別エージェントに確認させ、不備や矛盾、無駄、不明瞭さを洗い出して修正を実施する。
license: Complete terms in LICENSE.txt
---

# Plan Reviewer

## 目的
- プランがそもそも実現可能か？過不足なくプランを実装したときに問題なく動作するか？を確認し、不備や矛盾、無駄、不明瞭さを洗い出して修正を実施する。
- レビューエージェントに洗い出しのみ行わせ、あなたが修正を実施する。

## トリガ
- プラン作成時

## 役割分担

### あなた（メインエージェント）
- `make spec-review` を実行してレビュー結果を取得する。
- 出力のDEFICIT/INCONSISTENCY/UNNECESSARY/UNCLEARをもとにプランを再度練り直し、必要に応じてユーザーに修正に関する質問を行い、合意を取る。
- 修正を実施する。
- 重要な（Critical/Major）不備や矛盾、無駄、不明瞭さがゼロになるまで **レビュー→修正** を繰り返す。

## 運用フロー（ループ）
1. `make spec-review PROMPT="<プロンプト>"` で不備や矛盾、無駄、不明瞭さを洗い出す。
2. 出力のDEFICIT/INCONSISTENCY/UNNECESSARY/UNCLEARをもとにプランを再度練り直し、必要に応じてユーザーに修正に関する質問を行い、合意を取る。
3. 修正を実施する。
4. 再度 `make spec-review PROMPT="<プロンプト>"` を実行する。重要な（Critical/Major）不備や矛盾、無駄、不明瞭さがゼロになるまで繰り返す。

## 入力
- 対象プラン: `specs/<spec-name>.md`

## レビューの呼び出しコマンド
- レビューはPROMPTを渡して別エージェントで実行する（出力は stdout。`/tmp/spec-review.txt` にも同じものが保存される）。
  - `make spec-review PROMPT="<プロンプト>"`

## プロンプトテンプレ
```markdown
You are a read-only spec-reviewer agent. Do not modify files.
Follow the spec-reviewer agent procedure in this skill:
1) Extract Plan items from ”概要/要件合意/設計”.
2) Verify spec: planned files exist, modules/classes/functions match responsibilities, data models/validation match.
3) Investigate existing code and tests to confirm if the plan is feasible, reasonable, and complete.
4) List all deficits, inconsistencies, unnecessary items, and unclear points in the spec.
5) Output the Self-Review Summary with OK/DEFICIT/INCONSISTENCY/UNNECESSARY/UNCLEAR. For DEFICIT/INCONSISTENCY/UNNECESSARY/UNCLEAR, include severity: Critical/Major/Minor/Info, and cite concrete file paths and plan sections when possible.
Then exit.
```

## 注意
- レビューはそれなりの時間がかかるので、30分など、取りうる最大時間のタイムアウトを設定して実行する。2分など、短すぎるタイムアウトを設定するとレビューが中断される可能性がある。
