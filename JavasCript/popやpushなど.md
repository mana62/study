# push / pop / unshift / shift
＝push / pop / shift / unshift は
「配列専用」じゃなくて「配列っぽいオブジェクト」に対してlength を基準にプロパティを操作している

①push() = ()内に指定した値を配列の末尾に追加
```js
const num = [1,2,3];
const newLength = num.push(4);
// 結果  [1,2,3,4];
```

②pop() = 引数は取らない、末尾の値を一個取り除く
```js
const num = [1,2,3];
const newLength = num.pop();
// 結果  [1,2];
```

③unshift() = ()内に指定した値を配列の先頭に追加
```js
const num = [1,2,3];
const newLength = num.unshift(-1);
// 結果  [-1,1,2,3,];
```

④shift() = 引数は取らない、先頭の値を一個取り除く
```js
const num = [1,2,3];
const newLength = num.shift();
// 結果  [2,3];
```
＝戻り値：取り除いた先頭の値

＝`③unshift()`と`④shift()`は、なるべく使わない

## shift / unshift の内部ルール
「そのインデックスが存在するか？」は`in 演算子`でチェック
- なければ delete
- あれば代入

**繰り返し回数は length 基準**
だから：
- 穴あき配列 → 挙動がややこしく見える
- length が小さい → それ以上は無視


🟣全体を一言で言うと
- 配列メソッドは「配列を操作している」のではなく
「length を持つオブジェクトを仕様どおりにいじっている」だけ