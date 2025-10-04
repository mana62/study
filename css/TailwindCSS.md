# Tailwind CSS
➡️ class名を作り、CSSファイルに書き込まなくてもHTML内にclass＝でCSSを作成することができる

---

## インストールの流れ

### ①NPMとTailwind CSSのインストール（Laravel + Vite + Tailwind）

```jsx
npm install tailwindcss postcss autoprefixer vite laravel-vite-plugin
npx tailwindcss init
```

### ②tailwind.config.jsの設定

- `content`オプションが正しいファイルパスを指しているか確認

```bash
# tailwind.config.js
export default {
  content: [
    './resources/views/**/*.blade.php',
    './resources/js/**/*.vue',
    './resources/js/**/*.js',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### ③postcss.config.jsの設定

- プロジェクトのルートディレクトリに`postcss.config.js`ファイルを作成

```bash
# postcss.config.js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

### ④CSS ファイルの作成と Tailwind のインポート

- `resources/css/app.css`ファイルを作成し、Tailwind CSSのディレクティブを追加

```bash
# resources/css/app.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### `⑤vite.config.js`の作成

- プロジェクトのルートディレクトリに`vite.config.js`ファイルを作成

```jsx
// vite.config.js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
  plugins: [
    laravel({
      input: ['resources/css/app.css', 'resources/js/app.js'],
      refresh: true,
    }),
  ],
});
```

### `⑥package.json`ファイルにVite用のスクリプトを追加

```bash
# package.json の scripts
"scripts": {
  "dev": "vite",
  "build": "vite build"
}
```

### ⑦`resources/views/layouts/app.blade.php`を編集

```jsx
// resources/views/layouts/app.blade.php
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Laravel App</title>
  @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
<body class="bg-gray-100">
  <div class="container mx-auto">
    @yield('content')
  </div>
</body>
</html>
```

### ⑨Viteの開発サーバーを起動

- 開発サーバー起動

```bash
npm run dev
```

- 本番ビルドの場合

```bash
npm run production
```

### ⑦ブレードテンプレートでのCSSの読み込み

- `resources/views/layouts/app.blade.php`などのレイアウトファイルで、生成されたCSSファイルを読み込む

```jsx
<!DOCTYPE html>
<html>
<head>
<title>My Laravel App</title>
<link href="{{ mix('css/app.css') }}" rel="stylesheet">
</head>
<body>
<div class="container">
@yield('content')
</div>
<script src="{{ mix('js/app.js') }}"></script>
</body>
</html>
```

### ⑧Tailwind CSS のクラスを使用

- `resources/views/welcome.blade.php`などのBladeテンプレートファイルで、Tailwind CSSのユーティリティクラスを使用

```jsx
@extends('layouts.app')
@section('content')
<div class="text-center">

<p class="mt-4 text-gray-600">This is a simple example of using Tailwind CSS with Laravel.</p>
</div>
@endsection
```

- `http://localhost:5173/`にアクセスし、Tailwind CSSクラスが正しく適用されているか確認

---

## Tailwind CSSのクラス

### レイアウト

**コンテナ**

```html
<div class="container mx-auto"></div>
```

**フレックスボックス**

```html
<div class="flex">
<div class="flex-1"></div>
<div class="flex-1"></div>
</div>
```

**グリッド**
```html
<div class="grid grid-cols-3 gap-4">
<div class="col-span-1"></div>
<div class="col-span-1"></div>
<div class="col-span-1"></div>
</div>
 ```

---

### スペーシング

**マージン**
```html
    <div class="m-4"></div>
    <div class="mt-4"></div>
    <div class="mr-4"></div>
    <div class="mb-4"></div>
    <div class="ml-4"></div>
```

**パディング**
```html
    <div class="p-4"></div>
    <div class="pt-4"></div>
    <div class="pr-4"></div>
    <div class="pb-4"></div>
    <div class="pl-4"></div>
```

---

### サイズ

**幅**
```html
    <div class="w-1/2"></div>
    <div class="w-full"></div>
    <div class="w-screen"></div>
```

**高さ**
```html
    <div class="h-1/2"></div>
    <div class="h-full"></div>
    <div class="h-screen"></div>
```

---

### テキスト

**フォントサイズ**
```html
    <div class="text-xs"></div>
    <div class="text-sm"></div>
    <div class="text-lg"></div>
    <div class="text-xl"></div>
    <div class="text-2xl"></div>
    <div class="text-3xl"></div>
```

**フォントウェイト**
```html
    <div class="font-thin"></div>
    <div class="font-light"></div>
    <div class="font-normal"></div>
    <div class="font-medium"></div>
    <div class="font-bold"></div>
    <div class="font-extrabold"></div>
```

**テキスト色**
```html
    <div class="text-gray-500"></div>
    <div class="text-red-500"></div>
    <div class="text-blue-500"></div>
    <div class="text-green-500"></div>
```

---

### 背景

**背景色**
```html
    <div class="bg-gray-500"></div>
    <div class="bg-red-500"></div>
    <div class="bg-blue-500"></div>
    <div class="bg-green-500"></div>
```
**背景画像**
```html
    <div class="bg-cover"></div>
    <div class="bg-contain"></div>
    <div class="bg-center"></div>
```
---

### ボーダー

**ボーダーの幅**
```html
    <div class="border-2"></div>
    <div class="border-t-2"></div>
    <div class="border-r-2"></div>
    <div class="border-b-2"></div>
    <div class="border-l-2"></div>
```

**ボーダーの色**
```html
    <div class="border-gray-500"></div>
    <div class="border-red-500"></div>
    <div class="border-blue-500"></div>
    <div class="border-green-500"></div>
```
---

### シャドウと円角

**シャドウ**
```html
    <div class="shadow"></div>
    <div class="shadow-md"></div>
    <div class="shadow-lg"></div>
    <div class="shadow-xl"></div>
```

**円角**
```html
    <div class="rounded"></div>
    <div class="rounded-md"></div>
    <div class="rounded-lg"></div>
    <div class="rounded-full"></div>
```
---

### ショートカットの説明

- **`r + Enter`**: サーバーを再起動。設定の変更やエラーがある場合、再起動することで問題が解決することがある。
- **`u + Enter`**: サーバーのURLを表示。現在のサーバーのURLを再確認するために使用。
- **`o + Enter`**: ブラウザでサーバーURLを開く。自動的にブラウザが開かれるので、手動でURLを入力する手間を省ける。
- **`c + Enter`**: コンソールをクリア。コンソールログを見やすくするために使用。
- **`q + Enter`**: サーバーを終了。開発サーバーを停止したい場合に使用。