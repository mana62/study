# CSS・JS属性セレクター

## jsと属性セレクターの違い


| 項目     | 属性セレクター                 | 正規表現（RegExp）                   |
| -------- | ------------------------------ | ------------------------------------ |
| 主な用途 | HTML要素の検索（CSSやJS）      | 文字列のパターンマッチ               |
| 使う場所 | CSS / JavaScriptのセレクター   | JavaScript / Python / 他の言語       |
| 記法     | `[id^='form-']` など           | `/^form-/` や `form-\d+` など        |
| 検索対象 | DOMの属性値                    | テキスト・文字列                     |
| 柔軟性   | 限定的（始まり・終わり・含む） | 高い（繰り返し・条件分岐・否定など） |

- ➡️ jsの正規表現とはまた別

---

**属性セレクター（CSS/JS）**
```js
document.querySelectorAll("input[name^='user_']")
```
- ➡️ name="user_name" や name="user_email" などを取得

<br>

**正規表現（JS）**
```js
const inputs = document.querySelectorAll("input");
inputs.forEach(input => {
  if (/^user_/.test(input.name)) {
    // 条件に合う input に処理
  }
});
```
- ➡️ より複雑な条件も書ける（例：user_のあとに数字だけ、など）

<br>

---


# 属性セレクター一覧（CSS / JavaScript）

| セレクター        | 意味                                            | 使用例                                      |
| ----------------- | ----------------------------------------------- | ------------------------------------------- |
| `[attr]`          | 属性が存在する要素                              | `[disabled]` → disabled属性を持つ要素       |
| `[attr='value']`  | 属性値が完全一致                                | `[type='submit']` → typeが"submit"のinput   |
| `[attr^='value']` | 属性値が「value」で始まる                       | `[id^='form-']` → idが"form-"で始まる要素   |
| `[attr$='value']` | 属性値が「value」で終わる                       | `[href$='.pdf']` → PDFリンクだけ選択        |
| `[attr*='value']` | 属性値に「value」が含まれる                     | `[class*='active']` → "active"を含むclass   |
| `[attr            | ='value']`                                      | 属性値が「value」または「value-」で始まる   | `[lang | ='en']` → langが"en"または"en-US"など |
| `[attr~='value']` | 属性値がスペース区切りの中に「value」が含まれる | `[class~='btn']` → class="btn primary" など |

---

## よく使う場面
- フォーム操作：input[type='text'] や input[required]
- リンクのフィルタ：a[href$='.pdf'] や a[target='_blank']
- 状態管理：[disabled] や [checked]
- 動的スタイル：[data-status='error'] などカスタム属性に応じたCSS

---

## まとめ
**➡️ 属性セレクターは「HTML要素をざっくり探すための便利ツール」**
**➡️ 正規表現は「文字列を細かく分析・抽出するための超強力な武器」**