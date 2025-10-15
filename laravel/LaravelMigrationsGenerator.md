# Laravel Migrations Generator
- **➡️ 既に存在するデータベースから、自動でマイグレーションファイルを作ってくれるツール**
  - すでに MySQL にテーブルがある
  - 他の人が作ったデータベースを使ってる
  - Laravelを後から導入したい

---

## 手順
#### ①composerでインストール
```bash
composer require --dev "kitloong/laravel-migrations-generator"
```
→ `--dev` は「開発用ツールとしてインストール」という意味

---

## 基本コマンド
#### 全テーブルをマイグレーションに変換
```bash
php artisan migrate:generate
```
→ すべてのテーブルをスキャンして、
/database/migrations にマイグレーションファイルを自動生成


#### 特定のテーブルだけを生成
```bash
php artisan migrate:generate users posts comments
```
→ 複数のテーブル名をスペースで区切って指定


#### 別フォルダに出力したい場合
```bash
php artisan migrate:generate --path=database/migrations/generated
```
→ 出力先フォルダを変えらる


#### 接続するデータベースを指定
```bash
php artisan migrate:generate --connection=mysql2
```
→ もし .env に複数の接続がある場合、接続を切り替えられる

---

## 参考
- [Laravel Migrations Generator の使い方（2025年版](https://qiita.com/yama-github/items/5d55432c8092e36e527a)
- [git](https://github.com/kitloong/laravel-migrations-generator)