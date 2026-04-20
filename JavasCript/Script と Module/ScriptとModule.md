# Script と Module
- `Script`: 普通のJavaScript
- `Module`: import / exportが使える

## Script
＝ 「そのまま実行されるコード」のこと

**特徴**
- 単体で動く
- 依存関係を持たないことが多い
- 小さな処理や一時的な処理に向いている
- 読み込まれた瞬間に実行される（JavaScript の <script> など）

## Module（ES Modules）
＝ 「再利用するための部品として作られたコード」のこと

ECMAScriptでは
- export（外に公開）
- import（他のファイルから読み込み）
を使える

**特徴**
- 他のファイルから読み込んで使う
- 関数・クラス・定数などをまとめる
- 役割ごとに分割して管理できる
- 大規模開発で必須

例：
```js
// math.js
export function add(a, b) {
  return a + b;
}
```
```js
// main.js
import { add } from './math.js';
console.log(add(2, 3));
```

**用途**
- 関数やクラスの整理
- 大規模アプリの構造化
- 再利用可能なライブラリ
- 依存関係を管理する仕組み（Node.js の require / import）

⚠️ **import / export は Module でしか使えない**

## ブラウザでModuleにする方法
＝ HTMLの scriptタグに type="module" を付ける

```js
<script type="module" src="main.js"></script>
```
👉 これで main.js と import先のファイルが Module扱いになる

## ⚠️ さらに注意点（ブラウザ）
ブラウザでは ❌ file:// では動かない
必ず ✅ HTTPサーバーを立てる必要がある

## Node.jsの場合
＝ Node.jsでは package.json に設定を書く

```js
{
  "type": "module"
}
```
これで `node main.js` が Moduleとして実行される

## スクリプトとモジュールの違い
| 項目 | スクリプト | モジュール |
|------|------------|------------|
| 目的 | すぐ実行する | 再利用する |
| 実行タイミング | 読み込まれた瞬間 | import されたとき |
| 依存関係 | 基本なし | 他のモジュールを読み込む |
| 規模 | 小規模向け | 中〜大規模向け |
| 例 | `<script>` 内の JS | ES Modules, Node.js modules |

## なぜファイルを分割するのか
- コードを 1つのファイルにたくさん書くとスクロールが増える
- 読みにくい
- 管理しにくい
👉 だから JavaScriptではファイルを分割して整理する

## まとめ
- JavaScriptには Script と Module がある
- import / export は Moduleだけで使える
- ブラウザ → type="module"
- Node.js → package.json に type: "module"