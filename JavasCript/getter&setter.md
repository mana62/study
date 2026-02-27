# getter / setter
＝関数を作る仕組み（ほぼオブジェクトで使われる）

## getter（ゲッター）とは
＝プロパティを**読む**ときに呼ばれる関数

```js
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name.toUpperCase();
  }
}

const u = new User("taro");
console.log(u.name); // TARO ← 関数なのにプロパティのように読める
```
- u.name と書くだけで get name() が呼ばれる
- 関数呼び出しっぽく見えない
- 値の加工・計算・隠蔽に使える

## setter（セッター）とは
＝プロパティに**代入**したときに呼ばれる関数

```js
class User {
  constructor(name) {
    this._name = name;
  }

  set name(value) {
    if (value.length < 3) {
      throw new Error("名前は3文字以上にしてください");
    }
    this._name = value;
  }
}

const u = new User("taro");
u.name = "ji"; // エラー
```
- u.name = "ji" と書くと set name() が呼ばれる
- バリデーションやログ出力に便利
- 内部の _name を守れる

## getter / setter の本質
＝「プロパティのように見えるが、裏で関数が動いている」

## 比較
⚫︎普通の関数
```js
user.getName();
user.setName("Taro");
```

⚫︎getter/setter
```js
user.name;      // 読む → getter が動く
user.name = "Taro"; // 書く → setter が動く
```
＝見た目はプロパティ、裏は関数

## getter/setter を使うべき 4 つの場面
### ① 値を読み取るたびに自動で加工したいとき（getter）
例：フルネームを毎回計算したい
```js
get fullName() {
  return `${this.first} ${this.last}`;
}
```
- user.fullName と書くだけで毎回自動計算
- 関数呼び出しっぽく見せたくない時に最適

### ② 値を書き込むときにチェックしたいとき（setter）
例：バリデーション
```js
set age(v) {
  if (v < 0) throw new Error("年齢は0以上");
  this._age = v;
}
```
- user.age = -5 と書いた瞬間に自動チェック
- 「代入」という自然な操作にバリデーションを紐づけられる

### ③ 内部データを隠したいとき（カプセル化）
外からはプロパティっぽく見えるけど、内部では別の変数を使いたいとき
```js
get name() {
  return this._name;
}

set name(v) {
  this._name = v.trim();
}
```
- 外部に _name を直接触らせない
- オブジェクトの内部構造を守れる

### ④ 値の変化を監視したいとき（setter）
例：ログを取りたい
```js
set value(v) {
  console.log("値が変わった:", v);
  this._value = v;
}
```
- obj.value = 10 と書くだけでログが残る
- イベントリスナー的な使い方ができる

## 逆に「使わない方がいい」場面
- 単純なデータ入れ物（DTO）
- 処理が重い計算を getter に入れる（毎回呼ばれて重くなる）
- setter 内で複雑な処理をしすぎる（意図が見えにくい）
- getter/setter は魔法ではなく、「プロパティアクセスに自然に処理を挟みたい時だけ使う」これが鉄則（ほぼオブジェクトで使われる）

## 内部用プロパティ（例 : _age）のようなアンダースコアのこと
＝getter / setter 用の「中身専用の変数です」という目印

```js
class User {
  constructor(age) {
    this._age = age; // ← 内部用
  }

  get age() {
    return this._age; // ← 外向き
  }
}
```

例えば下記のsetter
①関数名も`age` 内部情報でも`age`を使用。setterは混乱して、無限にageをループする。
それをわかりやすく区別するために内部には_をつけるルール`_age`
```js
set age(value) {
  this.age = value;
}
```


## まとめ
**getter → プロパティを読むときに自動で呼ばれる関数。() を書かずに関数を実行できる**
**setter → プロパティに代入したときに自動で呼ばれる関数。 = と同時に関数を実行できる**
プロパティに見えるけど、実は関数