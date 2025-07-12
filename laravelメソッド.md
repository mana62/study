# larvelメソッド
| メソッド・関数                     | 意味・用途                                               | 使用例                                                             | 基本構文                                        |
| ---------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------ | --------------------------------------------------------- |
| `parse`                            | 文字列から日時オブジェクトに変換                         | `Carbon::parse('2023-10-01 10:00:00');`                            | Carbon::parse(文字列);                                    |
| `whereHas`                         | リレーションが存在し、かつその条件を満たすかで絞り込み   | `Model::whereHas('relation', fn($q) => $q->where(...))->get();`    | Model::whereHas('リレーション名', クロージャ関数)->get(); |
| `use`                              | クロージャ内で外部の変数を利用する                       | `function ($query) use ($date)`                                    | クロージャ関数の後ろに use(変数);                         |
| `whereDate`                        | 指定日と一致する日付カラムのレコードを取得               | `Model::whereDate('column_name', '2023-10-01')->get();`            | Model::whereDate(日付カラム, '日付文字列')->get();        |
| `with`                             | リレーションを事前に読み込んでN+1問題を回避              | `Model::with('relationName')->get();`                              | Model::with('リレーション名')->get();                     |
| `diffInMinutes`                    | 2つの日時の差を分単位で取得                              | `$start->diffInMinutes($end);`                                     | 開始日時->diffInMinutes(終了日時);                        |
| `reduce`                           | コレクションの値を合計などにまとめる                     | `$collection->reduce(fn($carry, $item) => $carry + $item->value);` | コレクション->reduce(処理関数, 初期値);                   |
| `floor`                            | 数値を小数点以下で切り捨て                               | `floor($number / 60);`                                             | floor(計算式);                                            |
| `firstOrNew`                       | 条件一致するレコードを取得、なければ新規インスタンス作成 | `Model::firstOrNew(['email' => $email]);`                          | Model::firstOrNew(検索条件, デフォルト値);                |
| `whereNull`                        | 指定カラムが null のレコードを取得                       | `Model::whereNull('deleted_at')->get();`                           | Model::whereNull('カラム名')->get();                      |
| `Auth::attempt`                    | メールとパスワードで認証できるか確認                     | `Auth::attempt(['email' => ..., 'password' => ...]);`              | Auth::attempt([キー => 値]);                              |
| `isToday`                          | 日付が今日かどうかを判定                                 | `$date->isToday();`                                                | 日付オブジェクト->isToday();                              |
| `isset`                            | 変数が存在しているか確認                                 | `if (isset($variable)) { ... }`                                    | isset(変数);                                              |
| `subDays`                          | 日付オブジェクトから指定日数を引く                       | `$date->subDays(3);`                                               | 日付オブジェクト->subDays(日数);                          |
| `str_pad`                          | 指定の長さまで文字列を埋める（ゼロ埋めなど）             | `str_pad(5, 2, '0', STR_PAD_LEFT);`                                | str_pad(元文字列, 長さ, 埋める文字, 埋める方向);          |
| `data_get()`                       | ネストされた配列やオブジェクトから安全に値を取得         | `data_get($user, 'profile.name')`                                  | data_get(対象配列またはオブジェクト, 'キー階層');         |
| `to_route()`                       | 名前付きルートにリダイレクト                             | `return to_route('home');`                                         | to_route('ルート名', 引数配列, ステータスコード);         |
| `fresh()`                          | DBから最新状態を取得（IDは同じ）                         | `$model->fresh();`                                                 | モデルインスタンス->fresh();                              |
| `refresh()`                        | インスタンスを再読み込み                                 | `$model->refresh();`                                               | モデルインスタンス->refresh();                            |
| `cursor()`                         | メモリ効率よく大量のレコードを順に処理                   | `Model::cursor()->each(fn($item) => ...)`                          | モデル::cursor()->each(処理関数);                         |
| `abort()`                          | 条件に関係なくステータスコードで終了                     | `abort(404)`                                                       | abort(ステータスコード);                                  |
| `abort_if()`                       | 条件が true の場合にステータスコードで終了               | `abort_if(!$user, 403)`                                            | abort_if(条件式, ステータスコード, エラーメッセージ);     |
| `abort_unless()`                   | 条件が false の場合にステータスコードで終了              | `abort_unless($user->isAdmin(), 403)`                              | abort_unless(条件式, ステータスコード, エラーメッセージ); |
| `abort(403, 'アクセスできません')` | 条件なしでステータスコード＋カスタムメッセージで終了     | `abort(403, 'アクセスできません');`                                | abort(ステータスコード, 'エラーメッセージ');              |

