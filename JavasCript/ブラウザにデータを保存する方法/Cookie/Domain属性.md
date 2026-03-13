# Domain属性
＝ **Cookieをサブドメインにも共有するための設定**

書き方:
```js
document.cookie = "name=John; domain=mozilla.org";
```

## 「ドメイン」と「サブドメイン」
例:
```js
// URL
developer.mozilla.org
```
構造:
```js
developer  ← サブドメイン
mozilla    ← ドメイン
org        ← トップレベルドメイン
```
つまり、`developer.mozilla.org`は`mozilla.org`の **サブドメイン**

## Domain属性を使わない場合（デフォルト）
- Cookieは **そのホストだけ** で使える

例:
```js
developer.mozilla.org
```
で作ったCookie
```js
document.cookie = "name=John";
```
このCookieは
```js
developer.mozilla.org
```
だけで使える

## Domain属性を指定すると
例:
```js
document.cookie = "name=John; domain=mozilla.org";
```
するとこのCookieは、
```bash
mozilla.org
developer.mozilla.org
docs.mozilla.org
blog.mozilla.org
```
で共有される
つまり、**サブドメイン全部で共有される**

## Domainに書ける値
書けるのは
- 今のドメイン
-  親ドメイン

例:
現在
```js
developer.mozilla.org
```
書けるもの
```js
developer.mozilla.org
mozilla.org
```
書けない
```js
google.com
org
com
```

## なぜ書けないか
例えば `domain=com` が許されたら世界中のサイトのCookieを操作できてしまう
👉 なので **セキュリティで禁止されている**

## Domainの先頭の「.」
```js
domain=.mozilla.org
```
と書いても
```js
domain=mozilla.org
```
と 同じ意味
昔の仕様の名残

## Domainが違うと別Cookie
例:
```js
document.cookie = "name=John";
document.cookie = "name=John; domain=mozilla.org";
```
これは **別Cookie**として保存される

## Cookieは何で区別される？
- Cookieは下記3つで区別される
  - name
  - domain
  - path

つまり:
- name=John
- domain=mozilla.org
- path=/

と
- name=John
- domain=developer.mozilla.org
- path=/

は 別Cookie

## 削除するときの注意
削除するときは
**domainとpathを同じにする必要がある**

例:
作成
```js
document.cookie = "name=John; domain=mozilla.org; path=/";
```
削除
```js
document.cookie = "name=John; domain=mozilla.org; path=/; max-age=0";
```

## まとめ
**Domain属性**
- Cookieの共有範囲（ドメイン）を広げる

例:
```js
domain=mozilla.org
```
意味:
```bash
mozilla.org
+
そのサブドメイン全部
```

## LocalStorageとの違い
|          | LocalStorage | Cookie |
| -------- | ------------ | ------ |
| 共有単位     | origin       | domain |
| サブドメイン共有 | できない         | できる    |
