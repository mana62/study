# Well-known Symbol（ウェルノウンシンボル）
- JS が内部であらかじめ用意している 特殊なシンボルのセット
- 言語のルールや機能を拡張するときに使われる

## 代表例と用途
| シンボル                        | 何に使う？                                |
| --------------------------- | ------------------------------------ |
| `Symbol.iterator`           | for…of で反復できるようにする                   |
| `Symbol.toStringTag`        | Object.prototype.toString の出力をカスタマイズ |
| `Symbol.isConcatSpreadable` | concat で展開するかどうかを制御                  |


```js
const obj = {
  [Symbol.toStringTag]: "MyObject"
};
console.log(Object.prototype.toString.call(obj)); // [object MyObject]
```