# for await of
**1つ終わるまで待つ終わったら次へ進む仕組み**

普通に書くと：
```js
promises.forEach(p => {
  p.then(console.log);
});
```
👉 順番バラバラになる
＝それぞれ勝手に終わるから

#### 「必ず 1 → 2 → 3 の順番で出したい」の解決方法
```js
for (const p of promises) {
  const result = await p;
  console.log(result);
}
```
👉 1つ終わるまで待つ、終わったら次へ進む
👉 await があると「終わるまで止まる」だから順番になる

## for await...of とは（上記の省略版）
```js
for await (const result of promises) {
  console.log(result);
}
```
内部で勝手に
```js
await p
```
をしている

| 書き方         | やってること |
| ----------- | ------ |
| for + await | 手動で待つ  |
| for await   | 自動で待つ  |