# Cookieに安全に文字列を保存する `encodeURIComponent` と `decodeURIComponent`

## Cookieにそのまま文字列を入れられない理由
- Cookieの値には セミコロン ; や特殊文字 を直接入れられない
- 入れると 途中で切れてしまう（名前だけになる）

## 解決方法
- 文字列を **エンコードして安全な文字列に変換**してからCookieに保存
- 取り出す時に **デコードして元の文字列に戻す**

## 使う関数
### `encodeURIComponent()`
```js
let safeValue = encodeURIComponent("値; セミコロン入り");
```
👉 特殊文字（セミコロンや日本語など）を %XX の形式に変換
👉 Cookieに安全に保存可能

### `decodeURIComponent()`
```js
let originalValue = decodeURIComponent(safeValue);
```
👉 エンコードされた文字列を元に戻す
👉 元の値（セミコロン含む文字列）を取得可能

## ⚠️ 注意点
- `encodeURI() / decodeURI()` も似ているが、Cookieに使う場合は セミコロンを変換しないので不適切
- Cookie用なら 必ず `encodeURIComponent` を使う

## 仕組みの簡単な理解
- 文字列 → UTF-8 → 16進数 → %XX に置換（encode）
- %XX を見て → 元のUTF-8 → 元の文字列（decode）

## まとめ
👉 Cookieに安全に文字列を保存したい時は
```js
document.cookie = `name=${encodeURIComponent(value)}`; // 文字列に変換
value = decodeURIComponent(getCookie("name")); // 元の文字に戻す
```