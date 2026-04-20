# Node.jsでファイル分けする仕組み



## Node.jsには2種類ある
| 種類              | 書き方                        |
| --------------- | -------------------------- |
| **ES Modules**  | `import / export`          |
| **CommonJS（昔）** | `require / module.exports` |

- require → 読み込み
- module.exports → 書き出し
＝ Node.jsは昔これを使ってた

## CommonJSとは？
👉 Node.jsが昔から使ってる独自の分割方法
```js
// 読み込み
const data = require('./a.js');

// 書き出し
module.exports = 'hello';
```

## 基本の使い方
▶ 書き出す側
```js
// a.js
module.exports = 'hello';
```
▶ 読み込む側
```js
// main.js
const data = require('./a.js');

console.log(data); // 'hello'
```
👉 requireの戻り値 = module.exports

▶ 複数データ出したいとき
```js
// a.js
module.exports = {
  name: 'taro',
  age: 20
};
```
➡️ CommonJSは、常に1つの値しかexportできない（オブジェクトでまとめる）

## exports と module.exports の違い
最初は同じ
```js
exports === module.exports // true
```

❌ でもこうすると壊れる
```js
exports = { name: 'taro' }; // ダメ
```
👉 無効になる（exportされない）

✅ 正しい使い方
```js
exports.name = 'taro';  // OK
```
または
```js
module.exports = { name: 'taro' }; // 一番安全
```

## 結論
```js
module.exports = ...
```
👉 これを使うのが一番いい

## Node.jsの裏の仕組み
各ファイルはこうなってる
```js
(function (exports, require, module, __filename, __dirname) {
  // あなたのコード
});
```
つまり：
- ファイルは関数で包まれてる
- 外から直接変数見えない

✅ グローバル変数でも外に漏れない
```js
// a.js
var x = 10;
```
👉 他のファイルから見えない
👉 ファイルごとにスコープがある

## 使える特別な変数
CommonJSでは最初から使える👇
- require 👉 ファイル読み込み
- module.exports 👉 外に出す
- exports 👉 module.exportsの省略
- __filename 👉 ファイルのパス
- __dirname 👉 フォルダのパス

## ES Modulesとの違い
|                 | CommonJS       | ES Modules |
| --------------- | -------------- | ---------- |
| 読み込み            | require        | import     |
| 書き出し            | module.exports | export     |
| 実行              | 同期             | 非同期        |
| strict mode     | ❌              | ✅          |
| top-level await | ❌              | ✅          |


## まとめ
- require() → 読み込み
- module.exports → 書き出し
- 1つの値しかexportできない
- exports = はダメ（罠）
- ファイルは関数で囲まれてる（スコープ独立）

👉 ES Modules → 今の標準
👉 CommonJS → Nodeの昔のやり方