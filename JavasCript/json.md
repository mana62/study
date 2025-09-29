# JSON
---

## JSONメソッド

| メソッド / テクニック             | 第一引数       | 第二引数                             | 第三引数                        | 説明                                                           | 使用例                                                                   |
| --------------------------------- | -------------- | ------------------------------------ | ------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------ |
| `JSON.parse()`                    | JSON文字列     | reviver関数（オプション）            | なし                            | JSON文字列をJSオブジェクトに変換する                           | `const obj = JSON.parse('{"name":"Alice"}');`                            |
| `JSON.stringify()`                | JSオブジェクト | replacer（関数 or 配列、オプション） | space（インデント、オプション） | JSオブジェクトをJSON文字列に変換する                           | `JSON.stringify({name:"Alice"}, ["name"], 2)`                            |
| `JSON.parse(JSON.stringify(obj))` | JSオブジェクト | なし                                 | なし                            | オブジェクトの**ディープコピー**を作るための裏技               | `const copy = JSON.parse(JSON.stringify(original));`                     |
| `localStorage.setItem()`          | キー           | JSON文字列                           | なし                            | JSONデータをローカルストレージに保存                           | `localStorage.setItem("user", JSON.stringify(user));`                    |
| `localStorage.getItem()`          | キー           | なし                                 | なし                            | ローカルストレージからJSON文字列を取得し、`JSON.parse()`で復元 | `const user = JSON.parse(localStorage.getItem("user"));`                 |
| `fetch()` + `.json()`             | URL            | オプション（headersなど）            | なし                            | APIからJSONデータを取得し、JSオブジェクトとして扱う            | `fetch('/api').then(res => res.json()).then(data => console.log(data));` |
| `JSON.stringify()` + `Blob`       | JSオブジェクト | なし                                 | なし                            | JSONをファイルとしてダウンロードする際にBlobとして扱う         | `new Blob([JSON.stringify(data)], { type: 'application/json' })`         |

---

