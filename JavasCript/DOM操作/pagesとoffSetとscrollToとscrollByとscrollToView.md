# pages / offSet / scrollTo / scrollBy / scrollToView

## ①ページ座標系
`pageXOffset / pageYOffset`

**意味**：ウィンドウの現在のスクロール量（ビューポートの左上からの距離）
**読み取り専用**
**例：**
```js
console.log(window.pageXOffset, window.pageYOffset);
// 横スクロール量, 縦スクロール量
```
`window.scrollX / window.scrollY` と同じ

## ②要素の座標系
`offsetTop / offsetLeft`

**意味**：要素の 親要素に対するオフセット位置
**特徴**：
- 親の `position`が `relative/absolute/fixed` に依存
- スクロール量は含まない
**例：**
```js
const box = document.querySelector('.box');
console.log(box.offsetTop, box.offsetLeft); 
// 親要素からの座標
```

`offsetParent`
- この要素の 位置計算の基準になっている親要素 を返す

```js
console.log(box.offsetParent);
```

## ③スクロールを操作する系
`scrollTo(x, y)`
- ページや要素を 絶対座標でスクロール

```js
window.scrollTo(0, 500); // 縦方向500pxの位置までスクロール
```

- オプションでスムーズスクロールも可能

```js
window.scrollTo({ top: 500, behavior: 'smooth' });
```

`scrollBy(dx, dy)`
- 現在位置からの相対スクロール

```js
window.scrollBy(0, 100); // 今の位置からさらに100px下にスクロール
```

`scrollIntoView()`
- 要素が画面内に入るようにスクロールさせる

```js
const box = document.querySelector('.box');
box.scrollIntoView({ behavior: 'smooth', block: 'center' });
```

`block: 'start' | 'center' | 'end' | 'nearest'`
`behavior: 'auto' | 'smooth'`

## まとめ
| メソッド/プロパティ                  | 用途          | 絶対/相対    | 対象          |
| --------------------------- | ----------- | -------- | ----------- |
| `pageXOffset / pageYOffset` | 現在のスクロール量取得 | N/A      | window      |
| `offsetTop / offsetLeft`    | 親要素からの座標取得  | 絶対       | HTMLElement |
| `scrollTo`                  | 絶対位置にスクロール  | 絶対       | window / 要素 |
| `scrollBy`                  | 現在位置からスクロール | 相対       | window / 要素 |
| `scrollIntoView`            | 要素を画面内に表示   | 絶対（要素基準） | HTMLElement |


`pageYOffset` = ページ全体のスクロール位置
`offsetTop` = 要素の親からの位置
`scrollTo / scrollBy` = スクロール操作
`scrollIntoView` = 「この要素を見せたい！」用