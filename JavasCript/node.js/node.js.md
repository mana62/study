# node.js と npm と npx

## Node.js とは
👉 JavaScript をブラウザ以外でも実行できる環境

通常 JavaScript は
- Chrome
- Safari
- Edge
などの **ブラウザの中だけで動く言語**

＝ **しかし Node.js を使うと**
- サーバー
- CLIツール
- バックエンド
- ビルドツール
などを **JavaScriptで作れるようになる**

例：
```js
console.log("Hello Node");
```
これを
```js
node app.js
```
で実行できる

## npmとは？
👉 JavaScriptのパッケージ管理ツール
👉 **「便利なプログラムをダウンロードして使うツール」**

例えば
- サーバー作成
- 日付処理
- React
- Vue
などの **世界中のライブラリをインストールできる**

例：
```js
npm install express
```
すると
```js
node_modules/
```
というフォルダに expressというライブラリが入る

## パッケージをインストールすると？

例えば
```js
npm install http-server
```
すると
```js
project
 ├ node_modules
 │   └ http-server
 ├ package.json
 ```

**ode_modules**
→ インストールされたライブラリ

**package.json**
→ プロジェクトの設定ファイル

## パッケージの使い方はパッケージごとに違う

例えば

**express**
```js
const express = require("express")
```

**lodash**
```js
const _ = require("lodash")
```
**http-server** → コマンドで使う

つまりパッケージは下記の **2種類ある**
| 種類     | 使い方        |
| ------ | ---------- |
| ライブラリ型 | コードでimport |
| CLI型   | コマンドで実行    |

## node_modules/.bin とは？

```js
node_modules/.bin
```
👉 **インストールされたコマンドが入る場所**

例えば
```js
npm install http-server
```
すると
```js
node_modules/.bin/http-server
```
が作られる

## コマンド実行の例
例えばサーバーを起動する場合、本来は
```js
./node_modules/.bin/http-server
```
これで
```bash
http://localhost:8080
```
などでアクセスできる
サーバーが`index.html`を返す

## localhost と IPアドレス
| アドレス        | 意味           |
| ----------- | ------------ |
| 127.0.0.1   | 自分のPC        |
| localhost   | 127.0.0.1と同じ |
| 192.168.x.x | ネットワーク内の自分   |
👉 **全部自分のパソコン**

## npm run
例えば毎回
```js
./node_modules/.bin/http-server
```
は少し面倒、そこで npm が用意している仕組みが
```js
npm run
```

## package.json の scripts

```JS
// package.json
{
 "scripts": {
   "start": "http-server"
 }
}
```
これを書くと
```JS
npm run start
```
で下記が実行される
```js
http-server
```

**ポイント**
npm run を使うと
```js
node_modules/.bin
```
を 自動で見てくれる

## npm の --save-dev（セーブデブ）オプション
- npmには2種類ある
- package.jsonの`dependencies` ＝ 👉 アプリが動くために必要なライブラリ

例：
- express
- react
- axios
- vue
- sass

つまり**本番で必要なもの**

```js
const express = require("express")
```
＝この場合`express`は、`dependencies`

## devDependenciesとは？
👉 開発のときだけ使うツール

例：
- prettier（コード整形）
- eslint（コードチェック）
- typescript
- vite
- webpack
- jest

これらは **アプリが動くのに必須ではない**

## --save-dev を付けるとどうなる？
例えば
```bash
npm install prettier --save-dev
```
すると
```json
{
  "devDependencies": {
    "prettier": "^3.0.0"
  }
}
```
になる

| インストール方法                        | package.json    |
| ------------------------------- | --------------- |
| npm install express             | dependencies    |
| npm install prettier --save-dev | devDependencies |

## なぜ分けるのか？
理由：👉 パッケージを公開するとき

例えば`my-library`という npm パッケージを公開
誰かが、`npm install my-library`すると、
**dependenciesは自動でインストールされる**

しかし
**devDependencies** 👉 インストールされません

理由：
- eslint
- prettier
- jest

などは **ユーザーには不要だから**

## npm install をすると全部入る
node_modules が空で npm install をした場合

これは
```js
dependencies
devDependencies
```
両方入る
👉 **開発者は全部必要だから**


| 用途     | コマンド           |
| ------ | -------------- |
| アプリで使う | npm install    |
| 開発ツール  | npm install -D |


## package.jsonのキャレットと、チルダ
- ^（キャレット）：メジャーバージョン以外なら全部 OK
- ~（チルダ）：パッチバージョンだけ OK

### キャレット（^）の例
```json
"vue": "^3.2.0"
```
許可される更新：
- 3.2.1
- 3.3.0
- 3.9.5
- 3.999.0
→ **3.x.x の範囲なら全部 OK**

### チルダ（~）の例
```json
"vue": "~3.2.0"
```
許可される更新：
- 3.2.1
- 3.2.5
- 3.2.99
→ **3.2.x のパッチだけ OK**

| 記号 | 許可される更新 | 例 | 使う場面 |
|------|----------------|---------------------------|---------------------------|
| ^    | マイナー・パッチ | ^1.2.3 → 1.x.x | アプリ開発でよく使う |
| ~    | パッチのみ | ~1.2.3 → 1.2.x | 安定性重視のとき |

## npm run の特別ルール
特定の名前は
```bash
run
```
を省略できる

```bash
npm start
npm test
npm stop
npm restart
```

## npxとは？
👉 パッケージを簡単に実行するコマンド
例
```bash
npx http-server
```
すると
```bash
node_modules/.bin/http-server
```
を実行する
つまり
```bash
./node_modules/.bin/http-server
```
の代わり

## npxのすごい機能
もしパッケージが **インストールされていない場合**
```js
npx http-server
```
を実行すると
①npmからダウンロード
②実行
③すぐ削除
する

👉 つまり **一時インストール**

**メリット**
- 環境を汚さない
- 最新版
- 試しやすい

## アンインストール
```js
npm uninstall http-server
```
すると
- node_modules から削除
- package.json から削除
される

## グローバルインストール
```js
npm install -g http-server
```
👉 **PC全体で使える（グローバル）**

## まとめ
**① npm install**
```bash
npm install パッケージ
```
**② 実行**
- 方法1
```bash
./node_modules/.bin/コマンド
```
- 方法2（普通はこれ）
```bash
npm run コマンド
```
- 方法3（便利）
```bash
npx コマンド
```

- Node.js
👉 JavaScriptをPCやサーバーで動かす

- npm
👉 JavaScriptのアプリをインストールする

- node_modules
👉 インストールされたプログラム

- npx
👉 パッケージを簡単に実行