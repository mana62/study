# staticとlive

## ①Static（静的）コレクション
**代表例**：document.querySelectorAll()
**特徴：**
- 一度取得すると、その後のDOM変更に 追従しない
- 取得時の状態が固定（＝static）

```js
const items = document.querySelectorAll('.item'); // static NodeList
console.log(items.length); // 3

// DOMに新しい.itemを追加
const newItem = document.createElement('div');
newItem.className = 'item';
document.body.appendChild(newItem);

console.log(items.length); // 3 → 変わらない
```
＝「あくまで取得した瞬間の要素だけを見る」イメージ

## ②Live（ライブ）コレクション
**代表例：**
- `document.getElementsByTagName('div')`
- `document.getElementsByClassName('item')`
- HTMLCollection が返るもの全般

**特徴：**
- DOMが変更されると 自動的に反映される
- 要素を追加したら、コレクションにもすぐ反映される

```js
const items = document.getElementsByClassName('item'); // live HTMLCollection
console.log(items.length); // 3

// DOMに新しい.itemを追加
const newItem = document.createElement('div');
newItem.className = 'item';
document.body.appendChild(newItem);

console.log(items.length); // 4 → 自動で反映
```
＝「生きてるコレクション」と覚えるとわかりやすい

## まとめ
| 特性       | Static               | Live                                              |
| -------- | -------------------- | ------------------------------------------------- |
| 代表例      | `querySelectorAll`   | `getElementsByClassName` / `getElementsByTagName` |
| DOM変更の追従 | ×                    | ○                                                 |
| 戻り値      | `NodeList`（固定）       | `HTMLCollection`（自動更新）                            |
| メリット     | 安定して処理できる            | 常に最新状態を参照できる                                      |
| 注意点      | DOM変化を反映したい場合は再取得が必要 | 要素追加・削除で自動更新するため、ループ中に注意                          |


## イメージ
[staticとliveの違い](../img/スタティック.png)

## どっちを使うべきか
| ポイント      | live (`getElementsByClassName`) | static (`querySelectorAll`) |
| --------- | ------------------------------- | --------------------------- |
| 動的DOM更新   | ◎ 自動追従                          | × 再取得が必要                    |
| 安定したリスト処理 | △ 長さが変わる可能性あり                   | ◎ 変化しないのでループが安全             |
| パフォーマンス   | やや注意（ループ中にDOM操作で長さ変化に注意）        | 安定して高速                      |



| 返す型              | メソッド / プロパティ                             | Live / Static | 備考             |
| ---------------- | ---------------------------------------- | ------------- | -------------- |
| `HTMLCollection` | `document.getElementsByTagName(tag)`     | **Live**      | DOM変更で自動更新     |
| `HTMLCollection` | `document.getElementsByClassName(class)` | **Live**      | DOM変更で自動更新     |
| `HTMLCollection` | `element.children`                       | **Live**      | 子要素の追加削除に自動反映  |
| `NodeList`       | `document.querySelectorAll(selector)`    | **Static**    | 取得時点で固定        |
| `NodeList`       | `element.querySelectorAll(selector)`     | **Static**    | 同上             |
| `NodeList`       | `document.childNodes`                    | **Live**      | 子ノードの追加削除で自動更新 |
| `NodeList`       | `element.childNodes`                     | **Live**      | 同上             |
| `HTMLCollection` | `document.forms`                         | **Live**      | フォーム追加で自動更新    |
| `HTMLCollection` | `document.images`                        | **Live**      | 画像追加で自動更新      |
