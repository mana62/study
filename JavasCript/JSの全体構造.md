# JSの全体構造

## ① JavaScript（言語そのもの）
**JavaScript = プログラミング言語**
これは文法そのものの話

```js
let x = 10;
function add(a, b) {
  return a + b;
}
```
上記のような 文法・ルール が JavaScript
👉 でも JavaScript単体では何もできない
（ファイル操作・通信・画面操作とかは不可）

## ② 標準ビルトインオブジェクト（JavaScriptの標準装備）
**JS言語に最初から付いている道具セット**

例：
- Math
- Array
- Object
- String
- Number
- Date
- JSON
- Promise
- Map
- Set

例
```js
Math.random()
[1,2,3].map(x => x * 2)
JSON.parse(...)
```
これらは どこでも使える純正JS機能
👉 ブラウザでも Node.js でも共通

## ③ Web APIs（ブラウザが提供する機能）
**ブラウザが追加で用意してくれている機能**

代表例：
- document
- window
- fetch
- alert
- localStorage
- navigator
- setTimeout

例
```js
document.querySelector("button")
fetch("/api/data")
```

👉 これは JavaScriptの仕様ではない
👉 ブラウザという環境が提供している
だから Node.js では基本使えない（一部例外あり）
👉 web APIsはエクマスクリプトではなく、Web APIs の仕様は W3C や WHATWG が策定している

## ④ Node.js APIs（Node.js が提供する機能）
**Node.js が追加で用意している機能**

代表例：
- fs
- path
- http
- os
- process
- crypto

例
```js
const fs = require("fs");
fs.readFileSync("test.txt");
```

👉 ファイル操作・サーバー・OS操作などが可能
👉 ブラウザでは使えない

### Node.js とは
**Node.js = JavaScript を「PC上で」動かすためのソフト**
👉 Node.js は JavaScript の一種ではない
👉 JavaScript を動かす「実行環境」

```bash
JavaScript（言語）
        ↓
   Node.js（実行環境）
```
＝Node.js は JavaScript を理解して実行できるプログラム

```bash
JavaScript（言語）
     ↓
ブラウザ or Node.js
```

| 実行場所     | 何がJSを動かす？                 |
| -------- | ------------------------- |
| ブラウザ     | Chrome / Safari / Firefox |
| PC / サーバ | Node.js                   |

### node.js とブラウザの違い
| できること           | ブラウザ | Node.js |
| --------------- | ---- | ------- |
| 画面操作（DOM）       | ✅    | ❌       |
| alert / confirm | ✅    | ❌       |
| ファイル読み書き        | ❌    | ✅       |
| サーバー構築          | ❌    | ✅       |
| OS情報取得          | ❌    | ✅       |
| DB接続            | 制限あり | ✅       |
| ネット通信           | ✅    | ✅       |


## まとめ

```bash
全体関係図（これが一番重要）
          JavaScript（言語）
                |
    ┌───────────┴───────────┐
    |                       |
標準ビルトイン         実行環境のAPI
   オブジェクト      （環境ごとに違う）
    |               |
    |        ┌──────┴──────┐
    |     Web APIs     Node.js APIs
    |    (ブラウザ)       (Node)
```

| 名前            | 正体           |
| ------------- | ------------ |
| JavaScript    | 言語そのもの       |
| 標準ビルトインオブジェクト | JSの標準装備      |
| Web APIs      | ブラウザ専用の機能    |
| Node.js APIs  | Node.js専用の機能 |


### よくある勘違い
❌ fetch() は JavaScript の機能
→ 違う → ブラウザの機能（Web API）

❌ fs は JavaScript の標準
→ 違う → Node.js の機能


⚫︎JavaScript = 日本語
⚫︎標準ビルトイン = 助詞・文法
⚫︎Web APIs = スマホ用アプリ
⚫︎Node.js APIs = パソコン用ソフト
👉 同じ日本語でも スマホとPCで使える機能が違う 感じ


- ECMAScript → JavaScript の言語仕様（変数、関数、クラス、Promise など）
- Web APIs → ブラウザが提供する追加機能（DOM、Canvas、Storage、Audio など）