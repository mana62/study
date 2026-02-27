# 外部APIテスト

### 主なテスト内容

| テスト内容  | 意味                     | 例                    |
| ------ | ---------------------- | -------------------- |
| 正常系    | ちゃんと登録できる              | 正しいJSON送ってDBに保存されるか  |
| 重複チェック | 同じデータを2回送っても1回しか登録されない | 同じexternal_idで登録しないか |
| 不正データ  | データ型が違う、必須が抜けてる        | エラー(422)を返すか         |
| 認証     | APIキーが間違ってたら拒否する       | 401や403を返すか          |

---

### 例コード
```php
<?php

namespace Tests\Feature;

use Tests\TestCase;
use Illuminate\Foundation\Testing\RefreshDatabase;

class ExternalApiTest extends TestCase
{
    use RefreshDatabase; // テスト前にDBをリセット

    public function test_正しいデータを送ると登録される()
    {
        // 外部から来るデータの例
        $data = [
            'external_id' => 'ABC123',
            'name' => '山田太郎',
            'email' => 'taro@example.com',
        ];

        // POSTで送信（APIキーもつける）
        $response = $this->withHeaders([
            'X-API-KEY' => 'test-key', // テスト環境用のダミー（フェイク）キー or モック処理を使う
        ])->postJson('/api/external/receive', $data);

        // ステータス201（登録成功）を期待
        $response->assertStatus(201);

        // DBに登録されているか確認
        $this->assertDatabaseHas('external_requests', [
            'external_id' => 'ABC123',
        ]);
    }

    public function test_不正なデータを送るとエラーになる()
    {
        $data = [
            'external_id' => '', // 空
            'email' => 'not-an-email', // メールじゃない
        ];

        $response = $this->withHeaders([
            'X-API-KEY' => 'test-key',
        ])->postJson('/api/external/receive', $data);

        // バリデーションエラーの422を期待
        $response->assertStatus(422);
    }

    public function test_APIキーが間違っていたら拒否される()
    {
        $data = [
            'external_id' => 'X1',
            'email' => 'a@b.com',
        ];

        $response = $this->withHeaders([
            'X-API-KEY' => 'wrong-key', // 間違ったキー
        ])->postJson('/api/external/receive', $data);

        $response->assertStatus(401);
    }
}
```
---

```php
public function test_api_receives_valid_json()
{
    // 1️⃣ テストデータを準備
    $data = [
        'external_id' => 'ABC123',
        'name' => '山田太郎',
        'email' => 'taro@example.com',
    ];

    // 2️⃣ ヘッダーにAPIキーをセットしてPOST送信
    $response = $this->withHeaders([
        'X-API-KEY' => 'test-key',
    ])->postJson('/api/external/receive', $data);

    // 3️⃣ 結果のアサート（確認）
    $response->assertStatus(200); // HTTPステータスが200か？
    $response->assertJson([
        'success' => true,
    ]);
}
```

---

#### withHeaders()とは？
- HTTPリクエストのヘッダーを設定するメソッド
- 本番で外部サービスが送るリクエストに近い形を再現できる

```php
$this->withHeaders([
    'X-API-KEY' => 'test-key',
    'Accept' => 'application/json',
]);
```

**使う場面**
- APIキーやトークン認証のテスト
- Content-Type や Accept ヘッダーの指定
- カスタムヘッダーの送信

---

#### postJson(), getJson(), putJson() とは？
- APIエンドポイントに「JSON形式」でリクエストを送るメソッド
- `$this->postJson('/url', $data)` は、内部的に POST /url に`Content-Type: application/json` 付きで送ってくれる

| メソッド                    | HTTP動詞 | 説明       |
| ----------------------- | ------ | -------- |
| `get()` / `getJson()`   | GET    | データ取得テスト |
| `post()` / `postJson()` | POST   | 登録・送信テスト |
| `put()` / `putJson()`   | PUT    | 更新テスト    |
| `delete()`              | DELETE | 削除テスト    |

---

### まとめ
**外部APIテストはこういう流れ**
- 1️⃣ 外部サービスが送ってくるデータを $data に準備
- 2️⃣ 必要な認証ヘッダーをつけて postJson()
- 3️⃣ ステータス・DB・JSONの内容を assert〜 で確認
- 4️⃣ 外部APIとの整合性を検証


| 概念      | Laravelのやり方                       | 備考            |
| ------- | --------------------------------- | ------------- |
| 外部API認証 | `withHeaders()` で再現               | X-API-KEY など  |
| データ送信   | `postJson()` など                   | JSON形式で送信     |
| 結果確認    | `assertStatus()` / `assertJson()` | ステータスや内容をチェック |
| DB確認    | `assertDatabaseHas()`             | 実際に登録されたか     |

---

### 参考
- [LaravelでPHPUnit入門](https://qiita.com/nakano-shingo/items/9446568a2f9e903922d4)
- [Laravel公式](https://laravel.com/docs/11.x/http-tests)
- 