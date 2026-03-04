# async / await
👉 Promiseを「順番に書けるようにする」仕組み
- **asyncを付けた関数は必ずPromiseを返す**
- **awit = Promiseが終わるまで待つ**
- **awitはasyncが付いた関数の中だけで使える**
- **awaitはエラーを throw するため必ず、try/catch を使う**

## ① async の意味
```js
async function test() {
  return 1;
}
```
これを呼ぶと：
```js
test();
```
👉 **Promiseが返る**
中で何を返しても、必ず Promise になる

## ② await の意味
```js
await Promise;
```
の意味は：
**「Promiseが終わるまで待つ」**

### Promise
```js
fetch("url")
  .then(res => res.json())
  .then(data => console.log(data));
  ```

### async/await
```js
async function test() {
  const res = await fetch("url");
  const data = await res.json();
  console.log(data);
}
```

await が来ると：
- そこで処理が一時停止
- Promiseが終わる
- その続きが実行される

## まとめ
- async = 「この関数はPromiseを返すよ」
- await = 「Promise終わるまで待つよ」
- awaitはasyncの中だけ
- エラーはtry/catch