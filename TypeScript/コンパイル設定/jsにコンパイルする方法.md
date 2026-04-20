# JSにコンパイルする方法
## ①タイプスクリプトのファイルを作成
- TSの拡張子は、**ts**
- 例：index.ts

## ②例えば下記のように書く

```ts
let hello: string = 'hello';
console.log(hello);
```

## ③JSにコンパイル
ターミナルで `tsc index.ts` と入力すると、`すると index.js` が生成される（`index.ts` はファイル名を入れる）

## ④Node.js で実行、ターミナルで `node index.js` を実行すると、コンソールに hello が表示される


### 補足①：tsc は TypeScript Compiler の略
- TypeScript を JS に変換するツール

### 補足②：毎回 tsc index.ts と打つのが面倒なら
`tsc --watch` を使うと自動コンパイルできる

```bash
tsc --watch
```
- 保存するたびに自動で JS が生成される

### 補足③：プロジェクトでは tsconfig.json を使う
本格的に TS を使うときは：

```bash
tsc --init
```
で設定ファイルを作るのが一般的

### 補足④: --targetオプション
TypeScript をコンパイルするときに、**どのバージョンの JavaScript に変換するか**を指定するオプション

例:
```bash
# ES2015（= ES6） にコンパイルする
tsc car.ts --target ES2015
```

#### --target で指定できる主な値
| 値 | 意味 |
|------|------|
| ES3 | かなり古いブラウザ向け |
| ES5 | IE11 など古い環境向け |
| ES2015 | ES6（class, let, const など） |
| ES2016〜ES2022 | 新しい JS 機能を使いたいとき |
| ESNext | 最新の JS にそのまま変換 |

[参考](https://compat-table.github.io/compat-table/es6/)
= **上の%** : どれくらい変換できるかを表す
= **ステージ** : TC39 の提案プロセスのことで、
Stage 4 が仕様書に載る段階

#### TC39 提案ステージ一覧

| ステージ | 意味 |
|---------|------|
| Stage 0 | アイデア段階 |
| Stage 1 | 提案として認められた |
| Stage 2 | 仕様として形が固まってきた |
| Stage 3 | ほぼ確定、実装が始まる |
| Stage 4 | 仕様に正式採用（次の ECMAScript に入る） |

👉 Stage 4 が **仕様書に載る**
👉 **Stage が高いほど実装に近い**