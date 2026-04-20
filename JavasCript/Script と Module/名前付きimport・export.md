# 名前付きimport / export
- export = 外に公開
- import = 外から取得
- as を付けると名前を変えて import / export できる

## モジュールの基本イメージ
⚫︎通常のJS（script）
```bash
ファイルA  ←→  ファイルB
変数が見える
```
⚫︎モジュール（module）
```bash
ファイルA  | 壁 |  ファイルB
変数は見えない
```
👉 ファイルは完全に分離だから、
- 外に出すには **`export`**
- 他から使うには **`import`** が必要

## export（外に公開）
ファイルA
```js
export const red = "RED";

export function func(){
  console.log("func")
}
```
図
```bash
ファイルA
 ├ red
 └ func

     ↓ export
外部から使える
```

## import（他のファイルを取り込む）
ファイルB
```js
import { red, func } from "./a.js";

console.log(red);
func();
```
図
```bash
ファイルA              ファイルB
 red   ───────────→   red
 func  ───────────→   func
```
👉 必要なものだけ取り出す

## 名前を変えてimportできる
```js
import { red as apple } from "./a.js";

console.log(apple);
```
図
```bash
Aファイル
 red

        ↓

Bファイル
 apple
 ```
 👉 **`as`を使うことで名前変更ができる**

## 全部まとめてimport
```js
import * as data from "./a.js";
```
図
```bash
Aファイル
 red
 func
 class

      ↓

data {
 red
 func
 class
}
```
使う時
```js
data.red
data.func()
```

## exportは後からまとめてもOK
exportを後に書くことも可能
```js
const red = "RED"
function func(){}

export { red, func }
```
図
```js
定義
 red
 func

      ↓

export { red, func }
```

## exportできるもの
主に下記8種類
```js
export const
export let
export var
export function
export async function
export generator function
export async generator function
export class
```

## ⚠️ ポイント
**importした値は変更できない**

```js
import { red } from "./a.js"

red = "BLUE"   // エラー
```
👉 **importはconst扱い**

## まとめ
```bash
ファイルA（module）

export const red
export function func


        ↓ import


ファイルB

import { red, func } from "./a.js"
```

- export = 外に公開
- import = 外から取得
- as を付けると名前を変えて import / export できる


## 名前付きとデフォルトの違い
| 種類             | exportできる数 |
| -------------- | ---------- |
| 名前付きexport     | **何個でもOK（関数や配列など）** |
| default export | **1つだけ**   |

- 名前付き export → 部品（何個でも）
- default export → メイン（1個だけ）
- ※ defaultは「export default」と書くだけ `export default`