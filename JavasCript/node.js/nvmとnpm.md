# npm と nvm
- nvm: Node.js **本体の「バージョン管理ツール」**
- npm: Node.js に**付属している「パッケージ管理ツール」**

つまり：
- nvm : Node.js を**管理するもの**
- npm : Node.js の中で使う**ライブラリを管理するもの**

## nvm（Node Version Manager）
### 何をするもの？
- Node.js のバージョンをインストールしたり切り替えたりする
- プロジェクトごとに Node.js のバージョンを変えられる

### できること
- nvm install 18 → Node.js 18 をインストール
- nvm use 18 → Node.js 18 を使う
- nvm ls → インストール済み Node.js の一覧

### 役割のイメージ
- Node.js 本体を管理する「バージョン切り替えスイッチ」

## npm（Node Package Manager）
### 何をするもの？
- Node.js のプロジェクトで使うライブラリ（パッケージ）を管理する
- package.json に書かれた依存関係をインストールする

### できること
- npm install → ライブラリをインストール
- npm run build → スクリプトを実行（Vite のビルドなど）
- npm update → パッケージ更新

### 役割のイメージ
- Node.js の中で使う「部品（ライブラリ）を管理する道具」

## 2つの関係性
- **nvm が Node.js 本体を準備する**
- **npm は Node.js の中で動くツール**

だから順番としては：
1. nvm で Node.js を入れる
2. npm が使えるようになる
3. npm で Vite や依存パッケージを入れる
4. npm run build でビルドする