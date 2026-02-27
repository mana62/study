# Symbol.iterator
- オブジェクトを**反復可能にするための Well-known Symbol**
- **これを持つオブジェクトは for…of でループできる**

## 使う場面
- 自作オブジェクトを配列のようにループしたいとき

```js
const iterableObj = {
  data: [1, 2, 3],
  [Symbol.iterator]() {
    let index = 0;
    return {
      next: () => ({
        value: this.data[index],
        done: index++ >= this.data.length
      })
    };
  }
};

for (const x of iterableObj) {
  console.log(x); // 1 2 3
}
```