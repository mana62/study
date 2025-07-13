# Docker

### 基本概念

- **イメージ**：アプリの設計図
- **コンテナ**：実行環境として動くアプリの実体
- **Dockerfile**：自作イメージ用レシピファイル
- **docker-compose.yml**：複数コンテナ管理設定ファイル

### 補足
- コンテナには必ずイメージが必要
- イメージは Docker Hub または Dockerfile によって生成
- Dockerfile は 1 コンテナ用
- 複数コンテナは docker-compose.yml を使用

---

## よく使うDocker基本コマンド

| コマンド                            | 説明                             |
| ----------------------------------- | -------------------------------- |
| `docker --version`                  | Dockerのバージョン確認           |
| `docker compose version`            | Composeのバージョン確認          |
| `docker pull イメージ名[:タグ]`     | イメージ取得                     |
| `docker images`                     | ローカルのイメージ一覧           |
| `docker search キーワード`          | イメージ検索                     |
| `docker rmi イメージ名`             | イメージ削除                     |
| `docker run イメージ名`             | コンテナ作成＆起動               |
| `docker ps`                         | 実行中のコンテナ表示             |
| `docker ps -a`                      | 全コンテナ表示                   |
| `docker start コンテナID`           | 起動                             |
| `docker stop コンテナID`            | 停止                             |
| `docker restart コンテナID`         | 再起動                           |
| `docker rm コンテナID`              | コンテナ削除                     |
| `docker build -t イメージ名:タグ .` | Dockerfileからイメージ作成       |
| `docker compose up -d`              | コンテナ起動（バックグラウンド） |
| `docker compose down`               | コンテナ停止＆削除               |
| `docker logs コンテナ名`            | ログ確認                         |
| `docker inspect コンテナ名`         | 詳細情報確認                     |
| `docker volume ls`                  | ボリューム一覧                   |
| `docker network ls`                 | ネットワーク一覧                 |
| `docker container prune`            | 停止コンテナ削除                 |
| `docker image prune`                | 未使用イメージ削除               |
| `docker system prune -f`            | 不要データ削除（強制）           |

---

## Dockerfile構文と命令

### 基本構文

```dockerfile
#phpのバージョンを指定
FROM php:8.1-fpm


#自分のPHP設定ファイル（php.ini）をコンテナに入れる
COPY php.ini /usr/local/etc/php/


#SSL証明書のディレクトリを作成
RUN mkdir -p /etc/ssl/certs /etc/ssl/private


#SSL証明書をコピーしてコンテナに入れる
COPY ssl/server.crt /etc/ssl/certs/
COPY ssl/server.key /etc/ssl/private/


#いろんな必要なソフトをインストールする
#PHPがデータベースとやり取りできるようにしたり、圧縮ファイルを扱えるようにする
RUN apt update \
  && apt install -y default-mysql-client zlib1g-dev libzip-dev unzip \
  && docker-php-ext-install pdo_mysql zip \
  && apt clean \
  && rm -rf /var/lib/apt/lists \*


#PHPの依存関係を管理するツール（Composer）をインストール
RUN curl -sS https://getcomposer.org/installer | php \
  && mv composer.phar /usr/local/bin/composer \
  && composer self-update

#作業する場所を「/var/www」に設定
WORKDIR /var/www
```


### 命令一覧

| 命令         | 内容                    |
| ------------ | ----------------------- |
| `FROM`       | ベースイメージ          |
| `RUN`        | コマンド実行            |
| `COPY`       | ホスト→コンテナへコピー |
| `WORKDIR`    | 作業ディレクトリ設定    |
| `CMD`        | デフォルト実行コマンド  |
| `EXPOSE`     | 使用ポート設定          |
| `ENV`        | 環境変数設定            |
| `ENTRYPOINT` | 必ず実行されるコマンド  |

---

## docker-compose.ymlの構文と例

### 定型文

```yaml
services:
  サービス名:
    image: イメージ名
    ports:
      - "8080:80"
    depends_on:
      - 他サービス名
    container_name: コンテナ名
```

### 構文例

```yaml
services:
  # サービス名
  nginx:
    # 作成するイメージを指定
    image: nginx:latest
    # コンテナ名を決める
    container_name: nginx

    # ポート番号を指定
    ports:
      # 左側はホスト、右側はdockerのポート番号
      # ローカル
      - "80:80"
      # 本番環境
      - "443:443"

    # マウントする設定ファイルのパスを指定(mysqlのconfなど)
    volumes:
      # 自分のNGINX設定ファイル（default.conf）をコンテナ内のNGINX設定場所に置く
      - ./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf
      # 自分のプロジェクトファイル（srcフォルダ）をコンテナ内の作業場所に入れる
      - ./src:/var/www/
      # 自分のSSL証明書や鍵をコンテナ内のSSLフォルダに入れる
      - ./docker/php/ssl:/etc/ssl

    # Service同士の依存関係の設定
    depends_on:
      - php
    # ネットワーク名指定
    networks:
      - ネットワーク名_network
    # パソコンを起動する度にDockerも起動
    restart: always

  php:
    build: ./docker/php
    volumes:
      - ./src:/var/www/
      - ./ssl:/etc/ssl
    environment:
      - PHP_INI_DIR=/usr/local/etc/php

  mysql:
    # Dockerfileがある場所を指定して、その場所からイメージを作成することを意味
    image: mysql:8.0.26

    # 環境変数設定(パスワードなど)
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: atte_app
      MYSQL_USER: user
      MYSQL_PASSWORD: pass

    # MySQLを起動して、みんなが使いやすいように設定する
    command: mysqld --default-authentication-plugin=mysql_native_password
    volumes:
      - ./docker/mysql/data:/var/lib/mysql
      - ./docker/mysql/my.cnf:/etc/mysql/conf.d/my.cnf

    # このコンテナはユーザーID1000、グループID1000の権限で動くという設定
    user: "1000:1000"

  phpmyadmin:
    image: phpmyadmin/phpmyadmin:latest
    environment:
      - PMA_ARBITRARY=1
      - PMA_HOST=mysql
      - PMA_USER=user
      - PMA_PASSWORD=pass
    depends_on:
      - mysql
    ports:
      - "8080:80"

  mailhog:
    image: mailhog/mailhog
    ports:
      - "1025:1025" # SMTP サーバーポート
      - "8025:8025" # Web UI

# ネットワーク名
networks:
  ネットワーク名_network:
    driver: bridge

```

---

## 補足

- 特定バージョン取得 → `docker pull nginx:1.23`
- コンテナの終了 → `docker stop`：安全 ／ `docker kill`：即強制
- ENTRYPOINT → 常に実行される処理
- コンテナIDのみ表示 → `docker ps -q`
- キャッシュ無効ビルド → `docker compose build --no-cache`
- ログファイル出力 → `docker logs > logs.txt`
- 一時実行 → `docker run --rm alpine echo hello`
- IP確認 → `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' コンテナ`
- ポート使用確認 → `lsof -i :8080` → `kill [PID]`

---

## 用語と構成

| 用語            | 説明                                     |
| --------------- | ---------------------------------------- |
| イメージ        | コンテナの設計図                         |
| コンテナ        | 実行可能なアプリ環境                     |
| Volume          | データ永続化用領域                       |
| Port Mapping    | ホストとコンテナ通信設定（例：8080:80）  |
| context         | Dockerfileのディレクトリ                 |
| depends_on      | 起動順序定義（サービス状態は保証しない） |
| restart: always | 自動再起動設定                           |

---

## ENTRYPOINTとCMDの違い

| 項目         | 説明                           |
| ------------ | ------------------------------ |
| `CMD`        | デフォルトで実行するコマンド   |
| `ENTRYPOINT` | 必ず実行されるコマンド（固定） |

---

## volumesについて

```yaml
volumes:
  - ./app:/var/www/html
```

- 左：ホスト
- 右：コンテナ
- 用途：永続化と共有

---

## buildとimageの違い

- `image` → Docker Hubから取得
- `build` → Dockerfileからビルド

👉 通常はどちらか一方を使う

---

## php.ini設定例

```ini
date.timezone = "Asia/Tokyo"
```

---

## トラブルシューティング

- ログ確認 → `docker compose logs サービス名`
- ポート解放 → `lsof -i :8080` → `kill PID`
- 不要キャッシュ削除 → `docker system prune -af`

---
