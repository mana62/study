# LookUp型
- **オブジェクト型から特定のキーの型だけを取り出す**仕組み
- 指定したプロパティの型を取り出す
- 書くときは**基本的に type を使う**

```ts
type T = SomeType["key"];
```
→ この **"key" の部分に指定したプロパティの 型だけを取り出す**

例：
```ts
type User = {
  id: number;
  name: string;
  isAdmin: boolean;
};

type UserName = User["taro"];
// → string（指定したプロパティの型を取り出す）
```

## LookUp 型の定型文
### ⚫︎ 一番よく使う基本形
```ts
type X = SomeType["key"];
```

### ⚫︎ ネストした LookUp
```ts
type X = SomeType["nested"]["key"];
```

### ⚫︎ keyof と組み合わせる
```ts
type ValueOf<T, K extends keyof T> = T[K];
```

## LookUp 型の使い所
- ① API レスポンス型から一部だけ再利用したいとき
- ② モデルの特定フィールドの型を再利用したいとき
- ③ Discriminated Union の LookUp
- ④ keyof と組み合わせて柔軟に使う

## LookUp 型のメリット
- 型の重複をなくせる（DRY）
- モデル変更に強くなる（1箇所変えればOK）
- ネストした型からも簡単に取り出せる
- Union 型にも使える

## まとめ
- LookUp 型 ＝「型の中から、キーを指定してその型だけ取り出す仕組み」