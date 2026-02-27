# Web APIs
＝Web API（Web APIs）は、**ブラウザが提供している機能を JavaScript から使えるようにした仕組み**のこと

＝**「JavaScript の機能」ではなく、ブラウザが持っている機能を JavaScript が呼び出せるようにしたもの**

＝web APIsのプロパティは、全てアクセサプロパティ（getter/setter）のみでできいる → つまり関数

## Web API とは
＝ブラウザは、ただ HTML を表示するだけじゃなくて、**大量の機能（API）を内部に持っている**

例えば：
- 画面に要素を追加する
- ボタンのクリックを検知する
- ローカルストレージにデータを保存する
- ネットワーク通信をする
- 位置情報を取得する
- カメラやマイクを使う
- アニメーションを動かす
- タイマーを動かす

＝これらは全部 ブラウザが提供している機能で、**JavaScript はそれを「借りて使っている」**だけ
＝この **「借りるための窓口」が Web API**

## よく使う Web API
| Web API              | 何ができる？                               |
|----------------------|---------------------------------------------|
| DOM API              | HTML 要素の取得・変更（document.getElementById など） |
| Event API            | クリックや入力などのイベント処理            |
| Fetch API            | ネットワーク通信（HTTP リクエスト）         |
| LocalStorage API     | ブラウザにデータ保存                        |
| Geolocation API      | 位置情報の取得                              |
| Canvas API           | 図形や画像の描画                            |
| Web Audio API        | 音声の生成・加工                            |
| Web Animations API   | アニメーション制御                          |
| History API          | ブラウザの履歴操作                          |

＝`beforeunload、addEventListener、querySelector`これらは全部 **Web API**

## JavaScript と Web API の関係
＝JavaScript は本来、計算しかできない言語

でもブラウザが Web API を提供してくれるおかげで、
- 画面操作
- イベント処理
- 通信
- 保存
- アニメーション
など、アプリのような動きができるようになっている

つまり、
**JavaScript が便利なのは、Web API があるから** と言っても過言じゃない

## 例：Web API を使っているコード
```js
document.getElementById("btn").addEventListener("click", () => {
  alert("クリックされた！");
});
```

```js
document.getElementById → DOM API
addEventListener → Event API
alert → Window API
```
全部 Web API

## まとめ
- Web API = ブラウザが提供する機能を JavaScript から使うための仕組み
- JavaScript 単体ではできないことを、ブラウザが API として提供している
- DOM 操作、イベント、通信、保存、アニメーションなどは全部 Web API