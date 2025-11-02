# Laravel Debugbar
- ブラウザ上での軽量デバッグバー
- 開発中のアプリの内部情報をブラウザ上にリアルタイム表示してくれる

---

## リアルタイムで確認できる情報
- ルート情報：どのコントローラ・メソッドが呼ばれたか
- SQLクエリ：実行されたクエリと時間
- HTTPリクエスト情報：GET/POST パラメータやヘッダー
- ビュー情報：Bladeに渡された変数
- ログ：Log::info()などで記録した内容

---

## インストール
```bash
composer require --dev barryvdh/laravel-debugbar
```
※ .env の `APP_DEBUG=true`にしておく (本番はfalse)

---

## 設定ファイルの公開 (デフォルトの設定を変えたい時のみ・デフォルトのままで十分な場合は不要)
```bash
php artisan vendor:publish --provider="Barryvdh\Debugbar\ServiceProvider"
```
→ `config/debugbar.php` が生成される <br>
→ 細かい設定を変更できる

---

## カスタムメッセージについて
- 独自のログやメッセージを表示する機能
- コントローラやサービスなど、任意の場所で使える
- 表示は Debugbar の Messages タブに出る

```php
\Debugbar::error('エラー');
\Debugbar::warning('警告');
\Debugbar::info($user);
\Debugbar::addMessage('任意メッセージ', 'custom');
```

---

## 補足
- キャッシュをクリアするとDebugbarもリフレッシュされる

```bash
php artisan config:clear
php artisan cache:clear
```

---

## 参考
- [Laravel Debugbarについて](https://qiita.com/goto_smv/items/b7be0985029ab3d03217)
- [【保存版】Laravel Debugbarで開発効率が3倍になる！完全導入・活用ガイド2024](https://dexall.co.jp/articles/?p=2545)
- [【Laravel入門】まだ使ってないの!?超便利なLaravelDebugbarを使おう](https://www.sejuku.net/blog/61948)
- [Laravel Debugbarを使ってみた](https://qiita.com/spm84/items/46bdbc04fc502e890952)

---

## 実行結果
### ①Laravel Debugbarをインストール

<details>
<summary>インストール結果</summary>

```bash
root@00f392c20d30:/var/www# composer require --dev barryvdh/laravel-debugbar
./composer.json has been updated
Running composer update barryvdh/laravel-debugbar
Loading composer repositories with package information
Updating dependencies
Lock file operations: 2 installs, 0 updates, 0 removals
  - Locking barryvdh/laravel-debugbar (v3.16.0)
  - Locking php-debugbar/php-debugbar (v2.2.4)
Writing lock file
Installing dependencies from lock file (including require-dev)
Package operations: 2 installs, 0 updates, 4 removals
  - Downloading php-debugbar/php-debugbar (v2.2.4)
  - Downloading barryvdh/laravel-debugbar (v3.16.0)
> Illuminate\Foundation\ComposerScripts::prePackageUninstall
  - Removing composer/pcre (3.3.2)
> Illuminate\Foundation\ComposerScripts::prePackageUninstall
  - Removing composer/class-map-generator (1.6.2)
> Illuminate\Foundation\ComposerScripts::prePackageUninstall
  - Removing barryvdh/reflection-docblock (v2.4.0)
> Illuminate\Foundation\ComposerScripts::prePackageUninstall
  - Removing barryvdh/laravel-ide-helper (v3.6.0)
  - Installing php-debugbar/php-debugbar (v2.2.4): Extracting archive
  - Installing barryvdh/laravel-debugbar (v3.16.0): Extracting archive
2 package suggestions were added by new dependencies, use `composer suggest` to see details.
Generating optimized autoload files
> Illuminate\Foundation\ComposerScripts::postAutoloadDump
> @php artisan package:discover --ansi

   INFO  Discovering packages.

  barryvdh/laravel-debugbar ................................................................................................................... DONE
  kitloong/laravel-migrations-generator ....................................................................................................... DONE
  laravel/pail ................................................................................................................................ DONE
  laravel/sail ................................................................................................................................ DONE
  laravel/tinker .............................................................................................................................. DONE
  nesbot/carbon ............................................................................................................................... DONE
  nunomaduro/collision ........................................................................................................................ DONE
  nunomaduro/termwind ......................................................................................................................... DONE

83 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
> @php artisan vendor:publish --tag=laravel-assets --ansi --force

   INFO  No publishable resources for tag [laravel-assets].

No security vulnerability advisories found.
Using version ^3.16 for barryvdh/laravel-debugbar
```
</details>