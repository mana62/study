# node.jsのビルトインモジュール
- node_modules → 他人が作ったライブラリ
- ビルトインモジュール → Node.jsが用意したもの

## ビルトインモジュールとは
インストールした際に最初から用意してくれている機能

## Node.jsが最初から用意してる機能
- fs → ファイル操作
- path → パス操作
- http → サーバー作れる
- crypto → 暗号化
- url → URL処理

## 使い方
**CommonJS**
```js
const fs = require("fs");
```

**ES Modules**
```js
import fs from "fs";
```
👉 インストール不要

つまり... **「文字列が名前だけ」のときの動き**

```js
require("fs");
```
👉 Node.jsはこう考える
1. これビルトインモジュール？
2. → YES → それ使う

もしビルトインモジュールではないけど名前のみだったら？
```js
require("express");
```
👉 Nodeの思考
1. ビルトイン？ → NO
2. node_modulesにある？ → YES
3. → それ読み込む

結論：
1. ビルトイン探す
2. なければnode_modules探す
- node_modules → 他人が作ったライブラリ

## まとめ
- 「ビルトインモジュール＝Node.jsの内蔵機能」
- 「名前だけ書いたとき最初に探される」
- node_modules = インストールしたライブラリが入ってるフォルダ = 他人が作ったコードの集まり

|                | node_modules | ビルトイン   |
| -------------- | ------------ | ------- |
| 誰が作った          | 他人（or自分）     | Node.js |
| インストール         | 必要           | 不要      |
| 場所             | フォルダにある      | Node内部  |
| require/import | `"express"`  | `"fs"`  |