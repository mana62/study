# Generator（ジェネレーター）
- **簡単にイテレータを作れる関数**
- **function* で定義して yield で値を返す**

## 使う場面
- 大量データを順番に処理したい（メモリ節約）
- イテレーションの処理を簡単に書きたい

```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

const g = gen();
console.log(g.next()); // { value: 1, done: false }
console.log(g.next()); // { value: 2, done: false }
console.log(g.next()); // { value: 3, done: false }
console.log(g.next()); // { value: undefined, done: true }

for (const val of gen()) {
  console.log(val); // 1 2 3
}
```