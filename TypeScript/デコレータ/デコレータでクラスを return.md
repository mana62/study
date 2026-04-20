# デコレータでクラスを return
**クラスを別のクラスに入れ替る**

```ts
return class {}
```
- これは **元のクラスを 差し替える** という意味

## 書き方
```ts
return class extends constructor {}
```
理由：
- 元のクラスを継承するので、元の機能を壊さずに拡張できる

### 例：クラスにプロパティを追加する
```ts
// 普通のクラス
class User {
  name = "Tom";
}


// デコレーターで差し替え
function WithId(constructor: Function) {
  return class {
    id = 123;
  };
}

// 結果
const user = new User();
console.log(user); // { id: 123 }
// 👉 元のUserは消えてる


// 🔥 だから問題が起きる
// 解決：extends
function WithId(constructor: any) {
  return class extends constructor {
    id = 123;
  };
}
// 👉 元のクラスに「上乗せ」してる
```

## イメージ
❌ extendsなし
```bash
User → 捨てる → 新しいクラス
```

✅ extendsあり
```bash
User → ベースにする → 追加する
```

## デコレーターでやるメリット
### 理由①：元のコードを触らずに変えられる
```ts
@WithId
class User {}
```
👉 User自体は一切変更してない

### 理由②：後から自動で機能を足せる
```ts
@Logging
@WithId
@Timestamp
class User {}
```
👉 「上から貼るだけ」で機能追加できる

### 理由③：複数クラスにまとめて適用できる
```ts
@WithId
class User {}

@WithId
class Product {}
```
👉 同じ処理をコピペしない


## 同じようなやり方（別の方法）
### ① 継承（extends）
```ts
class BaseUser {
  id = Math.random();
}

class User extends BaseUser {}
```
👉 シンプルで分かりやすい

### ② 関数で加工
```ts
function addId(obj) {
  return { ...obj, id: Math.random() };
}
```
👉 データ処理ならこっち

## デコレーターの立ち位置
**デコレーターはこういう用途**
- フレームワーク用（Angular / NestJS）
- 横断的処理（ログ・認証・DI）
- 「後から自動で機能を足す」

## 違い
| 方法      | 特徴           |
| ------- | ------------ |
| extends | 明示的で分かりやすい   |
| 関数      | データ操作向き      |
| デコレーター  | 自動で横から差し込む |

## まとめ
- デコレーターは「唯一の方法」じゃない
- でも「後から差し込む設計」が必要なときだけ強い