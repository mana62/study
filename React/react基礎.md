# React基礎

---
## React 基礎構文

**index.html (基本このまま)**
```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React</title>
  </head>
  <body>
    <div id="root"></div> <!-- index.htmlにはid = rootのみを置き、他はjsで生産する -->
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
```

**main.jsx (基本このまま)**
```jsx
import { StrictMode } from 'react'; // strictモード(あってもなくってもOK)
import { createRoot } from 'react-dom/client'; // 18以降から必須に
import { App } from './App'; // App.jsxをインポートするための記述

const rootElement = document.getElementById('root'); // index.htmlのrootを必ず取得
const root = createRoot(rootElement);

root.render(
  // strictモード適用 / インポータントしたファイルを取得
  <StrictMode>
    <App />
  </StrictMode>
);
```

**App.jsx (コンポーネントを書いていくファイル)**
```jsx
export const App = () => { // exportを書くと別のファイルで扱える。関数名は必ず大文字

const name = "太郎"; // 変数を定義
const num = 3;

return (
    // 複数行ある場合は、<div>タグや空のタグ<>を置く必要がある
    <>
      <h1>こんにちは {name} さん</h1>
      <p>{num * 2} 回クリックしました</p>
      <button onClick={() => alert("押された！")}>押す</button>
    </>
    // jsのイベントを使う場合は、すべてキャメルケース
    // コンポーネントの中でjsを使う場合は波括弧{}を使って識別する
  );
};
```

---

## 書き方

### Vanilla JS の書き方
```js
<div id="app"></div>
<script>
  const app = document.getElementById('app');
  const h1 = document.createElement('h1');
  h1.textContent = "こんにちは";
  app.appendChild(h1);
</script>
```

### React (JSX) の書き方
```jsx
export const App = () => {
  const name = "太郎";
  const num = 3;

  return (
    <div>
      <h1>こんにちは {name} さん</h1>
      <p>{num * 2} 回クリックしました</p>
      <button onClick={() => alert("押された！")}>押す</button>
    </div>
  );
};
```
- **「JSX 内で {} を使って JS を埋め込む」のが React の基本**
- {} の中には 任意の JavaScript 式 が書ける
- {name} → 変数展開
- {num * 2} → 計算結果を表示
- {items.map(...)} → 配列ループ
- イベントハンドラ（onClick など）は関数を渡す

---

## Reactで使われるイベントハンドラ一覧
- Reactの場合は**キャメルケース**で書く
- jsを書く場合は{}で囲みjsと識別させる


| 種類             | イベントハンドラ | 意味・用途                                 | 第1引数 (e)                   | 第2引数 | 使用例                                                                                                    |
| ---------------- | ---------------- | ------------------------------------------ | ----------------------------- | ------- | --------------------------------------------------------------------------------------------------------- |
| **マウス系**     | onClick          | クリック時に発火                           | MouseEvent (SyntheticEvent)   | なし    | `<button onClick={(e) => console.log(e)}>押す</button>`                                                   |
|                  | onDoubleClick    | ダブルクリック時に発火                     | MouseEvent                    | なし    | `<div onDoubleClick={(e) => console.log(e.type)}>対象</div>`                                              |
|                  | onMouseEnter     | マウスが要素に入った時に発火               | MouseEvent                    | なし    | `<div onMouseEnter={(e) => console.log("hover in")}>hover</div>`                                          |
|                  | onMouseLeave     | マウスが要素から離れた時に発火             | MouseEvent                    | なし    | `<div onMouseLeave={(e) => console.log("hover out")}>hover</div>`                                         |
|                  | onMouseDown      | マウスボタンを押した瞬間に発火             | MouseEvent                    | なし    | `<button onMouseDown={(e) => console.log(e.button)}>押す</button>`                                        |
|                  | onMouseUp        | マウスボタンを離した時に発火               | MouseEvent                    | なし    | `<button onMouseUp={(e) => console.log(e.button)}>押す</button>`                                          |
|                  | onMouseMove      | マウスが動いた時に発火                     | MouseEvent                    | なし    | `<div onMouseMove={(e) => console.log(e.clientX, e.clientY)}>動作</div>`                                  |
| **キーボード系** | onKeyDown        | キーを押した瞬間に発火                     | KeyboardEvent                 | なし    | `<input onKeyDown={(e) => console.log(e.key)} />`                                                         |
|                  | onKeyUp          | キーを離した時に発火                       | KeyboardEvent                 | なし    | `<input onKeyUp={(e) => console.log(e.key)} />`                                                           |
| **フォーム系**   | onChange         | 入力値が変更された時に発火                 | ChangeEvent<HTMLInputElement> | なし    | `<input onChange={(e) => console.log(e.target.value)} />`                                                 |
|                  | onInput          | 入力がリアルタイムで変化した時に発火       | FormEvent<HTMLInputElement>   | なし    | `<input onInput={(e) => console.log(e.target.value)} />`                                                  |
|                  | onSubmit         | フォーム送信時に発火                       | FormEvent<HTMLFormElement>    | なし    | `<form onSubmit={(e) => { e.preventDefault(); console.log("送信"); }}>...</form>`                         |
|                  | onFocus          | フォーカスを得た時に発火                   | FocusEvent<HTMLInputElement>  | なし    | `<input onFocus={(e) => console.log("focus")} />`                                                         |
|                  | onBlur           | フォーカスを失った時に発火                 | FocusEvent<HTMLInputElement>  | なし    | `<input onBlur={(e) => console.log("blur")} />`                                                           |
| **フォーム以外** | onSelect         | テキスト選択時に発火                       | SyntheticEvent                | なし    | `<input onSelect={(e) => console.log("選択")} />`                                                         |
|                  | onInvalid        | バリデーションエラー時に発火               | SyntheticEvent                | なし    | `<input required onInvalid={(e) => console.log("エラー")} />`                                             |
| **その他**       | onScroll         | スクロール時に発火                         | UIEvent                       | なし    | `<div onScroll={(e) => console.log("スクロール")} style={{overflow:"scroll"}}>...</div>`                  |
|                  | onWheel          | マウスホイール操作時に発火                 | WheelEvent                    | なし    | `<div onWheel={(e) => console.log(e.deltaY)}>...</div>`                                                   |
|                  | onContextMenu    | 右クリック（コンテキストメニュー）時に発火 | MouseEvent                    | なし    | `<div onContextMenu={(e) => { e.preventDefault(); console.log("右クリック禁止"); }}>右クリック禁止</div>` |


```jsx
// 例
<input onChange={(e) => console.log(e.target.value)} />  // 入力値をログに出す
<button onClick={(e) => alert("クリックされた！")}>押す</button> // クリックでアラート
<div onMouseEnter={(e) => console.log("マウスが入った")}>hover</div> // hover検知

```

---

## Reactでのstyleの当て方
➡️ CSSは３通りのやり方がある
- またCSSは`className`として`{}`で囲む必要がある
- CSSもキャメルケースで書く

### ①インラインスタイル
```jsx
const contentStyle = {
  color: "blue",
  fontSize: "18px",
};

<h1 style={contentStyle}>こんにちは</h1>
// CSSも埋め込む
```

- JS のオブジェクトを style に渡す
- 必ず キャメルケース（font-size → fontSize）

### ②外部 CSS ファイルを読み込む

**App.css**
```css
.title {
  color: red;
  font-size: 24px;
}
```

**App.jsx**
```jsx
import "./App.css";

<h1 className="title">こんにちは</h1>
```


- 普通の CSS と同じ感覚で書ける
- 規模が大きくなったらこれが基本スタイル
- ⚠️CSS がグローバルになるので、他のコンポーネントに影響しやすい

### ③CSS Modules（スコープ付き CSS、React で推奨されやすい）

**App.module.css**
```css
.title {
  color: green;
  font-size: 20px;
}
```

**App.jsx**
```jsx
import styles from "./App.module.css";

<h1 className={styles.title}>こんにちは</h1>
```

- ファイル名を xxx.module.css にする
- そのコンポーネント専用のクラス名として使える（他と衝突しない）
- 最近の React だとこの方法が一番多い

### まとめ
- 小規模 → インラインスタイル
- 中規模以上 → CSS ファイル（className）
- コンポーネント単位で管理したい → CSS Modules

---

## Component
➡️ **コンポーネントは「自分だけの HTML＋JSX を持った関数」**

- UIの部品（再利用可能なパーツ）
- HTML・JS・CSSをひとまとめにして、状態や振る舞いを持たせたもの
- 関数コンポーネントかクラスコンポーネントで作る

```jsx
// 関数コンポーネント
export const Button = () => {
  return <button>押す</button>;
};
```

```jsx
// App.jsx で使う
import { Button } from "./Button";

export const App = () => {
  return (
    <div>
      <Button />
    </div>
  );
};
```

---

## Props
➡️ **親から子に渡すデータ**

- コンポーネントの「外部からの入力値」変更不可（読み取り専用）
- Propsで渡せるものの例
  - 文字列 / 数値 / boolean
  - 配列 / オブジェクト
  - 関数（子から親に通知したい時に便利）

```jsx
// 親コンポーネント
export const App = () => {
  return <Greeting name="太郎" age={20} />;
};
```

```jsx
// 子コンポーネント
export const Greeting = (props) => {
  return <p>{props.name} さんは {props.age} 歳です</p>;
};
```
- 上記だと props.name は "太郎"、props.age は 20
- 親が「どんな値を渡すか」で子の挙動が変わる

---

## Props children
➡️ コンポーネントのタグで囲んだ中身を受け取る
- 親から子に「タグの中身」を渡すときに使う
- `props.children` でアクセス可能
- 単純な文字列だけでなく、他のコンポーネントや JSX も渡せる

```jsx
// 親コンポーネント
export const App = () => {
  return (
    <Card>
      <h1>こんにちは</h1>
      <p>今日はReactを勉強しています</p>
    </Card>
  );
};
```

```jsx
// 子コンポーネント
export const Card = (props) => {
  return <div className="card">{props.children}</div>;
};
```

- 上記の場合、Card コンポーネントの props.children は <h1>こんにちは</h1><p>今日はReactを勉強しています</p> に相当
- 親が何を <Card>...</Card> の中に書くかで、子コンポーネントの表示内容が変わる

---

## State
➡️ **コンポーネント内で変化する値**

- 「ユーザーの操作や時間経過で変わる値」を管理する
- useState フックで作る
- 「状態が変わるかもしれない値」を管理するために定義する
  - ユーザーが入力した値
  - ボタンを押した回数
  - モーダルの開閉状態

```jsx
import { useState } from "react";

export const Counter = () => {
  const [count, setCount] = useState(0); // useStateは分割代入で受け取る。()の中の0は初期値
  return (
    <div>
      <p>{count} 回クリックしました</p>
      <button onClick={() => setCount(count + 1)}>カウントアップ</button>
    </div>
  );
};
```

- ボタンを押すと count が変化 → UIが自動で再描画される
- Props は親から渡される値、State はコンポーネント内で変化する値

---

## まとめ

| 用語           | 説明                           | 誰が管理？     | 変更できる？               |
| -------------- | ------------------------------ | ------------------ | -------------------------- |
| コンポーネント | UIの部品（関数）               | 自分自身           | 関数の中身で制御可         |
| Props          | 外から渡される値（パラメータ） | 親コンポーネント   | 子コンポーネントは変更不可 |
| State          | 内部で変化する値               | コンポーネント自身 | 変更可能（useState）       |

---

## prevState
➡️ Reactの状態更新は非同期で行われることが多いため、うまく更新されないことがある。
そのため、「前の状態を確実に使いたいとき」は、**prevState**を使う

- Reactで setCount などの「更新関数」を使うとき、前の値をもとに新しい値を計算したいことがある。
- そのときに「prevState（前のstate）」を引数として使う。

```jsx
setCount((prevState) => prevState + 1);
```
-  prevState は「前の count の値」を表す

---

## レンダリング
| トリガー                                   | 説明                                        |
| ------------------------------------------ | ------------------------------------------- |
| `state` が変わったとき                     | 自分の内部状態が変わると再描画される        |
| `props` が変わったとき                     | 親から渡された値が変わると再描画される      |
| 親コンポーネントが再レンダリングされたとき | 子も再描画される（propsが変わってなくても） |

- 再レンダリングは「UIの再描画」なので、パフォーマンスに影響することがあるので本番環境では良くない
- 不要な再レンダリングを防ぐには、`React.memo` や `useMemo`, `useCallback` などの最適化手法が有効
- `StrictMode` を使っていると、開発環境ではレンダリングが2回発生することがある（副作用検出のため）

---

## useEffect
➡️ **「レンダリングが終わった後に実行される処理」を書くためのフック（関数）**

- コンポーネントが描画されるたびに関数が実行される のですが、時々「レンダリングの後に何かしたい」場合がある。そのときに useEffect を使うことができる。

#### 基本の書き方
```jsx
// 雛形
useEffect(() => {
  // 副作用の処理
}, [依存する値]); // 第二引数は必ず配列で
```

```jsx
import { useState, useEffect } from 'react';

const App = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log('レンダリング後に実行される');
  });

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>カウントアップ</button>
    </div>
  );
};
```

- useEffect の中の関数は レンダリングが終わった後に呼ばれる
- ボタンを押すたびに count が変わるので、レンダリング後にコンソールにログが出る


#### 第二引数に配列を渡す
```jsx
useEffect(() => {
  console.log('countが変わった時だけ実行される');
}, [count]);
```
- 第二引数に [count] を渡すと、countが変わった時だけ処理を実行
- 空配列 [] にすると、コンポーネントが最初に描画された時だけ 実行される

```jsx
useEffect(() => {
  console.log('最初の1回だけ実行');
}, []);
```

#### 使いどころの例
| タイミング                 | 使い方                                     | 目的                                 | 補足説明                          |
| -------------------------- | ------------------------------------------ | ------------------------------------ | --------------------------------- |
| 初回表示時（マウント時）   | `useEffect(() => {}, [])`                  | 初期化処理、API取得、イベント登録    | `[]` が空 → 初回のみ実行される    |
| 値が変わったとき           | `useEffect(() => {}, [value])`             | 監視、同期処理、ローカル保存         | `value` が変わるたびに実行される  |
| コンポーネントが消えるとき | `useEffect(() => { return () => {} }, [])` | 後片付け、イベント解除、タイマー停止 | `return` の中がクリーンアップ処理 |


#### イメージ
- Appコンポーネントレンダリング → 画面に描画 → useEffectの中の処理が実行
- **「レンダリング中にstateを直接更新すると無限ループになる問題」を避けるため、useEffect を使って副作用処理を行う、という感じ**


---

## Named Export と Default Export

#### Named Export（名前付きエクスポート）
➡️ ファイルから「複数のものを名前で外に出す」感じ。基本はこっちが主流

```jsx
// App.jsx
export const App = () => <h1>こんにちは</h1>;
export const Button = () => <button>押す</button>;
```

- App と Button を 名前付きで出す
- インポートするときは {} で名前を書く

```jsx
import { App, Button } from './App'; // {} が必要
<App />
<Button />
```


- 1つのファイルから複数のものを外に出せる
- インポートするときは名前を間違えちゃダメ

#### Default Export（デフォルトエクスポート）
➡️ ファイルから「これ1つだけをメインとして外に出す」感じ

```jsx
// App.jsx
const App = () => <h1>こんにちは</h1>;
export default App; // これがメイン
```

- ファイルには1つだけ
- インポートするときは {} は不要、名前も自由

```jsx
import MyApp from './App'; // 名前は自由
<MyApp />
```

- 「このファイルはこれがメインだよ」と示す
- 名前は自分の好きなものに変えてOK

---

## React Rooter

### インストール

**ターミナル**
```bash
npm install react-router-dom
# または
yarn add react-router-dom
```

**main.jsx**
```jsx
// main.jsx
// エントリーポイント
import React from 'react';
import { createRoot } from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom'; // インストールする
import App from './App';

createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

### Routes / Route / Link / Outlet
- Routes と Route で URL パスとコンポーネントを宣言的にマッピング
- Link（や NavLink）で画面遷移（アンカーと違いページ全体を再読み込み

**App.jsx**
```jsx
// App.jsx
// 親コンポーネント
import { Routes, Route, Link } from 'react-router-dom'; // これが必要
import Home from './pages/Home';
import About from './pages/About';
import Product from './pages/Product';

export default function App(){
  return (
    <>
      <nav>
        <Link to="/">Home</Link> ｜ <Link to="/about">About</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/products/:id" element={<Product />} />
      </Routes>
    </>
  );
}
```

- `import { Routes, Route, Link } from 'react-router-dom';` をインポートすることが必要
- `Linkタグ`の場合は、`to`にパスを指定
- `Routeタグ`の場合は、`path`にパスを指定


### 動的ルート（params） と フック（useParams, useNavigate, useLocation）

- URL パラメータ取得：useParams()

```jsx
import { useParams } from 'react-router-dom';
export default function Product(){
  const { id } = useParams(); // /products/:id の :id を取得
  return <div>product id: {id}</div>;
}
```


- プログラム的に遷移：useNavigate()

```jsx
import { useNavigate } from 'react-router-dom';
const navigate = useNavigate();
// 例：ボタンでページ遷移
<button onClick={() => navigate('/login')}>ログインへ</button>
// 戻る
navigate(-1); // 履歴で1つ戻る
```

- 現在の URL 情報：useLocation()（クエリや直前の state を見るときに便利）
- クエリ文字列を扱う：useSearchParams()（簡易ラッパー）

### ネスト（入れ子）ルートと Layout / Outlet

- ネストすると「共通レイアウト + 各子ルートの差分表示」ができる
- 子の出力位置には `<Outlet />` を置く

**Layout.jsx**
```jsx
// Layout.jsx
import { Outlet, Link } from 'react-router-dom';
export default function Layout(){
  return (
    <div>
      <header>ヘッダ</header>
      <nav>
        <Link to="/">Home</Link>｜<Link to="/about">About</Link>
      </nav>
      <main>
        <Outlet /> {/* ここに子ルートが描画される */}
      </main>
      <footer>フッタ</footer>
    </div>
  );
}

// ルート定義
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />            {/* path="/" と同じ */}
    <Route path="about" element={<About />} />   {/* /about */}
    <Route path="products/:id" element={<Product/>} />
  </Route>
</Routes>
```