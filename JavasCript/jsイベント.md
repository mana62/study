# イベント

---

## 雛形
`window.addEventListener(イベントタイプ, 処理関数, バブリング設定);`

| 引数    | 内容                                                                |
| ------- | ------------------------------------------------------------------- |
| 第1引数 | イベントタイプ（例：`click`, `pageshow`）                           |
| 第2引数 | イベントが発生したときに実行される関数                 |
| 第3引数 | イベントバブリング（親要素に伝播させるか）※デフォルトは`false` |

---

## イベント一覧


### ページ読み込み・アンロード関連

| イベント名         | 発生タイミング                       | 説明                                               | 対象        | 使用例                                                                     |
| ------------------ | ------------------------------------ | -------------------------------------------------- | ----------- | -------------------------------------------------------------------------- |
| `pageshow`         | ページが表示されたとき               | 戻るボタンなどでキャッシュから復元されたときも実行 | window      | `window.addEventListener('pageshow', () => { /* sessionStorage復元 */ });` |
| `pagehide`         | ページが非表示になったとき           | タブ切り替えなどでページが隠れたとき               | window      | `window.addEventListener('pagehide', () => { /* 状態保存 */ });`           |
| `beforeunload`     | ページ遷移直前                       | ページを離れる前に処理したいとき                   | window      | `window.addEventListener('beforeunload', () => { /* 確認ダイアログ */ });` |
| `unload`           | ページがアンロードされるとき         | リソース解放やログ送信など                         | window/body | `window.addEventListener('unload', () => { /* クリーンアップ */ });`       |
| `load`             | ページの全リソースが読み込まれたとき | 画像や動画も含むページ全体が準備できたタイミング   | window/body | `window.addEventListener('load', () => { /* 初期化 */ });`                 |
| `DOMContentLoaded` | HTMLが読み込まれたとき               | 画像や動画を待たずにスクリプト実行可能             | document    | `document.addEventListener('DOMContentLoaded', () => { /* 初期化 */ });`   |

---

### ユーザー操作イベント

| イベント名 | イベントハンドラ | 発生タイミング                       | 対象タグ                        | 使用例                                                                |
| ---------- | ---------------- | ------------------------------------ | ------------------------------- | --------------------------------------------------------------------- |
| click      | onclick          | クリックしたとき                     | ほとんどのタグ                  | `<button onclick="alert('clicked')">Click</button>`                   |
| dblclick   | ondblclick       | ダブルクリックしたとき               | ほとんどのタグ                  | `element.ondblclick = () => console.log('dbl');`                      |
| change     | onchange         | 値が変更され、フォーカスを失ったとき | input, select, textarea         | `input.onchange = () => console.log('changed');`                      |
| input      | oninput          | 値が入力されたとき                   | input, textarea                 | `input.oninput = () => console.log(input.value);`                     |
| select     | onselect         | テキストが選択されたとき             | input, textarea                 | `input.onselect = () => console.log('selected');`                     |
| submit     | onsubmit         | フォーム送信時                       | form                            | `form.onsubmit = e => { e.preventDefault(); console.log('submit'); }` |
| reset      | onreset          | フォームがリセットされたとき         | form                            | `form.onreset = () => console.log('reset');`                          |
| focus      | onfocus          | 要素がフォーカスを得たとき           | input, select, textarea, button | `input.onfocus = () => console.log('focused');`                       |
| blur       | onblur           | フォーカスを失ったとき               | input, select, textarea, button | `input.onblur = () => console.log('blurred');`                        |

---

### マウスイベント

| イベント名 | イベントハンドラ | 発生タイミング               | 対象           | 使用例                                                         |
| ---------- | ---------------- | ---------------------------- | -------------- | -------------------------------------------------------------- |
| mousedown  | onmousedown      | マウスボタンを押したとき     | ほとんどのタグ | `element.onmousedown = () => console.log('down');`             |
| mouseup    | onmouseup        | マウスボタンを離したとき     | ほとんどのタグ | `element.onmouseup = () => console.log('up');`                 |
| mousemove  | onmousemove      | マウスが要素上を移動したとき | 任意の要素     | `element.onmousemove = e => console.log(e.clientX,e.clientY);` |
| mouseover  | onmouseover      | マウスが要素に乗ったとき     | 任意の要素     | `element.onmouseover = () => console.log('hover');`            |
| mouseout   | onmouseout       | マウスが要素から離れたとき   | 任意の要素     | `element.onmouseout = () => console.log('leave');`             |
| mouseenter | onmouseenter     | マウスが要素に入ったとき     | 任意の要素     | `element.onmouseenter = () => console.log('enter');`           |
| mouseleave | onmouseleave     | マウスが要素から出たとき     | 任意の要素     | `element.onmouseleave = () => console.log('leave');`           |
| mousewheel | onmousewheel     | ホイール操作時               | 任意の要素     | `element.onmousewheel = e => console.log(e.deltaY);`           |

---

### キーボードイベント

| イベント名 | イベントハンドラ | 発生タイミング                   | 対象           | 使用例                                           |
| ---------- | ---------------- | -------------------------------- | -------------- | ------------------------------------------------ |
| keydown    | onkeydown        | キーが押されたとき               | ほとんどのタグ | `document.onkeydown = e => console.log(e.key);`  |
| keyup      | onkeyup          | キーが離されたとき               | ほとんどのタグ | `document.onkeyup = e => console.log(e.key);`    |
| keypress   | onkeypress       | キーが押されたとき（文字生成時） | ほとんどのタグ | `document.onkeypress = e => console.log(e.key);` |

---

### メディアイベント

| イベント名     | 発生タイミング               | 対象         | 使用例                                                        |
| -------------- | ---------------------------- | ------------ | ------------------------------------------------------------- |
| canplay        | 再生可能になったとき         | audio, video | `video.oncanplay = () => console.log('can play');`            |
| canplaythrough | 途切れず再生可能になったとき | audio, video | `video.oncanplaythrough = () => console.log('full play');`    |
| durationchange | コンテンツ長が変わったとき   | audio, video | `video.ondurationchange = () => console.log(video.duration);` |
| ended          | 再生終了時                   | audio, video | `video.onended = () => console.log('ended');`                 |
| pause          | 再生一時停止                 | audio, video | `video.onpause = () => console.log('paused');`                |
| play           | 再生開始                     | audio, video | `video.onplay = () => console.log('playing');`                |
| timeupdate     | 再生時間更新                 | audio, video | `video.ontimeupdate = () => console.log(video.currentTime);`  |
| volumechange   | 音量変更時                   | audio, video | `video.onvolumechange = () => console.log(video.volume);`     |

---

### ウィンドウ・リソースイベント

| イベント名 | 発生タイミング         | 対象      | 使用例                                                    |
| ---------- | ---------------------- | --------- | --------------------------------------------------------- |
| resize     | ウィンドウサイズ変更時 | window    | `window.onresize = () => console.log(window.innerWidth);` |
| scroll     | スクロール時           | body, div | `window.onscroll = () => console.log(window.scrollY);`    |
| online     | ネットワーク接続時     | window    | `window.ononline = () => console.log('online');`          |
| offline    | ネットワーク切断時     | window    | `window.onoffline = () => console.log('offline');`        |
| hashchange | URLハッシュ変更時      | window    | `window.onhashchange = () => console.log(location.hash);` |
| popstate   | 履歴操作時             | window    | `window.onpopstate = () => console.log(history.state);`   |
| storage    | Web Storage更新時      | window    | `window.onstorage = e => console.log(e.key, e.newValue);` |

---

### 印刷イベント

| イベント名  | 発生タイミング | 対象   | 使用例                                                      |
| ----------- | -------------- | ------ | ----------------------------------------------------------- |
| beforeprint | 印刷開始直前   | window | `window.onbeforeprint = () => console.log('before print');` |
| afterprint  | 印刷完了後     | window | `window.onafterprint = () => console.log('after print');`   |

---

### フォームイベント

| イベント名 | 発生タイミング     | 対象  | 使用例                                                                |
| ---------- | ------------------ | ----- | --------------------------------------------------------------------- |
| submit     | フォーム送信時     | form  | `form.onsubmit = e => { e.preventDefault(); console.log('submit'); }` |
| reset      | フォームリセット時 | form  | `form.onreset = () => console.log('reset');`                          |
| invalid    | 制約違反時         | input | `input.oninvalid = () => console.log('invalid');`                     |

---

