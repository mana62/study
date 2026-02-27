# Intlメソッド
＝JavaScript に標準で入っている 国際化（多言語対応）用の API
下記のようなフォーマットをしてくれるもの
- 日付（Date）
- 数値（Number）
- 通貨（Currency）
- 相対時間（◯分前）
- 文字列比較（ソート）
**ライブラリなしで国際化対応できる**

## Intl メソッド一覧
| メソッド | 役割 |
|---------|------|
| **Intl.DateTimeFormat** | 日付・時刻のフォーマット |
| **Intl.NumberFormat** | 数値・通貨・単位のフォーマット |
| **Intl.RelativeTimeFormat** | 「3分前」「2日後」などの相対時間フォーマット |
| **Intl.Collator** | 文字列の比較（ソート順の制御） |
| **Intl.ListFormat** | リストを自然な文章に整形（例：A、B、C） |
| **Intl.PluralRules** | 単数・複数形の判定 |


## ①Intl.DateTimeFormat（日時フォーマット）
基本形
```js
new Intl.DateTimeFormat('ja-JP').format(new Date());
```

## よく使うオプション
**dateStyle**
| 値 | 出力例（ja-JP） |
|------|------------------------|
| `"full"` | 2024年2月5日月曜日 |
| `"long"` | 2024年2月5日 |
| `"medium"` | 2024/02/05 |
| `"short"` | 24/02/05 |

```js
// 例
new Intl.DateTimeFormat('ja-JP', { dateStyle: 'medium' }).format(new Date());
```

### 個別指定（細かく制御）
```js
new Intl.DateTimeFormat('ja-JP', {
  year: 'numeric',
  month: '2-digit',
  day: '2-digit',
  hour: '2-digit',
  minute: '2-digit'
}).format(new Date());
```

## ② Intl.NumberFormat（数値・通貨）
**カンマ区切り**
```js
new Intl.NumberFormat('ja-JP').format(1234567);
// → "1,234,567"
```

**通貨**
```js
new Intl.NumberFormat('ja-JP', {
  style: 'currency',
  currency: 'JPY'
}).format(5000);
// → "￥5,000"
```

**単位**
```js
new Intl.NumberFormat('ja-JP', {
  style: 'unit',
  unit: 'kilometer'
}).format(5);
// → "5 km"
```

## ③ Intl.RelativeTimeFormat（相対時間）
```js
const rtf = new Intl.RelativeTimeFormat('ja-JP');

rtf.format(-3, 'minute'); // → "3 分前"
rtf.format(2, 'day');     // → "2 日後"
```

## ④ Intl.Collator（文字列比較）
```js
 const collator = new Intl.Collator('ja-JP');
['あ', 'ア', 'か'].sort(collator.compare);
```

## ⑤ Intl.ListFormat（リストを自然な文章に）
```js
new Intl.ListFormat('ja-JP').format(['りんご', 'バナナ', 'みかん']);
// → "りんご、バナナ、みかん"
```

## ⑥ Intl.PluralRules（単数・複数形の判定）
```js
const pr = new Intl.PluralRules('en-US');
pr.select(1); // → "one"
pr.select(2); // → "other"
```

## まとめ
- 多言語対応したい
- 日付や通貨のフォーマットを自動化したい
- 手書きフォーマットの面倒を減らしたい
- ライブラリなしで軽く済ませたい