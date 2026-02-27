# MapとweakMapについて

## Mapでキーとオブジェクトを扱う方法
＝Map は **キーに何でも使える** から、オブジェクトをキーにできる
＝ほぼオブジェクトと同じ機能

## オブジェクトとMapの違い
#### オブジェクト
```js
const obj = {
  name: "太郎",
  age: 20
};
```
👉 「ラベル（文字列）」が最初から決まってる棚
- キーは基本「文字」
- 設定データ・JSON向き
- 人が読むためのデータ

#### Map
```js
const map = new Map();
map.set(鍵, 中身);
```
👉 「何でも鍵にできるロッカー」
- 数字・オブジェクト・関数・DOMも鍵にできる
- プログラム内部の管理用
- 人が直接読む前提じゃない


## Map と オブジェクトの主な違い（比較表）

| 特徴 | Map | Object (Object literal) |
|------|------|---------------------------|
| **キーの型** | 何でも可（オブジェクト、数値、関数など） | 文字列、シンボルのみ |
| **順序** | 挿入順を保証 | 保証されない（ES2015 以降は一部ルールあり） |
| **要素数** | `map.size` で取得 | `Object.keys(obj).length` で計算 |
| **反復処理** | `for...of` で直接反復可能 | `Object.keys()` などを使用 |
| **パフォーマンス** | 追加・削除が多い場合に高速 | 少ない要素や読み込みが多い場合に高速 |
| **JSON 連携** | 直接 JSON 化不可（変換が必要） | `JSON.stringify` で直接 JSON 化可能 |


- [JavaScriptの`Map`と`Object`の違いと使い分け](https://qiita.com/oharu121/items/25cd17d1bc0b261d2f2c)


⚠️ シンボル（Symbol）とは
＝絶対に重複しない唯一無二の値を生成するプリミティブデータ型

⚠️ イテレーション（Iteration）とは
＝配列やオブジェクトなどのデータ集合に対して、要素を順に処理する「繰り返し（ループ）」のこと

⚠️ イテラブルオブジェクトとは？
＝1個ずつ順番に取り出せるデータのこと


## 使う場面
＝Mapを使うのはこの3パターンのみ
＝基本はオブジェクトでOK

### ①「キーが“文字列じゃない”と意味を持たないとき」
これがMapの存在理由ほぼ100%
```js
// 例：オブジェクトそのものをキーにしたい
const userState = new Map();

const userA = { name: "A" };
const userB = { name: "B" };

userState.set(userA, "online");
userState.set(userB, "offline");
```

❌ オブジェクトでは不可能
⭕ Mapなら普通にできる

👉 「この“モノ”に対して、この情報」
こう考えたらMap

### ②「プログラム内部の管理用データ」
Mapは**人に見せる用じゃない / JSONにしない / APIに投げない**
```js
// 例：DOMと状態を紐づける
const stateMap = new Map();

document.querySelectorAll("button").forEach(btn => {
  stateMap.set(btn, { clicked: false });
});
```
DOM要素 → 状態
一時的
内部処理用
👉 これ、オブジェクトだと地獄

## ③「頻繁に追加・削除する辞書」
```js
const activeUsers = new Map();

activeUsers.set(user1, true);
activeUsers.delete(user1);
```
size がすぐ取れる
clear がある
ループが安定してる
👉 **「生き物みたいに増減するデータ」**はMap向き

## 逆に「使わない時」は？
❌ Mapを使わない
- 設定データ
- APIレスポンス
- JSON
- Reactのstate
- keyが固定されているもの

## Map を使うのは “Object では扱いづらい状況” のときだけ
### ① キーにオブジェクトを使いたいとき
Object はキーが文字列に変換されるから、こうなる：
```js
const obj = {};
const user = { id: 1 };

obj[user] = "A"; 
console.log(obj); 
// { "[object Object]": "A" }
```

Map なら：
```js
const map = new Map();
map.set(user, "A");
```
👉 オブジェクトそのものをキーにできる

### ② キーの順番を保証したいとき
Object は順番が曖昧。
Map は 挿入順を必ず保持。

ログや履歴、キャッシュなど、順番が意味を持つデータに向いてる

### ③ 大量の追加・削除があるとき
Map は内部構造がハッシュマップなので高速。
Object はプロパティ追加・削除が多いと遅くなる。

👉 キャッシュやセッション管理に向いてる

### ④ サイズ（要素数）をすぐ知りたいとき
Object は数える必要がある：
```js
Object.keys(obj).length
```

Map は一発：
```js
map.size
```

### ⑤ 反復処理を簡単にしたいとき
Object は keys / values / entries を使う必要がある

Map はそのまま回せる：
```js
for (const [key, value] of map) {
  console.log(key, value);
}
```

## Object を使うべき場面
JSON として扱うデータ

設定情報

API レスポンス

単純なデータ構造（ユーザー情報など）

👉 “名前付きのデータ” をまとめる箱として最適

## WeakMap
＝表に出したいデータ → Map
＝裏で管理したいデータ → WeakMap

＝ガベージコレクションされる
＝キーにはオブジェクト or 非登録シンボルのみ

### 通常のMap
「キーと値をセットで保存する辞書」
```js
const map = new Map();

const obj = { name: "A" };
map.set(obj, "data");

map.get(obj); // "data"
```
### Map の特徴
- キー：なんでもOK
- ループできる
- size が取れる
- キーを強く保持する
- キーを定義してるため、いつか使うかもと、ガベージコレクションされない

### weakMap
「オブジェクト専用・裏方専用・勝手に消えるMap」
```js
const wm = new WeakMap();

const obj = {};
wm.set(obj, "秘密のデータ");
```

### weakMapの特徴
#### ① キーは「オブジェクト or 非登録シンボルのみ」
```js
wm.set({}, "ok");        // ✅
wm.set(() => {}, "ok"); // ✅（関数もオブジェクト）

wm.set(1, "ng");        // ❌
wm.set("a", "ng");      // ❌
wm.set(Symbol(), "ng"); // ❌
```

#### ② キーは「弱い参照」
```js
let obj = { name: "A" };
wm.set(obj, "data");

obj = null;
```
👉 この瞬間：
- プログラムから obj への参照が消える（ガベージコレクション）
- WeakMap は引き止めない
- GC が回収できる

### Map との決定的違い
```js
const map = new Map();
let obj = {};

map.set(obj, "data");
obj = null;

// map がまだ obj を握っている → GCされない
```
👉 Map は 「まだ使うかも」 と言い張る
👉 WeakMap は 「もう知らん」 と手放す

### ③ 中身は見えない・数えられない
```js
wm.size      // ❌
wm.forEach  // ❌
```

使えるものは下記メソッドのみ
```js
wm.set(obj, value);
wm.get(obj);
wm.has(obj);
wm.delete(obj);
```

## Map と WeakMap の使い分け
**Map を使うとき**
- データを一覧したい
- 永続的に持ちたい
- 数や中身を管理したい

**WeakMap を使うとき**
- オブジェクトに付随する情報
- 裏側の状態管理
- メモリを自動で掃除したい

| 型       | 何を保存？      | 典型用途  |
| ------- | ---------- | ----- |
| Set     | 値          | 一覧管理  |
| WeakSet | オブジェクト     | フラグ管理 |
| Map     | キー → 値     | 辞書    |
| WeakMap | オブジェクト → 値 | 裏データ  |


## MDN
[WeakMap](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/WeakMap)