# GitHub Actions
＝ あらかじめ定義した処理と条件の組合せ（＝ワークフロー/Workflow）を自動化するGitHub公式の機能

## ymlを作る場所
- アプリのルートに、`.github/workflows/ファイル名.yml`を定義する
- 複数置くことができる
- 役割ごとに変えるのがベスト（例: ステージング/テスト/本番 など）


## 定型文
```yml
name: Deploy to Server  # ワークフローの名前

# どのタイミングで実行するかを指定
# トリガー：mainブランチにpushされたとき実行
on:
  push:
    branches:
      - main

# 同時に同じワークフローを実行できる数や、古いジョブの扱いを制御する設定
# concurrencyで古いジョブをキャンセル
concurrency:
  group: deploy-staging # 同じ名前のジョブは1回だけ実行する
  cancel-in-progress: true # 古いジョブが実行中ならキャンセルして最新だけ実行

# 作業のまとまり
jobs:
  deploy:
    runs-on: ubuntu-latest  # GitHubランナーの環境
    steps: # 作業のまとまりをもっと細かく表したもの
      # コードをリポジトリから取得
      - name: Checkout code
        uses: actions/checkout@v3 # GitHub リポジトリからコードを取得する Action

      # SSHでサーバーに接続してコマンド実行
      - name: Deploy via SSH
        uses: appleboy/ssh-action@v1.2.0 # このOSSを使ってSSHを接続しますというのを指定（appleboyさんのssh-action@v1.2.0）
        with:
          host: ${{ secrets.SERVER_HOST }}          # SSH接続先ホスト
          username: ${{ secrets.SERVER_USER }}      # SSHユーザー名
          key: ${{ secrets.SERVER_PRIVATE_KEY }}    # 秘密鍵（GitHub Secretsに登録）
          port: 22                                  # SSHポート

          # サーバー上で実行するコマンド のまとまりを下記に記載
          script: |
            echo "Start deployment"                 # サーバーで実行するコマンド

            # Laravel サーバーの場合は Docker を使わず、PHP・Composer・npm・artisan コマンドを直接書く ことが多いが例として
            docker pull my-image:latest             # Dockerを更新
            docker stop my-container || true        # 古いコンテナを停止
            docker rm my-container || true          # 古いコンテナを削除
            docker run -d --name my-container my-image:latest # 新しいコンテナを起動
            echo "Deployment finished"

         # Laravelの場合、下記も設定することが多い

         export PATH="$HOME/bin:$PATH"
         # $HOME/bin を PATH に追加 → 独自コマンドを使えるように
        [ -s "$HOME/.nvm/nvm.sh" ] && . "$HOME/.nvm/nvm.sh"
        #NVM（Node Version Manager）を読み込む → Node / npm コマンドを使えるように

            REPO_DIR="$HOME/gitのリポジトリ名"
            # $REPO_DIR → git リポジトリの場所
            APP_DIR="$REPO_DIR/src"
            # $APP_DIR → Laravel アプリのディレクトリ

            maintenance_enabled=0

            # php artisan down / php artisan up でメンテナンスモードに切り替え
            # スクリプトが途中で失敗しても cleanup でアプリを 必ずメンテナンスモード解除 する安全策
            cleanup() {
              if [ "$maintenance_enabled" -eq 1 ]; then
                cd "$APP_DIR"
                php artisan up || true
              fi
            }
            trap cleanup EXIT INT TERM

            # Git操作
            # リポジトリが存在するかチェック
            [ -d "$REPO_DIR/.git" ] || { echo "Missing git repo: $REPO_DIR" >&2; exit 1; }
            [ -d "$APP_DIR" ] || { echo "Missing app dir: $APP_DIR" >&2; exit 1; }

            cd "$REPO_DIR"
            git fetch --prune origin

            # 最新コミット（$DEPLOY_SHA）に強制切り替え
            git checkout --force "$DEPLOY_SHA"

            # 古い変更を消してクリーンな状態に
            git reset --hard "$DEPLOY_SHA"

            # PHP / Composer / Node のセットアップ
            cd "$APP_DIR"

            # Composer → PHP の依存ライブラリをインストール
            composer install --no-interaction --prefer-dist --optimize-autoloader

            # npm → Node.js のパッケージをインストール
            npm ci --no-audit --no-fund

            # npm run build → フロントエンドのビルド（JS/CSSなどの生成）
            npm run build

            # メンテナンスモード + マイグレーション
            # アプリをメンテナンスモードにしてアクセス制限
            php artisan down || true
            maintenance_enabled=1

            # データベースの更新
            php artisan migrate --force --verbose

            # キャッシュをクリア＆再生成して高速化
            php artisan optimize:clear
            php artisan config:cache
            php artisan route:cache
            php artisan view:cache
            php artisan event:cache

            # メンテナンス終了
            php artisan up
            maintenance_enabled=0


上記らもよくわからないけど一般的に実行するもの？
```

## 各用語
- **CI/CD**
  - CI = Continuous Integration（継続的インテグレーション
    - → **「コードをこまめに統合・テストする仕組み」**
    - → 例：開発者がコードを GitHub に push したら、自動でテストが走る

  - CD = Continuous Deployment / Delivery（継続的デプロイ / 配信）
    - → **「テストが通ったコードを自動で本番やステージング環境に反映する仕組み」**
    - → 例：GitHub Actions がサーバーに SSH 接続して自動で更新する

- **トリガー（Trigger）**
  - ワークフローを いつ開始するかを決めるもの
    - 例：
      - push → コードを push したとき
      - pull_request → プルリクエストが作られたとき
      - schedule → 定期実行（cron）
```yml
on:
  push:
    branches:
      - main # main ブランチに push されたらワークフローが走る
```

- **ジョブ（Job）**
  - **作業のまとまり**
  - **ワークフロー内の 作業単位**
  - 1つのジョブは 1つの環境（ランナー）で順番に実行される処理のまとまり
  - ジョブの中に ステップ（Step） があって、コマンドや Action を順番に実行する

```yml
# 例
jobs:
  build: # build がジョブ名
    runs-on: ubuntu-latest
    steps: # steps の中でコードのチェックアウトとテスト実行をしている
      - name: Checkout code
        uses: actions/checkout@v3
      - name: Run tests
        run: npm test
```

- **SSH**（Secure Shellの略）
  - **遠いサーバ（AWS・エックスサーバーなど）に接続するためのプロトコル**
  - SSH で安全にサーバーに接続するには**鍵（公開鍵/秘密鍵）が必要**
    - 秘密鍵（Private Key） → **GitHub Actions に登録して使う**
    - 公開鍵（Public Key） → **サーバーの ~/.ssh/authorized_keys に登録してアクセスを許可**
```yml
with:
  host: ${{ secrets.SERVER_HOST }}
  username: ${{ secrets.SERVER_USER }}
  key: ${{ secrets.SERVER_PRIVATE_KEY }} # ここに秘密鍵を登録
```
  - リモートサーバーに安全に接続してコマンドを実行したりファイルを送る仕組み
  - GitHub Actions では、`appleboy/ssh-action`（これはOSSからとってきた接続するために利用するもの） のような Action を使って サーバーに接続してデプロイ するときに使う

**SSH と SSL の違い**
| 用語                     | 役割                        | 補足                                              |
| ---------------------- | ------------------------- | ----------------------------------------------- |
| **SSH (Secure Shell)** | サーバーに安全に接続してコマンド実行やファイル転送 | リモート操作用のプロトコル。例：GitHub Actions がサーバーにアクセスしてデプロイ |
| **SSL / TLS**          | Web通信を暗号化して安全にする          | 例：ブラウザとサーバー間の HTTPS 通信。デプロイとは直接関係なし             |

- SSH → リモート操作・ファイル転送
- SSL → 通信の暗号化

- **concurrency**
  - **同時に同じワークフローを実行できる数や、古いジョブの扱いを制御する設定**
  - デプロイのときによく使う → サーバーに古いジョブと新しいジョブが同時にアクセスするのを防ぐ

```yml
concurrency:
  group: deploy-staging # group = 同じ名前のジョブは1回だけ実行する
  cancel-in-progress: true # cancel-in-progress: true = 古いジョブが実行中ならキャンセルして最新だけ実行する
```

- **steps**
  - その作業（Job）の中の細かい手順
  - ジョブの中で実行される **1つ1つの処理単位**

```yml
steps:
  - name: Checkout code
    uses: actions/checkout@v3   # GitHubからコードを取得する
  - name: Run tests
    run: npm test                # コマンド実行
```

- **script**
  - **サーバー上で実行するコマンド のまとまり**
  - Laravel サーバーの場合は Docker を使わず、PHP・Composer・npm・artisan コマンドを直接書く ことが多い
  - 例：
    - Docker サーバーなら docker run 系
    - Laravel サーバーなら composer install / npm run build / php artisan migrate など
    - ※ エックスサーバーなどはdockerが使えない為この指定は不要

```yml
# 例：
script: |
    echo "Start deployment"                 # サーバーで実行するコマンド
    docker pull my-image:latest             # Dockerを更新
    docker stop my-container || true        # 古いコンテナを停止
    docker rm my-container || true          # 古いコンテナを削除
    docker run -d --name my-container my-image:latest # 新しいコンテナを起動
    echo "Deployment finished"
```
- **シンボリックリンク**
  - 参照先を同じにする設定

- **.htaccess**
  - Apache や LiteSpeed などの **Web サーバーの設定をディレクトリ単位で上書きするためのファイル**
  - 役割
    - ディレクトリごとに Web サーバーの挙動をカスタマイズ する
    - Web アプリや公開ディレクトリでよく使う
    - サーバー全体の設定ファイル（httpd.conf など）を書き換えなくても、ディレクトリ単位で設定できる
 - よく使われる例:

| 設定例                                    | 目的                                             |
| -------------------------------------- | ---------------------------------------------- |
| `RewriteEngine On`                     | URL 書き換え（例：`/about` → `/index.php?page=about`） |
| `RewriteRule ^.*$ index.php [L]`       | Laravel のフロントコントローラーにルーティング                    |
| `ErrorDocument 404 /404.html`          | 404 ページの指定                                     |
| `Options -Indexes`                     | ディレクトリの中身を一覧表示させない                             |
| `AddType application/x-httpd-php .php` | PHP ファイルとして認識させる                               |



## 参考
- [GitHub Actionsについて](https://docs.github.com/ja/actions/get-started/understand-github-actions)
- [【入門】GitHub Actionsとは？概要やメリット、使用例まとめ](https://www.kagoya.jp/howto/it-glossary/develop/githubactions/)
- [GitHub Actionsって何？触ってみて理解しよう！入門・逆引きリファレンス](https://qiita.com/s3i7h/items/b50ceb0008edc3c0312e)