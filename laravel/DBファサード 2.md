# DB ファサード

Laravel でデータベースを操作する際、最も基本的で直感的に使えるのが **DB ファサード** 
SQL に近い感覚で書ける

---

## 基本的な使い方

ファイルの上部で `DB` ファサードを読み込む必要がある

```php
use Illuminate\Support\Facades\DB;
```

---

## CRUD 操作 (Create, Read, Update, Delete)

### 1. データの取得 (SELECT)

#### 全件取得 (`get`)
テーブルの全てのデータを取得します。

```php
$users = DB::table('users')->get();

foreach ($users as $user) {
    echo $user->name;
}
```

#### 1件取得 (`first`)
条件に一致する最初の1件だけを取得します。

```php
$user = DB::table('users')->where('name', 'John')->first();

echo $user->email;
```

#### 特定の値だけ取得 (`value`)
レコード全体ではなく、特定のカラムの値だけが欲しい場合に便利です。

```php
$email = DB::table('users')->where('name', 'John')->value('email');
```

#### ID で取得 (`find`)
主キー（通常は `id`）を使って1件取得します。

```php
$user = DB::table('users')->find(3);
```

#### カラムの値をリストで取得 (`pluck`)
特定のカラムの値だけを配列（コレクション）として取得します。

```php
$titles = DB::table('roles')->pluck('title');

foreach ($titles as $title) {
    echo $title;
}
```

---

### 2. データの登録 (INSERT)

#### 1件登録 (`insert`)

```php
DB::table('users')->insert([
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'password' => 'secret',
]);
```

#### 複数件登録
配列の配列を渡すことで、まとめて登録できます。

```php
DB::table('users')->insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0],
]);
```

#### ID を取得して登録 (`insertGetId`)
登録したレコードの ID（自動増分 ID）を知りたい場合に使います。

```php
$id = DB::table('users')->insertGetId([
    'email' => 'john@example.com',
    'votes' => 0,
]);
```

---

### 3. データの更新 (UPDATE)

#### 条件に一致するレコードを更新 (`update`)

```php
DB::table('users')
    ->where('id', 1)
    ->update(['votes' => 1]);
```

#### 加算・減算 (`increment` / `decrement`)
数値を増やしたり減らしたりするのに便利です。

```php
// votes を 1 増やす
DB::table('users')->increment('votes');

// votes を 5 増やす
DB::table('users')->increment('votes', 5);

// votes を 1 減らす
DB::table('users')->decrement('votes');
```

---

### 4. データの削除 (DELETE)

#### 条件に一致するレコードを削除 (`delete`)

```php
DB::table('users')->where('votes', '<', 100)->delete();
```

#### 全件削除 (`truncate`)
テーブルのデータを全て削除し、ID の自動増分もリセットします。

```php
DB::table('users')->truncate();
```

---

## トランザクション

複数の処理をまとめて行い、途中で失敗したら全てなかったことにする（ロールバックする）機能です。データの整合性を保つために非常に重要です。

### 自動トランザクション (`transaction`)
クロージャの中の処理が全て成功すればコミット（確定）、例外が投げられたら自動的にロールバック（取り消し）されます。一番推奨される書き方です。

```php
use Illuminate\Support\Facades\DB;

DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});
```

### 手動トランザクション
自分で開始、コミット、ロールバックを制御したい場合に使います。

```php
DB::beginTransaction();

try {
    DB::table('users')->update(['votes' => 1]);
    DB::table('posts')->delete();
    
    DB::commit(); // 処理を確定
} catch (\Exception $e) {
    DB::rollBack(); // エラーが起きたら取り消す
    throw $e; // エラーを再スローして通知する
}
```

---

## デバッグ（実行された SQL を確認する）

「思った通りのデータが取れない…」という時、実際にどんな SQL が実行されているか確認すると原因がわかりやすいです。

### `enableQueryLog` と `getQueryLog`

```php
DB::enableQueryLog(); // ログの記録を開始

// 何かクエリを実行
DB::table('users')->get();

// 記録されたログを取得して表示
dd(DB::getQueryLog());
```

### `toSql` (SQL 文字列だけ確認)
実行はせずに、生成される SQL 文だけを確認したい場合に使います。

```php
$sql = DB::table('users')->where('votes', '>', 100)->toSql();
dd($sql);
// 結果: "select * from `users` where `votes` > ?"
```
※ `?` の部分はプレースホルダーといい、実際の値は入りません。

---

## 生の SQL を実行する (Raw Expressions)

複雑なクエリなど、クエリビルダでは書きにくい場合に生の SQL を書くことができます。
**注意:** SQL インジェクション脆弱性を防ぐため、ユーザーからの入力値を直接埋め込まないように注意してください。

```php
// SELECT
$users = DB::select('select * from users where active = ?', [1]);

// INSERT
DB::insert('insert into users (id, name) values (?, ?)', [1, 'Dayle']);

// UPDATE
$affected = DB::update('update users set votes = 100 where name = ?', ['John']);

// DELETE
$deleted = DB::delete('delete from users');

// 汎用的な文 (DROP TABLE など)
DB::statement('drop table users');
```
