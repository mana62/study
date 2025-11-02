# Laravel Migrations Generator
- 既存のDB構造からLaravelのマイグレーションファイルを自動生成するツール

---
## 前提
- `.sql`ファイルをMySQLにインポート済みであること
- `.env`のDB設定が正しいこと
- Laravelプロジェクトのルートで実行していること

---
## インストール
```bash
composer require --dev "kitloong/laravel-migrations-generator"
```
→ `--dev` は「開発用ツールとしてインストール」という意味

---
## 基本コマンド
### 全テーブルをマイグレーションに変換
```bash
php artisan migrate:generate
```
→ 全てのテーブルを、`/database/migrations`にマイグレーションファイルを自動生成

### 特定のテーブルだけを生成
```bash
php artisan migrate:generate users posts comments
```
→ 複数のテーブル名をスペースで区切って指定(上記の例：users posts comments)

### 別フォルダに出力したい場合
```bash
php artisan migrate:generate --path=database/migrations/generated
```
→ 出力先フォルダを変えらる

### 接続するデータベースを指定
```bash
php artisan migrate:generate --connection=mysql2
```
→ もし .envに複数の接続がある場合、接続を切り替えられる

---

## オプション

| オプション | 内容 |
|------------|------|
| `--tables=` | 特定のテーブルだけ生成 |
| `--ignore=` | 除外したいテーブルを指定 |
| `--path=` | 出力先フォルダを変更 |
| `--connection=` | 接続DBを切り替え |

---

## 補足
- 命名規則がLaravelと違っていても、基本的には生成できる
- 自動生成されたマイグレーションは、Laravelの命名規則と完全一致しないこともあるので生成後に確認が必要
- DBが古いと GENERATION_EXPRESSION エラーが出ることがある
  - → MySQL 5.7以上、MariaDB 10.2.5以上が推奨


---

## 参考
- [Laravel Migrations Generator の使い方（2025年版](https://qiita.com/yama-github/items/5d55432c8092e36e527a)
- [Laravelのkitloong/laravel-migrations-generator使ってphp artisan migrate:generateしたらColumn not found: 1054 Unknown column 'GENERATION_EXPRESSION' in 'field list' エラーでたので対応した](https://qiita.com/akagane99/items/414df6169a381179ec26)
- [Laravel Migrations Generator](https://laravel-news.com/package/kitloong-laravel-migrations-generator)
- [移行ジェネレーターパッケージを使用して既存のデータベースから移行を生成する](https://laravel-news.com/migration-generator-for-laravel?utm_source=chatgpt.com)

---

## 実行結果 (実行：2025/10/28)
### ①laravel-migrations-generatorをインストール

<details>
<summary>インストール結果</summary>

```bash
mana@fukumotomananoMacBook-Air src % composer require --dev kitloong/laravel-migrations-generator

./composer.json has been updated
Running composer update kitloong/laravel-migrations-generator
Loading composer repositories with package information
Updating dependencies
Lock file operations: 1 install, 0 updates, 0 removals
  - Locking kitloong/laravel-migrations-generator (v7.2.0)
Writing lock file
Installing dependencies from lock file (including require-dev)
Package operations: 1 install, 0 updates, 0 removals
  - Downloading kitloong/laravel-migrations-generator (v7.2.0)
  - Installing kitloong/laravel-migrations-generator (v7.2.0): Extracting archive
Generating optimized autoload files
> Illuminate\Foundation\ComposerScripts::postAutoloadDump
> @php artisan package:discover --ansi

   INFO  Discovering packages.  

  kitloong/laravel-migrations-generator ............... DONE
  laravel/pail ........................................ DONE
  laravel/sail ........................................ DONE
  laravel/tinker ...................................... DONE
  nesbot/carbon ....................................... DONE
  nunomaduro/collision ................................ DONE
  nunomaduro/termwind ................................. DONE

82 packages you are using are looking for funding.
Use the `composer fund` command to find out more!
> @php artisan vendor:publish --tag=laravel-assets --ansi --force

   INFO  No publishable resources for tag [laravel-assets].  

No security vulnerability advisories found.
Using version ^7.2 for kitloong/laravel-migrations-generator
```
</details>

### ②php artisan migrate:generateを実行

<details>
<summary>実行結果</summary>

```bash
mana@fukumotomananoMacBook-Air src % docker exec -it suq-app bash

root@b70a4f90d699:/var/www# php artisan migrate:generate
Using connection: mysql

Generating migrations for: admin,area,cache,cache_locks,company,failed_jobs,groups,grouptomember,iptable,job_batches,jobs,mail_template,maillog,member,memberdocument,memberhistory,membertoarea,membertomodel,membertomodeled,membertoroom,model,news,other_commission,panf,password_reset_tokens,prefecture,sales_commission,school,schoolrows,schooltomodel,sessions,users,website,websiterows

 Do you want to log these migrations in the migrations table? (yes/no) [yes]:
 > y

 Next Batch Number is: 2. We recommend using Batch Number 0 so that it becomes the "first" migration. [Default: 0] [0]:
 > 

Setting up Tables and Index migrations.
Created: /var/www/database/migrations/2025_10_28_130517_create_admin_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_area_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_cache_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_cache_locks_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_company_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_failed_jobs_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_groups_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_grouptomember_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_iptable_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_job_batches_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_jobs_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_mail_template_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_maillog_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_member_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_memberdocument_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_memberhistory_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_membertoarea_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_membertomodel_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_membertomodeled_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_membertoroom_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_model_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_news_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_other_commission_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_panf_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_password_reset_tokens_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_prefecture_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_sales_commission_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_school_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_schoolrows_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_schooltomodel_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_sessions_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_users_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_website_table.php
Created: /var/www/database/migrations/2025_10_28_130517_create_websiterows_table.php

Setting up Views migrations.

Setting up Stored Procedures migrations.

Setting up Foreign Key migrations.

Finished!
```
</details>

### php artisan migrate:generate時の質問
#### １個目の質問
```bash
Do you want to log these migrations in the migrations table? (yes/no)
```
→ 生成したマイグレーションファイルを Laravelの migrations テーブルに記録するかどうか
  - yes (y) → 記録する。Laravel側で「このマイグレーションはすでに存在する」と管理できるようになる
  - no (n) → 記録しない。Laravelのマイグレーション管理には反映されないが、ファイル自体は生成される


<br>

#### ２個目の質問
```bash
Next Batch Number is: 2. We recommend using Batch Number 0 so that it becomes the "first" migration. [Default: 0]
```
→ 生成するマイグレーションを どのバッチ番号で管理するか
  - migrations テーブルに記録されるとき、同じバッチ番号を持つマイグレーションは同時にまとめて実行されたものとして扱われる
  - Enter だけ押すとデフォルトの０を使える