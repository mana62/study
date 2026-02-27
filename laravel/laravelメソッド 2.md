# larvelメソッド

---

| メソッド・関数         | 第一引数（入力）                            | 第二引数（出力） / 説明                          | 基本構文                                               | 使用例                                                           |
| ---------------------- | ------------------------------------------- | ------------------------------------------------ | ------------------------------------------------------ | ---------------------------------------------------------------- |
| `parse`                | 日付文字列（例:"2023-10-01 10:00:00"）      | `Carbon` オブジェクトに変換                      | `Carbon::parse(文字列);`                               | `Carbon::parse('2023-10-01 10:00:00');`                          |
| `whereHas`             | リレーション名                              | 条件を満たすリレーションが存在するレコードを取得 | `Model::whereHas('relation', fn($q)=>...)->get();`     | `Model::whereHas('posts', fn($q)=>$q->where(...))->get();`       |
| `use`                  | クロージャで使いたい変数                    | クロージャ内で外部変数が使える                   | `fn() use ($変数)`                                     | `function () use ($date)`                                        |
| `whereDate`            | カラム名、日付文字列                        | 指定した日付と一致するレコードを取得             | `Model::whereDate('カラム名', '日付')->get();`         | `Model::whereDate('created_at', '2023-10-01')->get();`           |
| `with`                 | リレーション名                              | リレーションをEagerロード                        | `Model::with('relation')->get();`                      | `Model::with('user')->get();`                                    |
| `diffInMinutes`        | 開始日時（Carbon）                          | 終了日時との分差を返す                           | `$start->diffInMinutes($end);`                         | `$start->diffInMinutes($end);`                                   |
| `reduce`               | コールバック関数                            | 配列やコレクションを集約して1つの値に            | `$collection->reduce(fn($carry, $item)=>..., 初期値);` | `$collection->reduce(fn($carry, $item)=>$carry+$item->value);`   |
| `floor`                | 数値                                        | 小数点以下を切り捨てた整数                       | `floor(数値);`                                         | `floor($number / 60);`                                           |
| `firstOrNew`           | 条件配列、デフォルト配列                    | 存在しない場合は新インスタンスを返す             | `Model::firstOrNew([条件], [デフォルト]);`             | `Model::firstOrNew(['email' => $email]);`                        |
| `whereNull`            | カラム名                                    | NULLのレコードのみ取得                           | `Model::whereNull('カラム')->get();`                   | `Model::whereNull('deleted_at')->get();`                         |
| `Auth::attempt`        | emailとpassword                             | ログインに成功すればtrue                         | `Auth::attempt([email, password]);`                    | `Auth::attempt(['email' => ..., 'password' => ...]);`            |
| `isToday`              | Carbonインスタンス                          | 今日かどうかを判定                               | `$carbon->isToday();`                                  | `$date->isToday();`                                              |
| `isset`                | 任意の変数                                  | 変数がセットされていればtrue                     | `isset($変数);`                                        | `if (isset($variable)) { ... }`                                  |
| `subDays`              | 日数                                        | 指定日数だけ過去の日付を返す                     | `$carbon->subDays(日数);`                              | `$date->subDays(3);`                                             |
| `str_pad`              | 元文字列、長さ、埋め文字、方向              | 指定の長さまで文字を埋める                       | `str_pad(文字列, 長さ, 埋める文字, STR_PAD_方向);`     | `str_pad(5, 2, '0', STR_PAD_LEFT);`                              |
| `data_get()`           | オブジェクトまたは配列、キー階層            | ネストされたキーに安全にアクセス                 | `data_get($データ, 'キー');`                           | `data_get($user, 'profile.name')`                                |
| `to_route()`           | ルート名、引数配列、ステータスコード        | 指定のルートにリダイレクト                       | `to_route('ルート名', [...], ステータスコード);`       | `return to_route('home');`                                       |
| `fresh()`              | モデルインスタンス                          | DBの最新状態で上書き                             | `$model->fresh();`                                     | `$model->fresh();`                                               |
| `refresh()`            | モデルインスタンス                          | モデルを再読み込み                               | `$model->refresh();`                                   | `$model->refresh();`                                             |
| `cursor()`             | モデルクエリ                                | メモリを抑えつつ大量データを1件ずつ取得          | `Model::cursor()->each(fn...);`                        | `Model::cursor()->each(fn($item)=>...)`                          |
| `abort()`              | ステータスコード、メッセージ                | 即座にレスポンス終了                             | `abort(コード, 'メッセージ');`                         | `abort(404, 'ページが見つかりません');`                          |
| `abort_if()`           | 条件、コード、メッセージ                    | 条件がtrueならabort                              | `abort_if(条件, コード, 'メッセージ');`                | `abort_if(!$user, 403, '権限がありません');`                     |
| `abort_unless()`       | 条件、コード、メッセージ                    | 条件がfalseならabort                             | `abort_unless(条件, コード, 'メッセージ');`            | `abort_unless($user->isAdmin(), 403);`                           |
| `optional()`           | オブジェクト                                | nullを許容しつつプロパティにアクセス             | `optional($obj)->プロパティ`                           | `optional($user)->name`                                          |
| `tap()`                | 任意の値、クロージャ                        | 値を返しつつクロージャの中で何か処理             | `tap($value, fn($v)=>...)`                             | `tap($user, fn($u)=>$u->update([...]))`                          |
| `collect()`            | 配列、コレクション                          | Collectionとして扱える                           | `collect([...])`                                       | `collect([1, 2, 3])->sum();`                                     |
| `value()`              | カラム名                                    | 単一の値だけ取得                                 | `Model::where()->value('カラム名')`                    | `User::where('id',1)->value('email')`                            |
| `pluck()`              | カラム名                                    | 指定カラムの値を配列で取得                       | `Model::pluck('カラム名')`                             | `User::pluck('email')`                                           |
| `dump()`               | 任意の変数                                  | 中身を表示（デバッグ）                           | `dump($変数);`                                         | `dump($user);`                                                   |
| `dd()`                 | 任意の変数                                  | dump & die。処理を停止                           | `dd($変数);`                                           | `dd($user);`                                                     |
| `has()`                | キー名                                      | キーが存在するかチェック                         | `$request->has('email')`                               | `$request->has('password')`                                      |
| `filled()`             | キー名                                      | 値が空でない（nullや空文字列でない）かチェック   | `$request->filled('name')`                             | `$request->filled('email')`                                      |
| `when()`               | 条件、true時の処理、false時の処理（省略可） | 条件付きで処理を実行                             | `$query->when($条件, fn($q)=>...)`                     | `$query->when($request->filled('name'), fn($q)=>$q->where(...))` |
| `unless()`             | 条件、true時の処理                          | 条件がfalseの時だけ処理を実行                    | `unless($条件, fn()=>...)`                             | `unless(empty($name), fn()=>...)`                                |
| `blank()`              | 任意の値                                    | 空・null・空配列・空文字列などを判定             | `blank($value)`                                        | `blank('') // true`                                              |
| `filled()`             | 任意の値                                    | blankの逆、値があるかどうかを確認                | `filled($value)`                                       | `filled('Laravel') // true`                                      |
| `$model->touch`        | モデルインスタンス                          | `updated_at`を更新する                           | `$model->touch();`                                     | `$post->touch();`                                                |
| **自作クエリスコープ** | モデルに定義したスコープ関数                | よく使う条件をまとめて再利用できる               | `public function scopeName($query){...}`               | `PlanDetail::ordered()->get();`                                  |

---

## Request情報取得
| メソッド        | 意味・使い方                    | 使用例                     |
| --------------- | ------------------------------- | -------------------------- |
| `getHost()`     | ホスト名（ドメイン）を取得      | `example.com`              |
| `getScheme()`   | スキーム（http or https）を取得 | `https`                    |
| `getPort()`     | ポート番号を取得                | `443`                      |
| `getHttpHost()` | ホスト＋ポートを取得            | `example.com:443`          |
| `getUri()`      | フルURLを取得                   | `https://example.com/path` |

---

## Session系
| メソッド                               | 意味・使い方                                                                                  |
| -------------------------------------- | --------------------------------------------------------------------------------------------- |
| `session()->get('キー', デフォルト値)` | 値を取り出す。なければデフォルト値を返す。<br>例：`session()->get('user_id', 0)`              |
| `session()->put('キー', 値)`           | 値を保存する。セッションにデータをセット。<br>例：`session()->put('step', 2)`                 |
| `session()->flash('キー', 値)`         | 一時保存する。次のリクエストまでだけ保持。<br>例：`session()->flash('error', '失敗しました')` |
| `session()->has('キー')`               | 存在チェック。そのキーがあるかどうか。<br>例：`if (session()->has('token'))`                  |
""")

