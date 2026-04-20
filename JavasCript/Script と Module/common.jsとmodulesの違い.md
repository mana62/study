# CommonJSとES Modulesの違い

| 項目   | CommonJS | ES Modules |
| ---- | -------- | ---------- |
| 書き方  | require  | import     |
| 実行   | 同期       | 非同期（静的解析）  |
| 標準   | Node独自   | JS公式       |
| 今の流れ | 減ってる     | 主流         |

## どっちが扱われているかを確認する方法
### 1. package.json
```bash
{
  "type": "module"
}
```
- **"module"** → .js はESMになる
- **"commonjs" or 無し** → .js はCJSになる

### 2. 拡張子
| 拡張子    | 強制的に       |
| ------ | ---------- |
| `.cjs` | CommonJS   |
| `.mjs` | ES Modules |

👉 これは package.json無視して強制決定

## 優先順位
👉 拡張子 ＞ package.json

つまり：
- .mjs → 絶対ESM
- .cjs → 絶対CJS
- .js → package.jsonで決まる

## モジュール同士の関係
✅ ESM → CJS はOK
```js
import data from "./file.cjs";
```
👉 動く

❌ CJS → ESM はNG（requireでは無理）
```js
require("./file.mjs"); // エラー
```
理由：
- require = 同期
- import = 非同期の可能性あり
- 👉 仕組みが合わない