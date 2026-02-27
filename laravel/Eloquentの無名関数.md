# Eloquentで無名関数（クロージャ）を使う場面
＝with(リレーション) はリレーション先の全データを取得するだけ
＝**条件をつけたいなら必ず無名関数を使う**

**無名関数**を使うのは
**『リレーションに対して条件を付けたい時』** または
**『サブクエリに条件を付けたい時』** この2つだけ

## ① where は **「今のクエリ」**に条件を付けるだけ
```php
User::where('age', '>=', 20)
```
＝これは User テーブルに対する条件
＝つまり「今操作しているテーブル」に対しては、普通に where を書けばいい

## ② 無名関数（クロージャ）を使うのは「**別のクエリ**（リレーション先など）に条件を付けたい時」

```php
// withCount（リレーションの集計に条件を付けたい）
User::withCount([
    'posts as postCount' => function ($q) {
        $q->where('published', true);
    }
])
```
＝withCount の中は「別のクエリ」だから、無名関数を使う

・メインクエリ → users
・サブクエリ → posts の COUNT
・この「サブクエリ」に条件を付けたいから、function($q) を使う

## ③ with / load / whereHas なども同じ理由
### リレーションに条件を付けたい時
```php
User::whereHas('posts', function ($q) {
    $q->where('published', true);
})
```
・メインクエリ → users
・サブクエリ → posts の存在チェック
・だから無名関数が必要

### join の ON 条件に追加したい時
```php
User::join('posts', function ($join) {
    $join->on('posts.user_id', '=', 'users.id')
         ->where('posts.published', true);
})
```
・join の ON に where を入れたい時も「別のクエリ」扱いだからクロージャ

### selectSub（サブクエリを SELECT に入れる）
```php
User::selectSub(function ($q) {
    $q->from('posts')
      ->selectRaw('COUNT(*)')
      ->whereColumn('posts.user_id', 'users.id');
}, 'postCount')
```
・これも完全に別クエリなのでクロージ

## 無名関数（クロージャ）を使う場面まとめ

| 使う場面 | where で書ける？ | 無名関数が必要？ | 理由 |
|---------|------------------|------------------|------|
| 今のテーブルに条件 | ✔ | ✖ | 同じクエリだから |
| リレーションに条件 | ✖ | ✔ | 別クエリだから |
| withCount の条件 | ✖ | ✔ | サブクエリだから |
| whereHas の条件 | ✖ | ✔ | サブクエリだから |
| join の ON 条件 | ✖ | ✔ | 別クエリだから |
| selectSub | ✖ | ✔ | 別クエリだから |
