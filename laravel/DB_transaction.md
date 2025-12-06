# DB::transaction とは
→ **「全部成功するか、全部失敗するか」** を保証してくれる仕組み

例えば、銀行の振込
1. Aさんの残高を1000円減らす
2. Bさんの残高を1000円増やす

もし「1」だけ成功して、「2」でエラーが起きたら、
Aさんのお金だけ減って、Bさんには届いてない…っていう状況になってしまう

`DB::transaction` を使うと、こういう時に**「1」の処理もなかったこと（ロールバック）** にしてくれる
データがおかしくなるのを防ぐための機能

---

## どうやって使うの？

一番簡単でオススメなのが、クロージャ（関数）を使う方法
この中に書いた処理は、全部まとめてトランザクションとして扱われる

### 基本の書き方

```php
use Illuminate\Support\Facades\DB;

DB::transaction(function () {
    // ここにデータベースの処理を書く
    // 例：Aさんの残高を減らす
    // 例：Bさんの残高を増やす
});
```

### 具体的な例

```php
use Illuminate\Support\Facades\DB;
use App\Models\User;

// 例：ID:1の人からID:2の人へ1000ポイント移動する
DB::transaction(function () {
    // 1. 送る人のポイントを減らす
    $sender = User::find(1);
    $sender->points -= 1000;
    $sender->save();

    // 2. 受け取る人のポイントを増やす
    $receiver = User::find(2);
    $receiver->points += 1000;
    $receiver->save();
});
```

### 何がすごいの？
*   **自動で元通り**: もし `{ ... }` の中のどこかでエラー（例外）が起きると、Laravelが勝手に**全部なかったこと**にしてくれる！
*   **自動で確定**: エラーなく最後まで実行できたら、自動で変更を確定（コミット）してくれる！

---

## 手動でやりたい時

```php
DB::beginTransaction(); // スタート

try {
    // 処理...
    
    DB::commit(); // 成功！確定！
} catch (\Exception $e) {
    DB::rollBack(); // 失敗…全部戻す！
    throw $e; // エラーはそのまま投げる
}
```
見ての通り、`try-catch` とか書くのが面倒だから、基本はさっきの `DB::transaction(function() { ... })` を使うのがオススメ

## まとめ
*   **複数のDB操作をセットで行う時** は必ず使う
*   **`DB::transaction(function() { ... })`** で囲むだけ
*   失敗したら**勝手に元通り**にしてくれるから安心
