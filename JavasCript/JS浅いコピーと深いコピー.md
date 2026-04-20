# JS浅いコピーと深いコピー

## **浅いコピー**（shallow copy）→ **スプレッド構文**
```js
const shallow = { ...original };
```
＝ ネストしたオブジェクトは参照がコピーされる → **影響を受ける**

## 深いコピー（deep copy）→ **structuredClone() or JSON方式**
```js
const deep = structuredClone(original);
```
＝ **全部まるごと新しくコピー** → 影響を受けない

## 具体的な違い
```javascript
const original = { a: 1, b: { c: 2 } };

// 浅いコピー
const shallow = { ...original };
shallow.b.c = 99;
console.log(original.b.c); // 99 ← 影響を受ける

// 深いコピー
const deep = structuredClone(original);
deep.b.c = 99;
console.log(original.b.c); // 2 ← 影響を受けない
```

## まとめ
| 方法 | ネストへの影響 |
|------|----------------|
| **浅いコピー（スプレッド構文 `{...obj}`）** | 受ける（ネスト内部は参照のまま） |
| **深いコピー（structuredClone() / JSON方式）** | 受けない（ネストも完全に別オブジェクトになる） |
