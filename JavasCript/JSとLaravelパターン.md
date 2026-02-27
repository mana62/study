# JS / Laravel パターン

# JS
```js
// 1. fetch + async/await
load.addEventListener('click', async function() {
  const res = await fetch('URL');
  const data = await res.json();
});

// 2. querySelectorAll + forEach
const items = document.querySelectorAll('li');
items.forEach(item => { ... });

// 3. querySelectorAll + for...of
for (const item of items) { ... }

// 4. createElement + appendChild
const li = document.createElement('li');
li.textContent = 'text';
parent.appendChild(li);

// 5. addEventListener + remove
button.addEventListener('click', function() {
  element.remove();
});

// 6. 配列から選択
const selected = array.find(item => item.id === selectedId);

// 7. ランダム整数
Math.floor(Math.random() * (max - min + 1)) + min

// 8. 配列フィルター
array.filter(item => item.name.includes(keyword))

// 9. localStorage
localStorage.setItem('key', 'value');
const value = localStorage.getItem('key');

// 10. try-catch-finally
try {
  // 処理
} catch(error) {
  // エラー処理
} finally {
  // 必ず実行
}
```


# Laravel
```php
// 1. withSum + having
User::withSum('orders', 'total')
    ->having('orders_sum_total', '>=', 10000)
    ->get();

// 2. withSum（条件付き）
User::withSum([
    'orders' => function($q) {
        $q->where('status', 'completed');
    }
], 'total')
->having('orders_sum_total', '>=', 10000)
->get();

// 3. withCount + having
User::withCount('posts')
    ->having('posts_count', '>=', 10)
    ->get();

// 4. withAvg + having
User::withAvg('orders', 'total')
    ->having('orders_avg_total', '>=', 5000)
    ->get();

// 5. whereHas（条件）
User::whereHas('posts', function($q) {
    $q->where('published', true);
})->get();

// 6. whereHas（件数）
User::whereHas('posts', function($q) {
    $q->where('published', true);
}, '>=', 5)->get();

// 7. whereMonth + whereYear
Post::whereMonth('created_at', now()->month)
    ->whereYear('created_at', now()->year)
    ->get();

// 8. whereBetween（先週）
Post::whereBetween('created_at', [
    Carbon::now()->subWeek()->startOfWeek(),
    Carbon::now()->subWeek()->endOfWeek()
])->get();

// 9. whereIn + where
Product::whereIn('category_id', [1, 2, 3])
        ->where('stock', '>', 0)
        ->get();

// 10. orderByDesc + take
User::withSum('orders', 'total')
    ->orderByDesc('orders_sum_total')
    ->take(10)
    ->get();
```

