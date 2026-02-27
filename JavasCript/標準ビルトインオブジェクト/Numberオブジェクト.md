# MAX_VALUE と Number.MAX_SAFE_INTEGER

## 「Number.MAX_SAFE_INTEGER」
＝整数として、絶対に正確に計算できる最大の数
```js
Number.MAX_SAFE_INTEGER

// 結果
9007199254740991

// つまり
2^53 − 1
```

## 「安全」の意味
- 1を足したらちゃんと1増える
- 計算結果が正確に保証される
という意味

## 整数が何桁まで正確に持てるか
＝JavaScriptが正確に扱える整数の限界は: 2進数で53桁まで

```bash
2^53 − 1 まで
```

これ以上になると、バグが起きる
```bash
console.log(Number.MAX_SAFE_INTEGER);
// 9007199254740991

console.log(Number.MAX_SAFE_INTEGER + 1);
// 9007199254740992

console.log(Number.MAX_SAFE_INTEGER + 2);
// 9007199254740992 ←え？？？
```

つまり:
| 数                    | 結果        |
| -------------------- | --------- |
| MAX_SAFE_INTEGERまで   | 正確        |
| MAX_SAFE_INTEGERを超える | 1ずつ増えなくなる |
| さらに大きいと              | 2ずつ、4ずつ飛ぶ |

## 「MAX_VALUE」
## ① JavaScriptのNumberには上限がある
JSの数字は無限に大きくできない

```js
Number.MAX_VALUE
// 1.7976931348623157e+308
```
→ これは、JSで表現できる「最大の有限の数」

## ② なぜ上限があるのか

理由：JavaScriptのNumberは全部「64bitの浮動小数点」で保存されるから

つまり：
- メモリが64bitしかない
- 表せる範囲が決まっている
- IEEE 754（倍精度浮動小数点）という世界標準ルール

JSだけじゃなく
- Python
- Java
- C
- Ruby
も同じ仕組み

「中身の構造」
| 部分 | 内容           |
| -- | ------------ |
| 符号 | ±            |
| 指数 | どれくらい大きいか    |
| 仮数 | 数字の細かい部分（精度） |


## ③ MAX_VALUEを超えるとどうなるのか
```js
Number.MAX_VALUE * 2
// Infinity
```

上限を超えたら**無限大（Infinity）**になる

## Infinityとは？
＝Infinityは**特別な値**

```js
1 / 0  // Infinity
```

InfinityもNumber型
```js
typeof Infinity
// "number"
```

## NaNとは？
＝NaNは、**数字じゃないものを数字にしようとした結果**
＝not a number

```js
Number("abc")
// NaN
```

## 最小の正の数もある
```js
Number.MIN_VALUE
```
これは、0より大きい最小の数

```js
Number.MIN_VALUE
// 5e-324
```

## 重要なのは下記
JSのNumberには限界がある
| プロパティ              | 意味     |
| ------------------ | ------ |
| `Number.MAX_VALUE` | 最大の有限数 |
| `Number.MIN_VALUE` | 最小の正の数 |
| `Infinity`         | 上限超え   |
| `NaN`              | 数字じゃない |


## MAX_VALUE と Number.MAX_SAFE_INTEGER の違い

| 名前                      | 意味              |
| ----------------------- | --------------- |
| Number.MAX_VALUE        | 表現できる最大の数（超巨大）  |
| Number.MAX_SAFE_INTEGER | 整数として安全に計算できる最大 |


## 🟣これだけ抑えればOK
**JavaScriptのNumberには限界がある**
特にめちゃくちゃ大きい整数は正確に扱えないことがある


安全なのはここまで
```js
Number.MAX_SAFE_INTEGER
// 9007199254740991
```
これを超えると
- 1足しても増えない
- 計算がズレる ことがある

## 小数について
＝JavaScriptの小数は正確に保存できないことがある

```js
0.1 + 0.2

// 結果
0.30000000000000004 になる
```

## なぜか
理由：0.1や0.2は2進数でキレイに表せないから

## 対処法
### 対処法① 表示だけ丸める（toFixed）
＝小数の計算結果を「見た目として」整えたいときに使う

```js
const result = 0.1 + 0.2;
console.log(result.toFixed(1));
// "0.3"
```
⚠️注意：toFixedは文字列を返す

```js
Number(result.toFixed(1));
// 0.3
```

### 対処法② 整数にして計算する（最も安全）
＝お金など正確さが必要な計算では、小数を使わず整数で扱う

```js
const result = (10 + 20) / 100;

console.log(result);
// 0.3
```
例：円ではなく「銭」で計算するイメージ

### 対処法③ 誤差を許して比較する
＝小数同士をそのまま === で比較するのは危険

```js
const result = 0.1 + 0.2;

console.log(Math.abs(result - 0.3) < 1e-10);
// true
```

## まとめ
| 目的      | 対処法         |
| ------- | ----------- |
| 表示を整える  | `toFixed()` |
| 正確に計算する | 整数で扱う       |
| 比較したい   | 誤差を使う       |


「JavaScriptの小数は2進数で正確に表せないため誤差が出る」
対処法は以下：
- 表示は toFixed() で丸める
- 計算は整数で行うのが安全
- 比較は誤差を許して判定する

## NaNにつてい

JSには下記のような特殊な値が存在する
| 値         | 意味           |
| --------- | ------------ |
| NaN       | 数字じゃない（計算失敗） |
| Infinity  | 無限（でかすぎ）     |
| -Infinity | マイナス無限       |

それを判定するための道具が
`Number.isNaN()`
`Number.isFinite()`
の２つ

### ① Number.isNaN() とは？
＝NaNかどうかを調べる
```js
Number.isNaN(NaN); // true
Number.isNaN(123); // false
```
#### NaNとは
例えばこういう計算ミスで出る:

```js
0 / 0        // NaN
"abc" * 3    // NaN
Math.sqrt(-1)// NaN
```

### なぜ必要？
NaNは特別でこうなる:
```js
NaN === NaN  // false
```

つまり比較できない
＝だから`Number.isNaN()`でしか判定できない

### ② Number.isFinite() とは？
＝「普通の数か？」を調べる

```js
Number.isFinite(100);      // true
Number.isFinite(Infinity); // false
Number.isFinite(NaN);      // false
```

有限（finite）＝普通の数字
無限（infinite）＝Infinity

## まとめ
NaNは「数字じゃない値」
NaN判定には Number.isNaN() を使う


| メソッド              | 優先度         |
| ----------------- | ----------- |
| Number.isNaN()    | ★★★★☆（よく使う） |
| Number.isFinite() | ★★☆☆☆（たまに）  |
