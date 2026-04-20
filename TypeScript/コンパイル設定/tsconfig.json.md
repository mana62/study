# プロジェクト全体を一気にコンパイルしたいなら **tsconfig.json** が必要
**tsconfig.json** : TypeScript がどんな環境で動く JavaScript を作るか設計書
- どんな機能を使える前提でコンパイルする？
- どんな補助ファイルを作る？
- どんなエラーを出す？ を指定している

## tsconfig.json を作る方法
```bash
tsc --init
```
＝ これでプロジェクトの設定ファイルができる

tsconfig.json があると

```bash
tsc
```
と打つだけで **全ての .ts ファイルを一気にコンパイル**できる

## 不要なファイルを作らせない設定
- tsconfig.json の中にある下記設定を **コメントアウトすると、余計な .map や .d.ts が作られない**

```ts
"sourceMap": true,
"declaration": true,
"declarationMap": true,
```

## ブラウザで TypeScript を動かす設定
- ブラウザは **TypeScript を理解できないので、ES Modules として読み込めるようにする必要がある**

### ① module を esnext に変更
```ts
"module": "esnext",
```

### ② 下の方の moduleDetection をコメントアウト
```ts
// "moduleDetection": "force",
```

👉 これでブラウザで **import/export が使える JS が生成される**

## includes / excludes / files の違い
### ⚫︎excludes（除外するファイル）

```ts
"exclude": ["car.ts"]
```
- このファイルはコンパイルしないという意味になる
- `**` を使うとディレクトリごと除外できる
- exclude を書くと デフォルトの node_modules（デフォルトで除外されており、基本は除外） 除外が消えるので、
明示的に書く必要がある

例：
```ts
"exclude": ["node_modules", "dist"]
```

### ⚫︎include（コンパイル対象を指定）
```ts
"include": ["src/**/*"]
```
- 何も書かなければ 全部の .ts が対象
- ワイルドカードが使える

### ⚫︎files（特定のファイルだけ）
```ts
"files": ["index.ts", "app.ts"]
```
- ワイルドカード不可
- ディレクトリ指定不可
- 本当に **このファイルだけ** をコンパイルしたいとき用

## tsconfig.json のコンパイラオプション
### ①target
- どのバージョンの JavaScript に変換するか
- デフォルトは ES5（古い JS）

```josn
"target": "ES6"
```

### ② lib
- どの型定義を使うか
- ES6 の機能（Promise, Map, Set など）
- DOM の機能（document, window など） を 使える前提で型チェックするという意味

```json
"lib": ["ES6", "DOM"]
```

もし "DOM" を入れなかったら

```ts
document.querySelector("div")
```
→ TS が「document って何？」と怒る（DOM の型定義が読み込まれてないから）

### ③allowJs: true
- JavaScript ファイルもコンパイル対象にする設定
- つまり、`.ts` だけじゃなく`.js` もコンパイル対象に入れる
- プロジェクトに JS と TS が混ざってるときに便利

```json
"allowJs": true
```

### ④ checkJs: true
- JS ファイルにも TypeScript のエラーを出す
- ⚠️ allowJs とセットで使う必要がある（JS を対象にしないと、JS のチェックもできないから）

```json
"checkJs": true
```

### ⑤ jsx
- React を使うときに設定する
- JSX をどうコンパイルするかを指定する
- React プロジェクトで必須
- 普通の TS プロジェクトでは不要

```json
"jsx": "react-jsx"
```

### ⑥ declaration: true
- 型だけのファイル（拡張子：`.d.ts`）を作る
- ライブラリ開発者向けの設定
- npm ライブラリを作るとき、ユーザーが TypeScript で使えるようにするための型ファイル
- 型情報だけをまとめたファイル

```json
"declaration": true
```

### ⑦ declarationMap: true
- `.d.ts` と元の `.ts` の対応表を作るかの設定
- `.d.ts` と `.ts` の対応関係を記録する
- これも ライブラリ開発者向け
- 普通のアプリ開発ではほぼ使わない

```json
"declarationMap": true
```

### ⑧ sourceMap: true
- ブラウザで TS をデバッグできるようにする
- `.map` ファイルを作る
これがあると
- Chrome DevTools で TS ファイルを直接デバッグできる
- JS に変換された後でも、DevTools 上では TS のままブレークポイントを貼れる

```json
"sourceMap": true
```

### ⑨ outFile
- **複数のファイルを1つのJSにまとめる**
- 昔のやり方（今はあまり使わない）
- module: "amd" などのときに使われてた
- 今は → **bundler（Vite / Webpack）使うのが主流**

### ⑩ outDir
- **コンパイルされたファイルをまとまったディレクトリに格納できる**
- JSの出力先フォルダ

```json
"outDir": "./dist"
```
- TS → JS に変換されたファイルがここに入る
- 超よく使う

### ⑪ rootDir
- **「TypeScript のソースコードはここから始まるよ」**という基準フォルダ
- outDir に出力するとき、このフォルダ構造をそのまま保ってコピーされる


```json
"rootDir": "./src"
```

```bash
project/
  src/        ← rootDir
    utils/
    components/
    index.ts
  dist/       ← outDir
```
- rootDir: "./src" を指定すると：
  - src の中だけがコンパイル対象
  - dist には src と同じフォルダ構造で 出力される

### ⑫ removeComments
- **コメントを消すかどうか**
- true → コメント消える
- false → コメント残る

### ⑬ noEmit
- **JSを出力しない**
- **型チェックだけしたいときに使う**
- 開発中によく使う

### ⑭ noEmitOnError
- **エラーがあったらJSを出さない**
- 安全重視なら true にする

## 変換・互換系
### ⑮ downlevelIteration
- **古い JavaScript でも for-of やスプレッドを正しく動かすための設定**
- 特に **target が ES5 のときだけ必要**
- ES5（＝古いブラウザ向け）には：
```js
for...of
...spread
Array.from()
Map / Set のイテレーション
```
などの **イテレーションの仕組み** が存在しない
- そのため、TypeScript が **自動で イテレーションをエミュレートするコード を生成してくれる設定が downlevelIteration**

## strict系
### ⑯ strict
- **全部まとめて厳しくするスイッチ**

```json
"strict": true
```
- → 以下の7個が全部自動でONになる（コメントアウトされていた場合もONになる）

## ↓ strictの中身
### ⑰ noImplicitAny
- **型を書いてない = any になるのを禁止**

```json
let a; // ❌ エラー
```

### ⑱ strictNullChecks
- **null / undefined を別物として扱う**

```json
let a: string = null; // ❌ エラー
let a: string = undefind; // ❌ エラー
```
### ⑲ strictFunctionTypes
- **関数の型のズレを厳しくチェック**
- 特に コールバック と 継承（サブクラス） でバグを防ぐためのもの
- 引数の型がちょっと違うだけでも許さない」
- 関数の互換性をゆるくしない

### ⑳ strictBindCallApply
- クラスを使用する時に使う
- **bind / call / apply の型をちゃんとチェック**

#### bind / call / apply について
| メソッド | 何をする？ | いつ使う？ |
|----------|------------|------------|
| **call** | 今すぐ呼ぶ（引数は列挙） | this を指定して即実行したいとき |
| **apply** | 今すぐ呼ぶ（引数は配列） | 引数が配列で渡ってくるとき |
| **bind** | this を固定した “新しい関数” を作る | 後で呼びたいとき、イベントで使いたいとき |


```ts
fn.call(obj, arg) // 型が合ってないとエラー
```

### ㉑ strictPropertyInitialization
- **「クラスのプロパティは必ず初期化しろ」ルール**
- constructor で必ず初期化しろ
- またはフィールドで初期値を入れろ
- または !（definite assignment assertion）を使って "絶対に代入される" と宣言しろ
というルールが強制される

```ts
class A {
  name: string; // ❌ 初期化してないとエラー
}
```

### ㉒ noImplicitThis
- **「this が any になるのを禁止する」設定**
- this の型が不明 → any 扱いとなりエラーになる
- this の型が曖昧ならエラーにする
- this が any になる → ❌ エラー
- this の型を明示する → ✔ OK

```ts
// ❌ エラーになる例
function greet() {
  console.log(this.name); // this が any → エラー
}
```

```ts
// this の型を明示すれば OK
function greet(this: { name: string }) {
  console.log(this.name);
}
```

### ㉓ alwaysStrict
- **JSファイルをコンパイル時に "use strict" をつける** かどうか

## 型チェック強化
### ㉔ noUncheckedIndexedAccess
- 配列・オブジェクトのアクセス時に **undefined の可能性** を必ず型に含める
- 配列の要素が存在しないのに .toUpperCase() して落ちる
- オブジェクトのキーが存在しないのにアクセスして undefined になる
- API レスポンスの optional な配列での事故

```ts
// デフォルト
const arr: string[] = [];
arr[0]; // ← string と推論される（危険）
```
- 実際には arr[0] は undefined の可能性が高いのに、TS はそれを無視してしまう

```ts
// noUncheckedIndexedAccess を true にすると
arr[0] // → string | undefined
```

### ㉕ exactOptionalPropertyTypes
- **?（オプショナル）の意味を 厳密に する設定**
- undefined と「存在しない」を区別する
- API の optional プロパティでのバグ
- プロパティが存在しない と undefined が入っている の混同
- バリデーションの抜け漏れ

```ts
// デフォルト
type A = { name?: string }
```
この name? は 2つの意味を同時に持つ：
1. プロパティが存在しない
2. プロパティがあるけど undefined

つまり：
```ts
let a: A = {};
let b: A = { name: undefined };
```
どっちも 同じ扱い になってしまう

exactOptionalPropertyTypes を true にすると
**「存在しない」と「undefined」が別物として扱われる**
つまり：
- {} → name が **存在しない**
- { name: undefined } → name が “存在するが undefined”
この違いを 型レベルで区別する

## コード品質チェック
### ㉖ noUnusedLocals
- **使ってない変数はエラー**

### ㉗ noUnusedParameters
- **使ってない引数はエラー**

### ㉘ noImplicitReturns
- **「関数のすべての経路で return が必要」ルール**
- 「関数のどの分岐でも必ず return しろ」というルールが強制される

```ts
function test(a: boolean) {
  if (a) return 1;
  // else がない → return しないパスが存在する → エラー
}
```
- a === true → return 1
- a === false → return なし（undefined になる）
つまり return が “あるパス” と “ないパス” が混ざっているから


```ts
// 正しいコード
function test(a: boolean) {
  if (a) return 1;
  else return 0;
}
```

### ㉙ noFallthroughCasesInSwitch
- **switchのbreak忘れ防止**

```ts
case 1:
  // breakない → エラー
```

## Source Map: JSとTSを対応させること
- デバッグ時にTSの位置がわかる
- ブラウザで便利

### ㉚ Experimental
- まだ標準仕様じゃない機能を使えるようにする設定
- 実験的に使える
- まだ正式じゃないけど、使いたいなら自己責任でどうぞ

## その他重要設定
### ㉛ forceConsistentCasingInFileNames
- ファイル名の大文字小文字ミス防止
- **ファイルの大文字小文字を必ず区別するもの**

### ㉜ isolatedModules
- **「このファイル、単体で変換しても壊れない？」を確認するスイッチ**
- 「ファイル単体でコンパイルできるか？」をチェックする設定

### ㉝ skipLibCheck
- ライブラリ（node_modules）の型チェックをスキップする
- ypeScript はデフォルトだと：
  - node_modules 内の .d.ts
  - 外部ライブラリの型定義
も全部チェックしようとする
でも、これは 重いし、無駄なエラーが出る原因になる
- 基本 true でOK

## 設定の分割
### ㉞ extends
- **設定ファイルを継承する**
- 共通設定を使い回せる

```json
"extends": "./base.json"
```

### ㉟ project（複数管理）
- モノレポや大規模開発で 複数の tsconfig をまとめて管理 する仕組み
- 大規模開発で使う
- TypeScript には プロジェクト参照（Project References） という機能がある
これを使うと：
  - packages/api/tsconfig.json
  - packages/web/tsconfig.json
  - packages/shared/tsconfig.json
みたいに 複数の TS プロジェクトを連携させてビルドできる
大規模開発で、TS プロジェクトを分割して管理するための仕組み

```bash
/packages
  /core
    tsconfig.json
  /ui
    tsconfig.json
  /server
    tsconfig.json
tsconfig.json ← これが “project” で複数を管理
```
#### メリット
- 依存関係を TS が理解してくれる
- 差分ビルドが速くなる
- 大規模モノレポで必須レベル

## TypeScript tsconfig.json 設定まとめ
⚫︎tsconfig.json の主要設定一覧

| 設定名 | 何のため？ | 頻繁に使う？ | いつ使う？ |
|--------|------------|----------------|------------|
| target | どのバージョンの JS に変換するか | ✔ | 古いブラウザ対応（ES5）や最新 JS を使いたいとき（ES6+） |
| lib | どの環境の型定義を使うか | ✔ | DOM を使うとき / Promise など ES6 機能を使うとき |
| allowJs | JS もコンパイル対象にする | △ | JS と TS が混在しているプロジェクト |
| checkJs | JS に TS のエラーを出す | △ | JS を TS 的に厳しくチェックしたいとき（allowJs とセット） |
| jsx | React の JSX をどう扱うか | React のときだけ | React プロジェクトで JSX を使うとき |
| declaration | .d.ts（型宣言ファイル）を作る | ❌ | npm ライブラリを公開するとき |
| declarationMap | .d.ts と .ts の対応表を作る | ❌ | ライブラリ開発でデバッグしやすくしたいとき |
| sourceMap | TS をブラウザでデバッグできるようにする | ✔ | Chrome DevTools で TS を直接見たいとき |
| module | import/export をどう扱うか | ✔ | ブラウザなら esnext、Node なら commonjs |
| moduleResolution | モジュールの探し方 | ✔ | 基本 node のままでOK |
| rootDir | TS のソースコードの場所 | △ | src フォルダを使うとき |
| outDir | コンパイル後の JS の出力先 | ✔ | dist フォルダにまとめたいとき |
| strict | TS の厳格モード | ✔（推奨） | 型安全にしたいとき（基本 ON） |
| noImplicitAny | 暗黙の any を禁止 | ✔ | 型漏れを防ぎたいとき |
| include | コンパイル対象のファイル | ✔ | src 配下だけコンパイルしたいとき |
| exclude | コンパイル除外ファイル | ✔ | node_modules や dist を除外したいとき |
| files | 特定ファイルだけコンパイル | △ | 単体ファイルだけ TS にしたいとき |