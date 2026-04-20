# IndexedDB とトランザクションの仕組み

## トランザクションとは
👉 トランザクション = まとまった1連の処理
- 途中でエラーが起きたら 全部取り消される
- 全部成功するか、全部失敗するかのどちらか

## 基本IndexedDBではすべてトランザクションで処理する
IndexedDBでは
- get
- put
- add
- delete
- clear
などの すべてのデータ操作は **トランザクションの中でしか実行できない**

## 普通の処理では 自分で作る必要がある
```js
const tx = db.transaction("books", "readwrite");
```
| モード       | 意味   |
| --------- | ---- |
| readonly  | 読み取り |
| readwrite | 読み書き |


## エラーが起きると全部取り消される
例えば途中で
- エラーが発生
- throw
- PCが落ちる
→ **それまでの put や get も 全部無効になる**

## 手動で中止もできる
トランザクションは `abort()`で中止できる

```js
tx.abort();
```
→ **トランザクションを強制終了**

## トランザクションのイベント
**成功した場合**（complete）
```js
tx.addEventListener("complete", () => {
  console.log("成功");
});
```
**失敗した場合**（abort）
```js
tx.addEventListener("abort", () => {
  console.log("失敗");
});
```

## トランザクションは一度終わると使えない
トランザクションは **使い捨てオブジェクト**
- complete後
- abort後
👉 もう 使えない
👉 解決方法は新しいトランザクションを作る

## setTimeoutなどの非同期では使えない
例：
```js
setTimeout(()=>{
  store.get(1);
},0);
```
👉 これはエラーになる

理由：
- イベントループが1周すると
- トランザクションが inactiveになる

## 例外：successイベントの中は使える
これはOK
```js
request.onsuccess = () => {
  store.get(1); // OK
};
```
理由：
- IndexedDBが特別に許可している

## upgradeイベントではトランザクションは1つだけ
upgradeneeded の中では下記は使えない
```js
db.transaction()
```
👉 代わりにブラウザが 自動で1つ作る

**取得方法**
```js
openRequest.transaction
```

## イベントの順番
必ず下記順番
```bash
upgradeneeded
   ↓
transaction complete
   ↓
success
```
失敗した場合
```bash
upgradeneeded
   ↓
transaction abort
   ↓
error
```

## まとめ
IndexedDBのトランザクションは
- すべて成功 or 全部失敗
-  途中エラーなら全部ロールバック
-  完了後は使えない
-  非同期(setTimeout等)では使えない
-  successイベント内だけ例外
-  upgradeneededではトランザクションは1つ

| 状況            | トランザクション   |
| ------------- | ---------- |
| 通常のデータ操作      | 自分で作る      |
| upgradeneeded | ブラウザが自動で作る |

イメージ
```bash
IndexedDB
   ↓
Transaction
   ↓
ObjectStore
   ↓
get / put / delete
```