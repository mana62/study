# ID以外で検索する方法

## 問題点：ID以外で検索したい
- これまでの取得方法は `store.get(1)`
- これは `ID = 1` で検索している

でも実際はこうしたい場合がある👇
```bash
title = "JavaScriptガイド"
```
つまり：**ID以外のプロパティで検索したい**

## そのために使うのが Index
- IndexedDBでは **`Index（インデックス）`** を作ると **別のプロパティで検索できるようになる**

イメージ👇
```bash
ObjectStore
   ↓
Index
```

## indexは createIndex() で作る
- これは DB作成時（upgradeneeded）だけ書ける

```js
store.createIndex(
  "titleIndex", // indexの名前
  "title"       // 検索に使うプロパティ
);
```
意味：**title をキーとして検索できるようにする**

## indexを使って検索
```js
const index = store.index("titleIndex");

index.get("JavaScriptガイド");
```
これで `title = JavaScriptガイド` のデータを取得でき流

## indexで使えるメソッド
- indexは **取得だけできる**

⚫︎使えるもの
- get()
- getAll()
- getAllKeys()
- getKey()
- count()

⚫︎使えない
- put
- add
- delete
- clear

理由： **indexは 検索用**

## indexは自動で更新される
元データ
```js
store.put({title:"JS"})
```
すると **indexも自動更新**

つまり： `ObjectStore → Index` は常に同期されている

## indexではキー重複OK
**ObjectStore**：ID は重複NG

例：
```bash
ID=1
ID=1
❌ エラー
```
でも index は
- title="JavaScript"
- title="JavaScript"
キー重複OK

## unique設定
もし重複禁止にしたいなら
```js
store.createIndex("titleIndex", "title", {
  unique: true
});
```
すると**同じtitleは禁止**

## multiEntry（配列対応）
もしデータが
```bash
{
  title: ["JavaScript", "Ruby"]
}
```
だった場合

通常
- indexキー = ["JavaScript","Ruby"]

でも `multiEntry: true` にすると
- JavaScript
- Ruby
**別々のキーとして検索できる**

## まとめ
- IndexedDBでID以外で検索したい場合 **`createIndex()`**を使う

基本コード
```js
store.createIndex("titleIndex","title");
```
検索
```js
store.index("titleIndex").get("JavaScript");
```