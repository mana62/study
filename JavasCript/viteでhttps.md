# Laravel + Vite の開発で「httpsでローカル環境を動かす」

## 使用技術
- Laravel: Caddy
- JS/CSS: Vite

---

## Caddy設定
### ①compose.yml設定
```yml
  app:
    build:
      context: ./docker/php
      dockerfile: Dockerfile
    container_name: suq-app
    restart: unless-stopped
    working_dir: /var/www
    volumes:
      - ./src:/var/www
    ports:
      - "8000:8000"  # Laravel
      - "5173:5173"  # Vite
    depends_on:
      - db

  caddy:
    image: caddy:2.10.2
    container_name: suq-caddy
    restart: unless-stopped
    ports:
      - "80:80"      # HTTP
      - "443:443"    # HTTPS
    volumes:
      - ./src:/var/www # ソースコード
      - ./docker/caddy/Caddyfile:/etc/caddy/Caddyfile  # 設定ファイル
      - caddy_data:/data  # 証明書保存
      - caddy_config:/config    # 設定保存
    depends_on:
      - app

  volumes:
    caddy_data:   # Caddy証明書保存用
      driver: local
    caddy_config: # Caddy設定保存用
      driver: local
```

### ②Caddyfileを用意

```bash
    #http / https両方対応
localhost, https://localhost {
    # 公開ディレクトリを指定
    root * /var/www/public
    # PHPのリクエストをPHP-FPMに転送
    php_fastcgi app:9000
    # 静的ファイルをそのまま返す
    file_server
    # laravelルーティング用
    try_files {path} {path}/ /index.php
}
```

---

## Viteの設定
→ Viteの開発サーバー（npm run dev で起動するやつ）はデフォルトだと http（暗号化なし） <br>
→ そのためvite.config.js に SSL 設定を追加が必要

### ①SSL証明書を作る（ローカル用）
- 簡単なのは `Laravel Valet` か `mkcert`

#### 【方法A】Laravel Valetを使ってる人（Mac向け）
```bash
valet secure
```
- これでローカルの https 環境が有効になる
  - → この場合、vite.config.js の変更は不要

#### 【方法B】mkcertで自分用証明書を作る（どんな環境でもOK）(エムケーサート)
**①インストール**
```bash
brew install mkcert
brew install nss # Firefoxを使う人だけ
```

**②証明書を生成**
```bash
mkcert -install
mkcert localhost
```

**③これで以下のファイルが作られる**
```bash
localhost.pem         ← 証明書
localhost-key.pem     ← 鍵ファイル
```

**④上記２つをDockerコンテナから使える場所に置く**
- たとえばプロジェクトルートの docker/nginx/certs/ に置いておく
```bash
project/
  ├─ docker/
  │   └─ nginx/
  │       └─ certs/
  │           ├─ localhost.pem
  │           └─ localhost-key.pem
```


### ②vite.config.js を編集
```bash
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';
import fs from 'fs'; // ← 追加

export default defineConfig({
  server: {
    https: {
      key: fs.readFileSync('./localhost-key.pem'),
      cert: fs.readFileSync('./localhost.pem'),
    },
    host: 'localhost',
    port: 5173, // 好きなポート番号でOK
  },
  plugins: [
    laravel({
      input: ['resources/css/app.css', 'resources/js/app.js'],
      refresh: true,
    }),
  ],
});
```

### ③起動
```bash
docker compose up -d --build
npm run dev
```

### ④下記が出力されればOK
```bash
VITE v5.x.x  ready in 100ms
➜  Local:   https://localhost:5173/
```

### ④.env でAPP_URLを https にしておく
```bash
APP_URL=https://localhost
```

---

## よくあるエラー
| エラー内容                                                              | 原因・解決                                             |
| ------------------------------------------------------------------ | ------------------------------------------------- |
| `Error: ENOENT: no such file or directory, open './localhost.pem'` | → ファイルパスが間違ってる。vite.config.js の相対パスを確認。           |
| ブラウザで「この接続は安全ではありません」                                              | → mkcert が正しくインストールされていない。`mkcert -install` で再登録。 |
| `ERR_CERT_AUTHORITY_INVALID`                                       | → 自己署名証明書なので、最初だけブラウザで「許可」をクリックすればOK。             |

---

## 補足
### Laravel Valet とは？
→ Laravel Valet = Mac用の軽量ローカルサーバー、Apache や Docker の代わりに、Mac上で PHP サイトを簡単に動かせるツール

### Valet を使うと何ができるか
- valet link するだけでローカルURLができる
- 自動で https対応（証明書を自動発行）
- valet secure を実行すればSSL化OK
- Nginx + DnsMasq + PHP + Composer を自動でいい感じに構成してくれる（Dockerより軽い）

→ Macの任意のLaravelプロジェクトをただのフォルダとして置いておくだけで、`https://プロジェクト名.test`で即アクセスできる

### Valet をインストールして使う流れ(Mac 専用)
**①Homebrew があるか確認**
```bash
brew -v
```

**②Valet をインストール**
```bash
composer global require laravel/valet
```

**③Valet を起動**
```bash
valet install
```

**④プロジェクトを「リンク」**
```bash
valet link
```
→ 自動でドメインが作られる（https://myapp.test）

**⑤HTTPS対応したい場合**
```bash
valet secure
```
→ https://myapp.test が有効になる（証明書は Valet が自動で作ってくれるので、Vite の設定変更すら不要）

---

## Firefox とは？
→ Webブラウザのひとつ、Chrome や Safari と同じように、Webサイトを表示するソフト
**Firefox だけは、WindowsやMacの「共通証明書ストア」を使わず、独自の証明書ストア（NSS）を自分で持っている**

## NSS（Network Security Services）とは？
→ Firefox が使っている独自の「証明書管理システム」 の名前

| ブラウザ                   | 証明書をどこで管理してる？          |
| ---------------------- | ---------------------- |
| Chrome / Safari / Edge | OS（MacやWindows）の証明書ストア |
| Firefox                | 独自の「NSSストア」            |

---

## まとめ

| 用語                              | 意味                     | 関係                       |
| ------------------------------- | ---------------------- | ------------------------ |
| Firefox                         | ブラウザ                   | 独自の証明書ストアを使っている          |
| NSS (Network Security Services) | Firefoxが証明書を管理する仕組み    | mkcertが証明書を登録するために必要     |
| mkcert -install                 | システム全体に開発用証明書を登録するコマンド | NSSを入れておけばFirefoxにも登録される |


- ChromeやSafariだけで開発 するなら、`brew install nss` は不要
- Firefoxでも `https://localhost` を開きたい」という場合は、`brew install nss` を入れておく