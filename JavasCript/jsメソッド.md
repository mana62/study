# JSメソッド

---

## 基本関数・メソッド

| 名称                            | 第一引数                        | 第二引数               | 説明                       | 使用例                                        |
| ------------------------------- | ------------------------------- | ---------------------- | -------------------------- | --------------------------------------------- |
| `alert(message)`                | message: 表示する文字列         | なし                   | ダイアログを表示する       | `alert("Hello");`                             |
| `console.log(message)`          | message: 表示する値             | なし                   | コンソールに出力           | `console.log("Hello");`                       |
| `setTimeout(func, ms)`          | func: 実行関数                  | ms: ミリ秒             | 指定時間後に一度だけ実行   | `setTimeout(() => console.log("Hi"), 1000);`  |
| `setInterval(func, ms)`         | func: 実行関数                  | ms: ミリ秒             | 指定時間ごとに繰り返し実行 | `setInterval(() => console.log("Hi"), 1000);` |
| `clearTimeout(timerId)`         | timerId: setTimeoutの返り値     | なし                   | タイマーを解除             | `clearTimeout(id);`                           |
| `clearInterval(intervalId)`     | intervalId: setIntervalの返り値 | なし                   | 繰り返しタイマーを停止     | `clearInterval(id);`                          |
| `addEventListener(event, func)` | event: イベント名               | func: コールバック関数 | イベントを設定             | `window.addEventListener("load", () => {});`  |
| `innerWidth`                    | なし                            | なし                   | ウィンドウ幅を取得         | `console.log(window.innerWidth);`             |
| `innerHeight`                   | なし                            | なし                   | ウィンドウ高さを取得       | `console.log(window.innerHeight);`            |
| `location`                      | なし                            | なし                   | URL情報を取得/変更         | `console.log(location.pathname);`             |
| `navigator`                     | なし                            | なし                   | ブラウザ情報を取得         | `console.log(navigator.userAgent);`           |

---

## documentオブジェクトのメソッド・プロパティ

| 名称                                | 第一引数           | 第二引数          | 説明                         | 使用例                                        |
| ----------------------------------- | ------------------ | ----------------- | ---------------------------- | --------------------------------------------- |
| `getElementById(id)`                | id名               | なし              | 指定IDの要素を取得           | `document.getElementById("header");`          |
| `getElementsByClassName(className)` | クラス名           | なし              | クラス名に一致する要素を取得 | `document.getElementsByClassName("item");`    |
| `querySelector(selector)`           | CSSセレクタ        | なし              | 条件に一致する最初の要素     | `document.querySelector("#id .class");`       |
| `querySelectorAll(selector)`        | CSSセレクタ        | なし              | 条件に一致する全要素         | `document.querySelectorAll('[id^="item-"]');` |
| `appendChild(node)`                 | node: 追加する要素 | なし              | 子要素を追加                 | `parent.appendChild(child);`                  |
| `removeChild(node)`                 | node: 削除する要素 | なし              | 子要素を削除                 | `parent.removeChild(child);`                  |
| `replaceChild(newNode, oldNode)`    | newNode: 新要素    | oldNode: 置換対象 | 子要素を置換                 | `parent.replaceChild(newChild, oldChild);`    |
| `createElement(tag)`                | タグ名             | なし              | HTML要素作成                 | `document.createElement("div");`              |
| `getAttribute(name)`                | 属性名             | なし              | 属性値取得                   | `el.getAttribute("href");`                    |
| `setAttribute(name, value)`         | 属性名             | 値                | 属性値設定                   | `el.setAttribute("id", "newId");`             |
| `parentElement`                     | なし               | なし              | 親要素を取得                 | `el.parentElement;`                           |
| `nextElementSibling`                | なし               | なし              | 次の兄弟要素取得             | `el.nextElementSibling;`                      |
| `previousElementSibling`            | なし               | なし              | 前の兄弟要素取得             | `el.previousElementSibling;`                  |
| `innerHTML`                         | なし               | なし              | 内部HTML取得/変更            | `el.innerHTML = "<p>Hello</p>";`              |
| `innerText`                         | なし               | なし              | テキスト取得/変更            | `el.innerText = "Hello";`                     |
| `style`                             | なし               | なし              | CSS操作                      | `el.style.color = "red";`                     |
| `classList`                         | なし               | なし              | クラス操作                   | `el.classList.add("active");`                 |
| `closest(selector)`                 | CSSセレクタ        | なし              | 最も近い親要素を取得         | `el.closest(".container");`                   |

---

## windowイベント

| イベント名     | 発生タイミング                     | 使用例                                               |
| -------------- | ---------------------------------- | ---------------------------------------------------- |
| `load`         | ページ全体読み込み完了             | `window.addEventListener("load", () => {});`         |
| `scroll`       | スクロール時                       | `window.addEventListener("scroll", () => {});`       |
| `resize`       | ウィンドウサイズ変更時             | `window.addEventListener("resize", () => {});`       |
| `pageshow`     | 戻るボタンなどでキャッシュから表示 | `window.addEventListener("pageshow", () => {});`     |
| `beforeunload` | ページ離脱直前                     | `window.addEventListener("beforeunload", () => {});` |

---

## 文字列メソッド

| メソッド                      | 第一引数  | 第二引数 | 説明             | 使用例                      | 結果            |
| ----------------------------- | --------- | -------- | ---------------- | --------------------------- | --------------- |
| `.slice(start, end)`          | start     | end      | 部分文字列取得   | `"JavaScript".slice(0,4)`   | `"Java"`        |
| `.substring(start,end)`       | start     | end      | 部分文字列取得   | `"Hello".substring(1,4)`    | `"ell"`         |
| `.split(separator)`           | separator | なし     | 配列に変換       | `"a,b,c".split(",")`        | `["a","b","c"]` |
| `.indexOf(search)`            | search    | なし     | 文字列位置       | `"Hello".indexOf("e")`      | `1`             |
| `.toUpperCase()`              | なし      | なし     | 大文字に変換     | `"hi".toUpperCase()`        | `"HI"`          |
| `.toLowerCase()`              | なし      | なし     | 小文字に変換     | `"HI".toLowerCase()`        | `"hi"`          |
| `.replace(search,replace)`    | search    | replace  | 最初の一致置換   | `"abc".replace("a","A")`    | `"Abc"`         |
| `.replaceAll(search,replace)` | search    | replace  | 全置換           | `"aaa".replaceAll("a","b")` | `"bbb"`         |
| `.includes(sub)`              | sub       | なし     | 含むか判定       | `"abc".includes("b")`       | `true`          |
| `.trim()`                     | なし      | なし     | 前後空白削除     | `" a ".trim()`              | `"a"`           |
| `.startsWith(str)`            | str       | なし     | 開始文字列判定   | `"abc".startsWith("a")`     | `true`          |
| `.endsWith(str)`              | str       | なし     | 終了文字列判定   | `"abc".endsWith("c")`       | `true`          |
| `.charAt(pos)`                | pos       | なし     | 指定位置文字取得 | `"abc".charAt(1)`           | `"b"`           |

---

## 数値メソッド / Math

| メソッド            | 第一引数 | 第二引数 | 説明             | 使用例            | 結果   |
| ------------------- | -------- | -------- | ---------------- | ----------------- | ------ |
| `Math.random()`     | なし     | なし     | 0以上1未満の乱数 | `Math.random()`   | 0.123… |
| `Math.ceil(num)`    | num      | なし     | 小数切り上げ     | `Math.ceil(2.3)`  | 3      |
| `Math.floor(num)`   | num      | なし     | 小数切り捨て     | `Math.floor(2.7)` | 2      |
| `Math.max(...nums)` | 数値     | なし     | 最大値           | `Math.max(1,5)`   | 5      |
| `Math.min(...nums)` | 数値     | なし     | 最小値           | `Math.min(1,5)`   | 1      |
| `Math.sqrt(num)`    | num      | なし     | 平方根           | `Math.sqrt(16)`   | 4      |

---

## JSONメソッド

| メソッド              | 第一引数        | 第二引数              | 第三引数 | 説明                      | 使用例                           |
| --------------------- | --------------- | --------------------- | -------- | ------------------------- | -------------------------------- |
| `JSON.parse(str)`     | str: JSON文字列 | reviver（オプション） | なし     | 文字列 → JSオブジェクト   | `JSON.parse('{"a":1}')`          |
| `JSON.stringify(obj)` | obj             | replacer              | space    | オブジェクト → JSON文字列 | `JSON.stringify({a:1}, null, 2)` |

---

## Web Storage

| メソッド                             | 第一引数    | 第二引数      | 説明                 | 使用例                                |
| ------------------------------------ | ----------- | ------------- | -------------------- | ------------------------------------- |
| `localStorage.setItem(key, value)`   | key: 文字列 | value: 文字列 | 永続的に保存         | `localStorage.setItem("id","123");`   |
| `localStorage.getItem(key)`          | key         | なし          | データ取得           | `localStorage.getItem("id")`          |
| `sessionStorage.setItem(key, value)` | key         | value         | セッション単位で保存 | `sessionStorage.setItem("id","123");` |
| `sessionStorage.getItem(key)`        | key         | なし          | データ取得           | `sessionStorage.getItem("id")`        |

---

## 日付メソッド

| メソッド                                  | 第一引数 | 第二引数 | 説明                     | 使用例                         |
| ----------------------------------------- | -------- | -------- | ------------------------ | ------------------------------ |
| `new Date()`                              | なし     | なし     | 現在日時オブジェクト生成 | `const d = new Date();`        |
| `date.getDate()`                          | なし     | なし     | 日                       | `d.getDate();`                 |
| `date.getFullYear()`                      | なし     | なし     | 年（4桁）                | `d.getFullYear();`             |
| `date.getMonth()`                         | なし     | なし     | 月（0-11）               | `d.getMonth();`                |
| `date.setDate(num)`                       | num      | なし     | 日を設定                 | `d.setDate(15);`               |
| `String.prototype.padStart(length, char)` | length   | char     | 文字列先頭埋め           | `"5".padStart(2,"0")` → `"05"` |

---

## URL操作

| メソッド / プロパティ               | 第一引数 | 第二引数 | 説明                        | 使用例                                  |
| ----------------------------------- | -------- | -------- | --------------------------- | --------------------------------------- |
| `encodeURIComponent(str)`           | str      | なし     | URLパラメータ用にエンコード | `encodeURIComponent("a&b")` → `"a%26b"` |
| `location.pathname`                 | なし     | なし     | 現在ページのパス取得        | `console.log(location.pathname);`       |
| `URLSearchParams.get(key)`          | key      | なし     | クエリパラメータ取得        | `params.get("id")`                      |
| `URLSearchParams.set(key,value)`    | key      | value    | クエリ更新                  | `params.set("page","2")`                |
| `URLSearchParams.append(key,value)` | key      | value    | クエリ追加                  | `params.append("tag","news")`           |
| `URLSearchParams.delete(key)`       | key      | なし     | クエリ削除                  | `params.delete("sort")`                 |
| `URLSearchParams.toString()`        | なし     | なし     | クエリ文字列化              | `params.toString()` → `"id=123"`        |

---

## RegExp / 正規表現

| 構文                            | 説明                            | 使用例                                        |
| ------------------------------- | ------------------------------- | --------------------------------------------- |
| `new RegExp("pattern","flags")` | 正規表現オブジェクト生成        | `new RegExp("\\b3\\b","g")`                   |
| `\b`                            | 単語境界                        | `/\bword\b/`                                  |
| `[id^="item-"]`                 | 属性セレクタ、idがitem-で始まる | `document.querySelectorAll('[id^="item-"]');` |

---

## その他便利関数

| 関数                   | 第一引数 | 第二引数 | 説明         | 使用例                                |
| ---------------------- | -------- | -------- | ------------ | ------------------------------------- |
| `Array.isArray(value)` | value    | なし     | 配列か判定   | `Array.isArray([1,2])` → `true`       |
| `crypto.randomUUID()`  | なし     | なし     | UUID生成     | `crypto.randomUUID()`                 |
| `stripHtmlTags(str)`   | str      | なし     | HTMLタグ削除 | `stripHtmlTags("<p>Hi</p>")` → `"Hi"` |





`toLocaleString()`:数値にカンマを入れるメソッド 例: 1300 → 1,300

`Array.prototype.find()` : 配列の中から条件に合う最初の1件を返すメソッド