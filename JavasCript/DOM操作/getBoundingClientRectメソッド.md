# getBoundingClientRect
**対象**：DOMの要素（HTMLElement）
**目的**：その**要素の 位置とサイズ を取得**する
**戻り値**：DOMRect オブジェクト

### 主なプロパティ
| プロパティ        | 説明            |
| ------------ | ------------- |
| `x` / `left` | ビューポート左端からの距離 |
| `y` / `top`  | ビューポート上端からの距離 |
| `right`      | 要素右端の座標       |
| `bottom`     | 要素下端の座標       |
| `width`      | 要素の幅          |
| `height`     | 要素の高さ         |

＝つまり **「画面上でこの要素はどこにあるのか？」を教えてくれる**メソッド

## 使い方
①スクロール位置やアニメーション制御
```js
const box = document.querySelector('.box');
const rect = box.getBoundingClientRect();

if(rect.top < window.innerHeight) {
  console.log('要素が画面に見えている！');
}
```

②カスタムツールチップやポップアップの位置計算
```js
const button = document.querySelector('button');
const rect = button.getBoundingClientRect();

tooltip.style.top = `${rect.bottom + 5}px`;
tooltip.style.left = `${rect.left}px`;
```

③要素の衝突判定やドラッグ＆ドロップ
```js
const rect1 = element1.getBoundingClientRect();
const rect2 = element2.getBoundingClientRect();

if (!(rect1.right < rect2.left || rect1.left > rect2.right)) {
  console.log('横方向で重なってる！');
}
```

## ⚠️ 注意
- スクロール位置に依存：`getBoundingClientRect` の座標は **ビューポート基準**。ページ全体の位置を知りたい場合は、**window.scrollX / window.scrollY** と組み合わせる必要がある
- リアルタイム計算：呼ぶたびにレイアウト情報を取得するので、**頻繁に呼びすぎるとパフォーマンスに影響する**ことがある

## まとめ
- **ウェブ開発・フロントエンドでレイアウト制御や可視判定をする**便利メソッド
- 特に スクロール連動アニメーション、ツールチップ、ドラッグ＆ドロップ 系で活躍する