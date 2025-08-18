# 住所系
```php
fake()->postcode1();       	// 834（郵便番号3桁）
fake()->postcode2();       	// 8290（郵便番号4桁）
fake()->postcode();       	// 8348290（郵便番号7桁）
fake()->numerify('###-####');	// 834-8290（郵便番号3桁-4桁）
fake()->prefecture();     	// 東京都
fake()->ward();            	// 南区
fake()->city();            	// 鈴木市
fake()->streetAddress();   	// 斉藤町若松8-6-4
fake()->secondaryAddress();   	// ハイツ中村108号

fake()->areaNumber();		// 1～10 の間の値
fake()->buildingNumber();    	// 101～110 の間の値
```

# 個人情報（名前、メルアド、電話）
```php
fake()->name();			// 山田 太郎
fake()->firstName();		// 太郎
fake()->lastName();		// 山田
fake()->lastKanaName();		// ヤマダ
fake()->firstKanaName();		// タロウ or ハナコ（引数に'male', 'female' で性別指定可）
fake()->firstKanaNameMale();	// タロウ
fake()->firstKanaNameFemale();	// ハナコ

fake()->unique()->safeEmail();     // nakamura.ryohei@example.com（重複の無いメルアドで、実在しないドメイン）
fake()->phoneNumber(); 		// 0135-67-7343

fake()->company();  		// 株式会社 伊藤
bcrypt('password');         // パスワード (bcryptがハッシュ化)
```

# 文字列
```php
fake()->realText(10);    	// 日本語対応あり。最小 10～

fake()->sentence(8);            // タイトルなどに（英語）
fake()->paragraph(40);          // 本文などに（英語）
fake()->paragraphs(5, true);    // 改行コード付きの本文などに（英語）

fake()->paragraphs(2, true) // HTMLタグつき文章。複数文を1つの文字列で返す

fake()->regexify('[a-zA-Z0-9]{10}');   // 正規表現を使ったランダムな文字列
\Str::random(10);                            // laravelのヘルパー関数（英数字のみ）。例：「TkO41KdieO」

// 日本語でも改行コードほしい時は、以下。
preg_replace("/。/", "。\n\n", fake()->realText(150));
```

# 数字
```php
fake()->numerify('##');		// 2桁の数字（07 など 0 からスタートもあり）
fake()->numberBetween(1, 10);   // 1～10 の間の数字
fake()->biasedNumberBetween(1, 10, ['\Faker\Provider\Biased', 'linearLow']);
// 'linearLow'で、低い数字の出る確率大。反対は、'linearHigh'
```

# 配列
```php
fake()->randomElement(['a', 'b', 'c', 'd']);       // a～dの中からランダムに1つ
fake()->randomElements(['a', 'b', 'c', 'd'], 2);   // a～dの中からランダムに2つ（重複無し）
```

# 日時
```php
fake()->date('Y-m-d');			// 2002-12-10
fake()->time('H:i:00');			// 23:52:00
fake()->dateTime('now')->format('Y-m-d H:i:s');	  // 1977-08-13 09:40:21
fake()->dateTimeBetween('-3days', '3days')->format('Y-m-d');	// 2019-12-25

// フォーマットの指定が無い時は、DateTime オブジェクトが返る
```

# その他
```php
fake()->colorName();  	// Gold, Fuchsia, AntiqueWhite 等
fake()->url();         	// https://www.yahaoo.co.jp/
fake()->latitude(35.54915506146918, 36.06591802134296);    // 緯度
fake()->longitude(138.96409298125002, 140.30442501250002); // 経度

faker->shuffle() // 文字列や配列をランダムに並べ替える
```

# 修飾子
```php
unique()
// 重複しないようにデータを返す
// 但し、それ以上重複しない値を返せない時は、エラーとなる
fake()->unique()->safeEmail();
fake()->unique()->randomElement([1, 2, 3]);  // 4 回呼び出してしまうとエラー
```

# optional()
```php
// null（デフォルト値）を時折混ぜたい時に便利。
fake()->optional(0.1)->randomElement([1, 2, 3]);  // 90%の確率でnullを返す
```

# uniqueキーを自動生成
```php
fake()->unique()->slug(); //.  uniqueキーを自動生 slug()
```

# 状態を固定 state
```php
public function oda(): static {
    return $this->state(fn(array $attributes) => [ 'name' => '織田信長', ]); // 織田信長という人物を固定化
    }
```

# リレーション系
```php
// belongsTo: 関連モデルを1つ生成
PostFactory.php:
'user_id' => User::factory(),

// hasMany: 関連を複数生成
User::factory()
    ->hasPosts(3) // 投稿を３件持つユーザーを生成
    ->create();

// ネストした関係もOK（User → Post → Comment）
User::factory()
    ->has(Post::factory()->hasComments(5))
    ->create();

[補足]
// 例えばUserモデルに、下記を定義している👇

public function comments()
{
    return $this->hasMany(Comment::class);
}

public function profile()
{
    return $this->hasOne(Profile::class);
}

public function roles()
{
    return $this->belongsToMany(Role::class);
}

// するとFactory.phpで下記のように書ける👇

User::factory()->hasComments(5)->create();
User::factory()->hasProfile()->create();
User::factory()->hasRoles(3)->create(); // belongsToMany でも使える
```

# UUID
```php
// 通常のuuidを生成 (毎回違う)
Str::uuid

// UUIDを固定値としてテストしたいとき
Str::createUuidsUsingSequence([
    'uuid1',
    'uuid2',
    'uuid3',
]);
```

# 補足
```php
// laravel で Faker の日本語設定
APP_FAKER_LOCALE=ja_JP        // .env の場合（Laravel 11～）
'faker_locale' => 'ja_JP',    // config/app.php の場合
```
