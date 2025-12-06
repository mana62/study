# Laravel Fortify とは
- **認証機能のバックエンド実装のみ** を提供してくれるパッケージ
- Fortify はバックエンドのみなので、フロントエンドは自分で用意する必要がある

## Fortify と Breeze の違い

| | Laravel Fortify | Laravel Breeze |
|---|---|---|
| 提供範囲 | バックエンドのみ（ルート + コントローラー） | フルスタック（ビュー + CSS も含む） |
| UI | 自分で作る | 最初から用意されている |

## Fortify が提供する機能
- 認証関連の主要な機能がほぼ全部入ってる

*   **ログイン**
*   **ユーザー登録**
*   **パスワードリセット**
*   **メール認証**
*   **パスワード確認**
*   **二段階認証**

## インストール方法

### ① パッケージをインストール

```bash
composer require laravel/fortify
```

### ② Fortifyのリソースを公開

```bash
# Laravel 8〜11
php artisan vendor:publish --provider="Laravel\Fortify\FortifyServiceProvider"

# Laravel 12
php artisan fortify:install
```

→ これで以下のものが作成される
* `config/fortify.php`（機能のON/OFF設定）
* `app/Providers/FortifyServiceProvider.php`（ビューや認証処理のカスタマイズ）
* `app/Actions` ディレクトリ（登録やパスワード更新処理のカスタマイズ）

### ③ サービスプロバイダを有効化
- `config/app.php` の `providers` に以下を追加（※ Laravel 8 以降は自動登録される場合もあある）

```bash
App\Providers\FortifyServiceProvider::class,
```

### ④ マイグレーション実行
- ユーザー関連のテーブルが必要なので、まだ作成していない場合のみ実行

```bash
php artisan migrate
```

## Fortify の設定
### 有効化（`config/fortify.php`）
- 必要なものだけコメントを外して使う

```php
'features' => [
    Features::registration(),      // ユーザー登録
    Features::resetPasswords(),    // パスワードリセット
    Features::emailVerification(), // メール認証
    // Features::updateProfileInformation(), // プロフィール更新
    // Features::updatePasswords(),           // パスワード変更
    // Features::twoFactorAuthentication([    // 二段階認証
    //     'confirm' => true,
    //     'confirmPassword' => true,
    // ]),
],
```

### ビューの紐付け（`FortifyServiceProvider`）
```php
Fortify::loginView(fn() => view('auth.login'));
Fortify::registerView(fn() => view('auth.register'));
Fortify::requestPasswordResetLinkView(fn() => view('auth.forgot-password'));
Fortify::resetPasswordView(fn($r) => view('auth.reset-password', ['request' => $r]));
```

## まとめ

*   **Fortify** は認証のバックエンド実装だけを提供（ビューは自分で用意）
*   **SPA（React/Vue）** と相性が良い
*   機能のON/OFFは `config/fortify.php` で簡単に切り替え可能

## 参考
- [Laravel公式](https://laravel.com/docs/12.x/fortify)
- [Laravel 10.x Laravel Fortify](https://readouble.com/laravel/10.x/ja/fortify.html)
- [[Laravel / Fortify] 隠蔽された認証処理を可視化、カスタマイズしよう！](https://qiita.com/JonyTask/items/ce6c7d4ffef32980f994)
- [認証系ライブラリとは？Laravel Fortify（ララベル フォーティファイ）を利用した実装の流れを解説](https://www.gpol.co.jp/blog/289/)