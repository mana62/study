# fetchのデータ改ざん（プリフライトリクエスト）
👉 危険そうなリクエストは、ブラウザが先に「送っていい？」と確認す ＝ それ **OPTIONS**
👉 GET/POST → そのまま送る
👉 PUT/DELETE → OPTIONS確認してから送る

**例（社内サーバー）**
・会社の内部API
`https://internal.com`

**できること**
- POST   → データ追加
- PUT    → データ更新
- DELETE → データ削除

**攻撃の流れ**
- 社員が悪いサイトを開く
`https://trap.com`

- そのサイトのJS
```js
fetch("https://internal.com/user", {
  method: "DELETE"
})
```
- すると
```js
社員PC → internal.com
DELETEリクエスト
```
になってしまう

つまり **社内DB削除** できてしまう

**⚠️ でもCORSだけでは防げない**

そこで登場する仕組み

## プリフライトリクエスト
ブラウザはこうする👇
いきなりDELETEを送らない

まず「送っていい？」と確認する

```bash
ブラウザ
   │
   │ OPTIONS (確認)
   │
   ▼
サーバー
```
👉 この確認を **プリフライトリクエスト** という

### 実際の流れ
#### ① 確認リクエスト

ブラウザ
```bash
OPTIONS /user
```
ヘッダー
```bash
Origin: https://trap.com
Access-Control-Request-Method: DELETE
```
意味
`trap.com`から DELETEしていい？

#### ② サーバーが許可

サーバーが
```bash
Access-Control-Allow-Origin: https://trap.com
Access-Control-Allow-Methods: DELETE
```
返す

③ 本物のリクエスト

やっと送る
```bash
DELETE /user
```

```bash
①確認
Browser
   │
   │ OPTIONS
   │ "DELETEしていい？"
   ▼
Server

②許可
Server
   │
   │ OK
   │ Allow-Methods: DELETE
   ▼
Browser

③本番
Browser
   │
   │ PUT / DELETE
   ▼
Server
```

## いつプリフライトが起きる？
### ① 特殊メソッド
```bash
PUT
DELETE
PATCH
```

### ② 特殊ヘッダー
```bash
Authorization
Content-Type: application/json
```

## プリフライトが起きないリクエスト

これを **シンプルリクエスト** という

```bash
GET
POST
HEAD
```
かつ
**ヘッダーが普通のものだけ**

```bash
Browser
   │
   │ GET
   ▼
Server
```

## まとめ
**fetchのセキュリティは2つ**

| 問題      | 対策     |
| ------- | ------ |
| データ盗まれる | CORS   |
| データ改ざん  | プリフライト |