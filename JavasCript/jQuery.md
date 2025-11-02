# jQuery
→ JavaScriptを簡単に書けるようにしたライブラリ

## jQueryの読み込み
```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
```
---

## 基本の構文
- ①jQueryオブジェクトを作成 ($)
- ②そのjQueryオブジェクトに対してメソッド（機能）を呼び出す
- ③文末にセミコロンが必要
- ④コメントは //

```js
// 基本構文
$('セレクタ').(メソッド(引数));
  // 何を     // どうするか
```

```js
// セレクタ
$('#id名')        // id指定
$('.class名')     // class指定
$('タグ名')       // タグ指定（例：'p', 'div'）
```

```js
// イベント処理
$('#btn').click(function() {
  alert('クリックされた');
});
```

```js
// CSS
$('セレクタ').css('プロパティ名', '値');
```

```js
// CSSを変更
$('#box').css('background-color', 'blue');
```

```js
// 要素の表示・非表示
$('#box').hide();   // 非表示
$('#box').show();   // 表示
```

```js
// HTMLの中身を変更
$('#text').html('新しい内容');
```

```js
// アニメーション付きで要素を隠す
$('セレクタ').fadeOut('速度'(1500));
```

```js
// アニメーション付きで要素を上にスライドして隠す
$('セレクタ').slideUp('速度'(1500));
```

```js
// 要素を徐々に表示
$('セレクタ').fadeIn('速度'(1500));
```

```js
// 要素を上から下へ表示
$('セレクタ').slideDown('速度'(1500));
```
```js
// ()内の文字を置き換える
$('セレクタ').text('書き換える文字列');
```
---

## よく使うメソッド

| メソッド        | 説明                             |
|----------------|----------------------------------|
| `.hide()`       | 要素を隠す                        |
| `.show()`       | 要素を表示する                    |
| `.css()`        | CSSを変更する                     |
| `.html()`       | HTMLの中身を変更する              |
| `.val()`        | inputの値を取得・変更する         |
| `.addClass()`   | クラスを追加する                  |
| `.removeClass()`| クラスを削除する                  |

---

## 使い方の例

```javascript
$('#box').hide(); // boxを隠す
$('#box').css('color', 'red'); // 文字色を赤にする
$('#input').val(); // inputの値を取得
```

---

## jQueryの $.ajax() オプション

| オプション       | 説明 |
|------------------|------|
| `url`            | 通信先のURL |
| `method` または `type` | HTTPメソッド（例：`GET`, `POST`） |
| `data`           | サーバーに送るデータ（オブジェクト形式） |
| `success`        | 通信が成功したときに呼ばれる関数 |
| `error`          | 通信が失敗したときに呼ばれる関数 |
| `dataType`       | 受け取るデータの型（例：`json`, `text`） |
| `headers`        | リクエストヘッダーの設定 |



```bash
$.ajax({
  url: '/api/user',           # 通信先
  method: 'POST',             # POSTメソッドで送信
  data: { name: 'Taro' },     # 送るデータ
  success: function(response) {
    console.log(response);    # 成功時の処理
  }
});
```