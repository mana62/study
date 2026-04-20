# Laravel API Resource
＝ Eloquent モデルを **API 用の JSON 形式** に整える仕組み
- JSON APIで返したいフィールドだけを選べる
- 関連データ（author など）を自由に追加できる
- パスワードなど **返してはいけない情報** を除外できる
- API のレスポンスを統一できる（フロントが扱いやすい）

## なぜ必要なのか
❌ Resource を使わない場合
```php
return User::find(1);
```

返ってくる JSON：
```php
{
  "id": 1,
  "name": "田中",
  "password": "$2y$...",
  "created_at": "...",
  "updated_at": "..."
}
```
**→ password まで返ってしまう。危険。**

✅ Resource を使う場合
```php
return new PostResource(Post::find(1));
```

返ってくる JSON：
```php
{
  "id": 1,
  "title": "...",
  "body": "...",
  "author": "田中"
}
```
**→ 必要な情報だけ返せる。安全で綺麗。**

## Resource のコード例 （PostResource）
```php
class PostResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id'     => $this->id,
            'title'  => $this->title,
            'body'   => $this->body,
            'author' => $this->user->name,
        ];
    }
}
```

## どこで使うの？
```bash
ルート → コントローラ → Resource → JSONレスポンス
```

## コントローラでの使い方
### ① 単体データを返す場合
```php
public function show(Post $post)
{
    return new PostResource($post);
}
```

### ② 複数データを返す場合
```php
public function index()
{
    return PostResource::collection(Post::all());
}
```

## まとめ
- Resource は「API の見た目を整えるテンプレート」
- フロントと API をつなぐときに必ず使う重要パーツ

| 項目 | 内容 |
|------|------|
| **役割** | モデルを API 用 JSON に整形する |
| **メリット** | 不要なフィールドを除外、必要な情報を追加、安全で統一されたレスポンス |
| **使う場所** | コントローラの return 部分 |
| **単体** | `return new PostResource($post);` |
| **複数** | `return PostResource::collection($posts);` |
| **内部処理** | `toArray()` の内容がそのまま JSON になる |

