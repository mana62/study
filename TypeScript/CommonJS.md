# CommonJS

## まずモジュールとは？
**ファイルごとに機能を分けて、他ファイルから読み込める仕組み**

## CommonJS とは？
**Node.jsで昔から使われていたファイル読み込み方式**

```js
// 書き方
// 読み込み
const math = require('./math')

// 公開
module.exports = ...
```

## ① require = 読み込む
```js
const x = require('./file')
```
- これは今の import とほぼ同じ意味

```js
import x from './file.js'
```
**つまり、他ファイルの機能を使う**

## ② module.exports = 外に渡す
```js
module.exports = "hello"
```
別ファイルで：

```js
const x = require('./file')
console.log(x)
```
結果：hello

**つまり、require() したら module.exports の中身が返ってくる**

### ③ 古いNode.jsで使われていた方式

今主流はこれ↓
```js
import ...
export ...
```
なので、
- 新規開発 → ES Modules
- 古いコード読む時 → CommonJS

👉 **今主流は ES Modules**

```js
// 書き方
import x from './x.js'
export default ...
export const add = ...
```

## tsconfig.json設定
```bash
esModuleInterop: true
```
上記の設定を `true` にすることで、このライブラリ古い形式だけど、`import express from "express" `で使えるようにしてあげるね

つまり：
**古いCommonJSライブラリを、新しいES Modules風に使える変換機能**

・**Babel（バベル）** = 新しいJavaScriptを、古い環境でも動くJavaScriptに変換するツール