# TSの型安全性とJSの柔軟性
- TypeScriptは型チェックが厳しいイメージがあるけど、JavaScriptの柔軟さも残してある

## JSの暗黙の型変換とは？
**JSは型が違っても勝手に変換して計算しようとする言語**

```js
"hello" + 1        // → "hello1"（数字を文字列に変換した）
"1" + 1            // → "11"（数字を文字列に変換した）
[1, 2] + "hello"   // → "1,2hello"（配列を文字列に変換した）
```
→ これが **「暗黙の型変換」**
明示的に変換しろと言ってないのに、勝手に変換してくれるわけ

## TypeScriptはこれら👆を全部エラーにするは誤解
- TSは型が違うとぜんぶエラーと思われがちだが、実際は違う

TypeScriptの実際：
- 「JavaScriptで普通にやりそうなことはエラーにしない」
- 「さすがにこれは書かないだろってことだけエラーにする」

## 具体的に何がOKで何がNGか
```ts
// ✅ OK（JavaScriptでも普通にやる）
3 + 4              // number + number → number
"hello" + 1        // string + number → string
"hello" + "world"  // string + string → string
[1,2] + "hello"    // array  + string → string

// ❌ NG（さすがにこんな足し算しないよね）
[1,2] + 3          // array + number → エラー
```

## なぜこういう仕様なのか？
TypeScriptは2つの考えのバランスを取っている

```bash
JavaScriptの柔軟さ          TypeScriptの型の安全性
「空気読んでなんとかする」   「おかしいことはエラーにする」
       ↘                          ↙
          いい感じに折り合いをつける
```

- 「string + numberは実際のコードでもよく書くからOKにしよう」
- 「array + numberは普通書かないからエラーにしよう」
という**人間的な判断がTypeScriptに組み込まれている**