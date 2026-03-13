# FormData
👉 **テキスト＋画像を一緒に送る方法 → FormData を使う**

## どういう時に使うのか？
- ユーザーID（テキスト）
- パスワード（テキスト）
- プロフィール画像（ファイル）
👉 これを 一発でサーバーに送りたい とき

## ❌ できない方法
⚫︎ JSONだけだと
- 画像を入れられない（バイナリだから）

⚫︎ Blobだけだと
- テキスト情報を一緒に整理して送れない

## ✅ 解決策（multipart/form-data）
👉 `multipart/form-data` : 「1つのリクエストの中に、複数の種類のデータを入れられる形式」フォーム送信と同じ仕組み

## fetchでの基本的な書き方
```js
const formData = new FormData(); // ここで生成

formData.append("userId", "taro"); // appendで複数作れる
formData.append("password", "1234");
formData.append("profile", file); // ファイル

fetch("/register", {
  method: "POST",
  body: formData
});
```

## 内部の動き
```bash
----境界線
userIdのデータ
----境界線
passwordのデータ
----境界線
profileの画像データ
----境界線
```
👉 こんな感じで「区切り線」でデータを分けている
👉 この区切り線を **boundary（バウンダリー）** と呼ぶ

## ポイント
❗ **Content-Typeは自分で書かない**

```js
// ❌ これはダメ
headers: {
  "Content-Type": "multipart/form-data"
}
```
👉 これやると壊れる

なぜ：
👉 **boundary を自動で付けてくれるから**

FormDataを使えば：
- Content-Type
-  boundary
**全部ブラウザが自動でやってくれる**

## まとめ
| 送りたいもの  | 使うもの     |
| ------- | -------- |
| テキストだけ  | JSON     |
| 画像だけ    | Blob     |
| テキスト＋画像 | FormData |

- 複数種類のデータを一気に送るときは FormData を使う
- "Content-Type"は書かない
- `new FormData();`で生成して、`formData.append("キー", "バリュー");`のように複数書く