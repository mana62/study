# HttpOnly属性
👉 **JavaScriptからCookieを触れなくする属性**

例（サーバーが設定）:
```bash
Set-Cookie: id=123; HttpOnly
```
このCookieは:
| 操作         | 結果   |
| ---------- | ---- |
| サーバー通信     | 使える  |
| JavaScript | 使えない |

＝ **HttpOnly**が付いているため

## 何が起きるのか
ブラウザには保存される
```bash
id=123
```
そしてリクエスト時には
```bash
Cookie: id=123
```
として**サーバーへ送られる**
しかしJavaScriptでは
```js
document.cookie
```
↓
**id=123 は見えない**

## JavaScriptから操作できない
**HttpOnly Cookie**
```bash
id=123; HttpOnly
```
JSで
```bash
document.cookie = "id=999"
```
としても ➡ **無視される**

つまり
- **取得できない**
- **上書きできない**

## JavaScriptでは設定もできない
JSで
```js
document.cookie = "id=123; HttpOnly"
```
としても ➡ **Cookieは作られない**
理由: **HttpOnlyはサーバー専用だから**

## 設定できるのはサーバーだけ
サーバー:
```bash
Set-Cookie: id=123; HttpOnly
```
👉 **この方法でのみ設定できる**

## なぜこの仕組みが必要か？
目的: **XSS攻撃対策**

もしJSからCookieを読めると
```js
document.cookie
```
で
```bash
session_id=abc123
```
を盗まれる

しかしHttpOnlyなら ➡ **JavaScriptから取得できない**

## まとめ
**HttpOnly Cookie**

| 特徴      | 内容         |
| ------- | ---------- |
| JSから取得  | ❌          |
| JSから上書き | ❌          |
| サーバー通信  | 〇          |
| 設定方法    | Set-Cookie |

## Secureとの違い

| 属性       | 目的         |
| -------- | ---------- |
| Secure   | HTTPSだけ送信  |
| HttpOnly | JSからアクセス禁止 |


実際のログインCookieは
```js
Set-Cookie: session=abc; Secure; HttpOnly
```
のように **両方つけることが多い**

| 属性       | `document.cookie`で付けられる？ | `Set-Cookie`で付けられる？ |
| -------- | ------------------------ | ------------------- |
| Secure   | ✅ 付けられる                  | ✅ 付けられる             |
| HttpOnly | ❌ 付けられない                 | ✅ 付けられる             |
