# sort / toSorted
＝PC が内部で持っている Unicode（文字コード）順で並び替えるため、比較関数が必要

## sort()
＝破壊的メソッド
＝元の配列を並び替えて返す

```js
// 定型文
array.sort((a, b) => a - b); // 数値の昇順
array.sort((a, b) => b - a); // 数値の降順

// 例
const arr = [3, 1, 2];
arr.sort();

console.log(arr); // [1, 2, 3] ← 元の配列が書き換わる
```

## toSorted()
＝非壊的メソッド
＝値を新しい配列に並び替えて返す

```js
// 定型文
array.toSorted((a, b) => a - b); // 昇順
array.toSorted((a, b) => b - a); // 降順

// 例
const arr = [3, 1, 2];
const newArr = arr.toSorted();

console.log(arr);    // [3, 1, 2]
console.log(newArr); // [1, 2, 3]
```


```js
配列の中から2つ取り出す → a と b に入れる
↓
比較関数 (a, b) => a - b を実行
↓
結果が負なら a が先
結果が正なら b が先
結果が 0 なら順番そのまま


const arr = [3, 1, 2];

arr.sort((a, b) => {
  console.log(a, b);
  return a - b;
});

(3, 1)
(3, 2)
(1, 2)

```

- a - b が 負 → a のほうが小さい → a を前に
- a - b が 正 → b のほうが小さい → b を前に
- a - b が 0 → 同じ → そのまま