# DOM種類

## ①要素取得系
- `getElementById`, `getElementsByClassName`, `querySelectorAll`, `children` など
- Live / Static で整理すると混乱が減る

## ②位置・サイズ系
- `offsetTop`, `offsetLeft`, `clientWidth/Height`, `scrollWidth/Height`, `getBoundingClientRect()`
- 要素の「位置」「サイズ」「スクロール量」を測る系統

## ③スクロール操作系
- `scrollTop`, `scrollLeft`, `scrollTo`, `scrollBy`, `scrollIntoView`
- 「表示を動かす」系統

## ④座標取得系（特殊）
- `elementFromPoint`
- 「ある座標にある要素を取得する」特殊用途



| メソッド / プロパティ                    | インターフェイス                 | 意味 / 役割              | Live / Static | 使い方例                                      | 備考                                 |
| ------------------------------- | ------------------------ | -------------------- | ------------- | ----------------------------------------- | ---------------------------------- |
| `getElementById(id)`            | `Document`               | IDで要素取得              | N/A           | `document.getElementById("box")`          | 常に1つの要素、nullの場合あり                  |
| `getElementsByClassName(class)` | `Document` / `Element`   | クラス名で要素取得            | **Live**      | `document.getElementsByClassName("item")` | 自動更新される                            |
| `getElementsByTagName(tag)`     | `Document` / `Element`   | タグ名で要素取得             | **Live**      | `document.getElementsByTagName("div")`    | 自動更新される                            |
| `querySelector(selector)`       | `Document` / `Element`   | CSSセレクタで最初の要素取得      | N/A           | `document.querySelector(".item")`         | 単一要素                               |
| `querySelectorAll(selector)`    | `Document` / `Element`   | CSSセレクタで全要素取得        | **Static**    | `document.querySelectorAll(".item")`      | 取得時点で固定                            |
| `children`                      | `Element`                | 子要素のHTMLCollection取得 | **Live**      | `box.children`                            | テキストノードは含まない                       |
| `childNodes`                    | `Node` / `Element`       | 子ノード（要素・テキスト・コメント）   | **Live**      | `box.childNodes`                          | テキストノード含む                          |
| `offsetTop / offsetLeft`        | `HTMLElement`            | 親要素からの座標             | N/A           | `box.offsetTop`                           | スクロール量無視                           |
| `offsetParent`                  | `HTMLElement`            | 位置計算基準の親要素           | N/A           | `box.offsetParent`                        | `position: static`の場合注意            |
| `clientWidth / clientHeight`    | `HTMLElement`            | 見える領域の幅・高さ           | N/A           | `box.clientWidth`                         | padding込み、border除く                 |
| `scrollWidth / scrollHeight`    | `HTMLElement`            | スクロール可能な全体サイズ        | N/A           | `box.scrollHeight`                        | スクロール量含む                           |
| `scrollTop / scrollLeft`        | `HTMLElement`            | 現在のスクロール量            | N/A           | `box.scrollTop = 100`                     | 読み取り・設定可                           |
| `getBoundingClientRect()`       | `Element`                | ビューポート基準の位置とサイズ      | N/A           | `box.getBoundingClientRect()`             | left/top/right/bottom/width/height |
| `scrollTo(x, y)`                | `Window` / `HTMLElement` | 絶対座標スクロール            | N/A           | `window.scrollTo(0, 500)`                 | オプションで `behavior: smooth`          |
| `scrollBy(dx, dy)`              | `Window` / `HTMLElement` | 現在位置から相対スクロール        | N/A           | `window.scrollBy(0, 100)`                 | smooth対応可能                         |
| `scrollIntoView()`              | `Element`                | 要素が画面内に入るようスクロール     | N/A           | `box.scrollIntoView({behavior:"smooth"})` | block: start/center/end/nearest指定可 |
| `elementFromPoint(x, y)`        | `Document`               | 指定座標の最前面要素取得         | N/A           | `document.elementFromPoint(100,200)`      | pointer-events:none の要素は無視         |
