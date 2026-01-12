# プランタイトル（1開発単位）

## 概要
- 目的:
- 背景:
- 成果物:

## 要件合意（確定事項のみ）
- 目的/成果物:
- MVP:
- ノンゴール:
- 受け入れ条件:
- 制約/リスク:

## 設計（粒度を具体化）

### 変更対象ファイル
- 追加: `path` - 1行説明
- 更新: `path` - 1行説明
- 削除: `path` - 1行説明

### モジュール/クラス/関数
- モジュール:
  - クラス/関数:
  - 責務:
  - 入力/出力:
  - 依存:

### データモデル（Pydantic/Zod/TypeScript型）
- モデル名:
  - フィールド:
  - バリデーション:

### API（追加/変更がある場合）
- エンドポイント:
- メソッド:
- 認証/権限:
- 入力: params/body（型/必須/バリデーション）
- 出力: status/レスポンス（型/スキーマ）
- エラー: ステータス/条件/メッセージ

### DBスキーマ（追加/変更がある場合）
- テーブル/コレクション:
- 変更内容: 追加/変更/削除
- カラム: name/type/null/default
- 制約/インデックス:
- マイグレーション/移行手順:

### エラー/例外
- 例外名: 発生条件 / メッセージ

## タスク（実装レベル）
- [ ] 実装方針を決定（設計/分割/影響範囲）
- [ ] テストを先に作成（正常系/異常系/境界を具体列挙）
- [ ] 実装（対象ファイル/クラス/関数を明記）
- [ ] リファクタ（重複・複雑さの整理）
- [ ] ドキュメント更新（docs/REQUIREMENTS.md, docs/DESIGN.md など）

## テストケース（具体）
- 正常:
- 異常:
- 境界:

## 検証
- 実行コマンド: `make post-change`
- 追加の手動確認:

## 完了条件
- すべてのタスクが完了し、プランの内容が `docs/` に反映されている。

---

## 記入例（コピー後に削除すること）

### 概要
- 目的: 管理画面の一覧をCSVでエクスポートできるようにする
- 背景: 既存は画面表示のみで、外部共有の手段がない
- 成果物: CSVエクスポート機能（フィルタ適用後の一覧）

### 要件合意
- 目的/成果物: フィルタ適用済み一覧のCSVダウンロード
- MVP: フィルタ適用後の一覧のみ
- ノンゴール: PDF/Excel、スケジュール出力
- 受け入れ条件: 10件/1000件でCSVが取得できる
- 制約/リスク: 既存一覧APIの変更は不可

### 設計
- 変更対象ファイル
  - 追加: `src/app/exporter.py` - 一覧データをCSVに変換するロジック本体
  - 更新: `tests/test_exporter.py` - CSVエクスポートの正常/異常/境界テスト
- モジュール/クラス/関数
  - `CsvExporter.export(request: ExportRequest) -> bytes`
  - 責務: 一覧データをCSVに変換
  - 入力/出力: `ExportRequest` / CSV bytes
- データモデル
  - `ExportRequest(filters: dict[str, str], limit: int)`
- API
  - なし
- DBスキーマ
  - なし
- エラー/例外
  - `ValueError("invalid filter")`: 不正フィルタ指定時

### タスク
- [ ] `ExportRequest` と `CsvExporter` を追加
- [ ] `tests/test_exporter.py` に正常/異常/境界テスト追加
- [ ] docs更新（REQUIREMENTS/DESIGN/CHANGELOG）

### テストケース
- 正常: フィルタ適用後のCSV内容
- 異常: 不正フィルタで `ValueError`
- 境界: 0件/1000件

### 完了条件
- テストがすべて通る（`make post-change`）
- CSVエクスポートが要件どおり動作する
- docs更新が反映されている

---

## 記入例（API/DBスキーマが必要なケース）

### 概要
- 目的: ユーザーの通知設定をAPIで更新できるようにする
- 背景: 既存は管理画面からの手動変更のみ
- 成果物: 設定更新APIと保存先テーブル

### 要件合意
- 目的/成果物: 通知設定の取得/更新APIと永続化
- MVP: 更新APIのみ（取得は既存APIを利用）
- ノンゴール: バッチ更新、履歴管理
- 受け入れ条件: 正常更新で200が返り、DBに反映される
- 制約/リスク: 既存ユーザーAPIの後方互換を維持

### 設計
- 変更対象ファイル
  - 追加: `src/example/app/routes/notification_settings.py` - 通知設定のAPIルート
  - 追加: `src/example/app/usecases/notification_settings.py` - バリデーションとユースケース
  - 更新: `src/example/db/migrations/2026xxxx_add_notification_settings.sql` - テーブル追加マイグレーション
  - 更新: `tests/test_notification_settings.py` - APIの正常/異常/境界テスト
- モジュール/クラス/関数
  - `update_notification_settings(input: UpdateNotificationSettingsInput) -> NotificationSettings`
  - 責務: 入力検証とDB更新
  - 入力/出力: `UpdateNotificationSettingsInput` / `NotificationSettings`
- データモデル
  - `UpdateNotificationSettingsInput(user_id: str, email_enabled: bool, push_enabled: bool)`
- API
  - エンドポイント: `/api/v1/notification-settings`
  - メソッド: `PATCH`
  - 認証/権限: ログイン必須（本人のみ）
  - 入力: JSON body `{ emailEnabled: boolean, pushEnabled: boolean }`
  - 出力: `200` `{ userId: string, emailEnabled: boolean, pushEnabled: boolean, updatedAt: string }`
  - エラー: `400` バリデーションエラー / `401` 未認証 / `404` ユーザー不在
- DBスキーマ
  - テーブル/コレクション: `notification_settings`
  - 変更内容: 追加
  - カラム: `user_id uuid not null`, `email_enabled boolean not null default true`, `push_enabled boolean not null default true`, `updated_at timestamp not null`
  - 制約/インデックス: `user_id` PK, `user_id` FK -> `users.id`
  - マイグレーション/移行手順: 新規テーブル作成のみ（既存データ移行なし）
- エラー/例外
  - `ValidationError("invalid payload")`: ボディ不正時
  - `NotFoundError("user not found")`: 対象ユーザーが存在しない

### タスク
- [ ] `UpdateNotificationSettingsInput` と `update_notification_settings` を追加
- [ ] ルート/コントローラ追加と認証適用
- [ ] マイグレーション追加
- [ ] `test_notification_settings.py` に正常/異常/境界テスト追加
- [ ] docs更新（REQUIREMENTS/DESIGN/CHANGELOG）

### テストケース
- 正常: 設定更新で200、DBの値が更新される
- 異常: 不正な型で `400`、未認証で `401`
- 境界: 同一値で更新しても更新時刻が変わる

### 完了条件
- テストがすべて通る（`make post-change`）
- API/DBスキーマが要件どおり動作する
- docs更新が反映されている
