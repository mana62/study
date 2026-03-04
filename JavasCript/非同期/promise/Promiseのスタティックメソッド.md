# Promise のスタティックメソッド
👉 複数の Promise をまとめて扱う便利ツール

主に下記の５つ：
| メソッド                       | 何をする？                   |
| -------------------------- | ----------------------- |
| `Promise.all`              | **全部成功したらOK**           |
| `Promise.allSettled`       | **全部終わったらOK（失敗あってもOK）** |
| `Promise.race`             | **一番早い1つだけ**            |
| `Promise.any`              | **最初に成功した1つだけ**         |
| `Promise.resolve / reject` | **即Promise化**           |

## ①Promise.all → 全部成功したらまとめて受け取る
```js
Promise.all([p1, p2, p3])
  .then(results => console.log(results))
  .catch(err => console.log(err));
```
**動き**
- 全部成功 → then
- 1つでも失敗 → 即 catch

**使い所**
👉 複数 API を同時に叩いて、全部揃ったら処理

## ②Promise.allSettled → 全部終わればOK（失敗混ざっても）
```js
Promise.allSettled([p1, p2, p3])
  .then(results => console.log(results));
```
**動き**
- 成功・失敗 全部待つ
- 結果を全部受け取る

**使い所**
👉 成功も失敗もまとめて結果確認したい

## ③Promise.race → 一番早い1つだけ
```js
Promise.race([p1, p2, p3])
  .then(v => console.log(v))
  .catch(e => console.log(e));
```

**動き**
- 一番早く 成功 or 失敗した1つだけ採用

**使い所**
👉 タイムアウト処理
```js
Promise.race([api(), timeout(3000)])
```

## ④Promise.any → 最初に成功した1つだけ
```js
Promise.any([p1, p2, p3])
  .then(v => console.log(v))
  .catch(e => console.log(e));
```
**動き**
- 最初に 成功したもの
- 全部失敗 → catch

**使い所**
👉 どれか1つ成功すればOKな時

## ⑤Promise.resolve / reject → 即 Promise 化
```js
Promise.resolve(10)
Promise.reject("error")
```
**意味**
```js
new Promise(resolve => resolve(10))
```
の省略形

## その他
| メソッド                      | 意味                        |
| ------------------------- | ------------------------- |
| `Promise.withResolvers()` | resolve を外で使える Promise 作成 |
| `Promise.try(fn)`         | 同期・非同期どちらでも統一的に扱う         |

👉 ほぼ使わない

## まとめ
・全部 → all
・全部結果 → allSettled
・一番早い → race
・最初の成功 → any

| やりたいこと       | 使う         |
| ------------ | ---------- |
| 全部終わるまで待つ    | all        |
| 成否関係なく全部待つ   | allSettled |
| 早い者勝ち        | race       |
| どれか1つ成功すればOK | any        |