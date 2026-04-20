# TSのゲッターとセッター

## そもそもゲッター / セッターとは？
- プロパティにアクセスしたときに処理を挟める仕組み
- ゲッター: 取得する関数
- セッター: 代入する関数
- 呼ぶ時は、プロパティを呼ぶように書ける
- クラスでよく使われる

## ゲッター（getter）
→ 値を「取得する時」に実行される関数

```ts
// ゲッター
get subject() {
  return this._subject; // アンダースコアは、内部の関数と、意味を区別するためにつけるもの
}
```

```ts
// 呼ぶ時
teacher.subject  // ← 関数じゃなくプロパティっぽく使う。()などは不要
```

## セッター（setter）
→ 値を「代入する時」に実行される関数

```ts
// セッター
set subject(value: string) {
  this._subject = value; // 代入する部分
}
```
```ts
teacher.subject = "Math"  // ← ここでsetが動く
```

## setterメリット
👉 **代入時にチェック・加工・ログなどができる**

```ts
set subject(value: string) {
  if (!value) {
    throw new Error("空はダメ");
  }
  this._subject = value;
}
```

## getterメリット
👉 **取得時に検証・計算・変換ができる**

```ts
get subject() {
  if (!this._subject) {
    throw new Error("未設定");
  }
  return this._subject;
}
```

## JavaScriptとの違い
- 仕組みは同じ（ほぼ完全に同じ）
- 違うのは「型」だけ

```js
// JSの場合

class Teacher {
  constructor() {
    this._subject = "";
  }

  get subject() {
    return this._subject;
  }

  set subject(value) {
    this._subject = value;
  }
}
```

```ts
// TSの場合

class Teacher {
  private _subject: string = "";

  get subject(): string {
    return this._subject;
  }

  set subject(value: string) {
    this._subject = value;
  }
}
```

## TypeScript特有のポイント
### ① getter と setter の型は一致必須
```ts
get subject(): string
set subject(value: string)
// エラー
```
👉 違う型はNG

### ② 型推論が効く
getterがあると：
```ts
teacher.subject // string として扱われる（getterの型を読んで、型推論する）
```

### ③ private が使える
```ts
private _subject: string
```
👉 JSにはなかった概念（※最近は #private あり）

## まとめ
- getter → 取得時に処理を挟む
- setter → 代入時に処理を挟む
- JSでも使える（ESの機能）
- TypeScriptの違いは：👉 型があるだけ（＋アクセス修飾子）

```ts
teacher.subject       // ← getが動く
teacher.subject = "A" // ← setが動く
```
👉 見た目は変数、裏では関数