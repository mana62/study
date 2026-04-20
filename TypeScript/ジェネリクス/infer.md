# infer
- infer = **「型を取り出して、名前をつける」**
- 条件型の中で「ここの型が何であるか、TypeScriptに推測させて、その型を変数に入れる」という機能

## infer の定型文
```ts
① 関数の戻り値を取り出す
ts
// T が関数なら、その「戻り値の型」だけを取り出す
type 戻り値を取り出す<T> =
  T extends (...args: any[]) => infer 戻り値 ? 戻り値 : never;
② 関数の引数（1つ）を取り出す
ts
// T が「引数1つの関数」なら、その引数の型を取り出す
type 引数1つ<T> =
  T extends (infer 引数) => any ? 引数 : never;
③ 関数の引数（複数）をタプルで取り出す
ts
// T が関数なら、その「すべての引数」をタプルとして取り出す
type 引数全部<T> =
  T extends (...args: infer 引数タプル) => any ? 引数タプル : never;
④ コンストラクタ（new）の戻り値を取り出す
ts
// new で生成される「インスタンスの型」を取り出す
type インスタンス型<T> =
  T extends new (...args: any[]) => infer インスタンス ? インスタンス : never;
⑤ Promise の中身を取り出す
ts
// Promise<T> の T の部分だけを取り出す
type 中身を取り出す<T> =
  T extends Promise<infer 中身> ? 中身 : T;
⑥ 配列の要素を取り出す
ts
// 配列の「要素の型」を取り出す
type 要素<T> =
  T extends (infer 要素型)[] ? 要素型 : never;
⑦ タプルの先頭を取り出す
ts
// タプルの「最初の要素」を取り出す
type 先頭<T> =
  T extends [infer 最初, ...any[]] ? 最初 : never;
⑧ タプルの末尾を取り出す
ts
// タプルの「最後の要素」を取り出す
type 末尾<T> =
  T extends [...any[], infer 最後] ? 最後 : never;
⑨ オブジェクトの値を取り出す
ts
// オブジェクトの「値の union 型」を取り出す
type 値<T> =
  T extends { [key: string]: infer V } ? V : never;
⑩ オブジェクトのキーを取り出す
ts
// オブジェクトの「キーの union 型」を取り出す
type キー<T> =
  T extends { [K in infer Key]: any } ? Key : never;
⑪ 文字列の一部を取り出す（URL の ID 抽出）
ts
// "/users/◯◯" の「◯◯」の部分を取り出す
type ユーザーID抽出<T> =
  T extends `/users/${infer ID}` ? ID : never;
⑫ 文字列の複数部分を取り出す
ts
// "A/B" の A と B を取り出してタプルにする
type 分割<T> =
  T extends `${infer A}/${infer B}` ? [A, B] : never;
```

## 通常の条件型
```ts
// 「AがBを extends するなら X、しないなら Y」
type IsString<T> = T extends string ? "文字列だ" : "違う";

type A = IsString<string>;  // "文字列だ"
type B = IsString<number>;  // "違う"
```

## 例
```ts
type ExtractId<T> = T extends `/users/${infer Id}` ? Id : never;

type A = ExtractId<"/users/123">; // "123"
```
① `type ExtractId<T> = ...`
これは ジェネリック型
T に渡された文字列から ID を取り出すための型

② `T extends \/users/${infer Id}\`
ここが一番重要
意味：
T が /users/◯◯ という形に一致するなら、
その "◯◯" の部分を Id に入れてね

/users/ は固定文字列
${infer Id} は テンプレートリテラル型のパターンマッチ
infer Id は 一致した部分を型変数 Id に入れる

例：
"/users/123" が渡されたら
/users/ → 一致
${infer Id} → "123" が入る

つまり：
コード
Id = "123"

③ `? Id : never`
これは 条件型の三項演算子
パターンに一致したら → Id を返す
一致しなかったら → never を返す

例：
```ts
ExtractId<"/users/123">  // "123"
ExtractId<"/items/999">  // never（パターン不一致）
```

④ `type A = ExtractId<"/users/123">`
ここで実際に型を適用
T = "/users/123"
パターン /users/${infer Id} に一致

Id = "123"
結果：
type A = "123"

## infer
- **「extends でパターンマッチするとき、途中の型を変数に捕まえる」のが infer**

```ts
// 「もし T が Promise<何か> なら、その "何か" を取り出す」
type UnwrapPromise<T> = T extends Promise<infer U> ? U : T;
//                                           ↑
//                           "何か" を U という名前で捕まえる

type A = UnwrapPromise<Promise<string>>;  // string
type B = UnwrapPromise<Promise<number>>;  // number
type C = UnwrapPromise<boolean>;          // boolean（Promiseじゃないのでそのまま）
```
**infer U は「この部分の型を U という名前にしてね」という宣言**


### ① 関数の戻り値の型を取り出す
```ts
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
//                                                      ↑
//                                              戻り値の型を R に捕まえる

function greet(): string { return "hello"; }
function add(): number { return 42; }

type A = ReturnType<typeof greet>;  // string
type B = ReturnType<typeof add>;    // number
```

### ② 配列の要素の型を取り出す
```ts
type ElementType<T> = T extends (infer E)[] ? E : never;
//                                     ↑
//                             配列の要素型を E に捕まえる

type A = ElementType<string[]>;   // string
type B = ElementType<number[]>;   // number
```

### ③ 関数の引数の型を取り出す
```ts
type FirstArg<T> = T extends (first: infer A, ...rest: any[]) => any ? A : never;

function foo(x: number, y: string): void {}

type A = FirstArg<typeof foo>;  // number（最初の引数の型）
```

## イメージ
```bash
T extends Promise<infer U>
          ↑↑↑↑↑↑↑↑↑↑↑↑↑↑↑
          パターン

「T が Promise<????> という形だったら、
  ???? の部分を U という名前で覚えておいて、後で使う」
```

## 使いどころ
- ReturnType<T> のようなユーティリティ型
- Promise の中身を取り出す型
- 配列の要素型を抽出する型
- API レスポンスの型から必要な部分だけ抜く
- ライブラリの型を読み解くとき（React, Vue, Prisma など）

## infer の用途

| 用途 | 例 | infer が取り出すもの |
|------|------|----------------------|
| 関数の戻り値 | () => number | number |
| Promise の中身 | Promise<string> | string |
| 配列の要素 | string[] | string |
| タプルの要素 | [number, string] | number / string |
| オブジェクトの値 | { a: 1, b: 2 } | 1 \| 2 |
| 特定キーの型 | { id: number } | number |


## まとめ

| 項目 | 説明 | 何をするもの？ | どこで使う？ | 形（テンプレ） | 何が嬉しい？ |
|------|------|----------------|--------------|----------------|--------------|
| infer とは | 条件型の中で使う「型の変数」 | 型の一部を取り出して X に入れる | 関数の戻り値・Promise の中身・配列の要素などを抽出 | `T extends パターン<infer X> ? X : never` | 複雑な型から内側の型を自動で取り出せる |
| 関数の戻り値 | 関数の return 型を抽出 | `() => number` → `number` | ReturnType<T> の内部実装 | `T extends (...args: any[]) => infer R ? R : never` | 関数の戻り値を手で書かなくてよくなる |
| Promise の中身 | Promise が返す型を抽出 | `Promise<string>` → `string` | API レスポンス型の抽出 | `T extends Promise<infer R> ? R : T` | 非同期処理の型を簡単に扱える |
| 配列の要素 | 配列の中の型を抽出 | `string[]` → `string` | フォーム入力・リスト処理 | `T extends (infer U)[] ? U : T` | 配列の要素型を自動で取得 |
| オブジェクトの値 | 値の union を抽出 | `{a:1,b:2}` → `1 | 2` | 定数オブジェクトの型化 | `T extends { [key: string]: infer R } ? R : never` | 値の型をまとめて取り出せる |
| 特定キーの値 | 特定キーの型を抽出 | `{id:number}` → `number` | API モデルの部分抽出 | `T extends { id: infer R } ? R : never` | 必要なキーだけ取り出せる |


- infer = 型の中身を取り出すための “型の変数”
- 条件型の中でしか使えない
- 複雑な型の “内側” を抜き出すのに最強