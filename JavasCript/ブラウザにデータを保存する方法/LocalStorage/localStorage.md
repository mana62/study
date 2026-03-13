# LocalStorageでデータを保存する方法

## LocalStorageとは
👉 **ブラウザに データを保存できる仕組み**

特徴:
- ブラウザを閉じても残る
- PC再起動しても残る
- 明示的に削除するまで消えない

## データの保存方法
```js
// 定型文
localStorage.setItem("key", "value");
```
例:
```js
localStorage.setItem("name", "John");
```
ポイント:
- **キーと値のセットで保存**
- **値は文字列しか保存できない**

## オブジェクトを保存する方法
- オブジェクトは直接保存できないので`JSON.stringify()`で文字列に変換する

例:
```js
localStorage.setItem("user", JSON.stringify(user));
```

## データの取得
```js
// 定型文
localStorage.getItem("key");
```
例:
```js
const result = localStorage.getItem("name");
console.log(result); // John
```
- データが無い場合は`null`が返る

## データの更新
- **同じキーで setItem すると更新**される
```js
localStorage.setItem("name", "Smith");
```

## データ削除
**1つ削除**
```js
localStorage.removeItem("name");
```
**全部削除**
```js
localStorage.clear();
```

## 保存データの確認（Chrome）
Chromeの
```bash
DevTools
↓
Application
↓
Local Storage
```
で確認できる

## LocalStorageは「同じサイト」で共有される
- LocalStorageは **同じサイト（同じURLのグループ）なら全部のタブで共有される**

例:
タブ①
`example.com`

タブ②
`example.com`

この場合、タブ①で保存したデータは
👉 タブ②でも取得できる

例:
タブ①
`localStorage.setItem("name", "John");`

タブ②
`localStorage.getItem("name");`

結果
`John`

つまり:
**LocalStorage = 同じサイトのタブ同士で共有される**

## ただし「違うサイト」では共有されない

例:
```bash
example.com
google.com
```
この場合 ❌ 共有されない
理由: サイトが違うから

## ブラウザが違う場合も共有されない

例:
```bash
Chrome
Firefox
```
Chromeで保存したLocalStorageは ❌ Firefoxでは見えない

## Storage Event（ストレージイベント）
👉 これは少し便利な機能
👉 別タブでLocalStorageが変更されたら通知がくる

例:
タブ①
```js
window.addEventListener("storage", (event)=>{
 console.log(event);
});
```
タブ②
```js
localStorage.setItem("name","John");
```

すると、タブ①で **イベント発生**
👉 つまり **他のタブのLocalStorage変更を検知できる**

## イベントで分かる情報
| プロパティ    | 意味         |
| -------- | ---------- |
| key      | 変更されたキー    |
| newValue | 新しい値       |
| oldValue | 前の値        |
| url      | 変更したページURL |

例:
```bash
key: "name"
oldValue: "John"
newValue: "Smith"
```

## SessionStorageはタブ共有できない
- SessionStorageは ❌ タブ共有できない

例:
タブ①
`sessionStorage.setItem("name","John")`

タブ②
`sessionStorage.getItem("name")`

結果: null
理由: SessionStorageはタブ専用だから

## SessionStorageとの違い
👉 LocalStorageとほぼ同じだが違いがある

| 項目    | LocalStorage | SessionStorage |
| ----- | ------------ | -------------- |
| データ保存 | 永久           | タブが閉じると消える     |
| タブ共有  | 共有される        | 共有されない         |

**SessionStorage**
- 同じタブの中だけ
- タブを閉じると消える

## まとめ
**LocalStorage**
- ブラウザにデータ保存できる
- ブラウザ閉じても残る
- 文字列しか保存できない
- key-value形式
- 同じサイトならタブ共有できる
- 別サイトでは共有できない
- 別ブラウザでも共有できない
- storageイベントで変更を検知できる

「主なメソッド」
```bash
setItem()   保存
getItem()   取得
removeItem() 削除
clear()     全削除
```

**SessionStorage**
- タブ専用
- 他タブと共有できない


// localStorage
- ブラウザに永久保存（削除しない限り残る）
- 容量: 5-10MB
- JavaScript からのみアクセス
- サーバーに送信されない

// Cookie
- 有効期限を設定できる
- 容量: 4KB
- JavaScript とサーバー両方からアクセス
- リクエストごとにサーバーに送信される