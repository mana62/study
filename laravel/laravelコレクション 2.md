# Laravel コレクション

## コレクションの基本

| メソッド         | 意味           | 使い方                                | 例文                                                       |
|------------------|----------------|----------------------------------------|------------------------------------------------------------|
| count()          | 件数取得       | $数 = Model::all()->count();           | $リンゴの数 = Item::all()->count();                        |
| first()          | 最初の要素     | $最初 = Model::all()->first();         | $最初のリンゴ = Item::all()->first();                      |
| last()           | 最後の要素     | $最後 = Model::all()->last();          | $最後のリンゴ = Item::all()->last();                       |
| get()            | データ取得     | $リスト = Model::get();                | $3番目のリンゴ = Item::get();                              |
| all()            | 全取得         | $全て = Model::all();                  | $全てのリンゴ = Item::all();                               |
| toArray()        | 配列化         | $配列 = Model::all()->toArray();       | $果物配列 = Item::all()->toArray();                        |
| filter()         | 条件で絞る     | ->filter(fn($x) => $x->条件)           | $赤い = Item::all()->filter(fn($x) => $x->色 == '赤');     |
| where()          | DB絞り込み     | Model::where('カラム', 値)             | $ふじ = Item::where('名前', 'ふじ')->get();                |
| whereIn()        | 複数条件       | ->whereIn('カラム', [値1, 値2])        | $特定リンゴ = Item::whereIn('名前', ['ふじ', '紅玉'])->get(); |
| whereHas()       | リレーション条件 | ->whereHas('relation', fn($q)=>...)   | $赤い箱 = Box::whereHas('items', fn($q)=>$q->where('色', '赤'))->get(); |
| firstWhere()     | 最初の一致     | ->firstWhere('カラム', 値)             | $ふじリンゴ = Item::all()->firstWhere('名前', 'ふじ');     |
| whereMonth()     | 月指定         | ->whereMonth('created_at', 5)          | $五月 = Item::whereMonth('created_at', 5)->get();          |
| whereBetween()   | 範囲条件       | ->whereBetween('カラム', [最小, 最大]) | $価格帯 = Item::whereBetween('価格', [100, 300])->get();   |
| map()            | 値変換         | ->map(fn($x) => ...)                   | $倍価 = Item::all()->map(fn($x) => $x->価格 * 2);          |
| each()           | 順次処理       | ->each(fn($x) => ...)                  | Item::all()->each(fn($x) => Log::info($x->名前));          |
| sort()           | 並び替え       | ->sort()                               | $並べ替え = collect([3,1,2])->sort();                      |
| sortBy()         | キー順ソート   | ->sortBy('key')                        | $安い順 = Item::all()->sortBy('価格');                     |
| sortByDesc()     | 降順ソート     | ->sortByDesc('key')                    | $高い順 = Item::all()->sortByDesc('価格');                 |
| groupBy()        | グループ化     | ->groupBy('key')                       | $色別 = Item::all()->groupBy('色');                        |
| reduce()         | 合算計算       | ->reduce(fn($acc, $x) => ...)          | $合計 = Item::all()->reduce(fn($sum, $x) => $sum + $x->価格, 0); |
| sum()            | 合計           | ->sum('key')                           | $価格合計 = Item::sum('価格');                             |
| avg()            | 平均           | ->avg('key')                           | $平均価格 = Item::avg('価格');                             |
| min()            | 最小値         | ->min('key')                           | $最安 = Item::min('価格');                                 |
| max()            | 最大値         | ->max('key')                           | $最高 = Item::max('価格');                                 |
| contains()       | 含まれるか     | ->contains('key', 値)                  | $あるか = Item::all()->contains('名前', 'ふじ');           |
| find()           | ID検索         | Model::find(1)                          | $指定 = Item::find(3);                                     |
| pluck()          | 値だけ抽出     | ->pluck('key')                         | $名前一覧 = Item::all()->pluck('名前');                    |

---

## Raw系メソッド（SQL文を直接書く）

| メソッド        | 内容                 | 例                                     |
| --------------- | -------------------- | -------------------------------------- |
| `selectRaw()`   | 生SQLでカラム指定    | `selectRaw('COUNT(*) as count')`       |
| `whereRaw()`    | 生SQLで条件          | `whereRaw('price > ?', [100])`         |
| `orWhereRaw()`  | or条件を生SQLで      | `orWhereRaw('color = ?', ['赤'])`      |
| `havingRaw()`   | 集計後条件を生SQLで  | `havingRaw('SUM(price) > ?', [1000])`  |
| `orHavingRaw()` | or + 集計条件        | `orHavingRaw('SUM(price) > ?', [300])` |
| `orderByRaw()`  | 並び替えをSQLで      | `orderByRaw('price DESC')`             |
| `groupByRaw()`  | グループ条件を関数で | `groupByRaw('MONTH(created_at)')`      |

---

## join / withCount など

* `join()`：テーブル結合
* `withCount()`：リレーションの件数取得

```php
Post::withCount('comments')->orderByDesc('comments_count')->first();
```

---

## sort系 vs order系

| 種別           | コレクション           | クエリビルダ             |
| -------------- | ---------------------- | ------------------------ |
| 並び替え方法   | `sortBy`, `sortByDesc` | `orderBy`, `orderByDesc` |
| 実行タイミング | `get()`の後            | `get()`の前              |

---

## where() vs filter()

| 比較       | where()         | filter()             |
| ---------- | --------------- | -------------------- |
| タイミング | SQLクエリ実行前 | 取得後のコレクション |
| 処理場所   | DB              | PHP上                |
| 用途       | 取得前に絞る    | 取得後に処理         |

---

## SQL関数まとめ

| 関数      | 説明             | 例                        |
| --------- | ---------------- | ------------------------- |
| COUNT(\*) | 件数             | `COUNT(*) as total`       |
| AVG()     | 平均             | `AVG(score) as avg_score` |
| SUM()     | 合計             | `SUM(price)`              |
| MIN()     | 最小             | `MIN(age)`                |
| MAX()     | 最大             | `MAX(score)`              |
| DAYNAME() | 曜日             | `DAYNAME(created_at)`     |
| MONTH()   | 月               | `MONTH(created_at)`       |
| YEAR()    | 年               | `YEAR(created_at)`        |
| DATE()    | 日付（時刻除外） | `DATE(created_at)`        |

---

### 例：メンバーごとのレンタル回数取得

```php
Rental::selectRaw('member_id, COUNT(*) as rental_count')
  ->groupBy('member_id')
  ->havingRaw('COUNT(*) >= ?', [3])
  ->get();
```

---

## Eloquent vs クエリビルダ

| 比較         | Eloquent | クエリビルダ |
| ------------ | -------- | ------------ |
| モデル使用   | ✅        | ❌            |
| 書き方       | シンプル | SQLに近い    |
| リレーション | 簡単     | 手動結合     |
| SQL柔軟性    | 標準的   | 高い         |

---

