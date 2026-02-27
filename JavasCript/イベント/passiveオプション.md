# passive オプション

## passive: true とは？
＝「スクロールなどの **ブラウザ標準動作** を先にやっていいよ」とブラウザに許可する設定
👉 スクロールをカクつかせないため

## 何が問題だったのか
通常、イベントの流れは：
```bash
イベント発生
↓
イベントリスナー実行（重たい処理）
↓
デフォルト動作（スクロールなど）
```
つまり：
👉 重たい処理があると、スクロールが遅れる → ガクガクになる

## passive: true を付けると？
```js
addEventListener("wheel", handler, { passive: true });
```
＝第三引数に入れる

流れが変わる：
```bash
イベント発生
↓
【先に】スクロール
↓
あとからイベント処理
```
👉 スクロールが超なめらかになる

## どのイベントで使う？
主にこの3つ：
| イベント       | 目的       |
| ---------- | -------- |
| wheel      | マウススクロール |
| touchstart | スマホ指タッチ  |
| touchmove  | スマホスワイプ  |
👉 スクロール系専用

## passive の true / false の意味
| 設定             | 意味          |
| -------------- | ----------- |
| passive: true  | 先にスクロールしてOK |
| passive: false | 処理が終わるまで待つ  |

## デフォルトでは？
- 仕様上：false
- 実際のブラウザ：
Chrome / Firefox → 自動で true にしてくれることが多い
👉 でも 安全のため明示的に true を書くのがベスト

## 重要ルール①
`passive: true` のとき `preventDefault()` は使えない
```js
addEventListener("wheel", e => {
  e.preventDefault();
}, { passive: true });
```
👉 エラーになる & 無視される
「先にスクロールしていい」って言っといて「やっぱ止めて」は矛盾しているため

## 重要ルール②
1つでも passive:false があると全部無効

```js
window → passive:true
body   → passive:false
```
この場合：
👉 スクロールは“待ち”になる

つまり：
- 1人でも「待って」と言ったら全員待つ

## クリックイベントに passive:true つけた場合
👉 意味なし
- クリックはスクロールじゃない
- 先にやるデフォルト動作がない
→ 無視される

**でも preventDefault() は使えなくなる**
```js
click + passive:true + preventDefault()
```
👉 エラー & 無効

## まとめ
- `passive:true` = スクロールを先にやらせる
- 対象 = `wheel / touchstart / touchmove`
- 目的 = スクロールを滑らかに
- `passive:true` の時は `preventDefault` できない
