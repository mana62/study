# Laravel IDE Helper
➡️ Laravelでの開発をもっと快適にするための補助ツール

---

## できること
- **モデル補完** : $user->name のようなプロパティが補完される
- **ファサード補完** : Route::get() や DB::table() が補完される
- **型チェック** : $user->id が int か string かIDEが理解できる
- **ジャンプ機能** : User::find() からモデル定義にジャンプできる
- **DocBlock生成** : モデルやファサードにPHPDocを自動生成

---

## 手順
#### ①composerでインストール
```bash
composer require --dev barryvdh/laravel-ide-helper
```
→ `--dev`：本番では不要

#### ②補完ファイルを生成
```bash
php artisan ide-helper:generate         # ファサード補完用
php artisan ide-helper:models -N        # モデル補完用
php artisan ide-helper:meta             # IDE用メタファイル生成
```
→ -N はモデルファイルに直接DocBlockを書き込まないオプション

### ③実行タイミング
- 新しいモデルを作ったあと
- カラムを追加・変更したあと（マイグレーション後）
- 新しいファサードを追加したとき


---

## 注意
- 補完ファイルは **自動で更新されない** → モデルやDB構造を変えたら再実行が必要
- 生成ファイルは **本番環境では不要**（--dev）
- 生成時にエラーが出た場合はキャッシュをクリア
  `php artisan optimize:clear`

---

## 参考
- [【保存版】Laravel IDE Helperの完全ガイド：導入から応用まで5つの実践テクニック](https://dexall.co.jp/articles/?p=2614)
- [laravel-ide-helperを使ってみた](https://qiita.com/miriwo/items/5d1e82931ee182f709e2)