# Window API
＝ブラウザが window オブジェクトにぶら下げて提供している API

| 名前        | 正体（どのオブジェクト？） | Web API の分類     | 何ができる？                                   |
|-------------|------------------------------|----------------------|-----------------------------------------------|
| alert()     | window.alert                 | Window API           | アラート表示                                   |
| confirm()   | window.confirm               | Window API           | OK/Cancel ダイアログ                           |
| prompt()    | window.prompt                | Window API           | 入力ダイアログ                                 |
| navigator   | window.navigator             | Navigator API        | ブラウザ情報、デバイス情報、クリップボード、バッテリーなど |
| screen      | window.screen                | Screen API           | 画面サイズや解像度の取得                       |
| location    | window.location              | Location API         | URL の取得・変更、ページ遷移                   |
| history     | window.history               | History API          | ブラウザの戻る/進む、履歴操作                  |


## 詳しく
ブラウザ環境では、JavaScript は自動的に window を参照しているから
```js
alert("hi");
```
と書いても実際は
```js
window.alert("hi");
```
を呼んでいる


## まとめ
- window オブジェクトのプロパティ
- JavaScript ではなく ブラウザが提供している機能