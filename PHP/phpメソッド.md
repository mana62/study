### PHP関数

## 基本出力・文字列操作

| 関数・メソッド       | 機能説明                       | 使用例                                    | 基本構文                                    |
| -------------------- | ------------------------------ | ----------------------------------------- | ------------------------------------------- |
| `echo()`             | 画面に文字列を出力             | `echo "こんにちは";`                      | `echo "出力したい文字列";`                  |
| `print_r()`          | 配列などの中身を簡易表示       | `print_r($array);`                        | `print_r(変数);`                            |
| `var_dump()`         | デバッグ用に型と値を表示       | `var_dump($array);`                       | `var_dump(変数);`                           |
| `PHP_EOL`            | 改行コード（OS依存）           | `echo "改行" . PHP_EOL;`                  | `PHP_EOL`                                   |
| `strip_tags()`       | HTMLタグを除去                 | `strip_tags('<p>Hello</p>') // Hello`     | `strip_tags(文字列, 許可タグ);`             |
| `htmlspecialchars()` | HTML特殊文字をエスケープ       | `htmlspecialchars('<a>')`                 | `htmlspecialchars(文字列, 定数);`           |
| `strrev()`           | 文字列を逆順に                 | `strrev("abc") // "cba"`                  | `strrev(文字列);`                           |
| `strtolower()`       | 英字を小文字に変換             | `strtolower("HELLO") // "hello"`          | `strtolower(文字列);`                       |
| `strtoupper()`       | 英字を大文字に変換             | `strtoupper("hello") // "HELLO"`          | `strtoupper(文字列);`                       |
| `str_replace()`      | 文字列の一部を置換             | `str_replace(" ", "-", $str)`             | `str_replace($search, $replace, $subject);` |
| `substr()`           | 文字列の一部を取得             | `substr("Hello", 0, 3) // "Hel"`          | `substr(文字列, 開始位置, 長さ);`           |
| `substr_count()`     | 部分文字列の出現回数を取得     | `substr_count("abcabc", "a") // 2`        | `substr_count(文字列, 検索文字列);`         |
| `str_split()`        | 文字列を1文字ずつ分割          | `str_split("abc") // ['a','b','c']`       | `str_split(文字列);`                        |
| `mb_strlen()`        | マルチバイト文字列の長さ       | `mb_strlen("こんにちは") // 5`            | `mb_strlen(文字列, 文字コード);`            |
| `mb_substr()`        | マルチバイト文字列の一部を取得 | `mb_substr("こんにちは", 0, 2) // "こん"` | `mb_substr(文字列, 開始位置, 長さ);`        |

---

## 配列操作

| 関数名                   | 説明                         | 使用例                                    | 基本構文                            |
| ------------------------ | ---------------------------- | ----------------------------------------- | ----------------------------------- |
| `array_push()`           | 配列末尾に値を追加           | `array_push($arr, "A");`                  | `array_push(配列, 値);`             |
| `array_merge()`          | 配列を結合                   | `array_merge([1], [2]) // [1,2]`          | `array_merge(配列1, 配列2);`        |
| `array_sum()`            | 配列内の合計                 | `array_sum([1,2,3]) // 6`                 | `array_sum(配列);`                  |
| `array_unique()`         | 重複を除去                   | `array_unique([1,1,2]) // [1,2]`          | `array_unique(配列);`               |
| `array_keys()`           | キー一覧を取得               | `array_keys(['a'=>1]) // ['a']`           | `array_keys(連想配列);`             |
| `array_values()`         | 値一覧を取得                 | `array_values(['a'=>1]) // [1]`           | `array_values(連想配列);`           |
| `array_rand()`           | ランダムなキーを取得         | `array_rand(['a','b','c'])`               | `array_rand(配列);`                 |
| `in_array()`             | 値が含まれているか確認       | `in_array(2, [1,2,3]) // true`            | `in_array(値, 配列);`               |
| `array_map()`            | 各要素に関数を適用           | `array_map('strtoupper', ['a','b'])`      | `array_map(関数, 配列);`            |
| `array_reduce()`         | 配列を1つの値に集約          | `array_reduce([1,2,3], fn($a,$b)=>$a+$b)` | `array_reduce(配列, 関数, 初期値);` |
| `array_key_exists()`     | 指定キーが存在するか確認     | `array_key_exists('a', ['a'=>1])`         | `array_key_exists(キー, 配列);`     |
| `array_walk_recursive()` | 多次元配列に再帰処理         | `array_walk_recursive($arr, fn...)`       | `array_walk_recursive(配列, 関数);` |
| `usort()`                | ユーザー定義で配列を並び替え | `usort($arr, fn($a,$b)=>$a-$b);`          | `usort(配列, 比較関数);`            |
| `sort()`                 | 昇順に並び替え（破壊的）     | `sort($arr);`                             | `sort(配列);`                       |

---

## 日付・時間

| 関数名        | 説明                         | 使用例                          | 基本構文                    |
| ------------- | ---------------------------- | ------------------------------- | --------------------------- |
| `date()`      | 指定フォーマットで日付を取得 | `date("Y-m-d") // "2025-08-29"` | `date("フォーマット");`     |
| `strtotime()` | 文字列からタイムスタンプ取得 | `strtotime("next Monday")`      | `strtotime(文字列);`        |
| `DateTime`    | 日付操作用のクラス           | `new DateTime("2025-01-01")`    | `new DateTime(日付文字列);` |
| `diff`        | DateTime同士の差分を取得     | `$a->diff($b)`                  | `$date1->diff($date2);`     |

---

## ファイル操作

| 関数名                | 説明                       | 使用例                                    | 基本構文                                 |
| --------------------- | -------------------------- | ----------------------------------------- | ---------------------------------------- |
| `file_put_contents()` | ファイルにデータを書き込む | `file_put_contents("test.txt", "Hello");` | `file_put_contents(ファイル名, データ);` |
| `file_get_contents()` | ファイルの内容を取得       | `file_get_contents("test.txt");`          | `file_get_contents(ファイル名);`         |

---

## 変数・型・例外

| 関数名                  | 説明                       | 使用例                           | 基本構文                             |
| ----------------------- | -------------------------- | -------------------------------- | ------------------------------------ |
| `isset()`               | 変数が定義されているか確認 | `isset($name)`                   | `isset(変数);`                       |
| `empty()`               | 変数が空か確認             | `empty("") // true`              | `empty(変数);`                       |
| `is_null()`             | nullかどうか確認           | `is_null(null) // true`          | `is_null(変数);`                     |
| `ctype_digit()`         | 数字かどうか確認           | `ctype_digit("123") // true`     | `ctype_digit(文字列);`               |
| `throw new Exception()` | 例外を投げる               | `throw new Exception("エラー");` | `throw new Exception("メッセージ");` |

---

## JSON・HTTP・その他

| 関数名                    | 説明                                     | 基本構文                                        |
| ------------------------- | ---------------------------------------- | ----------------------------------------------- |
| `preg_match()`            | 正規表現で文字列内の一致を判定する       | `preg_match('/パターン/', 対象文字列);`         |
| `preg_replace()`          | 正規表現で文字列を置換する               | `preg_replace('/検索/', '置換後', 対象文字列);` |
| `stream_context_create()` | HTTPなどのリクエスト用のコンテキスト作成 | `stream_context_create(配列);`                  |


