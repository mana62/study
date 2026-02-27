# in演算子
=「そのキー（プロパティ名）がオブジェクトの中に存在するか調べる演算子
= 結果は`true` or `false` で返す

```js
// 例
const user = { name: "Taro", age: 20 };

"name" in user   // true
"email" in user  // false

// → user の中に "name" というプロパティがあるか？
// → あれば true、なければ false
```

## 配列にも使える
```js
const arr = ["A", "B"];

0 in arr   // true
2 in arr   // false（要素がない）

// → 配列の場合は「インデックスが存在するか」を調べる
```

## undefined でも “存在していれば” true
```js
const obj = { value: undefined };

"value" in obj   // true

// → ⚠️値が undefined でも「キーがある」なら true
```

## まとめ
- in は「キー（プロパティ）が存在するか」を調べる
- 値が undefined でもキーがあれば true
- オブジェクトにも配列にも使える