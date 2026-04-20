# インターフェイスや type でのジェネリクス
- 「中身の型だけ差し替えられる型のテンプレート」を作るイメージ
- インターフェイスや type でもジェネリクスが使える
⚠️ ただし **「型推論」ができないので、使うときは必ず型を書く必要がある**

### なぜ型推論できない？
- クラスや関数は「引数の値」を見て型を推論できる
- でも interface/type は値を受け取らないので、TypeScript が型を推測する手がかりがない
- だから使う側が明示する必要がある

## 書き方
```ts
interface ListResponse<T> {
  items: T[];
  total: number;
}

// ユーザー一覧
type User = { id: number; name: string };
const usersRes: ListResponse<User> = {
  items: [{ id: 1, name: "Taro" }],
  total: 1,
};

// 商品一覧
type Product = { id: number; price: number };
const productsRes: ListResponse<Product> = {
  items: [{ id: 10, price: 1000 }],
  total: 1,
};
```
👉 同じ interface ひとつで、「文字列のレスポンス」「ユーザーのレスポンス」どちらにも使える

## まとめ
- 関数やクラスと違って「値を受け取らない」ので型推論ができない
- 使う側が ApiResponse<string> のように必ず型を書く必要があ