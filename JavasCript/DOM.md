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

