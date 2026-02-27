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

## ViteをHTTPS対応する方法
### 方法① 「Laravel Valetを使用する (Macのみ)」
- Dockerの代わりに、Mac上で PHP サイトを簡単に動かせるツール

#### できること
- valet link するだけでローカルURLができる
- 自動で https対応（証明書を自動発行）
- valet secure を実行すればSSL化OK
- Nginx + DnsMasq + PHP + Composer を自動でいい感じに構成してくれる（Dockerより軽い）

---

### 方法② 「mkcertで自分用証明書を作る」
#### 流れ
- ①インストール
```bash
brew install mkcert
brew install nss # Firefoxを使う人だけ
```
- ②証明書を生成
```bash
mkcert -install
mkcert localhost
```
- ③下記ファイルが作られる
```bash
localhost.pem         # 証明書
localhost-key.pem     # 鍵ファイル
```
- ④上記をDockerコンテナから使える場所に置く
- ⑤vite.config.js を編集・追加
- ⑥.env でAPP_URLを https にしておく

#### でできること
- HTTPS化できる
- 複数ドメインに対応できる

---
### 方法③ 「Caddy経由のリバースプロキシ設定」
#### 流れ
- ①Caddyfileにプロキシを追記
```bash
# Vite dev server へのプロキシ
    @vite {
        path /@vite/*
        path /resources/*
    }
    # リクエストをViteのサーバーに転送
    reverse_proxy @vite http://host.docker.internal:5173
```

- ②src/vite.config.jsに、server設定を追記
```bash
server: {
        # dockerからアクセスできるようにする
        host: '0.0.0.0',
        port: 5173,
        # ポート固定
        strictPort: true,
        # HMR設定(ホットリロード)
        hmr: {
            # WebSocket接続先をCaddyのurlに
            host: 'localhost',
            # 通信方式(wss=https, ws=http)
            protocol: 'ws',
        },
        watch: {
            # ファイル更新検知(docker)
            usePolling: true,
        },
    },
```
- ③.envにURLを記載

#### どのような動きか
- Caddyに全部 HTTPSを任せる
     → Vite は HTTP（localhost:5173）で動かす
     → Caddyが `https://localhost` で `/resources` や `/@vite` のリクエストを Vite に代理で送る   (`/resources/*` や `/@vite/*` は Vite に転送（＝ CaddyにViteを通す)

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

---

## 参考
- [Laravel 9 + VITEの開発環境をdockerで実現する方法](https://qiita.com/hitotch/items/aa319c49d625c2a9b65e)
- [LaravelとViteでVue.jsのCSSが反映されない問題の解決手順](https://zenn.dev/milky/articles/docker-php-vite)
- [Caddy サーバー リバース プロキシ SSL 無限ループ #2563](https://github.com/vitejs/vite/discussions/2563)
- [【Laravel】Caddyでローカルhttps開発する【Vite】](https://www.masaakiota.net/2025/02/11/%e3%80%90laravel%e3%80%91caddy%e3%81%a7%e3%83%ad%e3%83%bc%e3%82%ab%e3%83%abhttps%e9%96%8b%e7%99%ba%e3%81%99%e3%82%8b/#mkcert%E3%81%A7%E8%87%AA%E5%89%8D%E3%81%A7%E7%94%A8%E6%84%8F%E3%81%99%E3%82%8B)
- [Caddyで独自ドメインのワイルドカードSSL証明書に対応したリバースプロキシを構築する](https://www.ritaiz.com/articles/steps-setup-caddy-reverse-proxy-with-wildcard-certificate)
- [リバースプロキシのクイックスタート](https://caddyserver.com/docs/quick-starts/reverse-proxy)
- [Let's EncryptでHTTPSを終端させたいだけならNginxよりCaddyを使うと楽だった件](https://qiita.com/ssc-ksaitou/items/ee0cda84dcf358a2b5eb)

---