# instanceof と isPrototypeOf

## instanceof とは
### 何をする？
＝オブジェクトが特定のクラス（コンストラクタ関数）から作られたかどうかを判定する

### 使い方
```js
obj instanceof Constructor
```
🟠引数
- 左：調べたいオブジェクト
- 右：コンストラクタ（class や function）

### 例
```js
class Animal {}
class Dog extends Animal {}

const d = new Dog();

console.log(d instanceof Dog);     // true
console.log(d instanceof Animal);  // true
console.log(d instanceof Object);  // true
```
**プロトタイプチェーンを遡って判定**するのがポイント


## isPrototypeOf とは
### 何をする？
＝あるオブジェクトが、別のオブジェクトのプロトタイプチェーンに含まれているかを調べる

### 使い方
```js
prototypeObj.isPrototypeOf(obj)
```

🟠引数
- 左：プロトタイプ（prototype オブジェクト）
- 右：調べたいオブジェクト

### 例
```js
function Animal() {}
function Dog() {}

Dog.prototype = Object.create(Animal.prototype);

const d = new Dog();

console.log(Animal.prototype.isPrototypeOf(d)); // true
console.log(Dog.prototype.isPrototypeOf(d));    // true
```
**プロトタイプそのものに対して使う**のがポイント

## instanceof と isPrototypeOf の違い
# instanceof と isPrototypeOf の比較

| 比較項目 | instanceof | isPrototypeOf |
|----------|------------|----------------|
| 何を判定？ | オブジェクトがどのクラスから作られたか | プロトタイプチェーンに含まれているか |
| 左側 | オブジェクト | プロトタイプ |
| 右側 | コンストラクタ | オブジェクト |
| よく使う？ | よく使う | あまり使わない（高度な場面） |

### 実際のコードで比較
```js
class A {}
class B extends A {}

const b = new B();

// instanceof
console.log(b instanceof B); // true
console.log(b instanceof A); // true

// isPrototypeOf
console.log(B.prototype.isPrototypeOf(b)); // true
console.log(A.prototype.isPrototypeOf(b)); // true
```

どちらも「継承関係」を調べられるけど、
instanceof は **クラスベース**
isPrototypeOf は **プロトタイプベース**
という違いがある

## まとめ
**instanceof**
→ オブジェクトがどのクラスから作られたか調べる
→ obj instanceof Class

**isPrototypeOf**
→ プロトタイプチェーンに含まれているか調べる
→ Class.prototype.isPrototypeOf(obj)

**一般的には instanceof の方がよく使う**
isPrototypeOf はプロトタイプを直接触るときに使う