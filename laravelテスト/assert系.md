# assert 系メソッド

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
// 値が等しくないかを確認

$this->assertSame('Laravel', $framework);
// 厳密に同じか（===）を確認

$this->assertNotSame('PHP', $framework);
// 厳密に異なるか（!==）を確認

$this->assertCount(3, $users);
// 配列の要素数が 3 か確認

$this->assertEmpty($items);// 空の配列や文字列かを確認

$this->assertNotEmpty($items);
// 空でないことを確認

$this->assertGreaterThan(5, $price);
// 5より大きいことを確認

$this->assertLessThan(100, $stock);
// 100より小さいことを確認

$this->assertContains('信長', $text);
// 配列や文字列に "信長" が含まれているか確認

$this->assertNotContains('家康', $text);
// 配列や文字列に "家康" が含まれていないか確認

$this->assertStringContainsString('Laravel', 'Laravel勉強中');
// 文字列の一部に "Laravel" が含まれているか
```

## データベース関連のチェック
```php
$$this->assertDatabaseHas('users', [ 'id' => 1 ]);
// users テーブルに id=1 のレコードが存在するか

$this->assertDatabaseMissing('users', [ 'email' => 'hoge@example.com' ]);
// users テーブルに該当のレコードが「ない」ことを確認

$this->assertDatabaseCount('posts', 3);
// posts テーブルに 3 件のデータがあるか

$this->assertModelExists($post);
// モデルがDBに存在するか（例：削除されていない）

$this->assertModelMissing($post);
// モデルがDBから削除されたか確認
```

## バリデーション・セッション関連
```php
$response->assertSessionHasErrors(['email']);
// セッションにバリデーションエラー（email）があるか

$response->assertSessionMissing('key');
// セッションに特定のキーが「ない」ことを確認

$response->assertSessionHas('key', 'value');
// セッションに指定のキーと値があるか確認

$response->assertValid(['title', 'body']);
// バリデーションエラーが「出なかった」項目を確認

$response->assertInvalid(['name']);
// name 項目にバリデーションエラーがあるか確認
```

## ビュー関連のアサーション
```php
$response->assertViewIs('posts.index');
// 表示されたビューが posts.index か確認

$response->assertViewHas('posts', Post::all());
// ビューに posts が渡され、内容が Post::all() か確認

$response->assertViewHasAll(['posts', 'user']);
// 複数の変数がビューに渡されているか確認

$response->assertViewHas('user', fn($u) => $u->email === 'mana@example.com');
// コールバックで値の中身をチェック（例：emailが一致しているか）
```

## ファイル・ストレージ関連
```php
Storage::fake('public');
// public ディスクを仮想にしてテスト用に

Storage::disk('public')->assertExists('avatars/' . $file->hashName());
// 仮想ストレージにファイルが存在するか確認

Storage::disk('local')->assertMissing('test.txt');
// ファイルが「存在しない」ことを確認

Storage::disk('s3')->assertExists('images/banner.jpg');
// S3上にファイルが存在するか確認（仮想でも可能）
```

## HTTPレスポンスやリダイレクトの確認
```php
$response->assertStatus(200);
// HTTPステータスが 200 OK か確認

$response->assertOk();
// 上と同じ：レスポンスが OK かどうか

$response->assertRedirect('/home');
// リダイレクト先が /home か

$response->assertSee('ようこそ');
// レスポンスに「ようこそ」という文字が含まれているか

$response->assertDontSee('エラー');
// レスポンスに「エラー」が含まれていないことを確認
```

## JSONレスポンス関連
```php
$response->assertJson(['name' => '太郎']);
// レスポンスの JSON に name=太郎 があるか

$response->assertJsonFragment(['role' => 'admin']);
// JSONの一部（フラグメント）に含まれているか

$response->assertJsonMissing(['error' => true]);
// 特定のキー・値が存在しないことを確認

$response->assertExactJson(['status' => 'ok', 'code' => 200]);
// JSON全体が完全に一致するか確認

$response->assertHeader('Content-Type', 'application/json');
// ヘッダーに特定の値（ここでは application/json）があるか
```

## リダイレクト時の内容確認
```php
$this->followingRedirects()->post('/login', [...])->assertSee('ようこそ！');
// /login → リダイレクト → リダイレクト先のページの中身も検証
```

## 例外発生の確認
```php
$this->expectException(OutOfRangeException::class);
// この後の処理で例外が発生することを期待

$this->expectExceptionMessage('文字数は1から100の間で指定してください');
// 例外のメッセージが一致するか確認

$this->expectExceptionCode(400);
// 例外コードが一致するか確認

$this->expectNotToPerformAssertions();
// アサーションが無いことを宣言（ログだけ検証したい場合など）
```

## テストをスキップ・一時停止したい時
```php
$this->markTestIncomplete();
// 「未完了のテスト」として一時的にマークする

$this->markTestSkipped('mysqli 拡張機能が入ってないため、スキップします');
// 特定の条件でテストをスキップする（環境依存など）
```

## その他チェック系（数値・型）
```php
$this->assertIsArray($result);
// 配列か
$this->assertIsString($title);
// 文字列か

$this->assertIsInt($price);
// 整数か

$this->assertIsBool($flag);
// true/falseか

$this->assertIsObject($user);
// オブジェクトか

$this->assertIsCallable($handler);
// コールバックとして実行可能か

$this->assertIsIterable($collection);
// foreach 可能なコレクションか

$this->assertInstanceOf(Post::class, $post);
// Post クラスのインスタンスか
```


