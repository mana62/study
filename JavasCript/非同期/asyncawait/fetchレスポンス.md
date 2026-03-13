# fetchのレスポンス処理

## ① fetch() は Promise を返す
```js
const response = await fetch(url);
```
👉 fetch() の戻り値 → Promise
👉 await を使うと結果を受け取れる

## ② Promise が resolve されるタイミング
⚠️ポイント
fetch() の Promise は
**レスポンスのヘッダーを受け取った時点で resolve される**

つまり：
- ヘッダー → 取得済み
- Body → まだ全部来てない可能性

## ③ ネットワークエラーの時だけ reject
例えば：
- ネットワーク切断
- サーバーに接続できない
👉 こういうときだけ **reject**

⚠️ でも
- 404
- 500
👉 こういう **HTTPエラーは reject されない**
👉 普通に resolve される

**なのでチェックが必要**
```js
if (!response.ok) {
  console.log("エラー");
}
```

## ④ Responseオブジェクト
fetch() の結果は Responseオブジェクト

**よく使うプロパティ**
| プロパティ              | 意味          |
| ------------------ | ----------- |
| `response.status`  | ステータスコード    |
| `response.ok`      | 200系なら true |
| `response.headers` | ヘッダー        |

```js
console.log(response.status); // 200
console.log(response.ok); // true
```

## ⑤ Bodyの取得方法
**Bodyは メソッドで取得する**
| メソッド                     | 内容              |
| ------------------------ | --------------- |
| `response.json()`        | JSON → JSオブジェクト |
| `response.text()`        | 文字列             |
| `response.blob()`        | Blob            |
| `response.arrayBuffer()` | バイナリ            |

よく使うのはこれ👇
```js
const data = await response.json();
```

## ⑥ Body取得メソッドも Promise
これも await 必須
```js
const response = await fetch(url);
const data = await response.json();
```
**理由**
→ Bodyの読み込みが終わるまで待つ

## ⑦ Bodyは1回しか読めない
**⚠️ 重要**
```js
response.json();
response.text(); // ❌ エラー
```
1つだけ
**確認プロパティ**
```js
response.bodyUsed
```

## まとめ
```js
const response = await fetch(url);

if (!response.ok) {
  throw new Error("エラー");
}

const data = await response.json();

console.log(data);
```

1️⃣ fetch() → Promise
2️⃣ response.ok でエラー確認
3️⃣ response.json() でデータ取得