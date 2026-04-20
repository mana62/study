# TypeScript
＝ TypeScript は JavaScript の上位互換（superset）であり、静的型システムを持ち、最終的に JavaScript にコンパイルされる言語

## 上位集合（上位互換）とは
TypeScript は JavaScript の**すべての文法をそのまま使える**

つまり：
- JS は TS の一部
- TS は JS を完全に含んでいる（＋型などの機能が追加されている）
👉 だから superset（上位集合） と呼ばれる

## 静的型システムとは
👉 コードを書く段階で型の誤りを検出できる仕組み

```js
// 例
let age: number = "20"; // ← ここでエラー
```

## コンパイルとは
TypeScript のコードは tsc（TypeScriptCompiler） によってJavaScript に変換される
👉 この変換を **コンパイル** と呼ぶ（厳密にはトランスパイルでもあるが、コンパイルでOK）

## インストール方法
```bash
npm install -g typescript
```
**npm** : Node.js に付属している パッケージ管理ツール
（＝Node.js がインストールされていれば npm が使える）
- Node.js が入っている環境で使うツール

**-g（オプション）** : グローバルインストール（どこでも使えるようにする）
- プロジェクトごとではなく、PC 全体にインストール
- tsc コマンドがどこでも使えるようになる

**tsc（TypeScript Compiler）** : TS → JS にコンパイルするコマンド