# URLパラメータ
→ Laravelのルートでは、URLパラメータを自動的にコントローラの引数に渡す仕組みがある

```bash
# ルート
Route::get('/users/{id}', [UserController::class, 'show']);
```


```bash
# コントローラ
public function show($id)
{
    // $id = 5 が自動的に入る
    $user = User::find($id);
    return view('user.show', compact('user'));
}
```
- $request->id と書く必要はない
- URLパラメータとコントローラの引数名が一致していれば自動で代入される
- $request は複数のパラメータを扱うときや、POSTデータ・ヘッダー情報も扱うときに便利

---

## まとめると
- URLパラメータが一つだけ → 直接引数で受けるのが一般的
- 複数パラメータやフォームデータ → $request->input('name') が便利