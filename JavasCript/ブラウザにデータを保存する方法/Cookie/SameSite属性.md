# SameSite属性
👉 **別サイトからのリクエストでCookieを送るかどうかを決める属性**
つまり: 他のサイトからアクセスされたとき Cookieを送る？送らない？ を決める

## なぜこの仕組みが必要？（CSRF攻撃）
例:
⚫︎銀行サイト `bank.com`

自分はログインしているので `session=abc123` というCookieを持っている

⚫︎攻撃サイト `trap.com`

このサイトにこんなコードがある
`bank.com/transfer?money=10000`

自分が `trap.com` を開くと

ブラウザは `Cookie: session=abc123` を 自動で `bank.com` に送る

銀行は **本人の操作だ** と思って勝手に送金される

👉 これが **CSRF攻撃**

## SameSiteはこれを防ぐ
SameSiteは **「他サイトからのリクエストでCookie送る？」** を決める

## SameSiteの3種類
### ① None（危険）
```bash
SameSite=None
```
結果:
| 状況     | Cookie |
| ------ | ------ |
| 他サイトから | 送る     |

👉 **常に送る**
**※この場合 Secure必須**
```bash
SameSite=None; Secure
```

### ② Strict（最も安全）
```bash
SameSite=Strict
```

結果:
| 状況    | Cookie |
| ----- | ------ |
| 同じサイト | 送る     |
| 他サイト  | 送らない   |

👉 **他サイトからは完全ブロック**

### ③ Lax（よく使われる）
```bash
SameSite=Lax
```
結果:
| 状況                 | Cookie |
| ------------------ | ------ |
| 同じサイト              | 送る     |
| 他サイトリンククリック        | 送る     |
| 他サイトのAPI・画像・script | 送らない   |

👉 普通のリンクだけOK

---
**None**
```bash
trap.com → bank.com
Cookie送る
```
**Strict**
```bash
trap.com → bank.com
Cookie送らない
```
**Lax**
```bash
trap.com → bank.com
リンククリック → OK
script / fetch → NG
```

## ブラウザのデフォルト
多くのブラウザ
```bash
SameSite=Lax
```
つまり: **特に設定しなくてもCSRF対策がある程度されている**

## 実務でよくある設定
ログインCookie
```bash
Set-Cookie: session=abc123; Secure; HttpOnly; SameSite=Lax
```

意味:
- Secure → HTTPSのみ
- HttpOnly → JS禁止
- SameSite → CSRF対策

## まとめ
| 属性       | 目的       |
| -------- | -------- |
| Secure   | HTTPSだけ  |
| HttpOnly | JSから触れない |
| SameSite | 他サイト攻撃防止 |
