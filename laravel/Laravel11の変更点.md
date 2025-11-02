# Laravel11の変更点

### マイグレーションファイル
-  ① 関連するテーブルを1ファイルにまとめる方式に変更
```bash
# cacheテーブル
cache + cache_locks → キャッシュ機能
# usersテーブル
users + password_reset_tokens + sessions → 認証機能
```

- ② デフォルトのマイグレーションファイルのみが 0001_01_01_000000_ という特殊な形式に変更
```bash
# 通常 (2025_10_28 → 作成日時、 130517 → 時刻（時分秒: 13時05分17秒）)
2025_10_28_130517_create_admin_table

# Laravel11以降 (デフォルトのマイグレーションファイル)
0001_01_01_000000_create_users_table.php
0001_01_01_000001_create_cache_table.php
```

---

### アプリの構造
**→ アプリ構造がよりシンプルになった**

- 削除されたフォルダ
  - app/Console
  - app/Exceptions
  - app/Http/Middleware

<br>

- 統合された場所
  - ルート、ミドルウェア、例外は現在`bootstrap/app.php`ファイルに登録される

---

### 設定ファイルの削減
**→ config/フォルダから複数の設定ファイルが削除された**

- 削除されたもの
  - broadcasting.php
  - cors.php
  - hashing.php
  - sanctum.php

<br>

→ 必要なら `php artisan config:publish` で復元できる <br>

---

### ルートファイルの簡素化

- デフォルトのルートファイル
  - console.php
  - web.php

- なくなったもの
  - routes/api.php
  - Sanctum もデフォルトでは入らない (Sanctum: 認証パッケージ)

<br>

- APIルート: `php artisan install:api`
- リアルタイム通信（Broadcasting） を使いたいとき: `php artisan install:broadcasting`

---

### ミドルウェアの統合
**→ 以前は9つのミドルウェアアプリケーションが含まれていましたが、フレームワーク内部に統合さた**
- カスタマイズは `bootstrap/app.php` でできる

---

### Model Castsの書き方の変更
**→ Model Castsがプロパティではなくメソッドとして定義されるようになった**

```php
# 以前
protected $casts = ['options' => 'array'];
```

```php
# Laravel11から
protected function casts(): array {
    return ['options' => 'array'];
}
```

---

### Controllerの簡素化
ベースコントローラークラスがダイエットしました。AuthorizesRequestsとValidatesRequestsトレイトは存在しますが、オプトインする必要があります Benjamin Crozat。

---

### 新しいArtisanコマンド
**→ 新しく `make:` コマンドで以下が作れるようになった**
  - make:enum
  - make:interface
  - make:class

---

### ネイティブヘルスチェック
**アプリの健康状態を確認する専用ルート /up が標準で用意された**

- /up ルートにアクセスすると
  - データベース接続の可否
  - キャッシュの利用可能性
  - キューの準備状態

- 確認方法
  - -k は 自己署名証明書でも接続を許可するオプション
  - -i は ヘッダー付きで表示するオプション

```bash
% curl -k -i https://localhost/up # ドメインを設定
```

- 設定されている場所
  - `bootstrap/app.php` の `withRouting()` に、`health: '/up'` が含まれてる
  - 独自のヘルスチェックロジックを追加することも可能

---
### once()ヘルパー関数
**同じ値を何度呼び出しても必ず取得できるonceヘルパーメソッドが導入**

```bash
$value = once(fn () => time());
# 一度だけ実行して、同じ値を再利用できる
```

---

###  データベースのデフォルト変更
**Laravelのデフォルトデータベースが SQLiteに変更**

---

### 暗号化キーのローテーションが簡単に
**.env に APP_PREVIOUS_KEYS を追加すれば、古いキーで暗号化されたデータも壊さずに使える**

---

### Laravel Reverbの導入
**公式の WebSocket サーバーの導入 → スケーラブルなリアルタイム通信が、Laravelだけで完結**

- インストールコマンド
```bash
php artisan install:broadcasting
```

- サーバー起動コマンド
```bash
php artisan reverb:start
```

---

### Dumpableトレイト
**dd() や dump() を使いやすくするために、共通の Dumpable トレイトが追加**

---

### PHP 8.2 が必須に
**Laravel 11 からは PHP 8.2以上が必要 → PHP 8.1 はサポート終了**

---

### 秒単位のレート制限
**これまでは分単位だけでしたが、1秒あたりのリクエスト数も制御できるようになった**

---

### バッチ処理
**app/Kernel.phpがなくなり、新しいScheduleファザードを使用することでタスクのスケジュールアプリケーションのroutes/console.phpに定義できるようになった**

```bash
use Illuminate\Support\Facades\Schedule;

Schedule::command('emails:send')->daily();
```


---

### PHPアトリビュートでルート定義
**コントローラーのメソッドに直接ルート情報を書けるようになった**

```bash
use Illuminate\Routing\Attributes\Route;

#[Route('/users', methods: ['GET'])]
public function index() {
    // ユーザー一覧を表示
}
```
→ ルートファイルに書かなくても自動的にルーティングされる

- 設定
  - bootstrap/app.php にある withRouting() の中で、 `attributes: true` を指定する必要がある

```bash
return Application::configure(basePath: dirname(__DIR__))
    ->withRouting(
        web: __DIR__.'/../routes/web.php',
        attributes: true,
    );
```

---

### Artisanコマンドの改善
**コマンドのメッセージが見やすくなって、オプションも充実**

- ヘルプ画面が見やすくなった
```bash
php artisan help migrate
```

- プロンプト入力が柔軟になった
- オプションの説明が明確になった

---


## 参考
- [Laravel 11の新機能と変更点](https://kinsta.com/jp/blog/laravel-11/)
- [Laravel 11完全ガイド：13の革新的な新機能と互換性まで徹底解説](https://dexall.co.jp/articles/?p=2485)
- [Laravel11の新機能・変更点をまとめてみたよ](https://zenn.dev/ryo080/articles/9bb81dd53457ca)
- [Laravel 11🐲🐉がリリースされたのだ🎉【Laravel 11 新機能・変更点】](https://qiita.com/7mpy/items/4f4f7608c5fe44226d3c)