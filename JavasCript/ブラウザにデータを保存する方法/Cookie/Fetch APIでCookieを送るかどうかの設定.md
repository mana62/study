# Fetch APIでCookieを送るかどうかの設定とCORSの追加ルール

## Fetchでは「Cookieを送るか」を設定できる
👉 fetch() の `credentials` オプションで制御できる

### ① `omit`
```js
fetch(url, { credentials: "omit" })
```
- Cookieを絶対に送らない
- 同じサイトでも送らない
👉 完全にCookieなし通信

### ② `same-origin`（デフォルト）
```js
fetch(url, { credentials: "same-origin" })
```
- 同じオリジン → Cookie送る
- 別オリジン → Cookie送らない
👉 Fetchのデフォルト設定

### ③ include
```js
fetch(url, { credentials: "include" })
```
- 別オリジンでもCookie送る
👉 クロスサイトでもCookie付きリクエスト可能

## ただし include の場合はCORSルールが厳しくなる
👉 サーバー側が **2つのヘッダーを必ず返す**必要がある

### 必須ヘッダー①
```bash
Access-Control-Allow-Origin: https://example.com
```
❌  ***（アスタリスク）禁止**
必ず **具体的なOrigin**

### 必須ヘッダー②
```bash
Access-Control-Allow-Credentials: true
```
❌ **これが無いと エラー**
**※プリフライトでも必要**


| 設定          | Cookie           |
| ----------- | ---------------- |
| omit        | 送らない             |
| same-origin | 同じサイトだけ送る（デフォルト） |
| include     | どこでも送る           |

## includeを使うときのサーバー条件
⭕️ サーバーは必ず
- `Access-Control-Allow-Origin: https://example.com`
- `Access-Control-Allow-Credentials: true`
を返す必要あり

❌ こういうのはダメ
- `Access-Control-Allow-Origin: *`

## なぜこの仕組みがあるか
**セキュリティ（CSRF防止）**のため
勝手に他サイトへ **Cookie付きリクエストを送れないようにするため**

## まとめ
① `omit` → Cookie送らない
② `same-origin` → 同じサイトだけ送る（デフォルト）
③ `include` → 他サイトにも送る

⚠️ ただし include の場合
サーバー側で
- Access-Control-Allow-Credentials: true
- Access-Control-Allow-Origin: 具体的Origin
が必要