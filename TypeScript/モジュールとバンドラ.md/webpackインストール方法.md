# webpackインストール方法

## 1. プロジェクト作成
```bash
npm init -y
```
意味：
`npm init`：Node.js のプロジェクトを開始するコマンド
`-y`：質問を全部 yes にして自動作成

実行すると package.json が作られる

## 2. webpack をインストール
```bash
npm install --save-dev webpack webpack-cli
```
意味：
`--save-dev`：開発時だけ使うパッケージとして追加
`webpack`：JavaScript をまとめる本体
`webpack-cli`：ターミナルで webpack コマンドを使うためのツール

この2つは基本セット

## 3. package.json に build コマンドを書く
```bash
"scripts": {
  "build": "webpack"
}
```
これで以下を実行できる

```bash
npm run build
```

## 4. webpack.config.js とは？
webpack の設定ファイル

```bash
どのファイルを読み込むか？
↓
どこに出力するか？
↓
どういうルールでまとめるか？
```
を指定


### 基本形（entry + output）
```js
const path = require('path');

module.exports = {
  // 入口ファイル
  entry: './src/main.js',

  // 出力設定
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

### entry（入口）
`entry: './src/main.js'`：webpack が最初に読むファイル
ここから import をたどって全部まとめる

例：
```js
import './app.js';
import './user.js';
output（出力）
output: {
  filename: '[contenthash]bundle.js',
  path: path.resolve(__dirname, 'dist')
}
```
意味：
- filename：出力されるファイル名
- path：保存先フォルダ ※ 絶対パスで書く必要がある
- __dirname：現在のJavaScriptファイルが置かれているディレクトリ（フォルダ）の絶対パス
- path.resolve：パスを正しい絶対パスに変換する関数
```js
// 定型文
path.resolve(元の場所, 追加のフォルダ名)
```
- `[contenthash]`：ファイル内容に応じたハッシュ値（識別番号）付きのファイル名

結果：
`dist/bundle.js` が作られる

フォルダ構成例
```bash
project/
├─ src/
│  └─ main.js
├─ dist/
├─ package.json
└─ webpack.config.js
```

実行すると：
```bash
npm run build
```
↓
```bash
src/main.js
＋ importされたJS全部
↓
dist/bundle.js にまとまる
```

## Webpackの **ソースマップ（source map）**
- Webpack のソースマップは **「変換後のコード」と「元のコード」を結びつけて、デバッグを圧倒的に楽にする仕組**
- devtool の設定で **どのレベルのソースマップを出すか** を選べる

### よく使うソースマップの種類

#### Development（開発）向け

| 設定 | 特徴 | 向いているケース |
|------|------|------------------|
| `eval` | 最速。品質は低い | とにかくビルド速度を最優先したい |
| `eval-source-map` | 高品質。やや遅い | デバッグ重視の開発 |
| `inline-source-map` | ソースマップを JS 内に埋め込む | 小規模プロジェクト、設定を簡単にしたい |

---

#### Production（本番）向け

| 設定 | 特徴 | 向いているケース |
|------|------|------------------|
| `source-map` | 高品質。ビルドは遅い | 本番でもデバッグしたい場合 |
| `hidden-source-map` | `.map` は出力するが JS にコメントを残さない | ソースマップを公開したくない場合 |
| `nosources-source-map` | ソースコードを含まない | セキュリティを保ちつつエラー位置だけ知りたい |

---

### 設定例（webpack.config.js）

```js
module.exports = {
  mode: 'development',
  devtool: 'eval-source-map'
}
```

## ts-loader（Webpack 専用）
- ts-loader は **「Webpack に TypeScript を読めるようにするための変換係」**
- watchモードとかは不要
- TypeScript を JavaScript にコンパイルする **通訳** のような存在

- TypeScript（.ts / .tsx）を JavaScript に変換して Webpack に渡すためのローダー
- **Webpack は JS しか扱えないので、「TS → JS に変換する人」が必要 → それが ts-loader**

### 何をするものか？
#### 1. TypeScript を JavaScript にコンパイルする
- TypeScript の公式コンパイラ（tsc）を内部で使って変換
- → 型チェックもできる（後述）

### 2. Webpack のビルドに TS を組み込める
- Webpack の module.rules に設定すると、.ts や .tsx を自動で処理してくれる

### 3. React（.tsx）にも対応
- .tsx も扱えるので React + TS でも使われる

### どんなときに使うのか？
- TypeScript を Webpack でバンドルしたいとき
- React + TypeScript + Webpack の構成
- Three.js や Node.js アプリを TS で書いて Webpack でまとめたいとき

### 使い方
#### ① インストール
```bash
npm install -D ts-loader typescript

# または
npm install --save-dev ts-loader npm install -D ts-loader typescript
```
（公式もこの方法を案内）

#### ② webpack.config.js に設定
```js
module.exports = {
  // Webpack が最初に読み込むファイル（エントリーポイント）
  entry: './src/index.ts',

  module: {
    rules: [
      {
        // .ts と .tsx のファイルを対象にする（TypeScript 全般）
        test: /\.tsx?$/,

        // TypeScript を JavaScript に変換する“通訳”として ts-loader を使う
        use: 'ts-loader',

        // node_modules 内は変換しない（高速化 & 不要だから）
        exclude: /node_modules/
      }
    ]
  },

  resolve: {
    // import 時に拡張子を書かなくても OK にする設定
    // 例: import './utils' → utils.ts / utils.tsx / utils.js を順番に探す
    extensions: ['.ts', '.tsx', '.js']
  }
}
```

- entry → Webpack が最初に読む TS ファイル
- test → TS/TSX を対象にする
- use: ts-loader → TS → JS に変換する
- exclude → node_modules は無視
- resolve.extensions → import の拡張子省略を許可

## webpack-dev-sever
- **開発用のローカルサーバーを自動で立ち上げて、変更したら即ブラウザを更新してくれる便利ツール**

Webpack 単体では「ビルドするだけ」
でも開発では、
- ブラウザを自動リロードしたい
- ファイル変更を即反映したい
- ローカルサーバーがほしい
**→ これを全部やってくれるのが、webpack-dev-server**

### 設定例
```js
module.exports = {
  // Webpack が最初に読み込むファイル
  entry: './src/index.ts',

  // 出力設定（開発ではあまり重要じゃない）
  output: {
    filename: 'bundle.js',
    path: __dirname + '/dist'
  },

  // webpack-dev-server の設定
  devServer: {
    // ローカルサーバーを立ち上げるディレクトリ
    static: {
      directory: __dirname + '/dist'
    },

    // http://localhost:8080 でアクセスできる
    port: 8080,

    // ファイル変更時にブラウザを自動リロード
    liveReload: true,

    // 自動でブラウザを開く
    open: true
  },

  module: {
    rules: [
      {
        // .ts / .tsx を対象にする
        test: /\.tsx?$/,
        // TypeScript → JavaScript に変換
        use: 'ts-loader',
        // node_modules は除外
        exclude: /node_modules/
      }
    ]
  },

  resolve: {
    // import の拡張子省略を許可
    extensions: ['.ts', '.tsx', '.js']
  }
}
```

### 何が便利なのか？
#### 1. ローカルサーバーを自動で立ち上げる
- npm run dev すると、→ http://localhost:8080 が自動で開く

#### 2. ファイルを保存するとブラウザが自動更新
- リロード不要 → React/Vue なら HMR（ホットリロード） も可能

#### 3. dist を毎回ビルドしなくていい
- メモリ上でビルドするので高速 → dist フォルダが汚れない

### インストール方法
```bash
npm install -D webpack-dev-server
```

### 実行方法（package.json）
```josn
{
  "scripts": {
    "dev": "webpack serve"
  }
}
```
→ npm run dev でサーバー起動