# JSON
👉 JSON ＝ **データをわかりやすく書くための決まり**（フォーマット）
👉 `JSON.stringify` ＝「オブジェクト → JSON文字列」
👉 `JSON.parse` ＝「JSON文字列 → オブジェクト」

```js
{
  "name": "Taro",
  "age": 20,
  "isStudent": true
}
```
- {} は「まとまり（オブジェクト）」
- "name" のように キー（項目名） がある
- JSONはキーにもダブルクォートが必要
- : の右に 値（データ） がある
- データは「文字」「数字」「true/false」など

## JSON.stringify(): JSON 形式の文字列に変換する
```js
const obj = { name: "Taro", age: 20 };
const json = JSON.stringify(obj);

console.log(json);
// → '{"name":"Taro","age":20}'
```
● どんな時に使う？
- fetch の body に JSON を送るとき
- localStorage に保存するとき（文字列しか保存できない）
- データをファイルとして保存するとき

## JSON.parse: JSON文字列をオブジェクトに戻す
＝ `JSON.stringify()` で文字列になったものを、
元のオブジェクトに戻すのが `JSON.parse()`

```js
const json = '{"name":"Taro","age":20}';
const obj = JSON.parse(json);

console.log(obj.name); // → "Taro"
```
● どんな時に使う？
- fetch のレスポンスを JSON として受け取るとき
``` js
fetch(url)
  .then(res => res.json()) // ← 内部的には JSON.parse と同じ
```
- localStorage から取り出した文字列をオブジェクトに戻すとき

## JSONを作るときの基本ルール
### ①キー（項目名）も必ずダブルクォートで囲む
```js
{ "name": "Taro" }
```

### ②文字列はダブルクォートのみ（シングルクォートは不可）
下記は❌
```js
{ "name": 'Taro' }
```

### ③最後のカンマはNG
下記は❌
```js
{
  "name": "Taro",
}
```

### ④使えるデータ型は決まっている
- 文字列（"text"）
- 数値（123）
- true / false
- null
- 配列（[]）
- オブジェクト（{}）

## 配列（リスト）も書ける
```js
{
  "fruits": ["apple", "banana", "orange"]
}
```
- [] を使うと、複数のデータを並べられる

## まとめ
JSONとは：
- データの書き方のルール
- コンピューター同士の共通言語
- シンプルで読みやすい