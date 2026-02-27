# Promise
**Promise = 未来に決まる値を入れる箱**
＝Promiseは「今すぐ実行する」のではなく、「後で実行する処理を登録する」仕組み

## Promiseは3つの状態しかない
| 状態        | 意味           |
| --------- | ------------ |
| pending   | まだ結果が決まっていない |
| fulfilled | 成功した         |
| rejected  | 失敗した         |

👉 上記の値は **一度決まったら二度と変わらない**

## Promiseの作り方（最小形）
```js
const p = new Promise((resolve, reject) => {
  resolve("成功");
});
```
このとき：
- **resolve("成功")** → 成功状態にする
- **reject("失敗")** → 失敗状態にする

## then / catch / finally の役割
```js
p.then(result => {
  console.log(result);
})
.catch(error => {
  console.log(error);
})
.finally(() => {
  console.log("終了");
});
```

| メソッド    | いつ実行？    |
| ------- | -------- |
| then    | 成功したとき   |
| catch   | 失敗したとき   |
| finally | 成功でも失敗でも |

**(resolve, reject) = 結果を「決める側」**
**then / catch / finally = 結果を「受け取る側」**

```js
作る側: new Promise((resolve, reject) => { ... })

使う側: promise.then(...)
        promise.catch(...)
        promise.finally(...)
```

### ① resolve / reject → 「約束の結果を決める」
```js
new Promise((resolve, reject) => {
  // 成功したら
  resolve("OK");

  // 失敗したら
  reject("NG");
});
```
👉 Promiseの中の人（処理を書く側）が使う

### ②then / catch / finally → 「結果を受け取る」
```js
promise
  .then(result => { ... })    // 成功時
  .catch(error => { ... })    // 失敗時
  .finally(() => { ... });    // どっちでも
```
👉 Promiseを使う人（呼び出し側）が使う

**Promise の中では、then / catch / finally は「使わない」**

| 役割                     | 何をする？     |
| ---------------------- | --------- |
| resolve / reject       | 結果を「決める」  |
| then / catch / finally | 結果を「受け取る」 |

### Promiseの中（resolve / reject）
```js
new Promise((resolve, reject) => {
  // ここは「処理を書く場所」
  setTimeout(() => {
    resolve("OK");
  }, 1000);
});
```

👉 仕事：結果を決める

### Promiseの外（then / catch / finally）
```js
promise
  .then(result => {...})
  .catch(err => {...})
  .finally(() => {...});
```
👉 仕事：結果を受け取って処理する


```js
function wait(ms) {
  return new Promise((resolve) => {
    setTimeout(resolve, ms);
  });
}

wait(1000)
  .then(() => console.log("完了"))
  .catch(err => console.log("エラー:", err))
  .finally(() => console.log("終了")); // 必ず呼ばれる
```

```js
// 別の例
function test() {
  return new Promise((resolve) => {
    if (Math.random() < 0.5) {
      throw new Error("失敗");
    }
    resolve("成功");
  });
}

test()
  .then(console.log)
  .catch(console.error);
```

👉 **throw すれば reject を書かなくても OK**

なぜなら：
**Promise の中で throw すると、自動的に reject 扱いになる**


## finallyとは
＝成功でも失敗でも、必ず実行される後処理

## まとめ
resolve → 成功 → then
reject  → 失敗 → catch
成功失敗どっちでも → finally

Promiseを作る → resolve / reject
Promiseを使う → then / catch / finally