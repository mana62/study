# restパラメータの時の型の付け方（...nums）
引数を好きなだけ受け取れるJSの仕組み

例：
```ts
function add(...nums: number[]) {
  return nums.reduce((a, b) => a + b, 0);
}

add(1, 2);        // OK
add(1, 2, 3, 4);  // OK
```

## ポイント
- (...nums) → まとめて配列になる
- **型は必ず配列にする**

## ⚠️ 注意
```ts
function add(...nums: number) // ❌
```