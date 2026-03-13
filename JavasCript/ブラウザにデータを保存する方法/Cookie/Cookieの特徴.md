# Cookieの特徴
👉 **Cookieはリクエスト時に自動でサーバーへ送られる**

例:
ブラウザにCookieがあると
```bash
Cookie: id=123; name=john
```
という **Cookieヘッダーが HTTP/HTTPSリクエストに自動で付く**

つまり:
```bash
ユーザー
↓
リクエスト送信

GET /index
Cookie: id=123

サーバー
→ Cookieを見てユーザー識別
```

## LocalStorageとの違い
| 機能        | Cookie | LocalStorage |
| --------- | ------ | ------------ |
| サーバーへ自動送信 | 〇      | ✕            |
| JSから操作    | 〇      | 〇            |
| ログイン管理    | よく使う   | ほぼ使わない       |

👉 Cookieはサーバーと通信するための保存データ

## Cookieが送られる条件
👉 **Cookieは条件に合う場合だけ送信される**
影響する属性
- Domain
- Path
- Secure
- SameSite など

例:
- Pathが違う → 送られない
- HTTPなのにSecure → 送られない
- ドメイン違う → 送られない

## ログインでCookieが使われる理由

ログイン時:
```bash
session_id=abc123
```
をCookieに保存
次回アクセス
```bash
GET /mypage
Cookie: session_id=abc123
```
サーバー `abc123 → ユーザーA` と判断できる
つまり: **Cookie = ユーザー識別**

## サーバーからCookieを保存する方法
サーバーは **Set-Cookieヘッダーを使う**

例:
```bash
Set-Cookie: name=john; Path=/
```
ブラウザはこれを受け取ると `name=john` というCookieを保存する

## Cookieの流れ
### ① サーバー
```bash
Set-Cookie: session=abc
```

### ② ブラウザ保存
```bash
session=abc
```

### ③ 次回アクセス
```bash
GET /home
Cookie: session=abc
```

### ④ サーバー
ユーザーを識別

### Cookie操作方法
**JavaScript**
保存
```js
document.cookie = "name=john"
```
取得
```js
document.cookie
```

**サーバー**
保存
```js
Set-Cookie: name=john
```
取得
```js
Cookie: name=john
```
## まとめ
**Cookieの本質**
- ① ブラウザに保存される
- ② リクエスト時に自動送信される
- ③サーバーがユーザー識別に使う

だから **ログイン機能でよく使われる**