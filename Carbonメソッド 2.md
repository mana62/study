# Carbonメソッド
| 機能カテゴリ     | メソッド名                        | 意味・使うとき                         | 使い方の例                           |
| ---------------- | --------------------------------- | -------------------------------------- | ------------------------------------ |
| 現在の日時       | `now()`                           | 現在の日時を取得                       | `Carbon::now()`                      |
| 文字列から生成   | `parse()`                         | 文字列からCarbonインスタンスを生成     | `Carbon::parse('2025-06-01')`        |
| 任意フォーマット | `format()`                        | 日時を好きな形式に整形                 | `Carbon::now()->format('Y-m-d')`     |
| 日数加算         | `addDays()`                       | N日後に移動                            | `Carbon::now()->addDays(3)`          |
| 月数減算         | `subMonths()`                     | Nヶ月前に移動                          | `Carbon::now()->subMonths(1)`        |
| 月の先頭・末尾   | `startOfMonth()` / `endOfMonth()` | 月初・月末にジャンプ                   | `Carbon::now()->startOfMonth()`      |
| 土日判定         | `isWeekend()`                     | 土日かどうか判定                       | `Carbon::now()->isWeekend()`         |
| 同じ月か判定     | `isSameMonth()`                   | 比較対象と同じ月かどうか               | `$a->isSameMonth($b)`                |
| 月の差分         | `diffInMonths()`                  | 月単位の差分を取得                     | `$a->diffInMonths($b)`               |
| 日付の大小比較   | `gte()` / `lt()`                  | >= や < で比較したいとき               | `$a->gte($b)`                        |
| 文字列変換       | `toDateString()`                  | `Y-m-d` 形式の文字列に変換             | `Carbon::now()->toDateString()`      |
| 配列検索         | `in_array()`                      | 祝日などの配列に含まれているか確認     | `in_array($date, $holidays)`         |
| 日付範囲操作     | `CarbonPeriod()`                  | 開始日〜終了日までの日付をループできる | `CarbonPeriod::create($start, $end)` |
| 月曜〜日曜判定   | `isMonday()`〜`isSunday()`        | 指定の日付が該当曜日かどうか           | `if ($date->isFriday())`             |
| 曜日名取得       | `dayName`                         | 曜日の名前（英語）を取得               | `$date->dayName`                     |
| 曜日番号取得     | `dayOfWeek`                       | 曜日を数値で取得（0:日曜〜6:土曜）     | `if ($date->dayOfWeek === 6)`        |
| 以下か判定       | `lte()`                           | 日付が比較対象より小さい（<=）か判定   | `$start->lte($end)`                  |

