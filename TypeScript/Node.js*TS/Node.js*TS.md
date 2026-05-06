# Node.js*TS

## ① ts.config.jsonの設定
Node.js で TypeScript を使う場合、ブラウザ向け（React）とは設定が少し違う

最低限は下記でOK
```json
{
  "compilerOptions": {
    "target": "ES2020",          // Node.js が理解できる JS のバージョン
    "module": "CommonJS",        // Node.js の標準モジュール方式（後述）
    "outDir": "dist",            // 変換後のJSを出力するフォルダ
    "rootDir": "src",            // TSファイルの場所
    "esModuleInterop": true,     // import を使いやすくする
    "strict": true               // 型チェックを厳しくする
  }
}
```

## nodemon とは？
- **ファイルを保存すると自動で Node.js を再起動してくれるツール**

TypeScript と一緒に使う場合は：
```bash
npm install -D ts-node nodemon
```

nodemon.json を作る：
```json
{
  "watch": ["src"],
  "ext": "ts",
  "exec": "ts-node src/server.ts"
}
```
→ npm run dev で自動再起動しながら開発できる

## ② node.jsモジュールシステムでタイプスクリプトを使う
Node.js には **2種類のモジュール方式** がある
- 1. ES Modules（ESM）
```ts
import fs from "fs"
export const x = 1
```

- 2. CommonJS（CJS）
```ts
const fs = require("fs")
module.exports = { x: 1 }
```

### 拡張子の意味（Node.js 特有）
| 拡張子 | 意味 |
|--------|------|
| **.mjs** | ES Modules 専用 |
| **.cjs** | CommonJS 専用 |
| **.js** | package.json の `"type"` によって決まる |
| **.mts** | TypeScript の ES Modules |
| **.cts** | TypeScript の CommonJS |

### Common.jsにしたい時
tsconfig.json の module を変更：

```json
{
  "compilerOptions": {
    "module": "CommonJS"
  }
}
```

## ③ Express を使ってサーバー構築（TypeScript 版）
### Express とは？
- **Node.js でサーバーを作るための超有名フレームワーク**
- TypeScript で使うと型推論が効くので安全

### 最小の Express サーバー（TS）
```ts
import express, { Request, Response, NextFunction } from "express"

const app = express()

app.get("/", (req: Request, res: Response) => {
  res.send("Hello!")
})

app.listen(3000)
```

### next() とは？
**次のミドルウェアに処理を渡す関数**

```ts
app.use((req, res, next) => {
  console.log("ログ")
  next() // 次へ進む
})
```

### ミドルウェアとは？
- リクエスト → レスポンスの間に挟まる処理

例：
- ログ
- 認証
- バリデーション
- JSON パース

**Express の本体は「ミドルウェアを積み重ねる仕組み」**

### エラーハンドリング（Express の特殊ルール）
```ts
app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  console.error(err)
  res.status(500).json({ message: "Server Error" })
})
```
ポイント：引数が 4 つある関数だけが “エラーハンドラ” として認識される

## ④ POST Request（TypeScript 版）
```ts
app.use(express.json()) // JSON を受け取るためのミドルウェア

app.post("/user", (req: Request, res: Response) => {
  const body = req.body as { name: string; age: number }

  res.json({
    message: "受け取りました",
    data: body
  })
})
```

## まとめ
| 項目 | 覚えるべきこと |
|------|----------------|
| **tsconfig** | Node.js は `"module": "CommonJS"` が基本 |
| **nodemon** | 自動再起動ツール（ts-node とセット） |
| **ESM / CJS** | Node.js には 2 種類のモジュール方式 |
| **拡張子** | `.mjs` / `.cjs` / `.mts` / `.cts` の違い |
| **Express** | ミドルウェアの積み重ねで動く |
| **next()** | 次のミドルウェアへ進む |
| **エラーハンドラ** | 引数 4 つの関数だけが特別扱い |
| **POST** | `express.json()` が必須 |