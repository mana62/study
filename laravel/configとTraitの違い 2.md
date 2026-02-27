# config と Trait の違い

| 項目     | config/                         | Trait                                |
|----------|----------------------------------|---------------------------------------|
| 目的     | 設定値・定数を保存               | 再利用可能なロジックを共有            |
| 中身     | 配列を返すだけ                   | メソッド（関数）を定義                |
| 使い方   | `config('app.name')`             | `use TraitName;` でクラスに追加       |


```php
┌─────────────────────────────────────────────────┐
│  config/  → 「設定値」を保存する場所           │
│            （数値、文字列、ON/OFFフラグなど）  │
│                                                 │
│  Trait    → 「処理・ロジック」を共有する場所   │
│            （メソッドを複数クラスで使いたい時）│
└─────────────────────────────────────────────────┘
```


### 例
```php
// ❌ config に書くべきでないもの（ロジック）
// config/app.php
return [
    'check_lock' => function($model) { ... }  // ← これはダメ！
];

// ✅ config に書くべきもの（設定値）
// config/app.php
return [
    'ip_filtering_enabled' => true,  // ON/OFF のフラグ
    'pagination_count' => 20,         // ページネーション件数
];
```

```php
// ✅ Trait に書くべきもの（再利用するロジック）
trait HasOptimisticLock
{
    public function updateWithLock($data, $updatedAt) { ... }
}
```