# URL（URL インターフェイス）
＝ URL を安全・正確に扱うための便利な仕組み
＝ブラウザ（や Node.js）が提供している URL を解析・操作するためのオブジェクト
＝文字列としての URL をそのまま扱うよりも、**「プロトコル」「ホスト」「パス」「クエリ」などを自動で分解してくれる**

## URL オブジェクトの基本
```js
const url = new URL("https://example.com/path/page?name=alice#top");
```
👉 これだけで、URL が以下のように分解される

| プロパティ | 値 |
|-----------|----------------|
| url.protocol | "https:" |
| url.hostname | "example.com" |
| url.pathname | "/path/page" |
| url.search   | "?name=alice" |
| url.hash     | "#top" |


## URL オブジェクトが便利な理由
### ① クエリパラメータを簡単に扱える
```js
const url = new URL("https://example.com/?name=alice&age=20");

console.log(url.searchParams.get("name")); // "alice"
console.log(url.searchParams.get("age"));  // "20"
```

追加もできる
```js
url.searchParams.set("city", "tokyo");
console.log(url.toString());
// https://example.com/?name=alice&age=20&city=tokyo
```

### ② 相対パスを解決してくれる
```js
const base = new URL("https://example.com/dir/");
const url = new URL("sub/page.html", base);

console.log(url.toString());
// https://example.com/dir/sub/page.html
```

### ③ URL を安全に組み立てられる
＝文字列連結で URL を作るとミスしやすいけど、URL オブジェクトなら安心
```js
const url = new URL("https://api.example.com/");
url.pathname = "/v1/users";
url.searchParams.set("limit", 10);

console.log(url.toString());
// https://api.example.com/v1/users?limit=10
```

## まとめ
- URL インターフェイスは「URL を安全に分解・操作するためのツール」
- new URL() で URL オブジェクトを作る
- プロパティで URL の各部分を取得できる
- searchParams でクエリを簡単に操作できる
- 相対パスの解決も自動でやってくれる