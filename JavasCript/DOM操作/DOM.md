# DOMとは
- DOM：「HTMLをJavaScriptから操作するための設計図」


## DOM基本

| 操作         | メソッド・プロパティ                | 説明                       | 使用例                              |
| ------------ | ----------------------------------- | -------------------------- | ----------------------------------- |
| 要素取得     | `document.getElementById(id)`       | ID指定で取得               | `document.getElementById('todo')`   |
| 要素作成     | `document.createElement(tag)`       | 新しい要素を作る           | `document.createElement('li')`      |
| 子要素追加   | `element.appendChild(child)`        | 子要素を追加               | `ul.appendChild(li)`                |
| クラス追加   | `element.classList.add(name)`       | クラスを追加（副作用少）   | `div.classList.add('active')`       |
| クラス削除   | `element.classList.remove(name)`    | クラスを削除               | `div.classList.remove('hidden')`    |
| クラス切替   | `element.classList.toggle(name)`    | クラスのON/OFF             | `btn.classList.toggle('open')`      |
| 属性設定     | `element.setAttribute(name, value)` | 任意属性を設定             | `a.setAttribute('href', url)`       |
| 属性取得     | `element.getAttribute(name)`        | 属性値を取得               | `img.getAttribute('src')`           |
| スタイル変更 | `element.style.property = value`    | インラインスタイル変更     | `div.style.color = 'red'`           |
| テキスト変更 | `element.textContent`               | テキストのみ変更           | `p.textContent = 'こんにちは'`      |
| HTML挿入     | `element.innerHTML`                 | HTMLごと挿入（副作用注意） | `div.innerHTML = '<span>OK</span>'` |

---

## DOMツリーの構造

| 操作           | プロパティ                       | 説明             | 使用例                      |
| -------------- | -------------------------------- | ---------------- | --------------------------- |
| 親要素取得     | `element.parentElement`          | 親の要素を取得   | `li.parentElement`          |
| 子要素取得     | `element.children`               | 子要素一覧       | `ul.children`               |
| 最初の子要素   | `element.firstElementChild`      | 最初の子要素     | `ul.firstElementChild`      |
| 最後の子要素   | `element.lastElementChild`       | 最後の子要素     | `ul.lastElementChild`       |
| 兄弟要素（前） | `element.previousElementSibling` | 前の兄弟要素     | `li.previousElementSibling` |
| 兄弟要素（後） | `element.nextElementSibling`     | 次の兄弟要素     | `li.nextElementSibling`     |
| 近くの親要素   | `element.closest(selector)`      | 条件に合う親要素 | `btn.closest('.container')` |

---

## 配列的に扱うとき(domではない)

| 操作     | メソッド                | 説明                   | 使用例                                           |
| -------- | ----------------------- | ---------------------- | ------------------------------------------------ |
| 配列化   | `Array.from(arrayLike)` | 子要素などを配列に変換 | `Array.from(ul.children)`                        |
| 変換     | `.map(fn)`              | 各要素を変換           | `items.map(x => x.textContent)`                  |
| 条件判定 | `.some(fn)`             | 1つでもtrueならtrue    | `items.some(x => x.classList.contains('error'))` |
| 絞り込み | `.filter(fn)`           | 条件に合う要素だけ抽出 | `items.filter(x => x.tagName === 'LI')`          |

---

## イベント操作(dom API)

| 操作         | メソッド                          | 説明               | 使用例                                       |
| ------------ | --------------------------------- | ------------------ | -------------------------------------------- |
| イベント追加 | `.addEventListener(event, fn)`    | イベントを登録     | `input.addEventListener('input', fn)`        |
| イベント削除 | `.removeEventListener(event, fn)` | 登録解除           | `btn.removeEventListener('click', fn)`       |
| イベント発火 | `.dispatchEvent(event)`           | 手動でイベント発火 | `element.dispatchEvent(new Event('change'))` |

---

## その他よく使うメソッド

| 操作               | メソッド                              | 説明                  | 使用例                              |
| ------------------ | ------------------------------------- | --------------------- | ----------------------------------- |
| 要素の存在確認     | `document.querySelector(selector)`    | CSSセレクタで1つ取得  | `document.querySelector('.active')` |
| 複数要素取得       | `document.querySelectorAll(selector)` | CSSセレクタで複数取得 | `document.querySelectorAll('li')`   |
| 要素削除           | `element.remove()`                    | DOMから削除           | `li.remove()`                       |
| スクロール位置取得 | `element.scrollTop`                   | スクロール量取得      | `div.scrollTop`                     |
| フォーカス制御     | `element.focus()` / `element.blur()`  | 入力フォーカス制御    | `input.focus()`                     |

---

## 補足：副作用ゼロ設計の観点

- `className = '...'` は既存クラスを上書きするため、**副作用あり**
- `classList.add/remove/toggle` は既存クラスを保持しながら操作できるため、**副作用少なめ**
- `innerHTML` はHTML構造を壊す可能性があるため、**再現性重視なら避ける**


### （append / prepend / before / after）

| メソッド          | 意味             | 使い方                    | どこに使うか（例）              | 引数                | 戻り値 |
| ------------- | -------------- | ---------------------- | ---------------------- | ----------------- | --- |
| **append()**  | 要素の**中の末尾**に追加 | `parent.append(node)`  | リスト追加 / チャット表示 / DOM構築 | Node または 文字列（複数可） | なし  |
| **prepend()** | 要素の**中の先頭**に追加 | `parent.prepend(node)` | 新着表示 / 通知 / ログ         | Node または 文字列（複数可） | なし  |
| **before()**  | 要素の**直前**に追加   | `el.before(node)`      | エラー文挿入 / 見出し追加         | Node または 文字列（複数可） | なし  |
| **after()**   | 要素の**直後**に追加   | `el.after(node)`       | 補足説明 / UI拡張            | Node または 文字列（複数可） | なし  |


| やりたいこと      | ベスト     |
| ----------- | ------- |
| リストに1行追加    | append  |
| 新着を上に表示     | prepend |
| エラー文を入力欄の下に | after   |
| 見出しの前に注釈    | before  |


#### in（中）か out（外）か だけ考える
prepend → 中の前
append  → 中の後

before  → 外の前
after   → 外の後

### ⚠️注意
`innerHTML`と、`insertAdjacentHTML`は第三者が悪質な内容の値を書き換えることができるため注意！

### matchesとcontains

matches → 自分が合ってる？
contains → 中に含む？

| メソッド           | 意味                   | 使い方                      | どこで使うか                 | 引数         | 戻り値     |
| -------------- | -------------------- | ------------------------ | ---------------------- | ---------- | ------- |
| **matches()**  | 要素が**セレクタに一致するか判定**  | `el.matches(selector)`   | クリック判定 / イベント分岐 / フィルタ | CSSセレクタ文字列 | boolean |
| **contains()** | 要素が**指定ノードを内包するか判定** | `parent.contains(child)` | 外側クリック検知 / DOM構造チェック   | Node       | boolean |




## よく使うDOM

| カテゴリ | API                    |
| ---- | ---------------------- |
| 取得   | querySelector          |
| 探索   | closest                |
| 追加   | append / prepend       |
| 位置   | before / after         |
| 判定   | **matches / contains** |
| 削除   | remove                 |
| クラス  | classList              |



| 操作カテゴリ          | メソッド / プロパティ                     | 所属インターフェイス         | 意味 / 役割          | Live / Static | 使用例                                              | 備考 / 注意点                           |
| --------------- | -------------------------------- | ------------------ | ---------------- | ------------- | ------------------------------------------------ | ---------------------------------- |
| **要素取得**        | `getElementById(id)`             | Document           | IDで要素取得          | N/A           | `document.getElementById('todo')`                | 1つしか取得できない                         |
|                 | `getElementsByClassName(class)`  | Document / Element | クラス名で要素取得        | Live          | `document.getElementsByClassName('item')`        | DOM変更で自動更新                         |
|                 | `getElementsByTagName(tag)`      | Document / Element | タグ名で要素取得         | Live          | `document.getElementsByTagName('div')`           | DOM変更で自動更新                         |
|                 | `querySelector(selector)`        | Document / Element | CSSセレクタで最初の要素取得  | N/A           | `document.querySelector('.active')`              | 単一要素取得                             |
|                 | `querySelectorAll(selector)`     | Document / Element | CSSセレクタで複数要素取得   | Static        | `document.querySelectorAll('li')`                | 取得時点で固定                            |
|                 | `children`                       | Element            | 子要素一覧            | Live          | `ul.children`                                    | テキストノードは含まない                       |
|                 | `childNodes`                     | Node / Element     | 子ノード一覧（テキスト含む）   | Live          | `ul.childNodes`                                  |                                    |
|                 | `parentElement`                  | Element            | 親要素取得            | N/A           | `li.parentElement`                               | nullになる場合あり                        |
|                 | `firstElementChild`              | Element            | 最初の子要素           | N/A           | `ul.firstElementChild`                           |                                    |
|                 | `lastElementChild`               | Element            | 最後の子要素           | N/A           | `ul.lastElementChild`                            |                                    |
|                 | `previousElementSibling`         | Element            | 前の兄弟要素           | N/A           | `li.previousElementSibling`                      |                                    |
|                 | `nextElementSibling`             | Element            | 次の兄弟要素           | N/A           | `li.nextElementSibling`                          |                                    |
|                 | `closest(selector)`              | Element            | 条件に合う親要素         | N/A           | `btn.closest('.container')`                      |                                    |
| **作成・追加・削除**    | `createElement(tag)`             | Document           | 新しい要素作成          | N/A           | `document.createElement('li')`                   |                                    |
|                 | `appendChild(child)`             | Node               | 子要素末尾に追加         | N/A           | `ul.appendChild(li)`                             | 古いメソッド                             |
|                 | `append()`                       | Element            | 中の末尾に追加          | N/A           | `ul.append(li, 'text')`                          | Node / 文字列複数可                      |
|                 | `prepend()`                      | Element            | 中の先頭に追加          | N/A           | `ul.prepend(li)`                                 |                                    |
|                 | `before()`                       | Element            | 外の直前に追加          | N/A           | `li.before(msg)`                                 |                                    |
|                 | `after()`                        | Element            | 外の直後に追加          | N/A           | `li.after(msg)`                                  |                                    |
|                 | `remove()`                       | Element            | 要素削除             | N/A           | `li.remove()`                                    |                                    |
| **属性・クラス操作**    | `classList.add(name)`            | Element            | クラス追加            | N/A           | `div.classList.add('active')`                    |                                    |
|                 | `classList.remove(name)`         | Element            | クラス削除            | N/A           | `div.classList.remove('hidden')`                 |                                    |
|                 | `classList.toggle(name)`         | Element            | クラスON/OFF        | N/A           | `btn.classList.toggle('open')`                   |                                    |
|                 | `setAttribute(name, value)`      | Element            | 属性設定             | N/A           | `a.setAttribute('href', url)`                    |                                    |
|                 | `getAttribute(name)`             | Element            | 属性取得             | N/A           | `img.getAttribute('src')`                        |                                    |
|                 | `style.property`                 | HTMLElement        | インラインスタイル変更      | N/A           | `div.style.color = 'red'`                        |                                    |
| **テキスト・HTML操作** | `textContent`                    | Node / Element     | テキストのみ変更         | N/A           | `p.textContent = 'こんにちは'`                        | 安全                                 |
|                 | `innerHTML`                      | Element            | HTML構造を変更        | N/A           | `div.innerHTML = '<span>OK</span>'`              | 副作用・XSS注意                          |
| **判定・探索**       | `matches(selector)`              | Element            | 自分がセレクタに一致するか    | N/A           | `el.matches('.active')`                          | boolean                            |
|                 | `contains(node)`                 | Node               | 指定ノードを内包しているか    | N/A           | `parent.contains(child)`                         | boolean                            |
| **イベント操作**      | `addEventListener(event, fn)`    | EventTarget        | イベント登録           | N/A           | `input.addEventListener('input', fn)`            |                                    |
|                 | `removeEventListener(event, fn)` | EventTarget        | イベント解除           | N/A           | `btn.removeEventListener('click', fn)`           |                                    |
|                 | `dispatchEvent(event)`           | EventTarget        | 手動イベント発火         | N/A           | `el.dispatchEvent(new Event('change'))`          |                                    |
| **スクロール・位置**    | `offsetTop` / `offsetLeft`       | HTMLElement        | 親要素からの座標         | N/A           | `box.offsetTop`                                  | スクロール量無視                           |
|                 | `offsetParent`                   | HTMLElement        | 位置計算基準の親要素       | N/A           | `box.offsetParent`                               |                                    |
|                 | `clientWidth` / `clientHeight`   | HTMLElement        | 見える領域サイズ         | N/A           | `box.clientWidth`                                | padding含む、border除く                 |
|                 | `scrollWidth` / `scrollHeight`   | HTMLElement        | スクロール可能な全体サイズ    | N/A           | `box.scrollHeight`                               |                                    |
|                 | `scrollTop` / `scrollLeft`       | HTMLElement        | 現在のスクロール位置       | N/A           | `box.scrollTop = 100`                            | 読み取り・設定可                           |
|                 | `getBoundingClientRect()`        | Element            | ビューポート基準の位置とサイズ  | N/A           | `box.getBoundingClientRect()`                    | left/top/right/bottom/width/height |
|                 | `scrollTo(x, y)`                 | Window / Element   | 絶対位置スクロール        | N/A           | `window.scrollTo(0, 500)`                        | smoothオプション可                       |
|                 | `scrollBy(dx, dy)`               | Window / Element   | 相対スクロール          | N/A           | `window.scrollBy(0, 100)`                        | smooth可                            |
|                 | `scrollIntoView()`               | Element            | 要素が画面内に入るようスクロール | N/A           | `box.scrollIntoView({behavior:'smooth'})`        | block指定可                           |
|                 | `elementFromPoint(x, y)`         | Document           | 指定座標の最前面要素取得     | N/A           | `document.elementFromPoint(100,200)`             | pointer-events:none は無視            |
| **配列操作（DOM以外）** | `Array.from(arrayLike)`          | -                  | 配列化              | N/A           | `Array.from(ul.children)`                        | NodeListやHTMLCollectionを配列化        |
|                 | `map(fn)`                        | Array              | 各要素変換            | N/A           | `items.map(x => x.textContent)`                  |                                    |
|                 | `filter(fn)`                     | Array              | 条件に合う要素抽出        | N/A           | `items.filter(x => x.tagName==='LI')`            |                                    |
|                 | `some(fn)`                       | Array              | 条件1つでもtrueならtrue | N/A           | `items.some(x => x.classList.contains('error'))` |                                    |
