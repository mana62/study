# IndexedDBでエラーが起きた場合
＝ get や put などの処理が 非同期で失敗すると2つのことが起こる
 - ① トランザクションが中止される（abort）
 - ② errorイベントが発生する

```bash
エラー発生
   ↓
トランザクション中止
   ↓
errorイベント発生
```

## add() メソッドとは
- add() は put() とほぼ同じですが1つだけ違いがある

| メソッド  | 動作      |
| ----- | ------- |
| put() | 追加 + 更新 |
| add() | 追加のみ    |

👉 **同じキーがあるとエラーになる**

```js
// 例
store.add({name:"JS"}, 1);
store.add({name:"React"}, 1); // エラー
```
→ **キー1は既に存在する**

## エラー処理の書き方
- エラーは **requestのerrorイベントで処理できる**

```js
const request = store.add(data, key);

request.addEventListener("error", (event) => {
  console.log(event.target.error.message);
});
```
**取得できる情報**
```js
event.target.error.message
```
```bash
// 例
Key already exists in the object store
```

## 通常はエラーが起きるとトランザクションが中止
**デフォルト動作**
```bash
add失敗
   ↓
transaction abort
   ↓
すべての処理が取り消し
```
→ それまでのputやaddも全部無かったことになる

## preventDefault()で中止を止められる
```js
event.preventDefault();
```
を書くと **トランザクションの中止を防げる**

```js
// 例
request.addEventListener("error", (event)=>{
  event.preventDefault();
});
```

```bash
# 結果
add → 失敗
でも
トランザクションは続く

# その処理だけ失敗
# 他の処理は成功
```

## errorイベントはバブリングする
- errorイベントは **上に伝わる（bubbling）**

順番
```bash
Request
 ↓
Transaction
 ↓
Database
```
例:
```js
request.onerror = () => console.log("request");
tx.onerror = () => console.log("transaction");
db.onerror = () => console.log("db");
```
出力
```bash
request
transaction
db
```

## successイベントはバブリングしない
| イベント    | バブリング |
| ------- | ----- |
| success | しない   |
| error   | する    |

## まとめ
IndexedDBのエラー処理
- add() はキー重複でエラー
-  エラー時は errorイベント 発生
-  通常は transaction abort になる
-  event.preventDefault()で中止を防げる
-  errorイベントは Request → Transaction → DB にバブリング