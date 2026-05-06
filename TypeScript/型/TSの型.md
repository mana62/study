# 型一覧
## ①`boolean`（true or false）
```ts
let isOpen: boolean = true;
```
**使いどころ**
- ON/OFF
- YES/NO

## ②`number`（数値）
```ts
let age: number = 20;
```
- 整数も小数も同じ

## ③`string`
```ts
let name: string = "Taro";
```
- "..."（ダブル）
- '...'（シングル）
- `...`（バッククォート）

## ④オブジェクト
= **キーと型のセット（構造）を定義する**
```ts
let user: {
  name: string; // ここに型を書く
  age: number;
} = {
  name: "Taro",
  age: 20
};
```
- 形（構造）を定義する

## ⑤配列
= **同じ種類のデータの集まり**
```ts
let numbers: number[] = [1, 2, 3];
```
または（上下同じ意味になる）
```ts
let names: Array<string> = ["A", "B"];
```
- number[]
- Array<string> のように書く
- ジェネリクス：「型を後から入れられる型」という

## ⑥Enum型（あまり使われない）
= **選択肢を限定する**
= **「型と値の両方を作る」**

```ts
enum Status {
  Pending,
  Approved,
  Rejected
}

let state: Status = Status.Pending;
```
- 決まった値しか使えない

```ts
state = "Pending"; // ❌
```
= `enum Status`に存在しない値を書くとエラーになる

## ⑦tuple型（タプル）
＝ **順番と型が決まっている配列**

[普通の配列]
```ts
let arr: string[] = ["A", "B", "C"];
```
- 全部同じ型
- 順番は意味を持たない

[tuple型]
```ts
let user: [string, number] = ["Taro", 20];
```
- 1番目：string
- 2番目：number
- **順番が超重要**
- `: [string, number]`の型通りに書かないとダメ
- ⚠️pushすると、型通りでなくても通ってしまうことがある（参照するときにエラーになる）

## ⑧any型
＝ **なんでも入れられる**
＝ 基本は使わない、なんでも入れられてしまうため

```ts
let anyhting: any = true;
anyhting = 'hello';
anyhting = ['hello', 33, true];
anyhting = {};
```

## ⑨Union型
＝ **複数の型を指定できる**

＝ `|`を使って、型を区切る

```ts
let unionType: number | string = 10; // numberもstringも入れられるようになる
```

#### Union型の配列を指定したい場合
- ()の中に型を書き、右側に`[]`を付ける
```ts
let unionType: (number | string)[] = [20, 'A'];
// ()の中に定義する
```

## ⑩リテラル型
＝ **特定の決まった値だけを許す型**のこと

```ts
// 例
let apple: "APPLE" = "APPLE"; // ← "APPLE" しか入れられない
```

#### ① リテラル型は **値そのもの** を型として扱う
```ts
let x: "HELLO" = "HELLO"; // "HELLO" 以外は全部エラー
```

#### ② const で宣言すると自動でリテラル型になる
```ts
const a = "APPLE";
// 型は "APPLE"（リテラル型）

let b = "APPLE";
// 型は string（書き換え可能だから）
```

#### ③ ユニオン型と組み合わせると便利
ENUM の代わりにこう書ける：

```ts
type Size = "S" | "M" | "L";
// → "S" "M" "L" の3つしか入れられない
```
- ENUM と違って **値を生成しない**
- ENUM は JS に変換するとオブジェクトができるけど、**リテラル型＋ユニオン型は 型だけで軽い**
- ENUM よりリテラル型ユニオンの方がシンプルで推奨されることが多い

| 概念 | 意味 |
|------|------|
| リテラル型 | `"APPLE"` のように **特定の値だけ許す型** |
| const | **自動でリテラル型になる**（値が固定されるため） |
| let | **string など広い型になる**（再代入できるため） |
| ユニオン型 | `"S" | "M" | "L"` のように **複数のリテラル型をまとめる型** |
| ENUM との違い | ENUM は **JS にオブジェクトが生成される**が、リテラル型は **型だけで軽量** |

## ⑪typeエイリアス
＝ **型に名前をつける機能**
＝ **よく使う型に名前をつけて再利用したい時** に使う
＝ 型に名前をつけることで、**読みやすく・再利用しやすくする**

```ts
// 例１
type Size = "S" | "M" | "L";
// こうしておくと
function order(size: Size) {}
// → "S" | "M" | "L" を毎回書かなくて済む
```

```ts
// 例２
type UserId = number;

function getUser(id: UserId) {}
function deleteUser(id: UserId) {}
// → number を何度も書くより UserId と書いた方が意味が明確
```

```ts
// 例３
type User = {
  id: number;
  name: string;
  age: number;
};
// これを使えば
function printUser(user: User) {}
// → 読みやすいし、再利用できる
```

## ⑫関数に型をつける方法
＝ 関数に型をつける箇所は **引数** と **戻り値**
＝ **引数には必ず型をつける**（引数に型を付けない場合、自動的にany型になってしまうため）

```ts
function add(num1: number, num2: number): number {
  return num1 + num2;
}
add(5, 3); // ここでnumber以外入れるとエラー
```

## ⑬関数に型をつける方法（void）
＝ **「何も返さない関数」の戻り値の型**
＝ **この関数は何も返さないよ** と TypeScript に伝えたい時に使う
＝ **ログ・イベント・コールバック** など

```ts
function log(message: string): void {
  console.log(message);
}
```

## ⑭undefind型とnull型
- undefined → 変数が「未定義」（基本使わない）
- null → 開発者が「空です」と明示する値

## ⑮関数を入れる変数に型を付ける
＝ **この変数には こういう関数 しか入れちゃダメだよ**と TypeScript に教える書き方

```ts
const otherAdd: (n1: number, n2: number) => number = add;
```

⚠️ 変数側で、型を指定する場合の戻り値は **コロンではなく`=>`を使う**
関数側であれば戻り値は **コロン**を使う

#### いつ使う？
- コールバック関数を受け取るとき
- 関数を変数に入れて渡すとき
- 関数の型を統一したいとき

#### 例
```ts
type CalcFn = (a: number, b: number) => number;

const add: CalcFn = (a, b) => a + b;
const sub: CalcFn = (a, b) => a - b;
```
- → CalcFn 型を使うことで、関数の形を統一できる
- → 間違った関数を入れたらエラーになるので安全

## ⑮アロー関数の型付け
- **変数に関数の型を付けるときだけ () が必要**
```ts
const doubleNum = (num: number): number => num * 2;
```
#### どこに型をつける？
引数：`num: number` ⚠️()で囲む必要がある
戻り値：`): number`

#### いつ使う？
- 短い処理
- コールバック
- 関数を値として扱うとき

## ⑯コールバック関数の型
- **変数に 関数の型を付けるときだけ () が必要**
```ts
function call(num: number, callback: (num: number) => number): void {
  const doubleNum = cb(num * 2);
  console.log(num * 2);
}
```
#### callback の型 (num: number) => number の意味
- callback は 引数に number を受け取る
- callback は number を返す

#### いつ使う？
- 処理を外から注入したいとき
- 柔軟に動作を変えたいとき

#### 呼び出し側
```ts
call(21, doubleNum => {
  return doubleNum;
});
```

## ⑰unknown 型（any より安全）
＝ **unknown は「型がわからない」ことを明示する型**
- unknown を使うときは **「使う前に型チェックが必要」**
- ただし、typeof である必要はない（Array.isArray, in, カスタム型ガードなどもOK）

```ts
let x: unknown = "hello";
// x は「何かわからない」扱い
x.toUpperCase(); // ❌ エラー

// 👉 使うにはチェックが必要
if (typeof x === "string") {
  x.toUpperCase(); // OK
}
```

#### anyとunknown の違い

| 型       | 特徴                     |
|----------|---------------------------|
| any      | なんでも入る             |
|          | なんでも使える           |
|          | 危険（型安全がない）     |
| unknown  | なんでも入る             |
|          | 使う前にチェック必須     |
|          | 安全（型チェックが必要） |

#### いつ使う？
- 外部から来る値（API、フォーム入力など）
- 型がわからないけど、勝手に使わせたくないとき

#### 例
```ts
let input: unknown = "hello";

if (typeof input === "string") {
  console.log(input.toUpperCase()); // OK
}
```

## ⑱satisfies 演算子
＝ satisfies は **型推論を維持したまま、型チェックだけ追加する** 仕組み
＝ **左側の値が右側の型に合っているかチェックする**
👉 型注釈より柔軟で安全

```ts
28 satisfies number
```
#### いつ使う？
- オブジェクトの型チェックを厳密にしたいとき
- 型推論を壊さずに型チェックしたいとき

#### 例
```ts
const user = {
  id: 1,
  name: "Taro",
} satisfies {
  id: number;
};
```
＝id が number かチェックする。でも name は 推論されたまま残る

- → 型チェックはするけど、型推論はそのまま維持される
- → as より安全

## ⑲never 型
＝ **「絶対に戻ってこない」関数の戻り値の型**
```ts
function error(message: string): never {
  throw new Error(message);
}
```
#### いつ使う？
- throw で例外を投げる
- 無限ループ

#### void と never の違い

| 型    | 意味                               |
|-------|------------------------------------|
| void  | 何も返さない（return しない）      |
| never | そもそも return に到達しない       |

例
```ts
function log(): void {
  console.log("hello"); // 終わる
}

function error(): never {
  throw new Error("err"); // 終わらない
}
```

#### 「型推論だと void になる」理由
```ts
const fn = () => {
  throw new Error("err");
};
```
- → TS は「throw しかないから never だな」と推論する
- → ただし、関数宣言だと void と推論されることがある

#### 「関数式だと never になる」理由
```ts
const fn = function () {
  throw new Error("err");
};
```
- → 関数式は **式の結果** を重視するため、never と推論されやすい

## まとめ
🔹 基本の型
```bash
boolean → true/false（スイッチ）
number → 数値（整数・小数）
string → 文字列
undefined → 値が「未定義」
null → 値が「空」であることを明示
any → なんでも入る（基本使わない）
unknown → なんでも入るが、使う前にチェックが必要（安全な any）
```

🔹 複合型
```bash
オブジェクト → キーと型で構造を定義する
配列 → 同じ型 or 許可された型の集まり
tuple（タプル） → 順番と型が決まった配列（ただし push できるので完全固定ではない）
enum → 限定された値（＋型と値を作る）
ユニオン型 → A | B のように複数の型を許可
リテラル型 → "APPLE" のように特定の値だけ許す型
type エイリアス → 型に名前をつけて再利用する
```

🔹 関数の型
```bash
引数に型をつける（必須）
戻り値に型をつける
void → 何も返さない関数
関数型のエイリアス
```

```ts
type Add = (a: number, b: number) => number;
```

🔹 その他の重要な型
```bash
never
→ 絶対に戻らない関数（例：例外を投げる関数）
unknown
→ any より安全な「なんでも入る型」
readonly
→ 書き換え禁止のプロパティ
as const
→ 配列やオブジェクトをリテラル型として固定する
```