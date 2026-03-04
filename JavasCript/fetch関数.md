# fetch関数
＝サーバーと通信してデータを取りに行くための、JavaScript 標準の非同期 API

## fetch の基本構造
```js
fetch(url, options)
```
- 第1引数：アクセス先の URL
- 第2引数：リクエストの設定（任意）

## 第2引数（options）で指定できる主な項目
### ①method（HTTP メソッド）
```js
method: 'POST'
```
👉 GET / POST / PUT / DELETE などを指定

### ②headers（ヘッダー）
```js
headers: {
  'Content-Type': 'application/json',
  'Authorization': 'Bearer token'
}
```
👉 `Content-Type`: このデータは何の種類なのかを伝えるためのもの
👉 サーバーに送る追加情報
👉 特に **JSON を送るときは Content-Type が必須**（bodyがある時も必須）
👉 **FormData の場合**はブラウザが
`Content-Type: multipart/form-data; boundary=xxxx`
を自動で付けるので **指定してはいけない**

### ③body（リクエストの中身）
```js
body: JSON.stringify({ name: 'Taro' })
```
- POST や PUT のときに送るデータ
- ※ **GETとHEAD のときは body を送れない**

### ④credentials（Cookie を送るかどうか）
```js
credentials: 'include'
```
- 'omit'（デフォルト）…Cookie を送らない
- 'same-origin'…同一ドメインなら送る
- 'include'…クロスドメインでも送る
👉 **Laravel の Sanctum などで必要になる**

### ⑤mode（CORS 制御）
```js
mode: 'cors'
```
- 'cors'（デフォルト）
- 'no-cors'
- 'same-origin'
👉 CORS の挙動を制御する

### ⑥cache（キャッシュ制御）
```js
cache: 'no-cache'
```
- 'default'
- 'no-cache'
- 'reload'
- 'force-cache' など

### ⑦redirect（リダイレクトの扱い）
```js
redirect: 'follow'
```
- 'follow'（自動で追う）
- 'error'（リダイレクトならエラー）
- 'manual'（自分で制御）

### ⑧signal（AbortController で中断）
```js
const controller = new AbortController();
fetch(url, { signal: controller.signal });
controller.abort();
```
👉 通信を途中でキャンセルできる


## POST で JSON を送る例
```js
fetch('/api/news', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json' // 'Content-Type': 何の種類のデータか伝えるためのもの
  },
  body: JSON.stringify({
    title: '新しいニュース',
    content: '内容です'
  })
});
```

## fetch の第二引数が重要な理由
- HTTP メソッドを変えられる
- JSON を送れる
- Cookie を送るか制御できる
- CORS の挙動を変えられる
- 通信を中断できる
- キャッシュの扱いを変えられる
👉  **「普通の通信」から「API 通信」まで全部 fetch でできるようになる**

## fetch の第二引数（options）一覧（主要なもの）
**通信の基本設定**
- method — GET / POST / PUT / DELETE
- headers — Content-Type など
- body — JSON / FormData / text
- mode — cors / no-cors / same-origin
- credentials — include / same-origin / omit
- cache — default / no-cache / reload
- redirect — follow / error / manual
- referrer — no-referrer など
- referrerPolicy — strict-origin-when-cross-origin など
- integrity — Subresource Integrity
- keepalive — ページ遷移中でも送信継続
- signal — AbortController で中断

## fetch の役割
- サーバーにリクエストを送る
- レスポンスを Promise で受け取る
- JSON やテキストを取得する
- 非同期で動く（メインスレッドを止めない）
👉 **「ネットワーク通信を簡単に書ける仕組み」**

## 基本の使い方
```js
fetch('/api/users')
  .then(response => response.json())
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error('エラー:', error);
  });
```
①fetch() がサーバーにリクエストを送る
②レスポンスが返ってくると Promise が解決される
③`response.json()` で JSON に変換
④data にサーバーのデータが入る

## async/awaitを使ったバージョン
```js
async function loadUsers() {
  try {
    const response = await fetch('/api/users');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('エラー:', error);
  }
}
```

## fetch が便利な理由
- Promise ベースで扱いやすい
- XMLHttpRequest よりシンプル
- 標準 APIなので追加ライブラリ不要
- JSON を簡単に扱える
- エラーハンドリングが明確

## fetch の使いどころ
- API からデータを取得
- フォーム送信（Ajax 的な使い方）
- ファイルアップロード
- JSON を使った SPA の通信
- Laravel の API と連携

## まとめ
- fetch は 非同期でサーバーと通信するための標準 API
- Promise ベースで扱いやすい
- JSON を簡単に扱える
- async/await と相性が良い
- エラー処理は response.ok を自分でチェックする必要がある