# Scss
➡️ SassはSCSS 記法（主流）と Sass 記法（非推奨）の２種類がある。現在はSCSSが主流
- SCSS では 変数・ネスト・mixin・関数・条件分岐・ループ が使える
- 保守性と再利用性が大幅に向上
- インポートは @use / @forward が推奨

---

## ① インストール

```bash
npm install -g sass
```

## ② Sass ファイル作成
- .scss または .sass 拡張子で保存
- 推奨は .scss

## ③ Sass ファイルをコンパイル（CSSに変換）
```bash
sass styles.scss styles.css
```

## ④ リアルタイムコンパイル
- styles.scss が更新されるたびに styles.css が自動で更新される

```bash
sass --watch styles.scss:styles.css
```

---

# SCSS の機能

## ① 変数
- 色やフォントサイズなどを変数として定義可能

```scss
$primary-color: #3498db;
$font-stack: Helvetica, sans-serif;

body {
  color: $primary-color;
  font-family: $font-stack;
}
```

## ② ネスト（入れ子構造）
```scss
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li {
    display: inline-block;
  }

  a {
    text-decoration: none;
    color: $primary-color;

    &:hover {
      color: darken($primary-color, 10%);
    }
  }
}
```

## ③ ミックスイン（mixin）
```scss
@mixin button-styles($color) {
  background-color: $color;
  padding: 10px;
  border-radius: 5px;
}

button {
  @include button-styles(#3498db);
}

.alert-button {
  @include button-styles(#e74c3c);
}
```

## ④ 継承（extend）
```scss
%button-base {
  padding: 10px;
  border-radius: 5px;
}

.button {
  @extend %button-base;
  background-color: #3498db;
}

.alert-button {
  @extend %button-base;
  background-color: #e74c3c;
}
```

## ⑤ 関数
```scss
@function calculate-percentage($partial, $total) {
  @return ($partial / $total) * 100%;
}

.container {
  width: calculate-percentage(300px, 1200px); // 25%
}
```

**色操作の組み込み関数**
- `lighten($color, $percentage)` → 明るくする
- `darken($color, $percentage)` → 暗くする
- `saturate($color, $percentage)` → 彩度を上げる
- `desaturate($color, $percentage)` → 彩度を下げる
- `mix($color1, $color2, $weight)` → 色を混ぜる

## ⑥ 演算
```scss
$base-padding: 10px;
$double-padding: $base-padding * 2;

.element {
  padding: $double-padding; // 20px
}
```

```scss
$base-color: #3498db;
$light-color: lighten($base-color, 20%);

.element {
  background-color: $light-color; // 明るい青
}
```

## ⑦ リスト
```scss
$font-sizes: 12px 14px 16px;
$colors: red, green, blue;

.element {
  font-size: nth($font-sizes, 2); // 14px
  color: nth($colors, 3); // blue
}
```

## ⑧ マップ（連想配列）
```scss
$theme-colors: (
  primary: #3498db,
  secondary: #2ecc71,
  danger: #e74c3c
);

.element {
  background-color: map-get($theme-colors, primary); // #3498db
}
```

## ⑨ 条件分岐とループ

**条件分岐**
```scss
@mixin respond-to($device) {
  @if $device == mobile {
    @media (max-width: 768px) { @content; }
  } @else if $device == desktop {
    @media (min-width: 1024px) { @content; }
  }
}

@include respond-to(mobile) {
  body {
    background-color: lightgray;
  }
}
```

**ループ**
```scss
@for $i from 1 through 3 {
  .column-#{$i} {
    width: 100% / $i;
  }
}
```

## ⑩ 自作関数
```scss
@function calculate-rem($px-value) {
  @return $px-value / 16px * 1rem;
}

.element {
  font-size: calculate-rem(32px); // 2rem
}
```

## ⑪ インポート（ファイル分割）

**@use / @forward**
```scss
// _variables.scss
$primary-color: #3498db;

// main.scss
@use 'variables';

body {
  color: variables.$primary-color;
}
@use : 変数や mixin を読み込む

@forward : 他のファイルに公開用として渡す
```

## ⑫ レスポンシブデザイン
```scss
$breakpoints: (
  mobile: 480px,
  tablet: 768px,
  desktop: 1024px
);

@mixin respond-to($breakpoint) {
  @media (max-width: map-get($breakpoints, $breakpoint)) {
    @content;
  }
}

.container {
  width: 100%;

  @include respond-to(tablet) {
    width: 50%;
  }

  @include respond-to(mobile) {
    width: 100%;
  }
}
```