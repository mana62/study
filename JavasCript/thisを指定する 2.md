# thisを指定することができるメソッド（call / apply / bind ）

| メソッド | 何をする？ | いつ使う？ |
|----------|-------------|-------------|
| **call** | this を指定して「即実行」 | 引数がバラバラの時 |
| **apply** | this を指定して「即実行」 | 引数が配列の時 |
| **bind** | this を指定して「新しい関数を作る」 | 後で呼びたい時 |

## thisを固定する
JavaScript の関数は、呼び出し方によって this が変わる

```js
function hello() {
  console.log(this.name)
}
```
この関数をそのまま渡すと、this が迷子になることがある
そこで call / apply / bind を使うと、**「this は絶対にこれ」**と指定できる

## ①call：this を指定して即実行（引数はバラで渡す）
### 使い方
```js
// 第一引数に指定したものがthisの値、第二引数からはパラメータ
func.call(thisArg, arg1, arg2, ...)
```

### 例
```js
function greet(a, b) {
  console.log(this.name, a, b)
}

greet.call({ name: "Hanako" }, "こんにちは", "！") // 文字列
// → Hanako こんにちは ！
```
=即実行される
=引数は 1つずつ 渡す

## ②apply：this を指定して即実行（引数は配列で渡す）
### 使い方
```js
// 引数は配列で渡す
func.apply(thisArg, [arg1, arg2, ...])
```

### 例
```js
function greet(a, b) {
  console.log(this.name, a, b)
}

greet.apply({ name: "Hanako" }, ["こんにちは", "！"]) // 配列
// → Hanako こんにちは ！
```

=即実行される
=引数は 配列で渡す
=call とほぼ同じだが、配列をそのまま渡したい時に便利

## ③bind：this を指定して「新しい関数を作る」
### 使い方
```js
const newFunc = func.bind(thisArg, arg1, arg2, ...)
```

### 例
```js
function greet(a, b) {
  console.log(this.name, a, b)
}

const greetHanako = greet.bind({ name: "Hanako" }, "こんにちは")
greetHanako("！")
// → Hanako こんにちは ！

- bind は「this と"先頭の引数"だけを固定して、残りの引数は後から渡せるようにする」仕組み
- bind の第2引数以降 → "先に決めておく引数"
- bind が返す関数の引数 → "後から渡す引数"
```
=即実行されない
=this が固定された新しい関数を返す
=React クラスコンポーネントなどでよく使われる

---
⚫︎call：this を指定してすぐ実行（引数はバラ）
⚫︎apply：this を指定してすぐ実行（引数は配列）
⚫︎bind：this を指定して後で実行する関数を作る
⚫︎アロー関数の場合は、固定されずグローバルオブジェクトを指す（アロー関数はthisを持たない）

---

| メソッド | this を指定 | 引数を渡す | 引数を固定（部分適用） | 実行タイミング |
|----------|-------------|------------|-------------------------|----------------|
| **call** | できる | バラで渡す | ❌ できない | 即実行 |
| **apply** | できる | 配列で渡す | ❌ できない | 即実行 |
| **bind** | できる | バラで渡す | ⭕ できる（部分適用） | 実行しない（関数を返す） |