# コンストラクタ関数にprivate修飾子をつける
- constructor に private を付ける理由は **new させないため**
  - **constructor が private だと 外から new できない**
  - だから **インスタンスを勝手に増やせない**
  - その代わり、**クラス自身が static を使って 1つだけインスタンスを作って返す**
  - これを **シングルトンパターン**（Singleton Pattern）という

例：
```ts
class Database {
  private static instance: Database; // インスタンスを保存しておく箱
  private constructor() {} // ← 外から new できない

  static getInstance() {
    if (!Database.instance) {
      Database.instance = new Database(); // ← クラス自身だけ new できる
    }
    return Database.instance;
  }
}

const db1 = Database.getInstance();
const db2 = Database.getInstance();

console.log(db1 === db2); // true（同じインスタンス）
```

## `private static instance: Database;` とは
- **インスタンスを保存しておく箱**（シングルトンを作りたい場合だけ必要）
  - static → クラスに属する（new しなくても使える）
  - instance → インスタンスを入れておく変数
  - Database → その型
- つまり：**Database クラスが、自分自身のインスタンスを保存するための箱を持っている**

クラス自身が持つ変数 → static
インスタンスが持つ変数 → 普通のプロパティ

## なぜ private static instance が必要なの？
- 理由：**作ったインスタンスを保存しておく必要があるから**

シングルトンはこう動く：
1. 最初の呼び出し → インスタンスを作る
2. 2回目以降 → 前に作ったインスタンスを返す

そのためには：**インスタンスをどこかに保存しておく必要がある**
その保存場所がこれ：`private static instance: Database;`

## なぜ constructor に private を付けるのか？
- **new されたら困るから**

```ts
new Database(); // ❌ エラー
```
- **private constructor は クラスの外から new できない**
- つまり **インスタンスを勝手に増やせない**
- これがシングルトンの第一条件

## ではどうやってインスタンスを作るのか？
- **static メソッドで作る**

```ts
static getInstance() {
  if (!Database.instance) {
    Database.instance = new Database();
  }
  return Database.instance;
}
```
- **static は new しなくても呼べる**
- **constructor が private でも クラス内部からは new できる**

つまり：
- **外から new は禁止**
- **クラス自身だけ new** できる
これが**シングルトンの仕組み**

## static プロパティが必要な理由
```ts
private static instance: Database;
```
- static は「クラスに属する」
- インスタンスを保存する場所として最適
- **インスタンスを保持するのは クラス自身 だから static が必要**

## シングルトンパターンとは
- **インスタンスを1つしか作らせないデザインパターン**

使いどころ：
- DB 接続
- 設定ファイル
- ログ管理
- キャッシュ管理
複数作られると困るものに使う

でも、わざわざクラスにしなくてもいいじゃんと思うが、クラスにすることによって：
- **「まとめる ＋ 制御するため」** ができる

- クラス = まとめる（OK）
- でもそれだけなら普通のオブジェクトでいい

👉 **「使い方を強制すること」ができる**

❌ 制御なし
```ts
new DB()
new DB()
new DB()
```
👉 誰でも好き勝手に作れる

✅ 制御あり
```ts
DB.getInstance()
```
👉 これしか使えない
つまり： **全員の使い方が統一される**