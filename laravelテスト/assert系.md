# assert 系メソッド

---

## 基本
```php
$this->assertTrue(true);
// 値が true であることを確認

$this->assertFalse(false);
// 値が false であることを確認

$this->assertNull($value);
// 値が null であることを確認

$this->assertNotNull($value);
// 値が null でないことを確認

$this->assertEquals(10, $actual);
// 値が等しい（==）か確認

$this->assertNotEquals(99, $actual);
// 値が等しくないか確認

$this->assertSame('Laravel', $framework);
// 厳密に同じか（===）を確認

$this->assertNotSame('PHP', $framework);
// 厳密に異なるか（!==）を確認

$this->assertCount(3, $users);
// 配列の要素数が 3 か確認

$this->assertEmpty($items);
// 空の配列や文字列かを確認

$this->assertNotEmpty($items);
// 空でないことを確認

$this->assertGreaterThan(5, $price);
// 5より大きいことを確認

$this->assertLessThan(100, $stock);
// 100より小さいことを確認

$this->assertContains('信長', $text);
// 配列や文字列に "信長" が含まれているか

$this->assertNotContains('家康', $text);
// 配列や文字列に "家康" が含まれていないか

$this->assertStringContainsString('Laravel', 'Laravel勉強中');
// 文字列の一部に "Laravel" が含まれているか

```
---

## データベース関連のチェック
```php
$this->assertDatabaseHas('users', ['id' => 1]);
// 指定レコードが存在するか

$this->assertDatabaseMissing('users', ['email' => 'hoge@example.com']);
// 指定レコードが存在しないか

$this->assertDatabaseCount('posts', 3);
// テーブルに指定件数のデータがあるか

$this->assertModelExists($post);
// モデルがDBに存在するか

$this->assertModelMissing($post);
// モデルがDBから削除されたか

```

---

## バリデーション・セッション関連
```php
$response->assertSessionHasErrors(['email']);
// バリデーションエラーがあるか

$response->assertSessionMissing('key');
// セッションにキーがないか

$response->assertSessionHas('key', 'value');
// セッションにキーと値があるか

$response->assertValid(['title', 'body']);
 // バリデーションエラーが出なかった項目

$response->assertInvalid(['name']);
// name にバリデーションエラーがあるか

```

---

## ビュー関連のアサーション
```php
$response->assertViewIs('posts.index');
// 指定ビューが表示されたか

$response->assertViewHas('posts', Post::all());
// ビューに渡された変数と値を確認

$response->assertViewHasAll(['posts', 'user']);
// 複数の変数が渡されたか

$response->assertViewHas('user', fn($u) => $u->email === 'mana@example.com');
// コールバックで値を検証

```

---

## ファイル・ストレージ関連
```php
Storage::fake('public');
// 仮想ストレージを用意

Storage::disk('public')->assertExists('avatars/' . $file->hashName());
// ファイルが存在するか

Storage::disk('local')->assertMissing('test.txt');
// ファイルが存在しないか

Storage::disk('s3')->assertExists('images/banner.jpg');
// S3上にファイルがあるか
```

---

## HTTPレスポンスやリダイレクトの確認
```php
$response->assertStatus(200);
// ステータスコードが 200 か

$response->assertOk();
// レスポンスが OK か

$response->assertSuccessful();
// 成功レスポンスか（2xx）

$response->assertRedirect('/home');
// リダイレクト先が /home か

$response->assertRedirectToRoute('dashboard');
// 指定ルートにリダイレクトされたか

$response->assertUnauthorized();
// ステータスコードが 401 か

$response->assertNotFound();
// ステータスコードが 404 か 「404 Not Found」

$response->assertNoContent();
// ステータスコードが 204 か 「204 No Content」

$response->assertUnprocessable();
// ステータスコードが 422 か 「422 Unprocessable Entity（validationエラー）」

$response->assertSee('ようこそ');
// レスポンスに文字列が含まれているか

$response->assertDontSee('エラー');
// レスポンスに文字列が含まれていないか

$response->assertSeeText()
// HTTPレスポンスの本文に含まれるテキストを検証する

$response->assertDontSeeText()
// HTTPレスポンスの本文に含まれていないかテキストを検証する

```

---

## JSONレスポンス関連
```php
$response->assertJson(['name' => '太郎']);
// JSONに指定キー・値があるか

$response->assertJsonFragment(['role' => 'admin']);
// JSONの一部に含まれているか

$response->assertJsonMissing(['error' => true]);
// JSONに指定キー・値がないか

$response->assertExactJson(['status' => 'ok', 'code' => 200]);
// JSON全体が一致するか(キーの順番なども)

$response->assertHeader('Content-Type', 'application/json');
// ヘッダーに指定値があるか

$response->assertJsonStructure(['status']);
// statusというキーがあるかどうか

$response->assertJsonCount(2);
// 件数確認（上記の場合は２件あるか）

```

---

## リダイレクト時の内容確認
```php
$this->followingRedirects()->post('/login', [...])->assertSee('ようこそ');
// リダイレクト先の内容を確認
```

---

## 例外発生の確認
```php
$this->withoutExceptionHandling();
// 例外を通常通り投げる

$this->expectException(OutOfRangeException::class);
// 例外クラスを期待

$this->expectExceptionMessage('文字数は1から100の間で指定してください');
// 例外メッセージを確認

$this->expectExceptionCode(400);
// 例外コードを確認

$this->expectNotToPerformAssertions();
// アサーションが無いことを宣言

```

---

## テストをスキップ・一時停止したい時
```php
$this->markTestIncomplete();
// 未完了としてマーク

$this->markTestSkipped('mysqli 拡張機能が入ってないため、スキップします');
// 条件付きスキップ

```

---

## その他チェック系（数値・型）
```php
$this->assertIsArray($result); // 配列か

$this->assertIsString($title); // 文字列か

$this->assertIsInt($price); // 整数か

$this->assertIsBool($flag); // 真偽値か

$this->assertIsObject($user); // オブジェクトか

$this->assertIsCallable($handler); // 実行可能か

$this->assertIsIterable($collection); // イテレータか

$this->assertInstanceOf(Post::class, $post); // 指定クラスのインスタンスか

```

## 確認
```php
$response->dump(); // 全体を表示
```


イベント専用テストメソッド
Event::fake() でイベントを偽装（実際には動かさない）
Event::assertDispatched() で発火したか確認

```php
public function test_event()
{
    Event::fake(); // イベントを偽装

    User::factory()->create(); // ここで UserRegistered が発火するはず

    Event::assertDispatched(UserRegistered::class); // 発火したか確認
}
```

## AJAX
- `postJson()` : JSONを送るPOSTリクエストをテストで送るためのメソッド
- `putJson()` : JSONを送るPUTリクエストをテストで送るためのメソッド
- `getJson()` : JSONレスポンスを期待するGETリクエストを送るメソッド
- `patchJson()` : JSONを送るPATCHリクエストをテストで送るためのメソッド
- `X-Requested-With: XMLHttpRequest` : AJAXリクエストとして扱われるようにするためのヘッダー