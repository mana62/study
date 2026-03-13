# Path属性
👉 Cookieが使えるURLの範囲を制限する設定

書き方:
```js
document.cookie = "user=John; path=/items";
```
意味:
```js
/items
```
**から始まるURLだけでそのCookieが使える**

例:
```js
document.cookie = "user=John; path=/items";
```
このCookieが使えるURL:
```bash
example.com/items
example.com/items/1
example.com/items/test
```
使えないURL:
```bash
example.com/
example.com/about
```
つまり: **/itemsから始まるページだけ**

## Pathを / にすると
```js
document.cookie = "user=John; path=/";
```
意味: **サイト全部**
つまり:
```bash
example.com/
example.com/about
example.com/items
```
全部使える
- 実務ではよく`path=/`を使用

## Pathを指定しない場合（デフォルト）
例:
今のページ
```js
example.com/items/1
```
このページで
```js
document.cookie = "user=John";
```
すると内部的には、`path=/items`になる
つまり: **/items 以下でしか使えない**

### なぜそうなるか
👉 **現在のURLの最後の / より前まで がPathになる**

例:
URL: `/items/1`
デフォルトpath: `/items`

## 同じCookie名でもPathが違えば別Cookie
例:
```js
document.cookie = "user=John; path=/";
document.cookie = "user=Mike; path=/items";
```
結果:
```bash
user=John
user=Mike
```
2つ存在
つまり: **key + path でCookieが区別される**

## Cookie削除の注意
- Cookie削除するとき、**同じpathを指定しないと消えない**

例:
作成
```js
document.cookie = "user=John; path=/items";
```
削除
```js
document.cookie = "user=John; path=/items; max-age=0";
```
もし、`path=/` で削除すると削除されない

## 実務のコツ
面倒な場合は、`path=/`で書いておく

```js
document.cookie = "user=John; path=/";
```
にしておけばサイト全体で使える

## まとめ
**Path属性**
- Cookieを使えるURLの範囲

例:
```js
path=/items
```
→ **/items 以下だけ**
```js
path=/
```
→ **サイト全部**