# Laravel12の変更点


### 新しいスターターキット
**React・Vue・Livewire 用に、最新技術を詰め込んだスターターキットの導入**

- **React版** [Inertia + React 19 + TypeScript + Tailwind CSS + shadcn]
- **Vue版** [Inertia + Vue 3 + TypeScript + Tailwind CSS + shadcn-vue]
- **Livewire版** [Livewire 3 + TypeScript + Tailwind CSS + Flux UI]

<br>

※ **注意点** Laravel Breeze や Jetstream は非推奨ではないけど、新規プロジェクトでは使われなくなってきている

---

### WorkOS AuthKit の統合
**AuthKit を使うと、エンタープライズ向けの認証機能が簡単に使える**

- WorkOS AuthKit を使うとできること
  - GoogleやFacebookなどのソーシャルログイン
  - パスキー認証
  - Magic Auth
  - SSO（シングルサインオン）
<br>
→ セキュリティ強化したいプロジェクトに使用される

- 使い方
```bash
laravel new your-app --kit=react --workos
```
→ これで WorkOS AuthKit が組み込まれた構成になる

- 費用
  - WorkOS は 月間100万アクティブユーザーまで無料で使える

---


### クエリビルダーの最適化
**nestedWhere()などの新しいメソッドが追加され、複雑なクエリを簡素化**

```bash
DB::table('products')
  ->where('status', 'active')
  ->nestedWhere(function ($query) {
      $query->where('price', '<', 1000)
            ->orWhere('discount', '>', 30);
  })
  ->get();
```

- メリット
  - 条件のグループ化が直感的に書ける
  - where() のネストが読みやすくなる
  - 複雑な検索ロジックでもスッキリかける

---

### ハッシュアルゴリズムの変更
**MD5 → xxHash に変更 → 小さなデータのハッシュが最大30倍高速に**

- xxHashとは
  - 超高速なハッシュアルゴリズム
  - 小さなデータに対して、MD5より最大30倍速い
  - 衝突率も低くて、軽量＆安全性のバランスが良い

- どこで使われるか
  - キャッシュキーの生成
  - ファイル識別
  - 一時的なユニークID生成 など

---

### UUID v7のサポート
**HasUuids トレイトを使うと、時系列順に並ぶ UUID v7 が使える**

- モデルに HasUuids トレイトを使うことで、 自動的に UUID を生成してくれる

```bash
use Illuminate\Database\Eloquent\Concerns\HasVersion7Uuids;

class Order extends Model
{
    use HasVersion7Uuids;
}
```

---

### MariaDB のネイティブサポート
**MariaDB の CLI コマンドがLaravelに統合されて、パフォーマンスが向上**

- これまで
  - MySQLドライバーを使ってMariaDBに接続するのが一般的だった

- Laravel12から
  - MariaDB専用のドライバーと設定が追加された
    - .env で `DB_CONNECTION=mariadb` を使えるようになった
    - MariaDbGrammar という専用の構文クラスが導入された
    - CLIコマンド（mariadb）との連携が強化された
- 注意点
  - MariaDB 10.7未満では uuid 型が使えないため、マイグレーションでエラーになる可能性がある

---

### チャンククエリの改善
**チャンク処理がユーザー定義の制限を正確に守るようになった**

- チャンククエリ : メモリ節約＆安定動作させるchunk() や chunkById()
- Laravel 11まで
  - chunk() は オフセット方式でデータを分割
  - 処理中にデータが変更されると、次のチャンクで一部のデータがスキップされることがあった

- Laravel 12から
  - ユーザー定義の制限（where条件など）を正確に守るようになった
  - chunkById() のような プライマリキー基準の分割が推奨されるようになった
  - ID順で分割することで、処理順序が保証されて、漏れがなくなる

- メリット
  - ページネーションや分割処理が正確に制御できる
  - データの整合性が保たれる
  - 大量データでも安心して処理できる

---

### Symfony 7への更新
**Laravelの内部コンポーネントが Symfony 6 → Symfony 7 にアップグレードされた**

- Symfony : 内部で使われている PHP コンポーネント群のこと
  - リクエスト処理
  - ルーティング
  - HTTPレスポンス
  - バリデーション
  - イベント管理 など

- メリット
  - リクエスト処理の高速化
  - モリ使用量の削減
  - セキュリティと保守性の向上

---

###  破壊的変更（少数）

**① restore() のグローバルスコープ使用が削除**
- これまで
  - 論理削除されたモデルに対して restore() を使うと、グローバルスコープが適用されたまま復元できた
- Laravel 12から
  - restore() に グローバルスコープが適用されなくなった。つまり、SoftDeletes などのスコープが効かない状態で復元されることになる
- 影響
  - restore() を使う場面では、明示的にスコープを外す必要がある

**② route() ヘルパーは文字列ベースのみ対応**
- これまで
  - route() ヘルパーに、コントローラークラスやアクション名を渡してルートを生成できた

```php
route([UserController::class, 'index']);
# Laravel 12から → 文字列ベースのルート名のみ対応になった
```

```php
route('users.index'); //  OK⚪︎
route([UserController::class, 'index']); // NG×
```

- 影響
  - コントローラークラスベースのルート生成は使えなくなる

**③ モデル内の「配列ベースのリレーション定義」が非推奨**
- これまで
  - モデル内で、複数のリレーションを配列でまとめて定義することができた

```php
protected $relations = [
    'posts',
    'comments',
];
# Laravel 12から → 配列ベースのリレーション定義は非推奨に
```

- 影響
  - リレーションは メソッドで個別に定義するのが推奨されるようになった

```php
public function posts() {
    return $this->hasMany(Post::class);
}
```
---

### Lazyコレクションの改善
**フィルタや早期終了など、メモリ効率の良い処理が柔軟に**

- 大量データを扱うときに、メモリを節約しながら1件ずつ処理できるコレクション
- Laravel 12では、フィルタリングや早期終了がより柔軟にできるようになった

- メリット
  - メモリ効率が超良い
  - takeUntil() などで早期終了が可能
  - 巨大データでもサクサク処理

---

### サービスプロバイダーの遅延読み込み
**必要なときだけロードされるようになって、起動が速くなった**

- サービスプロバイダー: 機能を登録・初期化する場所のこと (AppServiceProvider / RouteServiceProvider など)

- Laravel 11まで
  - すべてのサービスプロバイダーが起動時に読み込まれてた
- Laravel 12から
  - 必要なときだけロードされる「遅延読み込み」方式に変更

---

## 参照
- [Laravel12のバージョンアップ変更点を見てみる](https://tech.every.tv/entry/2025/03/06/131952)
- [Laravel11 12】この一年近くの変更点を確認してみた](https://qiita.com/sigeta/items/ef8a7f38063db54ec55b)
- [Laravel 12.x アップグレードガイド](https://readouble.com/laravel/12.x/ja/upgrade.html)
- [Laravel 12のリリース予定と新機能の詳細](https://www.issoh.co.jp/tech/details/5637/)
- [Laravel 12 Upgrade Memo](https://zenn.dev/at_yasu/articles/laravel_12_upgrade_memo)