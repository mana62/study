# API通信の3パターン

## API通信には3パターンある
| 送りたいもの    | 方法       |
| --------- | -------- |
| テキスト      | JSON     |
| ファイル1個    | File     |
| テキスト＋ファイル | FormData |


## ① JSON
- 普通のAPI通信はこれ
```js
fetch("/api", {
  method: "POST",
  body: JSON.stringify(data)
})
```
**送れるもの**
→ テキストデータ

例
- user
- password
- id

## ② でも画像はJSONでは送れない
画像は **バイナリデータ** → JSONでは表現できない

だから
- JSONでは画像を送れない

## ③ fetchのbodyにはFileも入れられる
```js
fetch("/upload", {
  method: "POST",
  body: file
})
```
つまり **ファイル単体なら送れる**

## ④ でも問題がある
例えば
- user
- comment
- image
を送りたい場合

**JSONでは → 画像送れない**
**fileだけだと → テキスト送れない**

## ⑤ そこで登場するのが FormData
```js
const fd = new FormData();

fd.append("user", "taro");
fd.append("comment", "hello");
fd.append("image", file);

fetch("/api", {
  method: "POST",
  body: fd
});
```
すると **`multipart/form-data`** という形式になり
**テキスト＋ファイルを一緒に送れる**

## `multipart/form-data`とは
👉 `multipart/form-data` =「複数のデータを、区切り線を入れて1つのリクエストで送る方法」

特に **テキスト + ファイル（画像など）を一緒に送るための形式**

「普通のJSON」
```js
{
  "user": "taro",
  "age": 20
}
```
👉 これは 全部テキスト

「画像の場合」
```js
101010010101010101001010101...
```
**バイナリデータ（２進数）** 👉 だから JSONに入れられない

**そこで multipart/form-data**！！
👉 データを 区切り線で分けて送る

```js
------boundary
Content-Disposition: form-data; name="user"

taro
------boundary
Content-Disposition: form-data; name="image"; filename="cat.png"
Content-Type: image/png

(画像のバイナリデータ)
------boundary--
```

つまり
```js
[ user ]
[ image ]
```
みたいに **パーツごとに分けて送る**


## まとめ
① 普通のAPI通信
→ JSON

② 画像送る
→ File

③ テキスト＋画像
→ FormData


