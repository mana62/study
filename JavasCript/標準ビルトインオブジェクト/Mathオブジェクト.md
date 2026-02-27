# Mathオブジェクト
＝数学の計算をするための機能が入ったオブジェクト

## よく使うMathメソッド
### ① Math.floor()（切り下げ）
小数を下に丸める
```js
Math.floor(3.9);
// 3
```

マイナスが注意！
```js
Math.floor(-3.2);
// -4
```

床（floor）なので下に落ちる

### ② Math.trunc()（小数を消すだけ）
```js
Math.trunc(3.9);
// 3

Math.trunc(-3.9);
// -3
```

単純に小数部分を消す

### ③ Math.ceil()（切り上げ）
```js
Math.ceil(3.1);
// 4

Math.ceil(-3.9);
// -3
```

天井（ceil）なので上に上がる

### ④ Math.round()（四捨五入）
```js
Math.round(3.4);
// 3

Math.round(3.5);
// 4
```

マイナスも普通に四捨五入
```js
Math.round(-3.5);
// -3
```

### ⑤ Math.random()（ランダム）
0以上1未満の乱数
```js
Math.random();
// 例：0.48392
```
よくある使い方：
①1〜10のランダム整数
```js
Math.floor(Math.random() * 10) + 1;
```

② 0〜1 の小数
```js
javascriptMath.random() // → 0.567... (0以上1未満)
```

② 0〜10 の小数
```js
javascriptMath.random() * 10 // → 5.67... (0以上10未満)
```

③ 1〜10 の小数
```js
javascriptMath.random() * 10 + 1 // → 6.67... (1以上11未満)
```

④ 1〜10 の整数
```js
javascriptMath.floor(Math.random() * 10 + 1) // → 6 (1以上10以下)
```

⑤ min 以上 max 以下のランダム整数
````js
Math.floor(Math.random() * (max - min + 1)) + min // これはよく使われる
```

### ⑥ Math.max()（最大値）
```js
Math.max(3, 10, 7);
// 10
```

### ⑦ Math.min()（最小値）
```js
Math.min(3, 10, 7);
// 3
```

## まとめ
| やりたいこと | 書き方           |
| ------ | ------------- |
| 小数切り下げ | Math.floor()  |
| 小数切り上げ | Math.ceil()   |
| 四捨五入   | Math.round()  |
| 小数を消す  | Math.trunc()  |
| ランダム   | Math.random() |
| 最大値    | Math.max()    |
| 最小値    | Math.min()    |