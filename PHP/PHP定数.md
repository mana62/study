# PHP定数
→ 変化することのない値のこと

---

## 基本構文
```bash
# 定数を定義
const 定数名 = 値;
```

```bash
# 定数にアクセス
クラス名::定数名
```

---

## 注意点
- 定数は一度決めたら変えられない
- 定数は大文字で定義する (VERSION, MAX_COUNT など)

---

| 定数名 | 意味 | 主な用途 |
|--------|------|----------|
| `JSON_ERROR_NONE` | エラーなし | `json_last_error()` の結果チェック時に使う。:contentReference[oaicite:1]{index=1} |
| `JSON_ERROR_DEPTH` | 最大スタック深度を超えた | 深いネスト構造の JSON を `json_decode()`／`json_encode()` した際のエラー。:contentReference[oaicite:2]{index=2} |
| `JSON_ERROR_STATE_MISMATCH` | モードの不一致、またはアンダーフロー | 不正な状態で JSON を扱ったとき。:contentReference[oaicite:3]{index=3} |
| `JSON_ERROR_CTRL_CHAR` | 制御文字エラー（不正なエンコード） | 文字列中に制御文字が混ざっている場合など。:contentReference[oaicite:4]{index=4} |
| `JSON_ERROR_SYNTAX` | 構文エラー、フォーマットが不正 | `json_decode()` で不正な JSON 文字列のとき。:contentReference[oaicite:5]{index=5} |
| `JSON_ERROR_UTF8` | UTF-8 文字列が不正または誤エンコード | マルチバイト文字やエンコーディングが異なる文字列処理で。:contentReference[oaicite:6]{index=6} |
| `JSON_ERROR_RECURSION` | 再帰参照が含まれていてエンコードできない | 配列／オブジェクトに循環参照がある場合。:contentReference[oaicite:7]{index=7} |
| `JSON_ERROR_INF_OR_NAN` | `INF` または `NAN` を含んでいてエンコードできない | 浮動小数点数を扱うときの注意。:contentReference[oaicite:8]{index=8} |
| `JSON_ERROR_UNSUPPORTED_TYPE` | サポートされていない型が含まれている | リソース型など JSON 化できないものを扱ったとき。:contentReference[oaicite:9]{index=9} |
| `JSON_ERROR_INVALID_PROPERTY_NAME` | 無効なプロパティ名がある | オブジェクトとしてデコードする際、プロパティ名に問題があるとき。:contentReference[oaicite:10]{index=10} |
| `JSON_ERROR_UTF16` | 不正な UTF-16 サロゲートを含む | UTF-16 エスケープが不適切な JSON 文字列をデコードしたとき。:contentReference[oaicite:11]{index=11} |
| `JSON_BIGINT_AS_STRING` | 大きな整数を文字列として処理する | `json_decode()` 使用時、大きな整数を失わず扱いたいとき。:contentReference[oaicite:12]{index=12} |
| `JSON_OBJECT_AS_ARRAY` | オブジェクトを配列として扱う（`json_decode()`） | `json_decode($json, true)` と同義的。:contentReference[oaicite:13]{index=13} |
| `JSON_HEX_TAG` | `<` と `>` を `\u003C`／`\u003E` にエスケープ | 出力 JSON 中の HTML タグ混在対応。:contentReference[oaicite:14]{index=14} |
| `JSON_HEX_AMP` | `&` を `\u0026` にエスケープ | HTML／URL データとの混在時。:contentReference[oaicite:15]{index=15} |
| `JSON_HEX_APOS` | `'` を `\u0027` にエスケープ | シングルクォート含む文字列を扱うとき。:contentReference[oaicite:16]{index=16} |
| `JSON_HEX_QUOT` | `"` を `\u0022` にエスケープ | ダブルクォート含む文字列を扱うとき。:contentReference[oaicite:17]{index=17} |
| `JSON_FORCE_OBJECT` | 配列をオブジェクトとして出力 | 空の配列をオブジェクトとして出したい API 仕様対応。:contentReference[oaicite:18]{index=18} |
| `JSON_NUMERIC_CHECK` | 文字列の数字を数値としてエンコード | `"123"` を `123` として扱いたいとき。:contentReference[oaicite:19]{index=19} |
| `JSON_PRETTY_PRINT` | 整形（インデント・改行）付き JSON 出力 | 人が読むためのログ出力等。:contentReference[oaicite:20]{index=20} |
| `JSON_UNESCAPED_SLASHES` | `/` を `\/` にせずそのまま出力 | URL やパス文字列を綺麗に表示したいとき。:contentReference[oaicite:21]{index=21} |
| `JSON_UNESCAPED_UNICODE` | マルチバイト Unicode 文字を `\uXXXX` 形式にせずそのまま出力 | 日本語や絵文字を可読のまま出力したいとき。:contentReference[oaicite:22]{index=22} |
| `JSON_PARTIAL_OUTPUT_ON_ERROR` | 一部エンコード不能でも処理を続けて出力 | 完全失敗より「出力可能な部分だけでも出したい」場合。:contentReference[oaicite:23]{index=23} |
| `JSON_PRESERVE_ZERO_FRACTION` | 浮動小数点数を必ず「小数点付き」で出力 | `123.0` を `123` ではなく `123.0` と表示したいとき。:contentReference[oaicite:24]{index=24} |
| `JSON_UNESCAPED_LINE_TERMINATORS` | 行終端（改行等）をエスケープせず出力 | 改行文字を可視的に出したいログ用途など。:contentReference[oaicite:25]{index=25} |
| `JSON_INVALID_UTF8_IGNORE` | 不正な UTF-8 を無視して出力 | エンコード元文字列に異常がある可能性があるとき。:contentReference[oaicite:26]{index=26} |
| `JSON_INVALID_UTF8_SUBSTITUTE` | 不正な UTF-8 を代替文字に置き換えて出力 | データ内に破損文字の可能性があるとき。:contentReference[oaicite:27]{index=27} |
| `JSON_THROW_ON_ERROR` | エラー時に `JsonException` を投げる | `json_encode()`／`json_decode()` で例外処理したいとき。:contentReference[oaicite:28]{index=28} |


MB_CASE_UPPER → 全部大文字

MB_CASE_LOWER → 全部小文字

MB_CASE_TITLE → タイトルケース