# super() と super.

## super()
＝親クラスのコンストラクタを呼ぶためのもの
＝クラスの子クラスでしか使えないもの
＝`this`や`return`より必ず前に置く


```js
class A {
  constructor(name) {
    this.name = name;
  }
}

class B extends A {
  constructor(name, age) {
    super(name); // 必須！書かないとエラー
  }
}

// constructor を書かない場合は自動で super() が呼ばれる
class C extends A {
  // constructor を書かないなら super() 不要
}
```
⚠️ コンストラクタの中でしか使えない


## super.
＝親クラスのメソッドを呼ぶためのもの
＝`super()`とは全くの別物
＝主に省略メソッドの中で使える `greet(){}みたいな関数のみ`
＝クラス以外でも使えるが基本はクラスでしか使わない

```js
class A {
  greet() {
    console.log("Hello from A");
  }
}

class B extends A {
  greet() {
    super.greet(); // 親の greet() を呼ぶ
    console.log("Hello from B");
  }
}
```
⚠️ これはどのメソッド内でも使える


## まとめ
`super()`
- 親のコンスタントを呼ぶためのもの
- 子クラス内からでしか使えない

`super.`
- 親のメソッドを呼ぶためのもの
- クラス外でも使えるが、アロー関数では使えない