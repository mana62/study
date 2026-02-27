# setTimeout と setInterval
⚫︎ **setTimeout** →「○秒後に1回だけ実行してね」
⚫︎ **setInterval** →「○秒ごとに何回も実行してね」

## setTimeout（1回だけ実行）
```js
setTimeout(() => {
  console.log("3秒後に1回だけ実行される");
}, 3000);
```
- 3000ms（3秒）後に 1回だけ 実行される
- その後はもう動かない

## setTimeoutの使用場面
- ちょっと遅らせて実行したい
- アニメーションの開始タイミングをずらしたい
- API のリトライを少し待ってから行いたい

## setTimeoutを止める方法
```js
// 定型分
clearTimeout（止めたい関数名）

const id = setTimeout(...);
clearTimeout(id);
```

## setInterval（繰り返し実行）
```js
setInterval(() => {
  console.log("1秒ごとに実行される");
}, 1000);
```
- 1000ms（1秒）ごとに ずっと繰り返す

## setIntervalの使用場面
- 時計の更新
- 定期的なデータ取得
- ゲームのループ処理

## setInterval を止める方法
```js
// 定型分
clearInterval(止めたい関数名);

const id = setInterval(...);
clearInterval(id);
```

## 違いのまとめ
| 機能 | setTimeout | setInterval |
|------|------------|-------------|
| 実行回数 | 1回だけ | 繰り返し |
| タイミング | 指定時間後に1回 | 指定時間ごとに何度も |
| 停止方法 | clearTimeout | clearInterval |
| 用途 | 遅延実行 | 定期実行 |

## ⚠️ 注意
- 実質`clearTimeout`でも`clearInterval`でもどっちでも処理を止めることができる
- `clearTimeout`や`clearInterval`は一意の値になっている
- `setInterval` はズレが起きやすい（処理が重いと、間隔が正確じゃなくなることがある）
- → そのため、最近は `setInterval` より `requestAnimationFrame` や再帰的 `setTimeout` が好まれることも多い