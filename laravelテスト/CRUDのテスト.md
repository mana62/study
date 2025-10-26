# CRUDのテスト

### 基本的なテスト内容
| 項目      | 書く場所         | 主なメソッド                            |
| ------- | ------------ | --------------------------------- |
| 一覧・表示   | Feature Test | `get()`, `assertSee()`            |
| 登録      | Feature Test | `post()`, `assertDatabaseHas()`   |
| 更新      | Feature Test | `put()`, `assertDatabaseHas()`    |
| 削除      | Feature Test | `delete()`, `assertDatabaseHas()` |
| バリデーション | Feature Test | `assertSessionHasErrors()`        |
| Ajax    | Feature Test | `postJson()`, `assertJson()`      |

---

### 前提(例:Customer)
- Laravel 11 or 12
- Model：Customer
- Controller：CustomerController
- DBテーブル：customers
- Route：/customers
- テストファイル：tests/Feature/CustomerTest.php

---

#### ステップ①：テストファイルを作成
```bash
php artisan make:test CustomerTest
```
- tests/Feature/CustomerTest.php 生成

#### ステップ②：テストの基本構造
```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use App\Models\Customer;
use Illuminate\Foundation\Testing\RefreshDatabase;

class CustomerTest extends TestCase
{
    use RefreshDatabase; // テストごとにDBをリセット

    public function test_一覧ページが表示できる()
    {
        Customer::factory()->count(3)->create();

        $response = $this->get('/customers');

        $response->assertStatus(200);
        $response->assertSee(Customer::first()->name);
    }
}
```
- 🔹 RefreshDatabase → 毎回テストDBを初期化
- 🔹 assertStatus(200) → 正常にページが表示されたか
- 🔹 assertSee() → レスポンス内に指定文字が含まれるか

#### ステップ③：登録（CREATE）のテスト
```php
public function test_顧客を新規登録できる()
{
    $data = [
        'name' => '山田太郎',
        'email' => 'taro@example.com',
        'phone' => '09011112222',
    ];

    $response = $this->post('/customers', $data);

    $response->assertRedirect('/customers');
    $this->assertDatabaseHas('customers', ['email' => 'taro@example.com']);
}
```
- 🔹 post() → /customers にフォーム送信をシミュレート
- 🔹 assertDatabaseHas() → DBにデータが登録されたか確認

#### ステップ④：編集・更新（UPDATE）のテスト
```php
public function test_顧客情報を更新できる()
{
    $customer = Customer::factory()->create(['name' => '山田太郎']);

    $response = $this->put("/customers/{$customer->id}", [
        'name' => '山田花子',
        'email' => $customer->email,
    ]);

    $response->assertRedirect('/customers');
    $this->assertDatabaseHas('customers', ['name' => '山田花子']);
}
```

#### ステップ⑤：削除（DELETE）のテスト

- （論理削除＝deleted_atやdelflagを立てるタイプ）

```php
public function test_顧客を論理削除できる()
{
    $customer = Customer::factory()->create(['delflag' => 0]);

    $response = $this->delete("/customers/{$customer->id}");

    $response->assertRedirect('/customers');
    $this->assertDatabaseHas('customers', [
        'id' => $customer->id,
        'delflag' => 1,
    ]);
}
```

#### ステップ⑥：バリデーションテスト
```php
public function test_必須項目が抜けていると登録できない()
{
    $response = $this->post('/customers', [
        'name' => '', // nameが必須
        'email' => 'invalid-email',
    ]);

    $response->assertSessionHasErrors(['name', 'email']);
}
```

#### ステップ⑦：検索・ソート・ページング
```php
public function test_名前で検索できる()
{
    Customer::factory()->create(['name' => '山田太郎']);
    Customer::factory()->create(['name' => '佐藤花子']);

    $response = $this->get('/customers?keyword=山田');

    $response->assertSee('山田太郎');
    $response->assertDontSee('佐藤花子');
}
```

#### ステップ⑧：Ajax（メモ更新など）

```php
public function test_Ajaxで顧客メモを更新できる()
{
    $customer = Customer::factory()->create();

    $response = $this->postJson("/api/customers/{$customer->id}/memo", [
        'memo' => 'テストメモ',
    ]);

    $response->assertStatus(200)
             ->assertJson(['success' => true]);

    $this->assertDatabaseHas('customers', [
        'id' => $customer->id,
        'memo' => 'テストメモ',
    ]);
}
```

---

### CRUDテストのまとめ
| CRUD機能                 | よく使うメソッド                                                        | 内容                    |
| ---------------------- | --------------------------------------------------------------- | --------------------- |
| **一覧（Read）**           | `get()` + `assertStatus()` + `assertSee()`                      | ページが開けるか、データが表示されるか確認 |
| **詳細（Show）**           | `get("/customers/{$id}")` + `assertSee()`                       | 個別データの内容を検証           |
| **登録（Create / Store）** | `post()` + `assertDatabaseHas()`                                | データが保存されるかチェック        |
| **更新（Update）**         | `put()` or `patch()` + `assertDatabaseHas()`                    | 更新後の値がDBに反映されてるか確認    |
| **削除（Delete）**         | `delete()` + `assertSoftDeleted()` or `assertDatabaseMissing()` | 論理削除／物理削除の確認          |

---

| メソッド                                    | 目的                  |
| --------------------------------------- | ------------------- |
| `get()`, `post()`, `put()`, `delete()`  | 各HTTPリクエストをシミュレーション |
| `withHeaders()`                         | APIキーなどを付ける         |
| `postJson()`                            | JSON形式で送信（API用）     |
| `assertStatus(200)`                     | ステータスコード確認          |
| `assertRedirect()`                      | リダイレクト確認            |
| `assertSee('文字')`                       | HTML内の文字確認          |
| `assertDatabaseHas('table', [...])`     | DBに存在するか確認          |
| `assertDatabaseMissing()`               | DBに存在しないこと確認        |
| `assertSoftDeleted()`                   | 論理削除確認              |
| `assertJson()` / `assertJsonFragment()` | JSONレスポンス検証（API用）   |
