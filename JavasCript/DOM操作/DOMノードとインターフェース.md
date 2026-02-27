# DOMノードとインターフェース

## インターフェイス継承とは
### ① ノードは単なるKEY-VALUEではない
＝DOMのノードは、単純な「キーと値のペア」ではなく、インターフェースの継承構造を持っている
→ JavaScriptのクラス継承とほぼ同じ考え方

### ② インターフェースは階層的に継承されている
```bash
# 例：<input> 要素の場合
EventTarget
   ↑
Node
   ↑
Element
   ↑
HTMLElement
   ↑
HTMLInputElement
```
＝つまり HTMLInputElement は、上位すべてのインターフェースの機能を持つ

### それぞれの役割
| インターフェース         | 役割                         |
| ---------------- | -------------------------- |
| EventTarget      | イベント処理（addEventListenerなど） |
| Node             | DOMノードの基本機能                |
| Element          | 要素ノード共通の機能                 |
| HTMLElement      | HTML要素共通の機能                |
| HTMLInputElement | input要素特有の機能               |

### ③ すべてのノードはイベントを扱える
Node は EventTarget を継承している → すべてのDOMノードでイベント処理が可能
```js
node.addEventListener(...)
```
がどのノードでも使える理由

### ④ HTML以外の要素も存在する
HTML要素以外にも、例えば：SVG要素 → SVGElementなどがあり、Element → Node → EventTarget の共通構造は同じ

### ⑤ Documentの継承構造は少し違う
Documentは「要素」ではないため：
```js
EventTarget
   ↑
Node
   ↑
Document
```
→ Element / HTMLElement を継承しない

### ⑥ MDNで継承構造が確認できる
MDNで HTMLInputElement などを検索すると、
継承関係の図 が表示される → 理解に非常に役立つ

### ⑦ 継承構造を理解する最大のメリット
「このメソッドはどのインターフェースのものか？」が分かるようになる

すると：
他のノードを見たとき
「あ、このインターフェースを継承している → 同じメソッド使える！」
と 応用力が一気に上がる

### ⑧ 実際のDOM実装もクラス継承で作られている
ブラウザ内部のDOM実装も：
- クラス
- クラス継承
を使って、この仕様どおりの構造で作られている

## 超要約
**DOMノードは、イベント → ノード → 要素 → HTML要素 → 個別要素、という継承構造で機能を受け継いでいる**

# 「インターフェース ≒ prototypeチェーン」
## ① 仕様書の「インターフェース継承」は、JSでは prototype chain で実現されている

```js
// 仕様書：
EventTarget
  ↑
Node
  ↑
Element
  ↑
HTMLElement
  ↑
HTMLInputElement
```

```js
// JavaScript 実装
input
  → [[Prototype]]: HTMLInputElement.prototype
        → HTMLElement.prototype
              → Element.prototype
                    → Node.prototype
                          → EventTarget.prototype
                                → Object.prototype
```
👉 これが prototype chain

```js
input = {}

↓ prototype

HTMLInputElement.prototype = { input固有の機能 }

↓ prototype

HTMLElement.prototype = { HTML共通の機能 }

↓ prototype

Element.prototype = { 要素共通の機能 }

↓ prototype

Node.prototype = { ノード共通の機能 }

↓ prototype

EventTarget.prototype = { イベント処理 }

↓ prototype

Object.prototype = { JS基本機能 }
```

## JSのすべてのオブジェクトは最終的に Object.prototype を継承する

| 種類       | prototype chain                  |
| -------- | -------------------------------- |
| DOM      | DOM専用ルート → Object.prototype      |
| Array    | Array専用ルート → Object.prototype    |
| String   | String専用ルート → Object.prototype   |
| Function | Function専用ルート → Object.prototype |


### DOM
```js
input
  ↓
HTMLInputElement
  ↓
HTMLElement
  ↓
Element
  ↓
Node
  ↓
EventTarget
  ↓
Object
```

### Array
```js

[]
 ↓
Array
 ↓
Object
```

### String
```js
""
 ↓
String
 ↓
Object
```