# Promise でエラーが出る条件
👉 **「reject されたのに、どこにも catch が付かなかった時」**のみ

## catch が無い場合
```js
Promise.reject("NG");
```
→ ❌ エラー出る
👉 catch してないから

## catch がある場合
```js
Promise.reject("NG").catch(() => {});
```
→ ⭕ エラー出ない
👉 catch があるから

## チェーンのcatch が無い場合
```js
Promise.resolve()
  .then(() => {
    throw new Error("boom");
  })
  .then(() => {
    console.log("ここは来ない");
  });
```
→ ❌ エラー出る
👉 throw された後、どこにも catch が無い

## チェーンのcatch がある場合
```js
Promise.resolve()
  .then(() => {
    throw new Error("boom");
  })
  .catch(() => {
    console.log("回収");
  });
```
→ ⭕ エラー出ない
👉 catch があるから

## 流れ
①Promise が reject される
②「これ catch されるかな？」と 少し待つ
③最後まで catch が付かなかった
④エラー表示

## まとめ
「Promise でエラーが出るのは、**reject されたのに、最後まで catch が無かった時**だけ」

| 状態                       | 結果        |
| ------------------------ | --------- |
| reject + catch あり        | エラー出ない    |
| reject + catch なし        | **エラー出る** |
| チェーン途中で throw → catch あり | エラー出ない    |
| チェーン途中で throw → catch なし | **エラー出る** |

👉 エラー = 未処理の reject