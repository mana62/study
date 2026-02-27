# Reflect API（リフレクト）
- **Proxy とセットで使われる「オブジェクト操作の標準メソッド」**
- Object の操作（読み書き、削除など）を関数として扱える

## 使う場面
- Proxy のハンドラ内で元の動作を呼びたいとき
- 既存のオブジェクト操作を関数化して扱いたいとき

## 例
```js
const obj = { name: "Alice" };

// Reflect.get を使って値を取得
console.log(Reflect.get(obj, "name")); // Alice

// Reflect.set で値を変更
Reflect.set(obj, "name", "Bob");
console.log(obj.name); // Bob
```

⬇️ Proxy の中で元の挙動を呼び出すときは下記のように書く

```js
const proxy = new Proxy(obj, {
  set(target, prop, value) {
    console.log(`${prop}を変更します`);
    return Reflect.set(target, prop, value); // 元の動作を呼ぶ
  }
});
proxy.name = "Charlie"; // ログ + 元のオブジェクトも変更
```