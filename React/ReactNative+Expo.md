# React Native + Expo
→ スマホアプリ開発をもっと簡単にしてくれるコンビ

---

## React Native とは
- React Native = React のスマホ版
- JavaScript + React の知識で iOS と Android のネイティブアプリ を同時に開発できるフレームワーク

**特徴**
  - 1つのコードで両OS対応
  - ネイティブ UI を直接操作できる
  - React のコンポーネント思想がそのまま使える


---

## [Expo](https://expo.dev/) とは
- Expo = React Native 開発のサポートツール

**目的**
  - 面倒な開発環境構築（Xcode、Android Studio、ネイティブSDKの設定など）を自動化
  - スマホでの確認や OTA 更新を簡単にする

**構成**
1. Expo CLI：コマンドラインツール（開発・ビルド・デバッグ）
2. Expo Go アプリ：スマホでアプリをリアルタイム確認
3. Managed workflow / Bare workflow
   Managed：Expo にすべて任せる簡単モード
   Bare：ネイティブコードも直接触る柔軟モード

---

## React Native と Expo の関係
- React Native が フレームワーク本体
- Expo が 開発を簡単にする補助ツール

---

## Expoのメリット

| 項目     | 内容                                   |
| ------ | ------------------------------------ |
| セットアップ | `npx create-expo-app` でプロジェクト作成が一瞬   |
| 実機確認   | スマホに「Expo Go」アプリを入れれば、QRコードでリアルタイム確認 |
| OTA更新  | アプリ再ビルド不要で最新コードを配信可能（Over The Air）   |
| 初心者向け  | 環境構築が簡単で、React の学習に集中できる             |
| デバッグ   | コンソールやホットリロードで即確認可能                  |

---

## 開発の流れ
- ① Node.js をインストール
- ② Expo CLI をインストール
```bash
npm install -g expo-cli
```
- ③ プロジェクト作成

```bash
npx create-expo-app my-app
cd my-app
```
- ④ 開発サーバー起動
```bash
npx expo start
```
- ⑤ スマホで QRコードを読み取って、リアルタイムで動作確認
- ⑥ コード変更 → 即反映（Hot Reload / Fast Refresh）

---

## 参考
- [Expo ではじめる React Native アプリ開発](https://zenn.dev/cloud_ace/articles/b14fa9fc56ecb6)
- [スマホ向けアプリを気軽に作れる　「React Native EXPO」](https://tech.iimon.co.jp/entry/2025/07/08/144549)
- [2025年にReact NativeでExpoを使用すべきか？](https://scriptide.tech/ja/blog/should-you-use-expo-for-react-native)
- [React Native + EXPO 触ってみた](https://qiita.com/Akihiro0711/items/bf9e96c36cc808052094)