# stopPropagation()とpreventDefault()

## event.preventDefault()
＝デフォルトの動作を止める

### いつ使うものか
- フォームの送信を止めたいとき
- リンクのジャンプを止めたいとき
- 右クリックメニューを無効にしたいとき

```html
<a href="https://example.com" id="link">リンク</a>
```

```js
document.getElementById('link').addEventListener('click', function(e) {
  e.preventDefault(); // ← ページ遷移を止める！
  console.log("リンクはクリックされたけど、移動はしないよ！");
});
```

## event.stopPropagation()
＝イベントの伝播を止める

### いつ使うものか
- 子要素のイベントが、親要素に伝わってほしくないとき

```html
<div id="parent">
  <button id="child">クリック</button>
</div>
```

```js
document.getElementById('parent').addEventListener('click', function() {
  console.log("親がクリックされた！");
});

document.getElementById('child').addEventListener('click', function(e) {
  e.stopPropagation(); // ← 親に伝わらないようにする！
  console.log("子だけがクリックされた！");
});
```

| メソッド             | 何を止める？               | 使う場面                             |
|----------------------|----------------------------|--------------------------------------|
| `preventDefault()`   | ブラウザの標準動作         | フォーム送信、リンク移動など         |
| `stopPropagation()`  | イベントの親要素への伝播   | 子だけで処理を止めたいとき           |
