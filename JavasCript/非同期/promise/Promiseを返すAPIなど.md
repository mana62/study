# Promise を返すAPI と 返さないAPI
① いまの新しい **Web API は、最初から Promise を返すものが増えている**
② **昔の API は Promise 非対応なので、自分で Promise 化すればいい**

## ① Promise を返す「新しい API」の紹介
**例①：カメラ・マイク取得**

```js
navigator.mediaDevices.getUserMedia(...)
```
👉 Promise を返す

だから：

```js
navigator.mediaDevices.getUserMedia({ video: true })
  .then(stream => {
    console.log("成功");
  })
  .catch(err => {
    console.log("失敗");
  });
```
✔ then → 成功
✔ catch → 失敗

👉 コールバック不要

**例②：クリップボード API**
```js
navigator.clipboard.readText()
```
👉 Promise を返す

```js
navigator.clipboard.readText()
  .then(text => console.log(text))
  .catch(err => console.log(err));
```

✔ エラー処理も簡単

つまり：
👉 **最近の Web API は Promise 前提で設計されている**

## ② 昔の API は Promise じゃない → Promise 化できる
代表例：**setTimeout**

昔：

```js
setTimeout(() => {
  console.log("1秒後");
}, 1000);
```
👉 コールバック式

Promise 化するとこう：

```js
function sleep(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}
```
使い方：
```js
sleep(1000)
  .then(() => console.log("1秒後"))
  .then(() => sleep(1000))
  .then(() => console.log("さらに1秒後"));
```
✔ ネストしない
✔ 読みやすい
✔ エラー処理も統一

## ⚠️ 注意点
Promise は **「1回しか完了しない処理」しか扱えない**

| API                            | Promise 化 |
| ------------------------------ | --------- |
| setTimeout                     | ⭕ 可能      |
| geolocation.getCurrentPosition | ⭕ 可能      |
| clickイベント                      | ❌ 不可      |
| setInterval                    | ❌ 不可      |

👉 何度も起きるイベントは Promise と相性が悪い
👉 イベント処理などでは使えない

## まとめ
👉 「Promise を使うと、非同期処理が綺麗に書ける」
👉 「Promise を返す API を使おう」
👉 「Promise 非対応 API は Promise 化しよう」

⚫︎最近の Web API は Promise を返す → だから then/catch で綺麗に書ける
⚫︎Promise 非対応 API は、自分で Promise 化すれば同じ書き方にできる