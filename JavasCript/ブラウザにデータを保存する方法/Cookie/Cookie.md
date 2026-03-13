# Cookie
👉 ブラウザにデータを保存する仕組み

特徴:
- ブラウザに保存される
- サイトがよく使う
- キーと値で保存する

## Cookieの保存方法
JavaScriptで保存
```js
document.cookie = "name=John";
```
これで
```js
key → name
value → John
```
のCookieが保存される

## Cookieは複数保存できる
例:
```js
document.cookie = "name=John";
document.cookie = "id=123";
```
保存結果:
```js
name=John
id=123
```

## Cookieはブラウザを閉じても残る
さらに
- 別タブでも共有される
- 同じサイトなら使える

## Cookieの取得
```js
document.cookie
```
で取得すると、下記のような文字列になる
```js
"id=123; name=Smith"
```
つまり **全部が1つの文字列で返る**

## 個別の値を取るには自分で分解する
例:
```js
document.cookie.split("; ")
```
すると
```js
["id=123","name=Smith"]
```
になる
```js
"id=123".split("=")
```
すると
```js
["id","123"]
```
になる。つまり、`key` `value` に分けられる

## Cookieは「ホスト単位」で共有される
- Cookieは **ホスト** で共有される

例:
同じホスト
```bash
127.0.0.1:8080
127.0.0.1:5500
```
ポートは違うけど 👉 **Cookieは共有される**

## でもホストが違うと共有されない

例:
```bash
127.0.0.1
192.168.16.16
```
これは ❌ Cookie共有されない


## LocalStorageとの違い
|      | LocalStorage | Cookie          |
| ---- | ------------ | --------------- |
| 共有単位 | Origin       | Host            |
| 保存方法 | API          | document.cookie |
| 取得   | 簡単           | 面倒              |


## Cookieが動かない原因
- ローカルファイルでは動かない

```bash
file://
```
なので `http://` などの **サーバー経由で開く必要がある**

## まとめ
Cookieは
- ブラウザにデータ保存
- key=value形式
- document.cookieで操作
- 同じホストで共有
- 値取得は面倒