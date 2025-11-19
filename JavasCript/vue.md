# Vue.js
→ JavaScriptのフレームワーク

---

## Vue.jsのメリット
- 学習コストが低い
- ライブラリやツールが豊富
- 速くて軽い(Vue.jsを使うと、ウェブページがとても速くて、サクサク動く)
- 何かが変わるとすぐにわかる( Vue.jsを使うと、何かが変わったら、自動的にその変化が表示される)
- いろんなパーツを組み立てられる
- HTML要素を部品化
- 環境構築しなくてもできる

---

## Vue.js の仕組み（リアクティブ）
- Vue.js は、データ (data) と 画面 (template) をつなげてくれる
- データが変わると自動的に画面が更新される
- この仕組みを「リアクティブシステム」と呼ぶ

---

## Vue.jsの読み込み

```html
<html>
<head>
  <meta charset="utf-8" />
  <!---CDN(Vue.jsが用意してくれるサーバーから読み込み)-->
  <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
  <!---ローカルにインストールしたい場合は、npm install vue@next -->
  </head>
  <body>
  </body>
</html>
```

---

## ローカル開発サーバーを立ち上げる場合
**Node.js＋npmを使って「Vite」で開発環境を作るのが一般的**

```bash
npm create vue@latest
cd プロジェクト名
npm install
npm run dev
```

---

## Vue.js の基本の書き方: 例
```bash
<div id="app">
  <p>{{ message }}</p>
  <button @click="reverseMessage">反転</button>
</div>

<script>
const app = Vue.createApp({
  data() {
    return {
      message: 'こんにちは Vue!'
    }
  },
  methods: {
    reverseMessage() {
      this.message = this.message.split('').reverse().join('')
    }
  }
})

app.mount('#app')
</script>
```

---

## Vue.js のよく使うディレクティブ

| ディレクティブ            | 説明                         | 使用例                                                                   |
| ------------------ | -------------------------- | --------------------------------------------------------------------- |
| `v-text`           | 要素内のテキストを動的に置き換える          | `<p v-text="message"></p>`                                            |
| `v-html`           | 要素内にHTMLを挿入（XSS注意）         | `<div v-html="rawHtml"></div>`                                        |
| `v-bind`（省略形：`:`）  | HTML属性を動的にバインドする           | `<img v-bind:src="imageUrl">` → `<img :src="imageUrl">`               |
| `v-model`          | フォーム要素とデータを双方向にバインド        | `<input v-model="name">`                                              |
| `v-if`             | 条件が真のときだけ要素を表示             | `<p v-if="isVisible">表示される</p>`                                       |
| `v-else`           | `v-if` が偽のときに表示            | `<p v-else>表示されない</p>`                                                |
| `v-else-if`        | 追加条件の分岐                    | `<p v-else-if="isLoading">読み込み中</p>`                                  |
| `v-show`           | 要素を表示・非表示（DOMは残る）          | `<p v-show="isVisible">表示/非表示</p>`                                    |
| `v-for`            | 繰り返し処理を行う                  | `<li v-for="item in items" :key="item.id">{{ item.name }}</li>`       |
| `v-on`（省略形：`@`）    | イベントを監視・実行                 | `<button v-on:click="doSomething">` → `<button @click="doSomething">` |
| `v-slot`（または `#`）  | スロットにデータを渡す（子コンポーネント用）     | `<template v-slot:header>ヘッダー</template>`                             |
| `v-pre`            | Vueのコンパイルをスキップ             | `<span v-pre>{{ この中は評価されない }}</span>`                                 |
| `v-once`           | 一度だけ描画し、その後はリアクティブ更新しない    | `<p v-once>{{ message }}</p>`                                         |
| `v-cloak`          | Vueの初期化前に一時的に非表示にするためのマーカー | `<div v-cloak>読み込み中...</div>`                                         |
| `v-memo`（Vue 3.3〜） | 特定条件下で再レンダリングをスキップ         | `<div v-memo="[count]">{{ count }}</div>`                             |
| `v-is`             | 動的にコンポーネントを切り替える           | `<component v-is="currentComponent"></component>`                     |
| `v-bind:class` / `v-bind:style` | クラス名やインラインスタイルを動的に切り替え | `<div :class="{ active: isActive }"></div>` |
| `v-model.lazy` / `.trim` / `.number` | `v-model`修飾子 | `<input v-model.trim="text">` |


**補足**
- v-bind と v-on はとてもよく使うので、省略形（: と @）が一般的
- v-if / v-show の違い
  - v-if → DOM自体を追加・削除
  - v-show → CSSでdisplay: noneするだけ
- v-cloak は、Vueが起動する前に {{ message }} などの未展開部分を隠すために使う（CSSで [v-cloak]{display:none} を指定）

---

## Alpine.js と Vue.js の違い

- Alpine.js は x- で始まる
- Vue.js は v- で始まる

<br>

- Alpine → 軽量なHTML属性操作
- Vue → 高機能なリアクティブテンプレート制御

---

## Vue と Alpine の書き方比較

| 機能       | Vue.js                      | Alpine.js                  |
| -------- | --------------------------- | -------------------------- |
| テキストバインド | `{{ message }}`             | `x-text="message"`         |
| 属性バインド   | `:src="url"`                | `x-bind:src="url"`         |
| 条件分岐     | `v-if="ok"` / `v-show="ok"` | `x-show="ok"`              |
| 繰り返し     | `v-for="item in items"`     | `x-for="item in items"`    |
| イベント     | `@click="doSomething"`      | `x-on:click="doSomething"` |
| 双方向バインド  | `v-model="value"`           | `x-model="value"`          |
| 初期データ    | `data() { return {...} }`   | `x-data="{ ... }"`         |


---

## Vue CLI は非推奨傾向
- Vue 3.5 以降では、CLI よりも Vite 推奨


---

## 参考
- [公式](https://ja.vuejs.org/guide/introduction)
- [Vue.js入門(Vue.jsの初心者は必ず読め‼︎)](https://qiita.com/Hashimoto-Noriaki/items/ea60d5932f13a9cd5707)
- [【Vue.js】今、ゼロから Vue を学び始めるならこうやるといいんじゃないか (2024) 【初学者向け】](https://zenn.dev/comm_vue_nuxt/articles/start-to-learn-vue-2024)
- [[初学者向け] Vueを始めてみたらめちゃくちゃ楽しかった(簡単家計簿 ハンズオン)](https://qiita.com/hayaharu3220/items/c06e49264f503cdec96a)
- [JavaScript初心者でもVue.jsをはじめられます！はじめてみましょう！](https://www.brainpad.co.jp/doors/contents/01_tech_2018-04-13-160000/)
- [Vue初心者が勉強し始めて詰まったところの話🤔](https://zenn.dev/comm_vue_nuxt/articles/2508e78ffb5e02)