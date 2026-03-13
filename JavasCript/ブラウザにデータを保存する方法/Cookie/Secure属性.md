# Cookieの Secure（セキュア）属性

## ① Secure属性とは
👉 **Secure属性を付けたCookieは「HTTPSのときだけ使えるCookie」**

例:
```js
document.cookie = "id=123; Secure";
```
意味:
- HTTPSのページ → Cookie保存できる
- HTTPのページ → Cookie保存されない（処理が無視される）

## ② HTTPページではどうなる？
HTTPページで実行すると
```js
document.cookie = "id=123; Secure";
```
➡ Cookieは保存されない

つまり:
- HTTP → 保存不可
- HTTPS → 保存可能

## ③ Secure付きCookieはHTTPでは取得できない
もしHTTPSページで
```js
document.cookie = "id=123; Secure";
```
を保存した場合、HTTPページに移動すると`document.cookie`
➡ **id=123は取得できない**

## ④ HTTPからSecure Cookieは上書きできない
Secure付きCookie
```js
id=123; Secure
```
がある場合HTTPページで`id=999`を書こうとしても
➡ **上書きできない**

つまり: **Secure CookieはHTTPサイトから攻撃されにくい**

## ⑤ HTTPSページなら上書き可能
HTTPSページなら`id=456` などに **上書き可能**
さらに**Secure属性を外すことも可能**

## ⑥ Localhostだけ例外
通常は
```js
Secure = HTTPSのみ
```
ですが以下は例外
```bash
127.0.0.1
```
**localhost** 開発環境では **HTTPでもSecure Cookieが使える**
つまり: **開発しやすくするための例外**

## ⑦ Secure属性のメリット
Secure属性を使うと
- HTTP通信でCookieが送られない
- HTTPサイトから上書きできない

つまり: **盗聴や攻撃を防ぎやすい**

## まとめ
| 状況           | 結果      |
| ------------ | ------- |
| HTTPページ      | 保存できない  |
| HTTPSページ     | 保存できる   |
| HTTPページから取得  | 不可      |
| HTTPページから上書き | 不可      |
| localhost    | 例外で使用可能 |

👉 Secure = HTTPS専用Cookie