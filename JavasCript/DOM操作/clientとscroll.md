# client と scroll

## client系`（clientWidth / clientHeight / clientTop / clientLeft）`

**意味**：要素の **「見える領域（コンテンツ＋パディング）」のサイズや位置**

**特徴**
- paddingを含む
- borderは含まない
- スクロールは無視（スクロール量によらず常に同じ）

**主なプロパティ**
| プロパティ          | 説明                         |
| -------------- | -------------------------- |
| `clientWidth`  | 要素の幅（padding込み、スクロールバー除く）  |
| `clientHeight` | 要素の高さ（padding込み、スクロールバー除く） |
| `clientTop`    | 上の境界線（border）の幅            |
| `clientLeft`   | 左の境界線（border）の幅            |

```js
const box = document.querySelector('.box');
console.log(box.clientWidth, box.clientHeight);
```

## scroll系`（scrollWidth / scrollHeight / scrollTop / scrollLeft）`

**意味**：要素の スクロール可能な領域のサイズや現在のスクロール量
**特徴**：
- contentの全体サイズを含む（visible部分だけでなくスクロールで隠れている部分も含む）
- スクロール量を操作できる

**主なプロパティ**
| プロパティ          | 説明                  |
| -------------- | ------------------- |
| `scrollWidth`  | 横方向にスクロール可能な領域の合計幅  |
| `scrollHeight` | 縦方向にスクロール可能な領域の合計高さ |
| `scrollTop`    | 上からどれだけスクロールしているか   |
| `scrollLeft`   | 左からどれだけスクロールしているか   |

```js
const box = document.querySelector('.box');
console.log(box.scrollHeight); // 全体の高さ
box.scrollTop = 100;           // スクロールを100px下に移動
```

## 違い
| 系統       | 見てるのは？               | スクロール量に影響される？ | 用途               |
| -------- | -------------------- | ------------- | ---------------- |
| `client` | 表示されている領域（padding込み） | いいえ           | レイアウト計算、可視範囲     |
| `scroll` | 全体のコンテンツ             | はい            | スクロール制御、スクロール量取得 |


## よくある組み合わせ
- `getBoundingClientRect()` … 画面上での要素位置
- `clientWidth / clientHeight` … 要素の見えるサイズ
- `scrollWidth / scrollHeight` … スクロール可能な全体サイズ
- `scrollTop / scrollLeft` … スクロール位置