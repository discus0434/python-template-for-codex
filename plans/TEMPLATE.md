# プランタイトル（1開発単位）

## 概要
- 本プランの目的・背景・成果物を要約する。
- 目的:
- 背景:
- 成果物:

## 要件合意（確定事項のみ）
- 仕様上の確定事項・MVP・受け入れ条件を列挙する。
- 目的/成果物:
- MVP:
- ノンゴール:
- 受け入れ条件:
- 制約/リスク:

## 設計（粒度を具体化）
- 実装粒度まで落とした設計要素を整理する。
### 対象パッケージ（モノレポ）
- 影響範囲となるディレクトリと言語を列挙する。
- 対象ディレクトリ:
- 言語（Python/TypeScript）:

### 変更対象ファイル
- 追加/更新/削除するファイルの一覧と目的を示す。
- 追加:
  - `path` - 1行説明
    - 特記事項（必要なら）:
      - 例: 互換性/移行/既存影響など
- 更新:
  - `path` - 1行説明
    - 特記事項（必要なら）:
      - 例: 既存挙動の変更点
- 削除:
  - `path` - 1行説明
    - 特記事項（必要なら）:
      - 例: 代替手段/移行先

### モジュール/クラス/関数
- 主要な責務単位（関数/クラス）の役割と依存を示す。
- モジュール: `path` - 1行説明
  - クラス/関数: `name` - 1行説明
    - 特記事項（必要なら）:
      - 例: 既存仕様との整合/副作用
  - 責務: 1行説明
  - 入力/出力: 1行説明
  - 依存: 1行説明

### データモデル（Pydantic/Zod/TypeScript型）
- 永続化するデータ構造とフィールドの意味を整理する。
- モデル名: `Model` - 1行説明
  - フィールド:
    - `name: type` - 1行説明
      - 特記事項（必要なら）:
        - 例: null許容/互換/保存形式
  - バリデーション: 1行説明
    - 特記事項（必要なら）:
      - 例: reasonsはLLMのみ参照

### API（追加/変更がある場合）
- 新規/変更APIの入出力と責務を定義する。
- エンドポイント: `/path` - 1行説明
  - メソッド: `GET/POST/...`
  - 認証/権限: 1行説明
  - 入力: params/body（型/必須/バリデーション）
  - 出力: status/レスポンス（型/スキーマ）
  - エラー: ステータス/条件/メッセージ
  - 特記事項（必要なら）:
    - 例: 後方互換/レート制限

### DBスキーマ（追加/変更がある場合）
- 追加/変更するテーブルの内容と移行方針を整理する。
- テーブル/コレクション: `name` - 1行説明
  - 変更内容: 追加/変更/削除
  - カラム: name/type/null/default
  - 制約/インデックス:
  - マイグレーション/移行手順:
  - 特記事項（必要なら）:
    - 例: backfill要否/段階移行

### エラー/例外
- 想定エラーとその扱い方針を列挙する。
- 例外名: 発生条件 / メッセージ - 1行説明
  - 特記事項（必要なら）:
    - 例: リトライ可否/ユーザー向け文言

## タスク（実装レベル）
- 実装〜テスト〜docs更新までの作業項目を示す。
- [ ] 実装方針を決定（設計/分割/影響範囲）
- [ ] テストを先に作成（正常系/異常系/境界を具体列挙）
- [ ] 実装（対象ファイル/クラス/関数を明記）
- [ ] リファクタ（重複・複雑さの整理）
- [ ] ドキュメント更新（docs/REQUIREMENTS.md, docs/DESIGN.md など）

## テストケース（具体）
- TDD要件を満たすための検証観点を列挙する。
- 正常:
- 異常:
- 境界:

## 検証
- 実行コマンドと手動確認項目を示す。
- 実行コマンド: `make post-change-py` / `make post-change-ts`（変更対象に合わせる）
- 追加の手動確認:

## 完了条件
- 完了判定の条件を明確化する。
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
  - 追加:
    - `src/app/exporter.py` - 一覧データをCSVに変換するロジック本体
      - 特記事項（必要なら）:
        - 例: 既存APIの順序と一致させる
  - 更新:
    - `tests/test_exporter.py` - CSVエクスポートの正常/異常/境界テスト
- モジュール/クラス/関数
  - モジュール: `src.app.exporter` - CSVエクスポート実装
    - クラス/関数: `CsvExporter.export(request: ExportRequest) -> bytes` - CSV生成
      - 特記事項（必要なら）:
        - 例: CSVヘッダ順は固定
    - 責務: 一覧データをCSVに変換
    - 入力/出力: `ExportRequest` / CSV bytes
- データモデル
  - `ExportRequest` - CSV出力のリクエスト
    - フィールド:
      - `filters: dict[str, str]` - CSV出力対象のフィルタ条件
      - `limit: int` - 出力件数の上限
        - 特記事項（必要なら）:
          - 例: limitは最大1000
- API
  - なし
- DBスキーマ
  - なし
- エラー/例外
  - `ValueError("invalid filter")`: 不正フィルタ指定時

### タスク
- [ ] `ExportRequest` と `CsvExporter` を追加
- [ ] `tests/test_exporter.py` or `*.test.ts` に正常/異常/境界テスト追加
- [ ] docs更新（REQUIREMENTS/DESIGN/CHANGELOG）

### テストケース
- 正常: フィルタ適用後のCSV内容
- 異常: 不正フィルタで `ValueError`
- 境界: 0件/1000件

### 完了条件
- テストがすべて通る（`make post-change-py` / `make post-change-ts`）
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
  - 追加:
    - `services/twitter-server/app/routes/notification_settings.py` - 通知設定のAPIルート
    - `services/twitter-server/app/usecases/notification_settings.py` - バリデーションとユースケース
  - 更新:
    - `services/twitter-server/db/migrations/2026xxxx_add_notification_settings.sql` - テーブル追加マイグレーション
    - `services/twitter-server/tests/test_notification_settings.py` - APIの正常/異常/境界テスト
- モジュール/クラス/関数
  - モジュール: `services.twitter_server.app.usecases.notification_settings` - 通知設定の更新処理
    - クラス/関数: `update_notification_settings(input: UpdateNotificationSettingsInput) -> NotificationSettings` - 更新実行
    - 責務: 入力検証とDB更新
    - 入力/出力: `UpdateNotificationSettingsInput` / `NotificationSettings`
- データモデル
  - `UpdateNotificationSettingsInput` - 通知設定更新の入力
    - フィールド:
      - `user_id: str` - 更新対象ユーザーID
      - `email_enabled: bool` - メール通知の有効/無効
      - `push_enabled: bool` - プッシュ通知の有効/無効
        - 特記事項（必要なら）:
          - 例: 未指定は現行値維持
- API
  - エンドポイント: `/api/v1/notification-settings` - 通知設定の更新
    - メソッド: `PATCH`
    - 認証/権限: ログイン必須（本人のみ）
    - 入力: JSON body `{ emailEnabled: boolean, pushEnabled: boolean }`
    - 出力: `200` `{ userId: string, emailEnabled: boolean, pushEnabled: boolean, updatedAt: string }`
    - エラー: `400` バリデーションエラー / `401` 未認証 / `404` ユーザー不在
    - 特記事項（必要なら）:
      - 例: 後方互換のため旧入力も許容
- DBスキーマ
  - テーブル/コレクション: `notification_settings` - 通知設定の保存先
    - 変更内容: 追加
    - カラム: `user_id uuid not null`, `email_enabled boolean not null default true`, `push_enabled boolean not null default true`, `updated_at timestamp not null`
    - 制約/インデックス: `user_id` PK, `user_id` FK -> `users.id`
    - マイグレーション/移行手順: 新規テーブル作成のみ（既存データ移行なし）
    - 特記事項（必要なら）:
      - 例: backfill不要
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
- テストがすべて通る（`make post-change-py`）
- API/DBスキーマが要件どおり動作する
- docs更新が反映されている
