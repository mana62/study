# デフォルトimport / export

## デフォルトエクスポート
＝ファイルから**1つだけ値をエクスポートする方法**

```js
export default "hello";
```
- 文字列・数値・オブジェクトなど 好きな式を書ける
- **1ファイルに1つしか書けない**

理由：名前がないため、複数あるとどれをインポートするか指定できない

## デフォルトインポート
＝ デフォルトエクスポートを読み込む方法

```js
import A from "./a.js";
```
- {} は使わない
- Aの部分は、好きな名前を付けられる

## 名前付きエクスポートとの違い
**名前付き**
```js
export const red = "red";

import { red } from "./a.js";
```
- 名前が固定

**デフォルト**
```js
export default "hello";

import anyName from "./a.js";
```
- 名前を自由につけられる

## デフォルト + 名前付きの同時インポート
```js
import A, { red } from "./a.js";
```
**ポイント**
- デフォルトを先に書く
- 逆順はエラー
- Aの部分は、好きな名前を付けてOK

## `*` を使った全インポート
```js
import * as data from "./a.js";
```
**結果**
```js
data.red
data.default
```
- defaultプロパティにデフォルトエクスポートが入る

## defaultを名前付きでインポートする方法
```js
import { default as A } from "./a.js";
```

## 名前付きからデフォルトエクスポートする方法
```js
function syncFunc() {}

export { syncFunc as default };
```

## まとめ
**名前付き**
```js
export const red = "red";
import { red } from "./a.js";
```

**デフォルト**
```js
export default "hello";
import A from "./a.js";
```

**両方**
```js
import A, { red } from "./a.js";
```