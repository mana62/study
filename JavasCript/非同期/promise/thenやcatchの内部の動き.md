# then / catch / finally の内部的な動き
＝Promise は **「成功ルート」と「失敗ルート」の2本の道を自動で用意している**

👉 成功したら then だけ通る
👉 失敗したら catch だけ通る

それ以外は「存在してないかのようにスキップ」される

```bash
        成功ルート → then → then → then
スタート
        失敗ルート → catch → catch → catch

# Promise には 最初から2本の道がある
```

## then を書いた場合
```js
promise.then(onFulfilled)
```
内部では：
⚫︎**成功用キュー**（Fulfill Reaction） → onFulfilled を加工した関数
⚫︎**失敗用キュー**（Reject Reaction） → そのまま次へ reject する関数
が 両方登録される

| 元のPromise   | 次に実行される                 |
| ----------- | ----------------------- |
| 成功(resolve) | then が実行される             |
| 失敗(reject)  | then はスキップされ、次の catch へ |

## catch を書いた場合
```js
promise.catch(onRejected)
```
内部では：
⚫︎**失敗用キュー** → onRejected を加工した関数
⚫︎**成功用キュー** → そのまま次へ resolve する関数
が 両方登録される

| 元のPromise   | 次に実行される        |
| ----------- | -------------- |
| 成功(resolve) | catch はスキップされる |
| 失敗(reject)  | catch が実行される   |

## なぜ「自動で次に伝播」するのか
**then の内部イメージ**
```js
成功時: 次のPromiseを resolve
失敗時: 次のPromiseを reject
```

**catch の内部イメージ**
```js
失敗時: 次のPromiseを resolve
成功時: 次のPromiseを resolve（そのまま流す）
```

つまり:
**何も書いていなくても、成功・失敗の状態は自動で次のPromiseに伝播する**
→ だから 勝手にスキップされたように見える

## 途中で reject した場合の内部の動き
最初は成功 → then を進む
途中で失敗 → その瞬間から 失敗ルートに切り替わる

👉 以降の then は全部スキップ
👉 最初に出てくる catch だけ実行される

```js
Promise.resolve()
  .then(() => {
    console.log("① 成功");
  })
  .then(() => {
    console.log("② ここで失敗！");
    throw new Error("boom");
  })
  .then(() => {
    console.log("③ ここは実行されない");
  })
  .catch(() => {
    console.log("④ catch 実行");
  });
```

```bash
# 内部

① then → 成功 → 次へ
② then → 失敗 → ルート切替！
③ then → スキップ
④ catch → 実行
```

## finally の内部の動き
finally は **「成功でも失敗でも必ず通る場所」**

👉 成功・失敗 どちらのルートでも必ず実行
👉 しかも 値もエラーも基本そのまま次へ流す

```bash
        成功ルート → then → then
START →
        失敗ルート → catch → catch

           ↓ どっちでも必ず通る

                 finally
```
#### finally の本質
finally は
・値を受け取らない
・値を変えない
・結果を変えない
・ただ必ず実行される

👉 finally = **成功・失敗どちらでも必ず実行されて、結果はそのまま流す**

## まとめ
**then / catch は、内部で「成功用」と「失敗用」の2つの関数を自動生成して登録している**ため、Promiseの状態に応じて自然にスキップや伝播が起こる

| 状態      | then | catch |
| ------- | ---- | ----- |
| resolve | 実行   | スキップ  |
| reject  | スキップ | 実行    |

理由 👉 内部で自動的に変形された関数が登録されているから

| 状況        | 何が起きる？                               |
| --------- | ------------------------------------ |
| 最初から失敗    | 最初の catch まで then 全スキップ              |
| 途中で失敗     | **その時点から then 全スキップ → 最初の catch 実行** |
| catch で回復 | その後は then に戻れる                       |


| 状況     | finally         |
| ------ | --------------- |
| 成功     | 実行される → 値そのまま   |
| 失敗     | 実行される → エラーそのまま |
| return | 無視              |
| throw  | 新しい失敗になる        |
