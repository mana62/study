# ブラウザでの「Module」と「Script」の違い
## ① moduleは自動で defer
普通のScript
```js
<script src="main.js"></script>
```
👉 HTML読み込みを止める

Module
```js
<script type="module" src="main.js"></script>
```
👉 自動で defer が付く
つまり
- HTMLを全部読み込む
- そのあとJS実行
なので `document.body` も取得できる

## ② moduleはscriptタグ内コードにも defer が効く
普通のScript
```js
<script>
console.log(document.body)
</script>
```
👉 HTML読み込み前に実行
👉 nullになることがある

Module
```html
<script type="module">
console.log(document.body)
</script>
```
👉 HTML解析後に実行
👉 body取得できる

## ③ 同じmoduleは1回しか実行されない
Script
```js
<script src="main.js"></script>
<script src="main.js"></script>
```
👉 2回実行

Module
```js
<script type="module" src="main.js"></script>
<script type="module" src="main.js"></script>
```
👉 1回しか実行されない

理由：モジュールは 自動キャッシュ

## ④ moduleはCORSチェックされる
Module
```js
<script type="module" src="https://example.com/main.js"></script>
```
👉 別ドメインだとCORSチェック
👉 サーバー側に `Access-Control-Allow-Origin` が必要

普通Script
```js
<script src="https://example.com/main.js"></script>
```
👉 CORSチェックなし

## ⑤ nomodule 属性
＝ モジュールに対応していない古いブラウザ向けにスクリプトを読み込ませるための属性

```html
<script type="module" src="app.js"></script>

<script nomodule src="old.js"></script>
```
意味：
| ブラウザ    | 実行     |
| ------- | ------ |
| 新しいブラウザ | app.js |
| 古いブラウザ  | old.js |

つまり：古いブラウザ用JS

## まとめ
ブラウザでのModuleの特徴
1️⃣ 自動で defer
2️⃣ HTML解析後に実行
3️⃣ 同じファイルは1回だけ実行
4️⃣ CORSチェックあり
5️⃣ nomoduleで古いブラウザ対応

Moduleは **「安全で効率的なJavaScript読み込み方式」**

だから
- HTMLブロックしない
- 重複実行しない
- import/export使える