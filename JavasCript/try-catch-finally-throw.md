# try / catch / finally / throw
＝ 「エラーが起きたときの流れをコントロールする仕組み」
⚫︎try：ここでエラーが出るかもしれない
⚫︎catch：エラーが出たらここで処理する
⚫︎finally：エラーが出ても出なくても最後に必ず実行
⚫︎throw：自分でエラーを発生させる

## ① try：エラーが起きるかもしれない処理を囲む
```js
try {
  riskyFunction(); // ここでエラーが出るかも
}
```
「この中で何か起きても止まらないようにしたい」ときに使う

## ② catch：エラーが起きたときの処理を書く
```js
try {
  riskyFunction();
} catch (error) {
  console.log("エラー発生:", error);
}
```
エラーが起きたら catch に飛ぶ
エラーが起きなければ catch はスキップされる

## ③ finally：エラーの有無に関係なく必ず実行される
```js
try {
  riskyFunction();
} catch (error) {
  console.log("エラー:", error);
} finally {
  console.log("必ず実行される");
}
```
例えば：
①ファイルを閉じる
②ローディング表示を消す
③DB 接続を切る
など「絶対にやりたい後処理」に使う

## ④ throw：自分でエラーを発生させる
```js
if (age < 0) {
  throw new Error("年齢は0以上にしてください");
}
```
throw は 「ここで処理を止めて、エラーとして外に投げる」 ための文
その関数の処理は即終了
呼び出し元にエラーが伝わる
try/catch があればそこで捕まる

## まとめた例
```js
function divide(a, b) {
  if (b === 0) {
    throw new Error("0で割ることはできません");
  }

  return a / b;
}

try {
  const result = divide(10, 0);
  console.log(result);
} catch (error) {
  console.log("エラーをキャッチ:", error.message);
} finally {
  console.log("処理終了");
}
```
実行結果 :
コード
エラーをキャッチ: 0で割ることはできません
処理終了


## まとめ
⚫︎try：危ない処理を囲む
⚫︎catch：エラーが起きたらここで処理
⚫︎finally：最後に必ず実行
⚫︎throw：自分でエラーを発生させる