# IndexedDB
👉 ブラウザの中に作るデータベース
- ローカルストレージのように ブラウザにデータを保存
- ただし より複雑で高度なデータ構造を扱える
- 保存は オリジンごと（サイト単位）

## データベースを開く手順
### ①`indexedDB.open("データベース名") `でデータベースを準備
- 存在しなければ自動で作成される
- 例: "shop" という名前で新規作成
### ②返り値は 「IDBOpenDBRequest」オブジェクト
### ③ データベース準備は非同期
- すぐに request.result にアクセスするとエラーになる

## データベース準備完了の検知
- IndexedDB は Promise 非対応
- イベントリスナー で処理する
  - success → データベース準備完了
  - error → 開けなかった場合
  - upgradeneeded → 初めて作るときに一度だけ発生

```js
let request = indexedDB.open("shop");

// upgradeneeded: 初期一回のみ
request.onupgradeneeded = (event) => {
    let db = event.target.result;
    // 初期設定やテーブル作成
};

// success: データベース準備完了
request.onsuccess = (event) => {
    let db = event.target.result;
    // DB利用可能
};

// error: 開けなかった場合
request.onerror = (event) => {
    console.error(event.target.error);
};
```
⚠️ 注意
- upgradeneeded は 初回作成時のみ
- 既存のDBなら発生しない

## データベース削除
- `indexedDB.deleteDatabase("shop")` で削除可能
- 存在しなくてもエラーは出ない

## まとめ
- IndexedDB = **ブラウザに長期保存できる高度なデータベース**
- 開くのは非同期 → success / error / upgradeneeded イベントで管理
- 保存はoriginベース
- 初回作成時のみ upgradeneeded が発生
- 削除も可能


- open() → DB準備
- upgradeneeded → 初期作成処理
- success → DB利用開始
- error → 開けなかった時の処理

## ３つを比べると
### ① Cookie
- 小さいデータ保存
- サーバーにも送られる

### ② LocalStorage
- シンプルな保存
- 文字列だけ
- 容量小さめ

### ③ IndexedDB
- ブラウザ内の本格的データベース
- 大量データ保存できる
- オブジェクトも保存できる

## 全体構造
```bash
IndexedDB
   ↓
Database（データベース）
   ↓
Object Store（テーブル）
   ↓
Record（データ）
```

## 具体的には
```bash
Database: shop
│
├ Object Store: books
│      ├ id:1  title:"JavaScript入門"  price:2000
│      ├ id:2  title:"React入門"       price:3000
│
└ Object Store: games
       ├ id:1  title:"Mario"      price:6000
       └ id:2  title:"Zelda"      price:7000
```
- Database → データベース本体
- Object Store → テーブル（データの箱）
- Record → 実際のデータ