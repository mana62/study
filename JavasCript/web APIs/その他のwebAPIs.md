# その他の Web APIs

| API名 | 綴り | 使用頻度の感覚 | 何に使う？ |
|-------|------------|----------------|------------|
| window.getSelection | getSelection | ★★☆☆☆ | 選択文字列の取得 |
| IntersectionObserver | IntersectionObserver | ★★★★★ | スクロール監視、LazyLoad |
| PaymentRequest | PaymentRequest | ★☆☆☆☆ | Web決済（対応サイトのみ） |
| Web Crypto | Web Crypto API | ★★★★☆ | 暗号化、ハッシュ、鍵生成 |
| Pointer Lock | Pointer Lock API | ★☆☆☆☆ | FPSゲームなどでマウス固定 |
| Fullscreen | Fullscreen API | ★★★☆☆ | 全画面表示 |
| Web Audio | Web Audio API | ★★★★☆ | 音声処理、ゲーム、音楽アプリ |
| Service Worker | Service Worker | ★★★★★ | PWA、オフライン対応 |
| Push API | Push API | ★★★☆☆ | プッシュ通知 |
| Web Notifications | Notification API | ★★★☆☆ | 通知表示 |
| Vibration API | Vibration API | ★☆☆☆☆ | スマホのバイブ（対応少ない） |
| Sensor API | Generic Sensor API | ★☆☆☆☆ | 加速度・ジャイロなど |



## よく使われる（Web開発で出番多い）
**IntersectionObserver**
→ LazyLoad、無限スクロール、アニメーション発火など

**Service Worker**
→ PWA、オフライン対応、キャッシュ

**Web Crypto API**
→ ハッシュ、署名、暗号化

**Web Audio API**
→ 音声処理、ゲーム、音楽アプリ

これらは現代の Web 開発でかなり重要

## 用途次第で使われる
**Fullscreen API**
**Push API**
**Notification API**
**getSelection**（テキストエディタ系で使う）

## かなりニッチ（特定用途のみ）
**PaymentRequest API**（対応サイトが少ない）
**Pointer Lock API**（FPSゲームなど）
**Vibration API**（スマホ限定、対応ブラウザ少ない）
**Sensor API**（セキュリティ制限が強い）

## まとめ
- 全部 Web APIs だけど、使用頻度はバラバラ
- IntersectionObserver
- Service Worker
- Web Crypto
- Web Audio
👉 このあたりは 現代の Web 開発でよく使われる重要 API

逆に、
- Vibration
- Pointer Lock
- PaymentRequest
- Sensor API
は 特定のアプリでしか使われない