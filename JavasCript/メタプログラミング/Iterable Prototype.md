# Iterable Prototype
- 反復可能オブジェクトのルールを持つ親オブジェクト
- 配列や Map、Set などは最初からプロトタイプに Symbol.iterator を持っている

## 使う場面
- 自作オブジェクトに配列のようなループ機能を付けたい
- for…of、スプレッド構文、Array.from などで使えるようにする

```js
const arrLike = {0:"a", 1:"b", length:2};
arrLike[Symbol.iterator] = Array.prototype[Symbol.iterator];
for (const x of arrLike) {
  console.log(x); // "a" "b"
}
```