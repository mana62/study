# Eloquentのjoin.md

## ① inner join
```php
User::join('posts', 'users.id', '=', 'posts.user_id')
    ->get();
```
・users と posts の両方にデータがあるものだけ取得

## ② left join（片方がなくても取得したい時）
```php
User::leftJoin('posts', 'users.id', '=', 'posts.user_id')
    ->get();
```
・投稿がないユーザーも取得したい時に使う

## ③ join に追加条件をつける（無名関数を使うパターン）
```php
User::join('posts', function ($join) {
    $join->on('users.id', '=', 'posts.user_id')
         ->where('posts.published', true);
})
->get();
```
・join の ON に条件を追加したい時は 無名関数が必要
・これは「join の中が別クエリ」だから

## ④ 複数 join
```php
User::join('posts', 'users.id', '=', 'posts.user_id')
    ->join('comments', 'posts.id', '=', 'comments.post_id')
    ->get();
```

## 全体の例
```php
User::select('users.*', 'posts.title')
    ->join('posts', 'users.id', '=', 'posts.user_id')
    ->where('posts.published', true)
    ->get();
```
・select で必要なカラムだけ取る
・join で結合
・where で条件
・get で取得

## まとめ

| やりたいこと | 書き方 |
|--------------|--------|
| 普通の join | `join('table', 'left', '=', 'right')` |
| 左外部結合 | `leftJoin('table', 'left', '=', 'right')` |
| join に条件追加 | `join('table', function($join){ ... })` |
| 複数 join | `join()` を連続で書く |



## joinの後の()

「一覧として取りたい方を左（基準）にして、
それに紐づく“逆側”のテーブルを join する」

```php
// ユーザー一覧を取りたい
User::join('posts', ...)

// 投稿一覧を取りたい
Post::join('users', ...)
```