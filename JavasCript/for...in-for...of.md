## `for...in`：キー（名前）を取り出す

<br>

- **対象**：オブジェクトや配列
- **取り出すもの**：プロパティ名（オブジェクト）やインデックス（配列）
- **使いどころ**：オブジェクトの中身を調べたいとき

<br>

```js
const obj = { name: 'Mana', age: 30 };
for (let key in obj) {
  console.log(key);        // 'name', 'age'
  console.log(obj[key]);   // 'Mana', 30
}
```

<br>

## `for...of`：値（中身）を取り出す

<br>

- **対象**：配列、文字列、Map、Setなど「イテラブル(繰り返し処理できるもの)」
- **取り出すもの**：値そのもの
- **使いどころ**：配列の中身を順番に使いたいとき

<br>

```js
const arr = ['🍎', '🍊', '🍇'];
for (let fruit of arr) {
  console.log(fruit);  // '🍎', '🍊', '🍇'
}
```

<br>

## 違いの例

 ```js
const arr = ['a', 'b', 'c'];

for (let i in arr) {
  console.log(i);        // '0', '1', '2' ←インデックス（キー）
}

for (let v of arr) {
  console.log(v);        // 'a', 'b', 'c' ←値
}
```

## まとめ
- **比較項目**：	for...in	for...of
- **対象**：	オブジェクト・配列	配列・文字列・Map・Setなど
- **取得するもの**：	キー（プロパティ名・インデックス）	値（中身）
- **よく使う場面**	オブジェクトの中身を調べる / 配列の値を順番に使う
