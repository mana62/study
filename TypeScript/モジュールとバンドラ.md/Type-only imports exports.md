# Type-only imports/exports
**「型だけを読み込む／型だけを外に出す」ための構文**

- import type → 型だけ読み込む
- export type → 型だけ書き出す
- 実行時には消える（JS には残らない）

## なぜ必要なのか？
- TypeScript は 型はコンパイル後に消える けど、普通の import を使うと 実行時にも import が残ってしまう ことがある

すると：
- 実行時に存在しないファイルを読み込もうとしてエラーバンドルサイズが無駄に増える
- 循環参照が起きる

👉 こういう問題を避けるために **「これは型だけだよ」 と明示するのが type-only**

## 使い方
### ❌ 普通の import（型だけなのに実行時にも残る）
```ts
// 型（TypeScript の型情報）だけを読み込む import
import { User } from "./types";
```
JS に変換すると：
```js
import { User } from "./types"; // ← 実行時にも残る
```

### ⭕️ type-only import
```ts
import type { User } from "./types";
```
JS に変換すると：
```js
// 何も残らない（型なので削除される）
```
export も同じ

## いつ使うか
- 型だけを使いたいとき
- 実行時に import が不要なとき
- 循環参照を避けたいとき
- バンドルサイズを減らしたいとき

## 設定（tsconfig.json）
```bash

verbatimModuleSyntax: false（デフォルト）

# これをtrueにする
verbatim module syntax: true
```
👉 必ず型だけを指定してるファイルはtypeを書かないとエラーになる設定（モジュールがES6以外だとエラーになる）

## まとめ
- type-only imports/exports = 型だけ扱う import/export
- 実行時には消える安全な書き方
