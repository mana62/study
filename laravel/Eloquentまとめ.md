# Eloquent

| メソッド                | 意味             | 例                                                          |
| ------------------- | -------------- | ---------------------------------------------------------- |
| `when()`            | 条件付きでクエリ追加     | `->when($staffId, fn($q)=>$q->where('staff_id',$staffId))` |
| `whereIn()`         | 複数値で検索         | `->whereIn('status',[1,2,3])`                              |
| `whereBetween()`    | 範囲検索           | `->whereBetween('created_at',[$from,$to])`                 |
| `whereNull()`       | NULL検索         | `->whereNull('deleted_at')`                                |
| `whereNotNull()`    | NULL以外検索       | `->whereNotNull('email_verified_at')`                      |
| `orWhere()`         | OR条件           | `->orWhere('status',1)`                                    |
| `orderBy()`         | 並び替え           | `->orderBy('created_at','desc')`                           |
| `latest()`          | 新しい順           | `->latest()`                                               |
| `oldest()`          | 古い順            | `->oldest()`                                               |
| `limit()`           | 件数制限           | `->limit(10)`                                              |
| `take()`            | 件数制限（limitの別名） | `->take(10)`                                               |
| `pluck()`           | 1カラムだけ取得       | `User::pluck('name')`                                      |
| `value()`           | 1つの値だけ取得       | `User::where('id',1)->value('name')`                       |
| `exists()`          | データ存在チェック      | `User::where('email',$mail)->exists()`                     |
| `count()`           | 件数取得           | `User::where('status',1)->count()`                         |
| `first()`           | 1件取得           | `User::where('id',1)->first()`                             |
| `firstOrFail()`     | 無ければ404        | `User::where('id',1)->firstOrFail()`                       |
| `find()`            | ID検索           | `User::find(1)`                                            |
| `findOrFail()`      | ID検索（無ければ404）  | `User::findOrFail(1)`                                      |
| `with()`            | リレーション事前取得     | `Post::with('user')->get()`                                |
| `whereHas()`        | リレーション条件       | `Post::whereHas('comments')`                               |
| `whereDoesntHave()` | リレーション無し       | `Post::whereDoesntHave('comments')`                        |
| `withCount()`       | リレーション件数       | `Post::withCount('comments')`                              |
| `chunk()`           | 分割取得（大量データ）    | `User::chunk(100,function($users){})`                      |
| `update()`          | 一括更新           | `User::where('status',0)->update(['status'=>1])`           |
| `delete()`          | 削除             | `User::where('id',1)->delete()`                            |

## 特によく使われるもの
| メソッド             | 用途        |
| ---------------- | --------- |
| `when()`         | 条件付き検索    |
| `whereIn()`      | ステータス検索   |
| `whereBetween()` | 日付範囲      |
| `with()`         | N+1問題防止   |
| `whereHas()`     | リレーション条件  |
| `exists()`       | 存在チェック    |
| `pluck()`        | カラム配列取得   |
| `latest()`       | 新しい順      |
| `firstOrFail()`  | APIや画面404 |
| `withCount()`    | 件数表示      |

| メソッド            | 意味       |
| --------------- | -------- |
| `tap()`         | クエリ途中処理  |
| `unless()`      | whenの逆   |
| `addSelect()`   | SELECT追加 |
| `selectRaw()`   | 生SQL     |
| `whereColumn()` | カラム同士比較  |
