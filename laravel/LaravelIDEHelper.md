# Laravel IDE Helper
→ Laravelのコード補完を強化するための開発支援ツール

---

## Laravel IDE Helper ができることと実行タイミング
#### ① ファサードやサービスの補完を強化
- **コマンド:** `php artisan ide-helper:generate`
- **できること:** app() や Auth::user() などのマジックメソッドを IDE が補完できるようになる
- **実行タイミング:** プロジェクト初期に一度実行するのが基本。新しいサービスやFacadeを追加した時も再実行

#### ② モデルのプロパティ補完
- **コマンド:** `php artisan ide-helper:models`（-N オプションでファイルを書き換えず生成可能）
- **できること:** Eloquentモデルのカラムやリレーションを IDE が補完できる
- **実行タイミング:** モデルを追加・修正した後、マイグレーションでテーブルを編集した後

#### ③ PHPStorm向けメタファイル生成
- **コマンド:** `php artisan ide-helper:meta`
- **できること:** Laravel が自動で管理している内部のオブジェクトやサービスも IDE が補完してくれる
- **実行タイミング:** 新しいサービスやカスタムバインディングを追加した時

#### ④ ファクトリの補完
- **コマンド:** `php artisan ide-helper:models` ※ 専用コマンドはないが ide-helper:models で対応
- Laravel 8以降のクラスベースファクトリに対応
- **できること:** UserFactory::new()->create() などで IDE が補完できる
- **実行タイミング:** ファクトリを追加・修正したとき

#### ⑤ CI/CDへの統合も可能
- CI では GitHub Actions のワークフロー内に実行コマンドを書いて自動実行
- **できること:** チーム全員が IDE 補完を共有できる
- **実行タイミング:** 自動で生成されるため、手動更新不要

---

## 手順
#### ①composerでインストール
```bash
composer require --dev barryvdh/laravel-ide-helper
```
→ `--dev`は開発環境の意味

#### ②補完ファイルを生成
```bash
# 全体のヘルパーファイル生成
php artisan ide-helper:generate

# モデルごとの PHPDoc を更新
php artisan ide-helper:models
php artisan ide-helper:models -N
# -N オプション: モデルファイルに直接DocBlockを書き込まないオプション

# Facade 補完
php artisan ide-helper:meta
```

### ③補完が効かない場合・生成時にエラーが出た場合はキャッシュをクリア
```bash
php artisan optimize:clear
```

---

## 注意
- 生成されるファイル（`_ide_helper.php`）は .gitignore に入れてローカルで生成する運用が一般的
- 補完ファイルは **自動で更新されない** → モデルやDB構造を変えたら再実行が必要
- 生成ファイルは **本番環境では不要**（--dev）

---

## 参考
- [【保存版】Laravel IDE Helperの完全ガイド：導入から応用まで5つの実践テクニック](https://dexall.co.jp/articles/?p=2614)
- [laravel-ide-helperを使ってみた](https://qiita.com/miriwo/items/5d1e82931ee182f709e2)

---

## 実行結果
### ①Laravel IDE Helperをインストール

<details>
<summary>インストール結果</summary>

```bash
root@00f392c20d30:/var/www# composer require --dev barryvdh/laravel-ide-helper
./composer.json has been updated
Running composer update barryvdh/laravel-ide-helper
Loading composer repositories with package information
Updating dependencies
Lock file operations: 4 installs, 0 updates, 0 removals
  - Locking barryvdh/laravel-ide-helper (v3.6.0)
  - Locking barryvdh/reflection-docblock (v2.4.0)
  - Locking composer/class-map-generator (1.6.2)
  - Locking composer/pcre (3.3.2)
Writing lock file
Installing dependencies from lock file (including require-dev)
Package operations: 4 installs, 0 updates, 0 removals
  - Downloading composer/pcre (3.3.2)
  - Downloading composer/class-map-generator (1.6.2)
  - Downloading barryvdh/reflection-docblock (v2.4.0)
  - Downloading barryvdh/laravel-ide-helper (v3.6.0)
  - Installing composer/pcre (3.3.2): Extracting archive
  - Installing composer/class-map-generator (1.6.2): Extracting archive
  - Installing barryvdh/reflection-docblock (v2.4.0): Extracting archive
  - Installing barryvdh/laravel-ide-helper (v3.6.0): Extracting archive
2 package suggestions were added by new dependencies, use `composer suggest` to see details.
Generating optimized autoload files
> Illuminate\Foundation\ComposerScripts::postAutoloadDump
> @php artisan package:discover --ansi

   INFO  Discovering packages.

  barryvdh/laravel-ide-helper ................................................................................................................. DONE
  kitloong/laravel-migrations-generator ....................................................................................................... DONE
  laravel/pail ................................................................................................................................ DONE
  laravel/sail ................................................................................................................................ DONE
  laravel/tinker .............................................................................................................................. DONE
  nesbot/carbon ............................................................................................................................... DONE
  nunomaduro/collision ........................................................................................................................ DONE
  nunomaduro/termwind ......................................................................................................................... DONE

85 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
> @php artisan vendor:publish --tag=laravel-assets --ansi --force

   INFO  No publishable resources for tag [laravel-assets].

No security vulnerability advisories found.
Using version ^3.6 for barryvdh/laravel-ide-helper
```
</details>
