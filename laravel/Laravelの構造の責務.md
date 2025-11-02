# Laravelの構造 ファイルの責務

| 層 / クラス                           | 役割                 | 詳細・使いどころ                               | 具体例                                                                                            |
| --------------------------------- | ------------------ | -------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **モデル (app/Models)**              | データとルールを管理         | DBの値の整形・検索条件・リレーション・属性管理               | アクセサ／ミューテタ（パスワードハッシュ化、名前整形）、クエリスコープ (`User::active()->get()`)、リレーション（`posts()`）、属性のキャストやデフォルト値 |
| **サービスクラス (app/Services)**        | ビジネスロジックをまとめる      | コントローラをスッキリさせ、DB操作＋計算＋他処理をまとめる         | `UserService@register` でユーザー登録＋メール送信＋ログ記録                                                      |
| **Providers (app/Providers)**     | アプリ全体の設定や権限管理      | サービスや権限の登録、外部パッケージの初期化                 | Gate / Policy（`Gate::allows('update-post', $post)`）、サービスプロバイダでバインディング                          |
| **コントローラ (app/Http/Controllers)** | HTTPリクエストの入口とレスポンス | リクエスト受け取り、バリデーション、モデル／サービス呼び出し、レスポンス返却 | `UserController@show`、`PostController@index`                                                   |
| **Middleware**                    | リクエストの前処理          | 認証、ログ記録、CORS制御など                       | `auth`, `log`, `checkRole`                                                                     |
| **Requestクラス**                    | 入力バリデーション & 認可     | リクエストデータの安全性を保証                        | `StoreUserRequest`、`UpdatePostRequest`                                                         |
| **Resource / ResourceCollection** | APIレスポンス整形         | JSONやAPI出力を統一、不要項目を除外                  | `UserResource`, `PostCollection`                                                               |
| **Job / Queue**                   | 非同期処理              | 時間がかかる処理をバックグラウンドで実行                   | メール送信、画像加工、外部API呼び出し                                                                           |
| **Event / Listener**              | イベント駆動処理           | モデルやアプリのアクションに反応して自動処理                 | `UserRegistered` → `SendWelcomeEmail`、`UpdateStats`                                            |
| **Console / Command**             | CLI用処理・バッチ         | 定期実行や手動コマンドの処理                         | `php artisan send:reminders`                                                                   |
| **Policy / Gate**                 | 認可（権限管理）           | 誰がどの操作をできるか定義                          | `PostPolicy@update` で投稿編集権限をチェック                                                               |
| **Observer**                      | モデルイベント監視          | モデルの保存・更新・削除に反応して処理                    | `UserObserver@created` で登録時にメール送信                                                              |
| **Trait**                         | 再利用可能な共通機能         | 複数クラスで共通処理をまとめる                        | `HasRoles` トレイトで権限機能を追加                                                                        |
| **Helper / Service Class**        | 汎用処理・ビジネスロジック      | アプリ内で使い回す処理をまとめる                       | `UserService`、`PaymentHelper`                                                                  |


---

## 全体像

```bash
HTTPリクエスト
      ↓
  Middleware (認証・ログ)
      ↓
  Controller (入口/出口)
      ↓
  Serviceクラス (ビジネスロジック)
      ↓
  Model (DB操作・値整形)
      ↓
      DB
      ↑
  Event / Listener (処理反応)
  Job / Queue (非同期処理)
  Observer (モデル監視)
  Resource / ResourceCollection (レスポンス整形)
```


---

- モデルはデータルール
- サービスはビジネスロジック
- コントローラは入口出口
- 他の層（Middleware, Request, Job, Event, Resource）は「補助的に処理を分ける」

- Middleware：リクエストが通る前にチェック
- Requestクラス：入力データを安全に
- Resource：返すデータを整形
- Job/Queue：重たい処理は非同期
- Event/Listener：イベント発生時に処理を自動実行
- Policy/Gate：誰が何をできるか管理
- Observer：モデルの変更に反応
- Trait：複数クラスで共通処理を使う
- Service Class：ビジネスロジックまとめ役
- Console/Command：定期処理やCLI用