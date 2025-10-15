# Alpine.js
➡️ JavaScriptフレームワーク

---

## 基本の書き方
#### ① HTML に CDN を貼る
```bash
<script src="//unpkg.com/alpinejs" defer></script>
```
- これだけで Alpine.js が使える
- ビルド環境不要で練習できる

####  x-data で変数を作る
```bash
<div x-data="{ count: 0 }">
  <p x-text="count"></p>
  <button @click="count++">増える</button>
</div>
```

- x-data="{ count: 0 }"：コンポーネントの状態（変数）
- x-text="count"：HTMLに変数を表示
- @click="count++"：クリックで変数を増やす
- React でいう useState + JSX の書き方に近い


####  x-show で表示/非表示 (トグル)
→ x-showは状態による切り替えの時だけ使う
```bash
<div x-data="{ open: false }">
  <button @click="open = !open">開閉</button>
  <p x-show="open">ここが表示されます</p>
</div>
```

- x-show は条件に応じて表示/非表示
- React でいう {open && <p>...</p>} と同じ感覚

####  x-bind 属性を動的に変える
→ 属性を変える（class, style, disabled, href など）
```bash
<div x-data="{ color: 'red' }">
  <p :style="'color:' + color">色が変わるテキスト</p>
  <button @click="color = 'blue'">青にする</button>
</div>
```

#### x-html タグがそのまま文字として表示される
→ `x-text`と似ているが、タグも表示するかしないかの違い
```bash
<div x-data="{ msg: '<b>重要</b>' }">
  <span x-text="msg"></span>
</div>

# 結果：<b>重要</b>
```

####  x-model でリアルタイム表示
```bash
<div x-data="{ message: '' }">
    <input type="text" x-model="message">

    <span x-text="message"></span>
</div>
```

####  x-for
```bash
<div x-data="{ items: ['りんご', 'バナナ', 'オレンジ'] }">
  <ul>
    <template x-for="(item, index) in items" :key="index">
      <li x-text="item"></li>
    </template>
  </ul>
</div>
```


- :style は x-bind:style の短縮
- 属性を動的に変えられる

---

## Alpine.js ディレクティブ一覧

| ディレクティブ | 説明 | 第一引数 | 第二引数 / オプション | 例 |
|----------------|------|----------|----------------------|----|
| x-data | コンポーネントの状態を定義(関数を定義する時も)親要素に書く必要がある | {変数名: 初期値} | なし | `<div x-data="{ count: 0 }">` |
| x-text | HTML要素内に変数の値を表示 | JS式（文字列・変数） | なし | `<p x-text="count"></p>` |
| x-html | HTMLを直接レンダリング | JS式 | なし | `<p x-html="htmlContent"></p>` |
| x-show | 条件で表示/非表示 | 真偽値を返す JS 式 | なし | `<div x-show="open">表示</div>` |
| x-if | 条件で DOM を作る/削除 | 真偽値を返す JS 式 | なし | `<template x-if="open"><p>表示</p></template>` |
| x-bind | 属性を動的に変更 | 属性名 | JS式 | `<img x-bind:src="imgUrl">` / `<img :src="imgUrl">` |
| x-model | 双方向バインディング(リアルタイムで何かしたい時) | 変数名 | 修飾子（.lazy, .number, .trim） | `<input x-model="name" type="text">` |
| x-on | イベントハンドラ(短縮して@だけでもOK) | イベント名 | 修飾子（.prevent, .stop, .once, .debounce） | `<button x-on:click="count++">` / `<button @click.prevent="submit()">` |
| x-init | 初期化処理 | JS式 | なし | `<div x-data="{count:0}" x-init="count = 5">` |
| x-cloak | 初期非表示 | なし | なし | `<div x-cloak>非表示</div>` |
| x-ref | 要素参照 | 文字列 | なし | `<input x-ref="input1">` → `this.$refs.input1` |
| x-transition | 表示/非表示にアニメーション | なし | オプション（enter, leave, duration, delay） | `<div x-show="open" x-transition:enter="ease-out duration-500">` |
| x-for | 配列やオブジェクトのループ | `item in items` | `:key="item.id"` | `<template x-for="item in items" :key="item.id"><p x-text="item.name"></p></template>` |
| x-effect | 依存変数が変化したときに処理 | JS式 | なし | `<div x-effect="console.log(count)"></div>` |

---

## Alpine.js(修飾子)

| 修飾子 | 意味 | 使い方（例） | どんな時に使うか |
|--------|------|--------------|----------------|
| `.prevent` | イベントのデフォルト動作を防ぐ | `<form @submit.prevent="submitForm()">` | フォーム送信時にページリロードさせずに処理したいとき |
| `.stop` | イベントのバブリング（親への伝播）を止める | `<button @click.stop="doSomething()">` | 親要素にあるclickイベントを発火させたくない場合 |
| `.once` | イベントを1回だけ実行 | `<button @click.once="alert('Hello')">Click</button>` | 一度だけ処理を実行したいボタン |
| `.self` | イベントが自分自身から発生した場合のみ処理 | `<div @click.self="closeModal()">` | モーダル背景をクリックしたときだけ閉じたいとき |
| `.window` | windowに対してイベントをバインド | `<div @keydown.window="onKeyDown()">` | キーボードショートカットなど全体に適用したいとき |
| `.document` | documentに対してイベントをバインド | `<div @click.document="closeDropdown()">` | ドキュメント内のクリックでドロップダウンを閉じるとき |
| `.debounce` | イベントの発火を遅延させる | `<input @input.debounce.500ms="search()">` | 入力途中で何度も処理を呼ばず、最後の入力だけ反応させたい場合 |
| `.throttle` | 一定時間ごとにしかイベントを発火させない | `<button @click.throttle.1000ms="submit()">` | クリック連打による多重処理を防ぎたいとき |
| `.blur` | イベントをフォーカスが外れたタイミングで発火 | `<input @blur="validate()">` | 入力チェックやバリデーションをフォーカスアウト時に行う |
| `.focus` | フォーカス時に発火 | `<input @focus="highlight()">` | 入力欄を選択したときにスタイル変更やヒント表示 |
| `.camel` | イベント名をキャメルケースで書けるようにする | `<div @my-custom-event.camel="handle()">` | カスタムイベント名がケバブケースの場合の互換性確保 |
| `.passive` | イベントリスナーをパッシブにする（スクロール等で有効） | `<div @scroll.passive="onScroll()">` | パフォーマンス重視のスクロールやタッチイベント時 |
| `.debounce.<ms>` | msミリ秒だけ遅延して発火 | `<input @input.debounce.300ms="search()">` | 入力フィールドで連打防止やAPI呼び出し制御 |

---

**補足** 
- 修飾子は **イベント名の後にドットで追加** する
- 複数の修飾子を組み合わせることも可能
  例：`@click.prevent.stop="doSomething()"`
- Alpine.jsの「動き」を細かく制御するツール

---

## バインド
→ : を使うと 属性の値を JS の式や変数で決めることができる

```bash
<div x-data="{ isRed: true }">
  <p :class="isRed ? 'text-red-500' : 'text-blue-500'">
    色が変わる文章
  </p>
</div>
```

## Alpine.js バインディング一覧

| バインディング | 意味 | 使い方（例） | どんな時に使うか |
|----------------|------|--------------|----------------|
| `x-text` | 要素内のテキストを変数で表示 | `<p x-text="message"></p>` | 文字列を変数と同期させたいとき |
| `x-html` | 要素内にHTMLを表示 | `<p x-html="htmlContent"></p>` | HTMLタグを含む文字列を表示したいとき |
| `x-show` | 条件によって表示・非表示 | `<p x-show="isVisible">表示</p>` | 条件付きで要素を出したいとき |
| `x-if` | 条件によってDOMを生成・削除 | `<template x-if="isVisible"><p>表示</p></template>` | 完全に要素をDOMから消したいとき |
| `x-bind` | 属性全般を動的に設定 | `<a x-bind:href="url">リンク</a>` | hrefやsrcなど任意の属性に変数を反映したい場合 |
| `:` (ショートカット) | 属性にJSの式や変数をバインド | `<p :class="isRed ? 'text-red-500' : 'text-blue-500'">色</p>` | class, src, href などを動的に変えたいとき |
| `x-model` | 入力欄と変数を双方向バインド | `<input x-model="name">` | 入力した値をリアルタイムで変数に反映したいとき |
| `x-ref` | 要素を参照する | `<div x-ref="box"></div>` | JSで特定の要素を操作したいとき |
| `x-init` | 初期化処理を実行 | `<div x-init="count=0"></div>` | 初期値設定やロード時の処理に使う |
| `x-cloak` | 初期表示を非表示にする | `<div x-cloak x-show="show">表示</div>` | Alpineが読み込まれるまで一瞬表示されたくない要素 |
