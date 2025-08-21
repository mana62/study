### PHP関数
| 関数・メソッド          | 機能説明                                   | 使用例                                             | 基本構文                                             |
| ----------------------- | ------------------------------------------ | -------------------------------------------------- | -------------------------------------------------------------- |
| `echo()`                | 画面に文字列を出力する                     | `echo "こんにちは";`                               | echo "出力したい文字列";                                       |
| `strlen()`              | 文字列のバイト数（英数字）を取得           | `strlen("Hello") // 5`                             | strlen(文字列);                                                |
| `mb_strlen()`           | マルチバイト文字列の文字数（日本語）を取得 | `mb_strlen("こんにちは", "UTF-8") // 5`            | mb_strlen(文字列, 文字コード);                                 |
| `substr()`              | 文字列の一部分を取得（英数字）             | `substr("Hello", 0, 3) // "Hel"`                   | substr(文字列, 開始位置, 取得文字数);                          |
| `mb_substr()`           | マルチバイト文字列の一部を取得             | `mb_substr("こんにちは", 0, 2, "UTF-8") // "こん"` | mb_substr(文字列, 開始位置, 取得文字数, 文字コード);           |
| `strtolower()`          | 英字を小文字に変換                         | `strtolower("HELLO") // "hello"`                   | strtolower(文字列);                                            |
| `strtoupper()`          | 英字を大文字に変換                         | `strtoupper("hello") // "HELLO"`                   | strtoupper(文字列);                                            |
| `str_replace()`         | 文字列の一部を別の文字列に置き換える       | `str_replace("りんご", "バナナ", $text);`          | str_replace("検索文字列", "置換文字列", 元文字列);             |
| `htmlspecialchars()`    | HTMLの特殊文字をエスケープ（安全に表示）   | `htmlspecialchars($_POST['name'], ENT_QUOTES);`    | htmlspecialchars(文字列, オプション定数);                      |
| `implode()`             | 配列の要素を連結して1つの文字列に変換      | `implode(", ", ["A", "B"]) // "A, B"`              | implode(区切り文字, 配列);                                     |
| `explode()`             | 文字列を区切って配列に変換                 | `explode(",", "A,B,C") // ["A", "B", "C"]`         | explode(区切り文字, 文字列);                                   |
| `trim()`                | 文字列の前後の空白（スペースなど）を削除   | `trim(" Hello ") // "Hello"`                       | trim(文字列);                                                  |
| `array_push()`          | 配列の末尾に値を追加                       | `array_push($array, "value");`                     | array_push(配列, 追加する値);                                  |
| `array_merge()`         | 配列同士を結合                             | `array_merge([1,2], [3,4]) // [1,2,3,4]`           | array_merge(配列1, 配列2);                                     |
| `array_sum()`           | 配列内の数値の合計を取得                   | `array_sum([1, 2, 3]) // 6`                        | array_sum(数値が入った配列);                                   |
| `array_unique()`        | 配列の重複した値を除外                     | `array_unique([1,1,2,3]) // [1,2,3]`               | array_unique(配列);                                            |
| `array_keys()`          | 配列のキー一覧を配列で取得                 | `array_keys(['a' => 1, 'b' => 2]) // ['a', 'b']`   | array_keys(連想配列);                                          |
| `array_values()`        | 配列の値一覧を取得                         | `array_values(['a' => 1, 'b' => 2]) // [1, 2]`     | array_values(連想配列);                                        |
| `array_rand()`          | 配列からランダムにキーを取得               | `array_rand(['a', 'b', 'c'])`                      | array_rand(配列);                                              |
| `in_array()`            | 指定した値が配列に含まれているかを確認     | `in_array(2, [1,2,3]) // true`                     | in_array(調べたい値, 配列, 厳密判定);                          |
| `array_map()`           | 配列の各要素に関数を適用                   | `array_map('strtoupper', ['a','b']) // ['A','B']`  | array_map(関数名, 配列);                                       |
| `json_encode()`         | 配列／オブジェクトをJSON文字列に変換       | `json_encode(['name'=>'John'])`                    | json_encode(データ);                                           |
| `json_decode()`         | JSON文字列を配列やオブジェクトに変換       | `json_decode('{"name":"John"}', true)`             | json_decode(JSON文字列, trueなら配列 / falseならオブジェクト); |
| `isset()`               | 変数が定義されているか確認                 | `isset($name)`                                     | isset(変数);                                                   |
| `empty()`               | 変数が空（null, "", 0など）か確認          | `empty("") // true`                                | empty(変数);                                                   |
| `is_null()`             | 変数がnullかどうか確認                     | `is_null(null) // true`                            | is_null(変数);                                                 |
| `date("Y-m-d")`         | 現在の日付を指定フォーマットで取得         | `date("Y-m-d") => "2025-07-12"`                    | date("フォーマット文字列");                                    |
| `date("H:i")`           | 現在の時間（時:分）を取得                  | `date("H:i") => "13:45"`                           | date("フォーマット文字列");                                    |
| `rand()` / `mt_rand()`  | ランダムな整数を取得                       | `rand(1, 100)` / `mt_rand(0, 3)`                   | rand(最小値, 最大値); / mt_rand(最小値, 最大値);               |
| `sort()`                | 配列を昇順にソート（元配列が上書きされる） | `sort($arr)`                                       | sort(配列);                                                    |
| `array_key_exists()`    | 配列内に指定キーが存在するか確認           | `array_key_exists('key', ['key' => 'value'])`      | array_key_exists(キー, 配列);                                  |
| `require_once()`        | PHPファイルを1度だけ読み込む               | `require_once('config.php');`                      | require_once('ファイル名.php');                                |
| `header('Location:〜')` | 指定URLにリダイレクト                      | `header('Location: index.php');`                   | header('Location: URL');                                       |
| `var_dump()`            | デバッグ用に変数の型と値を表示             | `var_dump($array);`                                | var_dump(変数);                                                |
| `print_r()`             | 配列などの中身を簡易表示                   | `print_r($array);`                                 | print_r(変数);                                                 |
|`strip_tags`         | HTMLやPHPタグを除去して**プレーンテキストに変換　 | `strip_tags('<p>Hello <b>World</b></p>', '<b>')` → `Hello <b>World</b>`    |  strip_tags( 文字列 , 許可するタグ );


# 少し難易度高メソッド
| 関数名                    | 説明                                | 基本構文（日本語）                            |
| ------------------------- | ----------------------------------- | --------------------------------------------- |
| `array_reduce()`          | 配列を1つの値（合計など）に集約する | array_reduce(配列, 集約関数, 初期値);         |
| `preg_match()`            | 正規表現で文字列内の一致を判定する  | preg_match('/パターン/', 対象文字列);         |
| `preg_replace()`          | 正規表現で文字列を置換する          | preg_replace('/検索/', '置換後', 対象文字列); |
| `array_walk_recursive()`  | 多次元配列の全要素に処理を適用する  | array_walk_recursive(配列, コールバック関数); |
| `stream_context_create()` | HTTPなどのリクエ                    |
