# Laravel Seeder Generator
➡️ 既存データベースの中身をそのままSeederファイルに変換できるツール

---

## できること
- **既存DB → Seeder自動生成**
  - 既に入っているデータをそのままSeederにして再利用できる
- **複数テーブル対応**
  - 任意のテーブルだけ、または複数テーブルまとめて変換できる
- **再現性のあるデータ投入**
  - 本番DBをコピーしてローカルやステージングで再現できる
- **Laravel標準Seeder形式で出力**
  - 生成されたSeederは `php artisan db:seed` でそのまま使える

---

## 手順

#### ① composerでインストール
```bash
composer require --dev orangehill/iseed
```
→ --dev：開発環境だけでOK

#### ② データベース接続を確認
- .env の設定で、正しいDB（例：mysql）を指定

```env
DB_CONNECTION=mysql
DB_DATABASE=myapp
DB_USERNAME=root
DB_PASSWORD=
```

#### ③ Seederを生成
```bash
php artisan iseed users
```
→ database/seeders/UsersTableSeeder.php が自動生成される

#### ④ 複数テーブルをまとめて生成する場合
```bash
php artisan iseed users,posts,comments
```

#### ⑤ 生成されたSeederを実行
```bash
php artisan db:seed --class=UsersTableSeeder
```
→ DBにデータが再投入される

**生成されるSeederのイメージ**
```bash
DB::table('users')->insert([
    [
        'id' => 1,
        'name' => 'Mana',
        'email' => 'mana@example.com',
        'created_at' => '2025-10-15 10:00:00',
        'updated_at' => '2025-10-15 10:00:00',
    ],
]);
```
→ 「既存データをそのまま再現」できる

---

## 実行タイミング
- 本番DBをコピーしたいとき
- 開発環境に初期データを入れたいとき
- テーブル構造を保ったまま別環境で再現したいとき
- チームメンバーに同じデータを共有したいとき

---

#### 注意
- データが多いテーブルは時間がかかる（数千件以上は注意）
- `Auto Increment`は再現されない場合がある（idが重複するならtruncateしてからseed）
- 本番DBの個人情報はそのまま入らないよう注意（開発用に加工して使う）
- マイグレーション後にテーブル構造が変わった場合は再生成が必要

---

## 応用テクニック
- **複数テーブルを一気にSeeder化**
```bash
php artisan iseed users,posts,comments --force
```
→ 既存Seederがあっても上書きできる

- **出力先を指定**
- `config/iseed.php` で出力パスを変更可能
- （例：database/seeders/backup/ に保存など）

- **Faker（Factory）との併用**
- iSeed → 本番や初期データを固定で再現
- Factory → テスト用のランダムデータを追加生成

---

## 参考
- [Git](https://github.com/orangehill/iseed)
- [Laravel シーダージェネレーター](https://laravel-news.com/laravel-seeder-generator)
- [【実務で使えるLaravel】seederでデータベースにダミーデータを投入しよう](https://qiita.com/kamome_susume/items/7565577e82edc64f017d)

