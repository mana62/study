# laravel + React 環境構築

## ①compose.yml用意
```yml
# ※本番環境にはnginxもあったほうがいい
services:
  php:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: quiz_php
    ports:
      - "8000:8000"
    volumes:
      - ./:/var/www/html
    working_dir: /var/www/html
    depends_on:
      - db

  db:
    image: mysql:8.0
    container_name: quiz_mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: quiz_db
      MYSQL_USER: quiz_user
      MYSQL_PASSWORD: quiz_pass
    ports:
      - "3306:3306"
    volumes:
      - ./docker/mysql/data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: quiz_phpmyadmin
    environment:
      PMA_HOST: db
      PMA_USER: quiz_user
      PMA_PASSWORD: quiz_pass
    ports:
      - "8080:80"
    depends_on:
      - db

  node:
    image: node:22
    container_name: quiz_react
    working_dir: /var/www/html
    volumes:
      - ./:/var/www/html
    command: tail -f /dev/null
    ports:
      - "5173:5173"

volumes:
  db-data:
```

## ②Dockerfile
```dockerfile
FROM php:8.3-fpm

# 必要なパッケージと拡張
RUN apt-get update && apt-get install -y \
    git zip unzip curl libpng-dev libonig-dev libxml2-dev \
    && docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# Composerインストール
COPY --from=composer:2.6 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
```

## ③laravel環境構築

**①laravelインストール**

```bash
# laravel最新版
docker compose run --rm php composer create-project laravel/laravel backend
```
**②起動**
```bash
# docker起動
docker compose up -d
cd backend
# appコンテナに入る
docker compose exec php bash
# 起動
php artisan serve --host=0.0.0.0 --port=8000
```

## ④react環境構築(vite)

**①viteインストール**
```bash
# vite最新版
docker compose run --rm node npm create vite@latest frontend -- --template react
```

**②`vite.config.js`設定**
```bash
# vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vite.dev/config/
export default defineConfig({
  plugins: [react()],
  server: {
    host: true,
    port: 5173,
    watch: {
      usePolling: true,
    },
    proxy: {
      '/api': {
    target: 'http://localhost:8000',
    changeOrigin: true,
    secure: false,
        },
      },
    },
});
```

**③依存パッケージインストール**
```bash
docker compose exec node bash
cd frontend
npm install
```

**④起動**
```bash
npm run dev
```