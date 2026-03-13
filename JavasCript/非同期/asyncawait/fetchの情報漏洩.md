# fetchの情報漏洩問題（CORS）
👉 **CORSは「別ORIGINアクセスをサーバーが許可するかどうかの仕組み」**

## ① 昔はJSで自由にHTTP通信できなかった
昔のブラウザでは
`<img>`
`<script>`
`<form>`
くらいしか通信できなかった

つまり
JavaScriptから自由にHTTPリクエストはできなかった

## ② fetch が登場して問題が出た

fetch() によって
`fetch("https://example.com")`

みたいに
JSから自由に通信できるようになった

👉すると セキュリティ問題が発生する

## ③ 問題① 情報漏洩

例👇
会社の社内サイト
`https://internal.com`

社員のPCからだけアクセス可能

悪いサイト
`https://trap.com`

社員がこれを開く
すると trap.com のJSが
```js
fetch("https://internal.com")
```

を実行できてしまう。

すると

1 社員PCからアクセス
2 社内サイトが情報を返す
3 JSがその情報を trap.com に送る

👉 会社の秘密情報が盗まれる

## ④ これを防ぐ仕組み → CORS
正式名
**CORS**
= Cross Origin Resource Sharing

これは
**ブラウザのセキュリティ機能**
（HTTPの仕様ではない）

## ⑤ ORIGINとは
ORIGIN = URLのこの部分
`https://example.com:443`

構成
**scheme + host + port**

例
https://google.com
https://api.example.com

これが違うと
別ORIGIN

## ⑥ CORSが発動する条件
CORSは
違うORIGINにアクセスしたときだけ

例
```bash
https://trap.com → https://internal.com
```
👉 CORS発動

同じサイトなら
```bash
https://google.com → https://google.com
```
👉 CORSなし

## ⑦ CORSの仕組み
### ① ブラウザがリクエスト送る

自動でこのヘッダーがつく
**Origin: https://trap.com**

つまり **「このサイトから来ました」**とサーバーに伝える

### ② サーバーがレスポンス返す

もし許可するなら
`Access-Control-Allow-Origin: https://trap.com`

または
`Access-Control-Allow-Origin: *`

### ③ 許可がない場合

ブラウザが
❌ レスポンスをJSに渡さない

つまり
```js
fetch() → reject
```
になる

## ⑧ つまりCORSのルール

違うORIGINでfetchする場合
サーバーに
`Access-Control-Allow-Origin`

がないとJSはレスポンスを読めない

## ⑨ ヘッダーも制限される

違うORIGINの場合
JSから見れるヘッダーは
一部だけ
全部見たい場合

サーバー側で
```js
Access-Control-Expose-Headers
```
を設定する

## まとめ
**fetchのセキュリティ**

問題
👉 他サイトのデータ盗める可能性

対策
👉 CORS

CORSルール
👉 違うORIGINにfetchすると

・ブラウザが**Origin: 元のサイト**を送る
・サーバーが**Access-Control-Allow-Origin**を返さないと ❌ JSはレスポンスを読めない


