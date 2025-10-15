# Laravel Model Generator
➡️ 既存データベースのテーブル構造から **Eloquentモデルを自動生成** できるツール

---

## できること
- **既存DB → モデル自動生成**
  - テーブル名やカラム構造をもとに、Eloquentモデルクラスを自動で作成
- **リレーションも自動検出**
  - 外部キーから `hasMany` / `belongsTo` などの関係を自動定義
- **fillable, hidden, table名なども自動設定**
  - モデルに必要な基本設定をすべて生成
- **マイグレーション不要でもOK**
  - DBから直接モデルを生成できる

---

## 手順

#### ① composerでインストール
```bash
composer require --dev krlove/eloquent-model-generator
```
→ --dev：開発環境専用でOK

#### ② 設定ファイルを公開（任意）
```bash
php artisan vendor:publish --provider="Krlove\EloquentModelGenerator\Provider\GeneratorServiceProvider"
```
→ `config/eloquent_model_generator.php` が生成される
  - ここで「出力先フォルダ」などを設定できる（デフォルトは app/Models/）

#### ③ モデルを自動生成
```bash
php artisan krlove:generate:model User --table-name=users
```
→ app/Models/User.php が自動で生成される

#### 生成されたコードイメージ
```bash
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $table = 'users';
    protected $fillable = ['name', 'email', 'password', 'created_at', 'updated_at'];

    public function posts()
    {
        return $this->hasMany(Post::class);
    }
}
```

---

## オプション例
| オプション                                | 内容                              |
| ------------------------------------ | ------------------------------- |
| `--table-name=users`                 | 対象のテーブルを指定                      |
| `--output-path=app/Models/Generated` | 出力先を変更                          |
| `--namespace="App\Models\Generated"` | 名前空間を変更                         |
| `--no-timestamps`                    | `created_at`, `updated_at` を無効化 |
| `--date-format="Y-m-d H:i:s"`        | 日付フォーマットを指定                     |


---

## 複数テーブルをまとめて生成したい場合
- シェルやスクリプトでまとめて実行すればOK
```bash
php artisan krlove:generate:model User --table-name=users
php artisan krlove:generate:model Post --table-name=posts
php artisan krlove:generate:model Comment --table-name=comments
```

---

## 実行タイミング
- 既存DBをLaravelプロジェクトに導入するとき
- MigrationがないけどModelだけ欲しいとき
- Migrations Generatorでマイグレーションを作った後に、Modelも自動で作りたいとき
- FactoryやSeederと連携してデータ操作したいとき

---

## 注意
- リレーションは外部キー制約（foreign key） が設定されていないと自動認識できない
- DBスキーマとLaravelモデルを一致させたい場合は、再生成が必要
- ファイルが上書きされる場合があるため、`--output-path` を分けるのが安全
- DBが正しく設定されていないと生成に失敗するので .env を要確認

---

## 応用テクニック
- Migration GeneratorやSeeder Generatorと組み合わせると最強
```bash
php artisan migrate:generate
# 既存DB → migration生成
php artisan krlove:generate:model User --table-name=users
# migration構造に対応するモデル生成
php artisan iseed users
# データをSeeder化
```
→ 自動生成モデルを分離して管理できる

---

## 参考
- [git](https://github.com/krlove/eloquent-model-generator)
- [laravel DBからモデルファイルを自動生成](https://zenn.dev/maibear3/articles/8236c78d8f8200)