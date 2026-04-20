# importするファイルを指定する方法

## import の "./a.js" などの右側の正体
👉 これはただの文字列じゃなくて
👉 **URL（場所）を指定している**

## 書き方ルール
### ✅ できる
```js
import { a } from "./a.js";
import { a } from "../a.js";
import { a } from "/a.js";
```

### ❌ できない
```js
import { a } from "a.js";   // NG（パスではない）
import { a } from `./a.js`; // NG（バッククォート）
import { a } from someVar;  // NG（変数）
```
👉 **固定の文字列で書く必要がある**
👉 固定の文字列とは：変わらないそのままの文字

## なぜ文字列固定
👉 JSが事前に解析するため
👉 **「どのファイルが必要か最初に全部把握するため」**

## URLとして解釈される
```js
import { a } from "./a.js";
```
👉 実際はURLとして解釈されている
```bash
https://example.com/a.js
```

## 相対パスのルール
基準になるのは **そのJSファイルの場所**

例：
```js
/main.js
/sub/a.js
```
```js
// main.js
import "./sub/a.js";
```
👉 OK

## 相対パスの記号
| 書き方   | 意味   |
| ----- | ---- |
| `./`  | 同じ階層 |
| `../` | 1つ上  |
| `/`   | ルート  |


## 重要ルール
👉 必ずこれで始める必要あり
```bash
./
../
/
```
👉 つまり下記はダメ
```js
import "./a.js"; ← OK
import "a.js";   ← ❌
```

## import.meta.url
👉 そのファイルのURLが取れる
```js
console.log(import.meta.url);
```
👉**「このJSファイルどこから読み込まれたか」**

## import は裏で何してる？
①ファイルをダウンロード
②すぐ解析
③さらにimportあれば連鎖
👉 全部並列で進む（速い）

## 同じファイルは1回だけ
```js
import "./a.js";
import "./a.js";
```
👉 **1回しかダウンロード・実行されない**

## 例外
👉 **URLが違えば別扱い**
```js
import "./a.js";
import "./a.js?x=1";
```
👉 **別ファイル扱い → 2回実行**

## import map
＝ **importのパスにあだ名（エイリアス）をつける仕組み**
```js
import { a } from "lib";
```
👉 上記は実際は下記に変換
```js
<script type="importmap">
{
  "imports": {
    "lib": "./lib.js"
  }
}
</script>
```

## まとめ
👉 import の "〜" は
- ファイルの場所（URL）を指定してる

👉 ルール
- 文字列固定のみ（変数NG）
- ./ などが必要
- 実際はURLとして解釈される

👉 挙動
- 並列で読み込まれる
- 同じURLは1回だけ実行
- URL違うと別物扱い

**「importはURLベースでファイルを読み込む仕組み」**