# Mapped Types（型のfor文）
- オブジェクトの全プロパティに対して、**同じ変換を一括でかける仕組み**
- プログラムの **for 文と同じイメージ**

## 定型文
```ts
type NewType<T> = {
  [K in keyof T]: T[K];
};
```
- これが 型の for 文の最小テンプレ
- keyof T → T の全プロパティ名を取り出す
- [K in keyof T] → そのプロパティを1つずつ回す（for 文）
- T[K] → 元の型のプロパティの型をそのまま使う

## 例
```ts
// Vegetable の全プロパティを string に変換する例
type Vegetable = 'tomato' | 'pumpkin' | 'carrot';

type VegeMap = {
  [P in Vegetable]: string
  // ↑「Vegetable の一個一個に対して」「string 型のプロパティを作る」
  // P はなんでもOK
}
// 結果: { tomato: string; pumpkin: string; carrot: string; }
```

### ① 全プロパティをオプショナルにする（Partial の自作版）
```ts
type MyPartial<T> = {
  [K in keyof T]?: T[K];
};
```
- `[K in keyof T]?` ここに?を付ける

### ② 全プロパティを必須にする（Required の自作版）
```ts
type MyRequired<T> = {
  [K in keyof T]-?: T[K];
};
```
- `-?` は **「? を外す」**という意味

### ③ 全プロパティを readonly にする（Readonly の自作版）
```ts
type MyReadonly<T> = {
  readonly [K in keyof T]: T[K];
};
```
- 最初に `readonly [K in keyof T]` を付ける

### ④ readonly を外す（Mutable の自作版）
```ts
type MyMutable<T> = {
  -readonly [K in keyof T]: T[K];
};
```
- マイナス ` -readonly` を付ける

### ⑤ 全プロパティの型を別の型に変換する
```ts
type ToStringProps<T> = {
  [K in keyof T]: string;
};
```

例：
```ts
type User = { name: string; age: number };
type UserString = ToStringProps<User>;
// { name: string; age: string }
```

### ⑥ プロパティ名を変換する（Key Remapping）
```ts
type PrefixKeys<T> = {
  [K in keyof T as `prefix_${string & K}`]: T[K];
};
```

例：
```ts
type User = { name: string; age: number };
type Prefixed = PrefixKeys<User>;
// { prefix_name: string; prefix_age: number }
```

### ⑦ 特定のプロパティだけを変換する
```ts
type OptionalName<T> = {
  [K in keyof T]: K extends "name" ? T[K] | undefined : T[K];
};
```

### ⑧ プロパティを削除する（Omit の自作版）
```ts
type MyOmit<T, K extends keyof T> = {
  [P in keyof T as P extends K ? never : P]: T[P];
};
```

### ⑨ プロパティを抽出する（Pick の自作版）
```ts
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

## まとめ
```ts
// 基本形
type Map<T> = {
  [K in keyof T]: T[K];
};

// オプショナル化
type MyPartial<T> = {
  [K in keyof T]?: T[K];
};

// 必須化
type MyRequired<T> = {
  [K in keyof T]-?: T[K];
};

// readonly 化
type MyReadonly<T> = {
  readonly [K in keyof T]: T[K];
};

// readonly を外す
type MyMutable<T> = {
  -readonly [K in keyof T]: T[K];
};

// 型を変換
type ToStringProps<T> = {
  [K in keyof T]: string;
};

// キー名を変換
type PrefixKeys<T> = {
  [K in keyof T as `prefix_${string & K}`]: T[K];
};

// プロパティ削除
type MyOmit<T, K extends keyof T> = {
  [P in keyof T as P extends K ? never : P]: T[P];
};

// プロパティ抽出
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

⚠️ マイナス（-）は「その修飾子を削除する」という意味です。-readonly で readonly を取り除き、-? でオプショナルを取り除く

⚠️ [P in ...] が for文の代わりで、全プロパティに一括変換を適用する
-readonly や -? で修飾子を「消す」こともできる