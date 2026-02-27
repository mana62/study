# response と this
$response は「リクエスト結果」
$this は「テストクラス自身」


🟦 $response は「画面の結果」
つまり…
HTTPリクエストの返り値（レスポンス）をチェックするとき
例：ページが表示される？
```php
$response = $this->get('/about');
$response->assertOk();
$response->assertSee('About Page');
```

例：リダイレクトされた？
```php
$response = $this->post('/register');
$response->assertRedirect('/welcome');
```

例：バリデーションエラーが返った？
```php
$response->assertSessionHasErrors('name');
```

👉 $response は「ブラウザで見える結果」
🟩 $this は「裏側の確認」
つまり…
DBや認証など、レスポンスとは別のものを確認するとき
例：DBに保存された？
```php
$this->assertDatabaseHas('users', [
    'name' => 'Bob',
]);
```

例：DBに保存されてない？
```php
$this->assertDatabaseMissing('users', [
    'name' => '',
]);
```

例：ログインしている？
```php
$this->assertAuthenticated();
```

👉 $this は「システム内部を直接チェック」
画面（レスポンス）を見る → $response
| 確認したいこと  | 書き方                                   |
| -------- | ------------------------------------- |
| ステータス200 | `$response->assertOk()`               |
| 文字が表示される | `$response->assertSee()`              |
| リダイレクト   | `$response->assertRedirect()`         |
| セッションエラー | `$response->assertSessionHasErrors()` |


DBや裏側を見る → $this
| 確認したいこと | 書き方                              |
| ------- | -------------------------------- |
| DBにある   | `$this->assertDatabaseHas()`     |
| DBにない   | `$this->assertDatabaseMissing()` |
| ログイン中   | `$this->assertAuthenticated()`   |


$response = 「画面の通知」
- 表示された？
- 移動した？
- エラー出た？

ブラウザ目線
$this = 「裏で本当に保存されたか調査」
- DBに入った？
- 入ってない？
- ログイン状態？


「管理者目線」
例：登録処理テストを分解すると…
① POSTする（行動）
```php
$response = $this->post('/register', [
    'name' => 'Bob',
]);
```

② 画面がどうなった？（response）
```php
$response->assertRedirect('/welcome');
```

③ DBに入った？（this）
```php
$this->assertDatabaseHas('users', [
    'name' => 'Bob',
]);
```

## まとめ
- response = 表の結果
-  this = 裏の事実