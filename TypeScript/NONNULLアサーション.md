# NONNULLアサーション（!）
- **絶対にnullじゃないから安心して使っていいよ** とTSに言い張る

例：
```ts
const input = document.getElementById('input')!;
input.value = "hello";
```
本来は：
```ts
const input = document.getElementById('input');
// inputは null の可能性あり（input or string）
```

## 使い方
```ts
// 定型文（式の最後に!を付けることでnullじゃないよという意味）
変数!


// 例
const input = document.getElementById('input')!;
// inputは絶対nullじゃないよという意味
```

## 使いどころ
- 「絶対存在する」と自信があるとき
- HTMLで確実にある要素など

## 同じ意味の別の方法
### ① if文（安全）
```ts
// if文であったら処理してね
if (input) {
  input.value = "hello";
}
```

### ② 型アサーション
```ts
// as HTMLInputElement でinputのHTMLだとと教える
const input = document.getElementById('input') as HTMLInputElement;
```

## JSにある？
❌ ない（TSのもの）

## ⚠️ 注意
間違ってたら普通にエラーになる（危険）