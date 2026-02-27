# ブラウザのHTML読み込み＆script最適化
＝「結論」htmlのheadタグの中にscriptタグを読み込みを書き、`defer`属性を追加する

```html
<head>
  <script src="main.js" defer></script>
</head>
```


## ① ブラウザの基本動作
ブラウザは：
**HTML → DOM → CSS → JavaScript → 画面描画**
の順で処理する

## ② HTMLは上から順番に読む
```html
<html>
  ...
</html>
```
👉 上 → 下へ順番にDOMを作る

## ③ CSSはDOMと同時進行で作られる
```html
<link rel="stylesheet">
```
👉 DOM構築と並行でCSSをダウンロード＆解析

## ④ script に遭遇するとDOM構築が止まる（重要）
```js
<script src="main.js"></script>
// DOMを読み込んでからじゃないと実行できないから
```
👉 DOM構築停止 → JSダウンロード → JS実行 → 再開
＝ 表示が遅くなる原因

## ⑤ 対策①：bodyの一番下に置く（昔の常識）
```html
<body>
  ...
  <script src="main.js"></script>
</body>
```
👉 DOM完成後にJS実行 → 安全
❌ でも ダウンロード開始が遅い

## ⑥ 対策②：defer（✅今の最適解）
```html
<head>
  <script src="main.js" defer></script>
</head>
```
👉 最速＆安全

| 項目     | 動作       |
| ------ | -------- |
| ダウンロード | すぐ開始（並行） |
| DOM停止  | しない      |
| 実行     | DOM完成後   |
| 実行順    | 保証される    |

✅ **実務では基本これ一択**

## ⑦ async（広告・解析用）
```html
<script src="analytics.js" async></script>
```
| 項目    | 動作       |
| ----- | -------- |
| DL    | 並行       |
| DOM停止 | 実行時に止まる  |
| 実行    | DL完了次第すぐ |
| 実行順   | 保証なし     |

👉 ページ内容と関係ないJSのみ使用

## ⑧ defer と async の違い
|       | defer | async |
| ----- | ----- | ----- |
| DL    | 並行    | 並行    |
| DOM停止 | ❌     | ⭕     |
| 実行    | DOM後  | 即     |
| 順序保証  | ⭕     | ❌     |
| 用途    | 通常JS  | 広告・解析 |

＝基本は`defer`のみ使用

## ⑨ 実務では
```html
<head>
  <link rel="stylesheet" href="style.css">
  <script src="main.js" defer></script>
</head>
```

## ⑩ 全体フロー図
```bash
HTML解析 → DOM構築 ───────────→ 完成
        ↓
   CSS DL + CSS構築（並行）
        ↓
   JS DL（defer）（並行）
                             ↓
                       DOM完成 → JS実行
```

## まとめ
- HTMLは上からDOMを作る
- CSSはDOMと並行処理
- 通常scriptはDOMを止める
- deferを使えばDLは並行、実行は最後
- asyncは広告・解析専用