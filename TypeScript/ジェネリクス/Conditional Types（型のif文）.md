# Conditional Types（型のif文）
- **三項演算子**のように **「この型なら〇〇、そうでなければ××」 と型を分岐する仕組み**

## 定型文
```ts
type Result<T> = T extends 条件 ? 真の型 : 偽の型;
```

## 例
```ts
// "tomato" は string に代入できる？ → できる → number
type A = "tomato" extends string ? number : boolean;
// A = number

// 42 は string に代入できる？ → できない → boolean
type B = 42 extends string ? number : boolean;
// B = boolean
```

### ① 型が string なら true、違うなら false
```ts
type IsString<T> = T extends string ? true : false;
```

使用例：
```ts
type A = IsString<string>; // true
type B = IsString<number>; // false
```

### ② 型が string ならそのまま返す、違うなら never
```ts
type OnlyString<T> = T extends string ? T : never;
```

使用例：
```ts
type A = OnlyString<string>; // string
type B = OnlyString<number>; // never
```

### ③ 関数の戻り値の型を取り出す（infer の定型文）
- infer：**条件の中で型を取り出して名前をつける**
⚠️ infer は「Conditional Types の中でだけ使える」特別なキーワード、単体では使えない
- 「型の中を分解して、一部を取り出して名前をつける」道具
- パターンが決まっているので、まず形を丸暗記→意味を後で理解する

```ts
// infer無し
type Check = T extends (...args: any[]) => number ? "数値返す" : "違う";
// Fn が「何かを返す関数」かどうかだけチェックできる
```

```ts
// inferあり
type MyReturnType =
  T extends (...args: any[]) => infer R ? R : never;
//                               ↑「返り値の型」を R という名前で取り出す
```

```ts
type MyReturnType<T> =
  T extends (...args: any[]) => infer R ? R : never;
```

使用例：
```ts
type Fn = () => number;
type R = MyReturnType<Fn>; // number
```

### ④ Promise の中身の型を取り出す（infer の定型文）
```ts
type UnwrapPromise<T> =
  T extends Promise<infer R> ? R : T;
```

使用例：
```ts
type A = UnwrapPromise<Promise<string>>; // string
```

### ⑤ 配列の要素の型を取り出す（infer の定型文）
```ts
type ElementOf<T> =
  T extends (infer U)[] ? U : T;
```

使用例：
```ts
type A = ElementOf<string[]>; // string
```

### ⑥ オブジェクトのプロパティの型を取り出す（infer）
```ts
type ValueOf<T> =
  T extends { [key: string]: infer R } ? R : never;
```

使用例：
```ts
type Obj = { a: number; b: string };
type V = ValueOf<Obj>; // number | string
```

### ⑦ Distributive Conditional Types（ユニオンに分配される）
- ユニオン型に Conditional Types を使うと、自動で一個ずつ処理してくれる

```ts
type ToBoolean<T> = T extends string ? true : false;
```

使用例：
```ts
type A = ToBoolean<"a" | "b" | 1>;
// = true | true | false
```

### ⑧ null / undefined を除外する（NonNullable の定型文）
```ts
type MyNonNullable<T> =
  T extends null | undefined ? never : T;
```

使用例：
```ts
type A = MyNonNullable<string | null>; // string
```

### ⑨ 型が特定の構造を持っていたら、その一部を infer で抜き出す
```ts
type ExtractTomato<T> =
  T extends { tomato: infer R } ? R : never;
```

使用例：
```ts
type A = ExtractTomato<{ tomato: "red" }>; // "red"
```

## infer
- **「型の中から一部を取り出して、名前をつける仕組み」**
- extends の中限定で使える
- 全パターンに共通する「infer の読み方」は1つだけ
- **T extends 〇〇<infer R> = 「T が 〇〇 という形をしているなら、その中の型を R として取り出す」**

| パターン名 | 形（テンプレ） | 取り出せるもの |
|------------|----------------|----------------|
| ③ 関数の戻り値 | `T extends (...args: any[]) => infer R ? R : never` | 関数の戻り値の型 |
| ④ Promise の中身 | `T extends Promise<infer R> ? R : T` | Promise が解決する型 |
| ⑤ 配列の要素 | `T extends (infer U)[] ? U : T` | 配列の要素の型 |
| ⑥ オブジェクトの値 | `T extends { [key: string]: infer R } ? R : never` | オブジェクトの全値のユニオン |
| ⑨ 特定キーの値 | `T extends { tomato: infer R } ? R : never` | 特定キー（tomato）の値の型 |

## まとめ
```ts
// 基本形
type Cond<T> = T extends 条件 ? 真 : 偽;

// string 判定
type IsString<T> = T extends string ? true : false;

// string だけ通す
type OnlyString<T> = T extends string ? T : never;

// 関数の戻り値を取り出す
type MyReturnType<T> =
  T extends (...args: any[]) => infer R ? R : never;

// Promise の中身を取り出す
type UnwrapPromise<T> =
  T extends Promise<infer R> ? R : T;

// 配列の要素を取り出す
type ElementOf<T> =
  T extends (infer U)[] ? U : T;

// オブジェクトの値の型を取り出す
type ValueOf<T> =
  T extends { [key: string]: infer R } ? R : never;

// null / undefined を除外
type MyNonNullable<T> =
  T extends null | undefined ? never : T;

// 特定プロパティの型を抽出
type ExtractTomato<T> =
  T extends { tomato: infer R } ? R : never;
```