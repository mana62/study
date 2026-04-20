# TSでクラスを書く

## 1. TSのクラスの基本とJSとの違い
```ts
class Person {
  name: string; // keyを表す
  age: number;

  constructor(initName: string, initAge: number) {
    this.name = initName; // valueを表す
    this.age = initAge;
  }
}
```
ここでTSがやっていること↓

**フィールド宣言**:
```ts
name: string;
age: number;
```

TSの意味:
- Personインスタンスは必ずname（string）とage（number）を持つ
- フィールド宣言はキーを表している
- 型チェック用の情報であって、コンパイル後のJSには消える

**コンストラクタ**（初期化処理）:
```ts
constructor(initName: string, initAge: number) {
  this.name = initName;
  this.age = initAge;
}
```
- コンストラクタはvalueを表す
- initName, initAgeにも型を付ける（これもJSには消える）
- this.nameやthis.ageは、上で宣言したフィールドに代入している

**コンパイル後のJS（ES6ターゲットの場合）**
```js
class Person {
  constructor(initName, initAge) {
    this.name = initName;
    this.age = initAge;
  }
}
```
- name: string;やage: number;は完全に消えている
- 型情報は開発時だけのサポートで、実行時には存在しない

**ES5ターゲットの場合（イメージ）**（ほぼ使わない）
```js
function Person(initName, initAge) {
  this.name = initName;
  this.age = initAge;
}
```
- classがコンストラクタ関数に変換される
- extendsなどはprototypeを使った継承に変換される

### TSクラスとJSクラスのざっくり比較
| 項目 | TypeScriptのクラス | JavaScriptのクラス |
|------|----------------------|----------------------|
| フィールドの型 | `name: string;` のように書ける | 型は書けない |
| フィールド宣言 | クラス内で事前に宣言必須（strict だと特に） | コンストラクタ内でいきなり `this.name` でも OK |
| コンパイル後の型情報 | 全部消える | もともとない |
| アクセス修飾子 | `public`, `private`, `protected`, `readonly` が使える | 言語レベルではなし（`#field` は別仕様） |
| クラス自体を型として利用 | `let p: Person` のように使える | 型システムがないのでできない |

## 2. クラスのメソッドとthisの型
- **引数の第一引数にクラスを型として定義できる**
- **thisは、ドットの左側を指す**
- アロー関数では this は使えない

```ts
class Person {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greeting(this: Person) {
    console.log(`Hello! ${this.name}!`);
  }
}
```
**greeting(this: Person) の意味**：
- メソッドの **第一引数としてthisの型を明示している**
- 必ず **第一引数** にthisの引数の型を書く必要がある、第二引数だとエラー
- これはTSだけの機能で、JSには存在しない
- 実際の呼び出しでは、this引数を渡さない

```ts
const p = new Person('taro', 20);
p.greeting(); // OK
```

### なんでthisに型をつけるの？
- thisの文脈が壊れたときにエラーにできるから

```ts
const p = new Person('taro', 20);
const greet = p.greeting;

// ここでthisが`undefined`になる可能性がある
greet(); // this: undefined になる（ドットの左側に何もないから） → エラーにしたい

// this: Person と書いておくと、こういう呼び出しをTSがエラーにしてくれる
```

### JSとの違い（this引数）
| 項目 | TypeScript | JavaScript |
|------|-------------|-------------|
| this を引数に書く | `greeting(this: Person)` のように書ける | そもそも書けない |
| 目的 | this の型チェック、誤った呼び出しの検出 | 言語仕様としてサポートなし |
| 実行時の挙動 | コンパイル後は this 引数は消える | this は呼び出し方で決まるだけ |

## 3. thisは「ドットの左側」を指す & アロー関数との違い
### 通常のメソッドの場合
```ts
p.greeting();
// ↑ このときの this は「p」
```
- **「ドットの左側」がthisになる**

### this引数のルール（TS）
- thisは必ず第一引数
- 第二引数以降にthisを書くとエラー
- JSではそもそもthisを引数に書けない

### アロー関数の場合
- アロー関数でもthisは使える
- ただし、アロー関数は自分自身のthisを持たず、外側のthisを指す

```ts
class Person {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  // 通常メソッド
  greeting() {
    console.log('normal:', this.name);
  }
  // → greetingは呼び出し方によってthisが変わる

  // アロー関数プロパティ
  greetingArrow = () => {
    console.log('arrow:', this.name);
  };
  // → greetingArrowは、インスタンス生成時にthisが固定される
}
```
```ts
// 呼び出し側
const p = new Person('taro');

const g1 = p.greeting;
g1(); // thisがundefinedになる可能性 → エラーや意図しない挙動

const g2 = p.greetingArrow;
g2(); // thisは常にpを指す（外側のthisをキャプチャしている）
```

### 使い分け
| 目的 | 選ぶべきもの |
|------|--------------|
| this を安全に固定したい |  アロー関数 |
| メモリ効率・プロトタイプ利用 |  通常メソッド |
| this の型チェックをしたい |  通常メソッド（this パラメータが使える） |

## まとめ（アロー関数とthis）
| 項目 | 通常のメソッド | アロー関数プロパティ |
|------|----------------|------------------------|
| this の決まり方 | 呼び出し方（ドットの左側）で決まる | 定義されたときの外側の this に固定される |
| this 引数の型指定 | `method(this: Person)` と書ける | this 引数は書けない |
| コールバックでの安全性 | 文脈が変わると this が壊れやすい | 文脈が変わっても this は固定されていて安全 |

## 4. クラスを「型」として使う
```ts
class Person {
  name: string;
  age: number;
}

const p1: Person = {
  name: 'taro',
  age: 20,
};

const p2: Person = new Person();
```
- Personは値（コンストラクタ）でもあり、型でもある
- `(this: Person)`のように、thisの型としても使える

```ts
class Person {
  name: string;

  greeting(this: Person) {
    console.log(this.name);
  }
}
```

### JSとの違い
- JSには「型」という概念がないので、Personを「型」として使うことはできない
- TSではクラス宣言がそのまま型としても使えるのが便利ポイント

## 5. public / private / protected とメソッド
```ts
class Person {
  public name: string;
  private age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  incrementAge() {
    this.age = this.age + 1;
  }
}
```
```ts
incrementAge() {
  this.age = this.age + 1;
}
```

### privateの意味
```ts
class Person {
  private age: number;
}
```
- **クラスの外からはアクセスできない**

```ts
const p = new Person('taro', 20);
p.age; // エラー（privateだから）
```

- クラスの中からはOK

```ts
class Person {
  private age: number;

  getAge() {
    return this.age; // OK
  }
}
```

### publicの意味
- どこからでもアクセス可能
- フィールドに何も書かない場合、デフォルトでpublic

```ts
class Person {
  name: string; // = public name: string と同じ
}
```

### protectedの意味
- extendsしたクラスの、プロパティがprivateだった場合、**継承先でも使えるのが、protected**

正確には：**protectedは同じクラス + サブクラス（継承先）からアクセス可能**
- でもクラスの外からはアクセス不可

```ts
class Person {
  protected age: number;

  constructor(age: number) {
    this.age = age;
  }
}

class Teacher extends Person {
  increment() {
    this.age++; // OK（継承先なので）
  }
}

const t = new Teacher(20);
t.age; // エラー（外からは見えない）
```

### アクセス修飾子まとめ
| 修飾子 | TypeScriptの意味 | JavaScript（ES2023時点） |
|--------|-------------------|---------------------------|
| public | どこからでもアクセス可能（デフォルト） | 明示的な public キーワードはない |
| private | クラス内からのみアクセス可能 | `#field` で似た機能はあるが別仕様 |
| protected | クラス内＋継承先からアクセス可能 | 言語仕様としては存在しない |
| readonly | 書き込み禁止（初期化時のみ書き込み可・読み込みのみOK） | 言語レベルではなし（Object.freeze など別） |

## 6. コンストラクタの省略記法（パラメータプロパティ）

```ts
class Person {
  public name: string;
  private age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
```
これを省略記法で書くと：

```ts
class Person {
  constructor(public name: string, private age: number) {}
}
```
- constructorの引数にpublicやprivateをつけると、
  - フィールド宣言
  - コンストラクタ内での代入
を自動でやってくれる

例：
```ts
class Person {
  constructor(
    public name: string,
    private age: number,
  ) {}
}

const p = new Person('taro', 20);
console.log(p.name); // OK
// console.log(p.age); // エラー（private）
```

## 7. readonlyとは
- **readonlyは「再代入禁止」**
- 読むだけならOK,書き込むはダメ
- ただし、コンストラクタ内では書き換えOK

```ts
class Person {
  constructor(
    public readonly name: string,
    private readonly id: number,
  ) {}
}
```
```ts
class Person {
  readonly name: string;

  constructor(name: string) {
    this.name = name; // OK（コンストラクタ内）
  }

  changeName(newName: string) {
    this.name = newName; // エラー（readonlyだから）
  }
}
```

### よく使う場面
- 変わらない値：id, createdAt, email（変更不可設計の場合）など

### JSとの違い
- JSにはreadonlyキーワードはない

## 8. 継承 extendsとsuper
```ts
class Person {
  constructor(public name: string, public age: number) {}
}

class Teacher extends Person {
  constructor(name: string, age: number, public subject: string) {
    super(name, age);
  }
}
```

- **extends Person**：Personを継承する
- **super(name, age)**：親クラスPersonのコンストラクタを呼ぶ

### ルール
- 継承先でconstructorを書く場合、**必ずsuper(...)を最初に呼ぶ必要がある**
- **superを呼ぶ前にthisは使えない**

```ts
class Teacher extends Person {
  constructor(name: string, age: number, public subject: string) {
    // this.name = name; // これはエラー（superより前）
    super(name, age);
    // ここからthisが使える
  }
}
```

### JSとの違い
- JSのclass/extends/superとほぼ同じ構文
- 違いは、TSでは型情報がついていること

## ここまでのまとめ
| トピック | TypeScript | JavaScript |
|----------|-------------|-------------|
| クラスの型情報 | フィールド・引数に型をつける | 型は書けない |
| this 引数 | `method(this: Person)` と書ける | そもそも書けない |
| アクセス修飾子 | `public`, `private`, `protected`, `readonly` | 言語仕様としてはなし（`#field` は別） |
| パラメータプロパティ | `constructor(public name: string)` で宣言＋代入を省略 | そのような構文はない |
| クラスを型として利用 | `let p: Person` のように使える | 型システムがないので不可 |
| extends / super | JS と同じ構文＋型チェック | 構文は同じだが型チェックはない |