# Symbol（シンボル）
- **他のキーと絶対にぶつからない「特殊なプロパティ名」**

```js
const sym = Symbol('説明'); // のように作る
```

## 使う場面
- オブジェクトのプロパティを「安全に隠す」
- 言語やライブラリが内部で使う特別なキー

## 例
```js
const secret = Symbol("secret");
const obj = {};
obj[secret] = 123;

console.log(obj[secret]); // 123
console.log(obj["secret"]); // undefined
```
👉 "secret" という文字列キーとは 別物 なので他とぶつからない