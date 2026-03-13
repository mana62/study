## Cookieには「属性」がある
- Cookieは `key=value` だけではなく **追加設定（属性）** を付けられる

書き方:
```js
document.cookie = "name=John; 属性=値";
```

例:
```js
document.cookie = "name=John; max-age=10";
```

## max-age（寿命）
**Cookieが何秒後に消えるか**

例:
```js
document.cookie = "name=John; max-age=3";
```
意味: **3秒後にCookie削除**

## expires（期限）
**いつ消えるかを日時で指定**

例:
```js
document.cookie = "name=John; expires=Wed, 12 Mar 2026 12:00:00 GMT";
```
意味: **この日時に削除**

## max-age と expires の違い
| 属性      | 指定方法   |
| ------- | ------ |
| max-age | ○秒後    |
| expires | ○年○月○日 |

## 両方書いた場合
```js
document.cookie = "name=John; max-age=3; expires=明日";
```
この場合 **max-ageが優先** つまり **3秒後に削除**

## Cookieの削除方法
- Cookieは普通の削除APIがない
- なので **期限を過去にする**

```js
document.cookie = "name=John; max-age=0";
```
または
```js
document.cookie = "name=John; expires=Thu, 01 Jan 1970 00:00:00 GMT";
```
すると、**Cookie削除**

## Cookieのデフォルト寿命
何も指定しない場合:
```js
document.cookie = "name=John";
```
このCookieは **ブラウザを閉じたら消える**
👉 つまり **セッションCookie**

## Cookie属性は複数書ける
例:
```js
document.cookie = "name=John; max-age=10; path=/";
```
のように、`;（セミコロン）`で繋げる

## 属性は大文字小文字関係ない
- 下記全部OK
```bash
max-age
MAX-AGE
Max-Age
```

## LocalStorageとの違い
|      | LocalStorage | Cookie     |
| ---- | ------------ | ---------- |
| 寿命   | 永久           | ブラウザ終了で消える |
| 期限指定 | できない         | できる        |
| 削除   | API          | 期限変更       |


Originとは:
**scheme + host + port**

例:
**https + example.com + 443**

## まとめ
- 追加設定（属性）

主な属性:
| 属性      | 意味     |
| ------- | ------ |
| max-age | 何秒後に削除 |
| expires | 何日に削除  |

- 削除方法: `max-age=0`