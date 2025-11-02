# Laravel Boost
→ Laravel開発に特化したAI支援ツールキット (dd() や dump() の役割をAIが強化した版)

---

## 導入方法
```bash
composer require laravel/boost --dev
php artisan boost:install
```
→ .env に AIプロバイダーのAPIキー（OpenAIなど）を設定が必要

```bash
AI_PROVIDER=openai
AI_KEY=sk-xxxxxxxxxxxxxxxxxxxx
AI_MODEL=gpt-4
```

- **AI_PROVIDER**：利用するAIプロバイダー名
- **AI_KEY**：プロバイダーから取得したAPI キー
- **AI_MODEL**：利用するモデル名（省略可能。デフォルトで設定されることもある）

---

## できること
- 変数の中身を渡すと、AIがその内容を分析して次に何をすべきかを提案してくれる

```bash
debug()->suggest($user);
```
→ $user に空のプロパティがあれば「このフィールドが未設定です」と教えてくれる

---

## 使える場面
- バグの原因がわからないとき
- データ構造が複雑なとき

---

## 注意
- 本番環境では使わない

---

## 参考
- [Laravel Boost のセットアップ手順と機能まとめ](https://zenn.dev/fusic/articles/f364a82926a377)
- [Laravel Boost 超入門](https://speakerdeck.com/fire_arlo/laravel-boost-chao-ru-men)
- [Laravel Boost(Laravel公式MCP)をVSCode + GitHub Copilot + Docker環境で使ってみた](https://qiita.com/dialog-riku/items/23ff34f7b0b89a5ff742)