# `multipart/form-data`（ファイルも送れる形式）を作る方法
👉 使うのは **FormData というWeb API**

## 基本の流れ
```js
const formData = new FormData();
formData.append("user", "taro");
formData.append("password", "1234");

fetch("/api", {
  method: "POST",
  body: formData
});
```
これだけで：
- 自動で multipart/form-data になる
- boundaryも自動で付く
- Content-Typeも自動で設定される
👉 自分でヘッダーを書く必要はない

## ✅ データの追加方法
**append（追加）**
```js
formData.append("name", 値);
```
「入れられる値は3種類だけ」
- 文字列
- Blob
- File
❌ ArrayBufferは入れられない

## ✅ 文字列を入れた場合
```js
formData.append("user", "taro");
```
- Content-Typeは付かない
- 普通のテキストとして送られる

## ✅ Blobを入れた場合
```js
const blob = new Blob(["Hello"]);
formData.append("data", blob);
```
- 中身のバイナリが送られる
- type未指定なら → application/octet-stream になる

## ✅ Fileを入れた場合
```js
formData.append("profile", fileInput.files[0]);
```
- ファイルの中身が送られる
- 自動で filename が付く

## ✅ filenameを変更したい場合
```js
formData.append("profile", file, "picture.png");
```
⚠ これは BlobやFileのときだけ使える
文字列のときはエラー

## ✅ append と set の違い
**`append`**
同じ名前を追加すると…
```js
formData.append("profile", file1);
formData.append("profile", file2);
```
→ 2つ入る

**`set`**
```js
formData.set("profile", file2);
```
→ 同じ名前のデータを全部削除してから追加
→ 1つだけになる

## ✅ 値を取得する方法
**has**
```js
formData.has("user");
```
→ あるかどうか（true / false）

**get**
```js
formData.get("profile");
```
→ 最初の1つだけ取る

**getAll**
```js
formData.getAll("profile");
```
→ 全部配列で取れる

## ✅ delete
```js
formData.delete("profile");
```
→ その名前のデータを削除

## ✅ Form要素から自動生成する方法
```js
const form = document.querySelector("form");
const formData = new FormData(form);
```
これをすると：
- form内のinputが全部入る
- ただし name属性が必要

```js
<input name="user">
<input name="password">
<input type="file" name="profile">
```
**⚠️ nameがないと送られない**

## まとめ
- FormDataを使えば自動でmultipartになる
- appendでデータ追加
- 文字列 / Blob / File だけ入れられる
- appendは追加、setは上書き
- getは1つ、getAllは全部
- Formから直接作ることもできる