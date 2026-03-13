# fetch()の第二引数でファイルを送る方法

## ① fetchのBODYには「ファイル」もそのまま入れられる

通常：
```js
body: JSON.stringify(data)
```
のようにJSON文字列を送る

しかし：
👉 Fileオブジェクトをそのままbodyに入れられる

```js
body: file
```

## ② Fileは実はBlobの仲間
- ユーザーが <input type="file"> で選ぶ
- input.files[0] で取得
- これは Fileオブジェクト

このFileは実は：
👉 **Blobを継承している**

## ③ Blobオブジェクトとは？
Blob = **バイナリデータのかたまり**（バイナリ=２進数）

簡単に言うと：
- 画像
- PDF
- 動画
- 生データ
みたいな「**そのままのデータ**」

| 項目   | 内容                   |
| ---- | -------------------- |
| size | データサイズ               |
| type | MIMEタイプ（例：image/png） |
| 中身   | 変更できない（固定）           |

## ④ fetchにBlobを入れると？
```js
fetch(url, {
  method: "POST",
  body: file
})
```
すると：
- ファイルの中身がそのまま送信される
- Content-Type ヘッダーは自動で設定される（image/pngなど）
- ※ ArrayBufferを使うと自動設定されない

## ⑤ Blobは自分でも作れる
＝newでインスタンスを生成する
```js
const blob = new Blob(["Hello"], {
  type: "text/plain"
});
```
- 文字列から作れる
- ArrayBufferからも作れる
- Blob同士をくっつけることもできる

## ⑥ Blob ⇄ 文字列 の変換
**Blob → 文字列**
```js
blob.text()
```

**文字列 → バイナリ**
```js
new TextEncoder().encode("Hello")
```

**バイナリ → 文字列**
```js
new TextDecoder().decode(buffer)
```

## ⑦ Blobを画像表示する方法
- `<img>`タグはURLしか受け取れない
- でもBlobはURLじゃない

そこで使うのが：
```js
URL.createObjectURL(blob)
```

これで：
👉 一時的なURLが作られる（blob:〜 というURL）
```js
img.src = URL.createObjectURL(file);
```
これで画像表示できる

## ⑧ 使い終わったら解放する（revokeObjectURL()）
- **Blob URLは自動で消えない**

なので使い終わったら：
```js
URL.revokeObjectURL(url);
```
これをしないと**メモリに残り続ける可能性あり**

ベストタイミングは：
```js
img.onload = () => {
  URL.revokeObjectURL(url);
};
```

## まとめ
- 画像はJSONではなく Blob/Fileで送る
- FileはBlobの一種
- fetchのbodyにそのまま入れられる
- 画像表示には `URL.createObjectURL()`
- い終わったら `URL.revokeObjectURL()`