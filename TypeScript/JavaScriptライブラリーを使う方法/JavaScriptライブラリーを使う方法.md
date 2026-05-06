# TypeScriptでJavaScriptライブラリを使う方法
- TypeScriptでもJavaScriptライブラリは普通に使える

## サードパーティライブラリとは？
- **自分で作ったコードではなく、他人が作って公開している便利ツール集**

## サードパーティ性種類
```bash
npm install axios
npm install lodash
npm install dayjs
npm install chart.js
npm install swiper
```

| ライブラリ    | 用途       |
| -------- | -------- |
| axios    | API通信    |
| lodash   | 配列・便利関数集 |
| dayjs    | 日付操作     |
| chart.js | グラフ作成    |
| swiper   | スライダー    |

👉 多くは JavaScript製ライブラリ

## TypeScriptでそのまま使えるのか？
- 使える場合も多い
- 最近の人気ライブラリは最初から TypeScript 対応済み

例：
```bash
import axios from 'axios'
```
- そのまま使える
- 補完も出る
- エラー検知もしてくれる

## なぜ使えるのか？
- それは 型定義ファイル があるから

## .d.ts とは？
```bash
index.d.ts
```
- このライブラリにはこういう関数がある
- 引数はこれです
- 戻り値はこれです
と TypeScript に教える説明書


**イメージ**
```bash
declare function hello(name: string): void;
```
これは
- hello関数がある
- stringを受け取る
- 戻り値なし
という意味

## tsconfig.json の declaration: true
```bash
{
  "compilerOptions": {
    "declaration": true
  }
}
```
- これを設定すると、TypeScriptファイルから .d.ts を自動生成できる

### いつ使うのか？
- 普通の学習ではあまり使わない

主に：
- ライブラリを自作して公開するとき
- 他人に使ってもらうパッケージ作成時
に使う

## DefinitelyTyped@types
- **JavaScriptライブラリに型定義が無い場合、世界中の人が作った型定義を使える**

👉 それが：**@types/ライブラリ名**

例：
```bash
npm install jquery
npm install -D @types/jquery
```
これで jQuery が TypeScript対応する

## どうやって使うのか？
### ① 型定義付きライブラリ
```bash
import axios from 'axios'
```
- そのまま使える

### ② 型定義を別で入れる
```bash
npm install jquery
npm install -D @types/jquery
```

- TypeScript向け型情報を追加

### ③ 型なしJSライブラリも使える
```bash
declare const SomeLib: any;
```
- 無理やり使うことも可能

## declare
```bash
declare const SomeLib: any;
```
- declare は、
**実体は別の場所にあるけど、存在するとみなしてね**
と TypeScript に伝える書き方

実際の例：
CDNで読み込んだJS：
```js
<script src="some-lib.js"></script>
```
このとき TypeScript は SomeLib を知らないので、
`declare const SomeLib: any;`と書く

## アンビエント宣言とは？
- **declare を使った宣言の総称**

例：
```bash
declare const foo: string;
declare function hello(): void;
```
- 実務ではあまり使わない

## アンビエントモジュール宣言
- **型のない npm パッケージに型を付ける方法**

```ts
declare module "lodash" {
  export function shuffle<T>(array: T[]): T[];
}
```
- 意味：lodash には shuffle 関数がある
- 実務ではあまり使わない

## モジュール拡張
- **既存ライブラリの型に追加する方法**

例えば：
```ts
declare module "axios" {
  export const apple: string;
}
```
- 既存の axios 型に追加
- 上級者向け

## トリプルディッシュディレクティブ
- **昔の TypeScript の書き方**

```ts
/// <reference path="./types.d.ts" />
```
- 昔は型ファイル読み込みに使っていた
- 今は tsconfig や import が主流

## まとめ
- TypeScriptでライブラリ使ってエラーが出たら：
#### ① まず普通にインストール
```bash
npm install lodash
```
#### ② ダメなら @types
```bash
npm install -D @types/lodash
```
#### ③ それでも無ければ any
```bash
declare module "lodash";
```
#### ④ 本当に必要なら型を書く