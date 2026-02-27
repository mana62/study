# Proxy（プロキシ）
- オブジェクトへのアクセスを **「監視・制御」できるラッパー**
- 元のオブジェクトを直接触らずに、**読み書き・削除・関数呼び出しなどを「フック」できる**

## 使う場面
- 値が変更されたら自動でログを取りたい
- データのバリデーションを自動化したい
- 外部ライブラリがオブジェクトにアクセスする動きを制御したい

## 例文
```js
const obj = { name: "Alice", age: 20 };

const proxy = new Proxy(obj, {
  get(target, prop) {
    console.log(`${prop}を取得しました`);
    return target[prop];
  },
  set(target, prop, value) {
    console.log(`${prop}に${value}を設定しました`);
    target[prop] = value;
    return true;
  }
});

console.log(proxy.name);  // "Alice" + ログ: nameを取得しました
proxy.age = 30;           // ログ: ageに30を設定しました
```
＝オブジェクトの操作を 監視してログを出す ことができる