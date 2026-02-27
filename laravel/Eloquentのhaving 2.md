# Eloquentのhaving
＝having は「集計した結果に条件をかけたい時」に使う
＝GROUP BYを使った後はhavingしか使えない
＝「having は groupBy で集計した結果に条件をかけるために使う

|       | where     | having                    |
| ----- | --------- | ------------------------- |
| いつ使う？ | **集計する前** | **集計した後**                 |
| 対象    | 元データ      | `count / sum / avg` などの結果 |
| 例     | 年齢 ≥ 20   | 投稿数 ≥ 3                   |

```php
GROUP BY users.id
HAVING count(posts.id) >= 3
```

## 例
### 投稿数が3件以上のユーザー
```php
User::join('posts', 'users.id', '=', 'posts.user_id')
  ->select('users.name', DB::raw('count(posts.id) as postCount'))
  ->groupBy('users.name')
  ->having('postCount', '>=', 3)
  ->get();
```
「ユーザーごとに投稿数を数えて、その結果が3以上のユーザーだけ残す」

### where + having を両方使う
```php
User::join('posts', 'users.id', '=', 'posts.user_id')
  ->where('posts.published', true)     // ← 集計前
  ->select('users.name', DB::raw('count(posts.id) as postCount'))
  ->groupBy('users.name')
  ->having('postCount', '>=', 3)       // ← 集計後
  ->get();
```
「公開済みの投稿だけを対象にして、ユーザーごとの投稿数を出し、3件以上あるユーザーを取得する」

## まとめ
行そのものの条件？ → where
件数・合計・平均？ → having