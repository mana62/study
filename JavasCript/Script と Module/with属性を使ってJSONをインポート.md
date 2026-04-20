# with属性を使ってJSONをインポートする方法
👉 **モジュールを読み込むときに追加のオプション（属性）を渡すための仕組み**（2023〜2024 以降にブラウザへ入り始めた新しい仕様）

## with インポート属性とは
＝ モジュールを読み込むときに、追加の設定（attributes）を一緒に渡すための構文

```js
// 書き方
import data from "./config.json" with { type: "json" };
// {の中にオブジェクトとして記載}
```

## 一番大事な使い道
**JSONを直接importできる**

```js
import data from './data.json' with { type: 'json' };

console.log(data);
```
＝ JSON → 自動でオブジェクトになる

## ポイント
**値は「静的な文字列だけ」**

```js
// ❌ ダメ
with { type: json }         // 変数
with { type: 123 }          // 数値
with { [key]: 'json' }      // 動的キー

// ✅ OK
with { type: 'json' }
```

## JSONインポートの制限
**デフォルトインポートしか使えない**
```js
// ✅ OK
import data from './data.json' with { type: 'json' };

// ❌ ダメ
import { id } from './data.json' with { type: 'json' };
```

## 内部で何が起きてる？
**2段階チェックされる**
1️⃣ with { type: 'json' } があるか
2️⃣ 本当にJSONか（環境がチェック）
- ブラウザ → MIMEタイプ確認
- Node.js → 拡張子 .json 確認
👉 合わないとエラー

## 同じJSONをimportすると？
全部同じオブジェクト
```js
import a from './data.json' with { type: 'json' };
import b from './data.json' with { type: 'json' };

console.log(a === b); // true
```
👉 キャッシュされて共有される

## 動的インポートでも使える
書き方
```js
const data = await import('./data.json', {
  with: { type: 'json' }
});
```
- data.default にJSONが入る

## 静的との違い
|       | 静的import | 動的import |
| ----- | -------- | -------- |
| 値     | 固定       | 動的OK     |
| 書き方   | with { } | 第2引数     |
| タイミング | 最初       | 実行時      |

## CSSもimportできる（ブラウザ）
```js
import style from './style.css' with { type: 'css' };
```
👉 CSSStyleSheet が返る
※ 優先度は低い

## まとめ
👉 with = **importにオプションを付ける新機能**
👉 その代表が JSON読み込み

- **with { type: 'json' }** → JSONをimportできる
- JSONは自動でオブジェクト化
- デフォルトimportのみ
- 値は文字列固定（静的）
- 同じJSON → 同じオブジェクト
- 動的importでも使える