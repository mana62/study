# elementFromPoint
**対象**：ドキュメント (document)
**目的**：**指定した座標の 一番上にある要素 を取得**する
**引数**：
```js
elementFromPoint(x, y)

// x … ビューポート左端からの横座標
// y … ビューポート上端からの縦座標
```
**戻り値**：その位置にある 最前面の要素 (HTMLElement)

## 使い方
①マウス座標の下にある要素を取得
```js
document.addEventListener('click', (e) => {
  const el = document.elementFromPoint(e.clientX, e.clientY);
  console.log('クリック位置にあった要素:', el);
});
```

②ドラッグ＆ドロップ時の衝突判定
```js
function onDragMove(x, y) {
  const hovered = document.elementFromPoint(x, y);
  if(hovered && hovered.classList.contains('drop-target')) {
    console.log('ドロップ可能な場所に重なった！');
  }
}
```

③レイヤー重なりの判定
```js
const topEl = document.elementFromPoint(100, 200);
console.log('座標(100,200)の一番上の要素は', topEl.tagName);
```

## ⚠️ 注意
- 固定座標：`elementFromPoint` は ビューポート基準 なので、スクロール位置を考慮する必要がある
- 透明な要素の扱い：`pointer-events: none` が設定されている要素は無視される
- 重なり順の影響：CSSの z-index によって結果が変わる

## まとめ
- `getBoundingClientRect` が **「要素の位置・サイズを知る」**ためなら`elementFromPoin`t は **「座標にある最前面の要素を知る」**ため、という感じ
- ドラッグ＆ドロップや座標依存の操作 ではけっこう便利ですが、普段のレイアウト調整ではあまり登場しない