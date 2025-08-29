# laravelのモックについて
➡️ 本物の代わりに、テスト用のニセモノを使うこと
(実際のサービス（メール送信、API通信など）を使うと、時間がかかったり、失敗したりすることがある。そこで「モック」を使って、安全で速くて確実なテストをする。)
➡️ 目的は、外部依存を排除して、テスト対象のロジックだけを純粋に検証すること

---

## モックを使う場面
**① 外部サービスを使っているとき（API・メール・通知など）**
例：天気予報APIを呼び出す処理があるとき → 本番のAPIを毎回呼ぶと遅いし、料金がかかる。 → モックで「晴れです」って返すようにすれば、テストが速くて安全

```bash
Http::fake([
    'weather.example.com/*' => Http::response(['forecast' => 'sunny'], 200),
]);
```

**② メールや通知を送る処理があるとき**
- 例：ユーザー登録後に確認メールを送る処理 → 本当にメール送っちゃうと迷惑 → モックで「送ったことにする」だけでOK

```bash
Mail::fake();

$this->post('/register', [...]);

Mail::assertSent(WelcomeMail::class);
```

**③ 自作のサービスクラスがあるとき**
例：PaymentService で決済処理をしている → テストで本当にお金を動かしたくない → モックで「成功した」と返す

```bash
$this->mock(PaymentService::class, function ($mock) {
    $mock->shouldReceive('charge')->once()->andReturn(true);
});
```

**④ テスト対象のロジックだけに集中したいとき**
例：コントローラーが複雑で、外部依存が多い → モックで外部の動きを固定すれば、コントローラーのロジックだけをテストできる

---

| 状況                               | モックを使う理由                 |
| ---------------------------------- | -------------------------------- |
| 外部APIを呼ぶ                      | 通信を省略して速く・安定させる   |
| メール・通知を送る                 | 実際に送らず、送信確認だけする   |
| 決済・ファイル操作など副作用がある | 本番処理を避けて安全にテストする |
| 複雑な依存がある                   |

---

## モックを使ってるか見分けるポイント
**①::fake() を使ってるかどうか**
```bash
Mail::fake();
Notification::fake();
Queue::fake();
```
→ これがあったら本物じゃなくてモック使ってる

---

**② mock() や partialMock() を使ってるかどうか**

```bash
$this->mock(SomeService::class, function ($mock) {
    $mock->shouldReceive('run')->andReturn('mocked result');
});
```
→ mock() が出てきたら、クラスをモックしてる

**③ Http::fake() を使ってるかどうか**
```bash
Http::fake([
    'api.example.com/*' => Http::response(['data' => 'mocked'], 200),
]);
```
→ 外部APIの呼び出しをモックしてる

**④ shouldReceive() があるかどうか**
```bash
$mock->shouldReceive('methodName')->andReturn('value');
```
→ モックの振る舞いを設定してる部分

---

| キーワード        | 意味・目的                                 |
| ----------------- | ------------------------------------------ |
| `::fake()`        | Facadeをモック化（Mail, Queueなど）        |
| `mock()`          | クラス全体をモック化                       |
| `partialMock()`   | クラスの一部だけをモック化                 |
| `Http::fake()`    | HTTP通信（外部APIなど）をモック化          |
| `shouldReceive()` | モックの振る舞いを定義（返す値などを指定） |
