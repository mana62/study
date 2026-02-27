# Laravel Gate(ゲート)、Policy(ポリシー)
→ どちらもAuthorization(認可)に関する機能

---
## Gate（ゲート）とは
→ 管理者だけが設定ページを見られるなどのルールを作りたいとき <br>
  - クロージャで定義する
  - モデルに依存しないシンプルな条件に向いてる

```php
# App\Providers\AuthServiceProvider の中で定義する
# 管理者だけがダッシュボードを見られる
public function boot(){
Gate::define('view-dashboard', function ($user) {
    return $user->is_admin;
});
}
```
- `Gate::define()` : 認可ルールを定義する関数

```bash
# Blade または コントローラ
@can('view-dashboard')
    <a href="/admin">管理画面</a>
@endcan
```
- `@can` : Bladeテンプレートで使える認可チェックの構文

---

## Policy（ポリシー）とは
→ 投稿を編集できるのは投稿者だけなどのルールを作りたいとき
<br>
  - `php artisan make:policy PostPolicy --model=Post` で作成
  - CRUD操作ごとにメソッドを定義

```bash
# Policyクラスの中で定義する
# 自分の投稿のみ編集可能
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
```

```bash
# Blade または コントローラ
if ($user->can('update', $post)) {
    // 編集ボタンを表示
}
```

---

## 参考
- [Laravel Gate(ゲート)、Policy(ポリシー)を完全理解](https://reffect.co.jp/laravel/laravel-gate-policy-understand)
- [[Laravel]モデルの認可ポリシーを作る](https://zenn.dev/redheadchloe/articles/d60fd178757e96)

---

## まとめ
- 使う場合はBladeでもコントローラでも使える
- Gateは @can('ルール名')
- Policyは @can('操作名', モデル)
- 定義場所はそれぞれ決まってる（GateはAuthServiceProvider、PolicyはPolicyクラス）