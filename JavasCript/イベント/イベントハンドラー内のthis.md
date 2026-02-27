# イベントハンドラー内の this
＝結論：イベントハンドラー内の **this は そのイベントハンドラーが登録されている要素** を指す

```html
<button id="btn">Click</button>
```
```js
const btn = document.getElementById('btn');

btn.addEventListener('click', function() {
    console.log(this); // → この場合は btn 要素
});
```
- function キーワードで定義した関数では this がイベントターゲット（ボタン）を指す
- アロー関数では this は外側のスコープを参照するため、イベントターゲットにはならない

## なぜブラウザはこうしているか？
- ブラウザはイベントを発生させた要素を イベントハンドラーの this に設定して呼び出す
- そのため、関数の中で this を使えば その要素自身にアクセスできる

```js
btn.addEventListener('click', function() {
    this.style.backgroundColor = 'red'; // クリックされたボタンの色を変える
});
```

## bind() を使う場合
＝bind() を使うと、**this を 好きな値に固定 できる**

```js
function handleClick(e) {
    console.log(this); // bindで固定したオブジェクト
    console.log(e);    // イベントオブジェクトは自動で渡される
}

const obj = { name: '固定' };
btn.addEventListener('click', handleClick.bind(obj));
```
- この場合 **this はボタンではなく、obj になる**
- 引数も固定できるので、イベントオブジェクトと一緒に渡したい情報も設定可能

## まとめ
| 定義方法 / 呼び出し    | `this` の値                  |
| -------------- | -------------------------- |
| function キーワード | イベントを発生させた要素（event target） |
| アロー関数          | 外側のスコープの `this` を継承        |
| bind() 使用      | bind で指定したオブジェクト           |

- イベントの this = イベントが登録されている要素 が基本形
- アロー関数や bind で変更できる


```bash
<button>クリック</button>
          │
          ▼
    イベント発生
          │
          ▼
   functionハンドラー(this = ボタン)
```