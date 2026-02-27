# reverse と toReversed

| メソッド     | 配列を変更する？       | 返り値         |
|--------------|-------------------------|----------------|
| reverse      | 変更する（破壊的）     | 反転後の同じ配列 |
| toReversed   | 変更しない（非破壊）   | 反転した新しい配列 |

## reverse（破壊的）
＝元の配列をそのまま反転させる
＝reverse は 引数を取らない

```js
const arr = [1, 2, 3];
arr.reverse();

console.log(arr); // [3, 2, 1]
```

## toReversed（非破壊的）
＝元の配列はそのまま、新しい配列を返す
＝reverse は 引数を取らない

```js
const arr = [1, 2, 3];
const newArr = arr.toReversed();

console.log(arr);    // [1, 2, 3] ← 変わらない
console.log(newArr); // [3, 2, 1]
```

## まとめ
- reverse → 元の配列をひっくり返す（破壊的）
- toReversed → 新しい配列を返す（非破壊的）
- やってることは同じだけど「元の配列を壊すかどうか」が違う