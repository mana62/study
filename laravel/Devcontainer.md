# Devcontainer
→ 開発環境をコード化してコンテナで動かす仕組み

---

## 前提
- Dockerを使用している
- VSCodeを使用 + VSCode内で「Dev Containers」を検索してインストールしている

---

## Devcontainer設定の流れ
#### ① プロジェクトのルートに `.devcontainer` フォルダを作成し、`devcontainer.json` を配置
#### ② コンテナをビルド・起動
- ①VS Code のコマンドパレット（⌘+Shift+P）を開く
- ②`Dev Containers: Reopen in Container` を選択
- ③コンテナ内で開発環境が立ち上がる
  - **Dev Containers: Rebuild Without Cache（キャッシュなし）**
    - 完全に最初からビルド (数分〜10分)
  - **Dev Containers: Rebuild（キャッシュあり）**
    - 前回のビルド結果（キャッシュ）を使って、早く再構築する (数十秒〜1分)

---

## `devcontainer.json`に記載すること
→ VSCodeが入る設定を記載する
<details>
<summary>devcontainer.json</summary>

```bash
{
  # VSCodeで表示される名前
  "name": "Suq CRM DevContainer",

  # compose.yml の場所を指定
  # DevContainerはこのcomposeファイルを使ってコンテナを起動する
  "dockerComposeFile": "../compose.yml",

  # VSCodeがどのコンテナの中で動くか指定
  # 現状は app が PHP+Node を持つコンテナなので app を指定
  "service": "app",

  # VSCodeで開くワークスペースの中身（＝Laravelのプロジェクトルート）
  # compose.ymlで app の working_dir が /var/www なので同じように指定
  "workspaceFolder": "/var/www",

  # DevContainer起動時に一緒に立ち上げるサービスを指定
  "runServices": ["app", "db", "caddy", "phpmyadmin", "mail"],

  # VSCodeを閉じた時に自動でコンテナも停止するか
  "shutdownAction": "stopCompose",

  # 外部に公開するポートを指定 (この配列はVSCode側でブラウザプレビューするためのポート指定・.env の APP_PORTと一致させる)
  "forwardPorts": [8000, 5173, 8080, 8025],

  # コンテナ初回起動後に実行するコマンド
  # composer install でPHP依存、npm install でJS依存を自動インストール
  "postCreateCommand": "composer install && npm install",

  # VSCodeの中で使うユーザー
  "remoteUser": "root",

  # VSCode側のカスタマイズ設定（拡張機能やエディタ設定）
  "customizations": {
    "vscode": {
      # DevContainer内で自動的に入れておくVSCode拡張機能
      "extensions": [
        "bmewburn.vscode-intelephense-client", # PHP補完
        "onecentlin.laravel-blade",            # Bladeテンプレート構文ハイライト
        "onecentlin.laravel5-snippets",        # Laravelコードスニペット
        "esbenp.prettier-vscode",              # JS/Blade整形用Prettier
        "xdebug.php-debug",                    # PHPデバッグ用
        "mikestead.dotenv"                     # .envファイルハイライト
      ],

      # VSCode内の設定
      "settings": {
        "editor.formatOnSave": true,                        # ファイル保存時に自動整形
        "php.validate.executablePath": "/usr/local/bin/php" # PHPコマンドの場所
      }
    }
  }
}
```
</details>

---

## 参考
- [公式](https://containers.dev/)
- [Devcontainer ってなに？ 実際につかってみる](https://zenn.dev/conecone/articles/ab8d71d81e64bb)
- [Dev Containerについて基礎を学習する](https://qiita.com/smr1/items/137b912de86c4947ead0)
- [Dev Containerを使ってみよう](https://zenn.dev/bells17/articles/devcontainer-2024)
- [VSCode の便利拡張、Dev Containers の使い方](https://zenn.dev/tom_tan/articles/9af1818fcde7d2)
- [Devcontainer(Remote Container) いいぞという話 ~開発環境を整える~](https://qiita.com/yoshii0110/items/c480e98cfe981e36dd56)

---

## 実行結果 (実行：2025/10/30)
#### ①今回は、Dev Containers: Rebuild（キャッシュあり）を選択

<details>
<summary>ターミナル結果</summary>

```bash
Running the postCreateCommand from devcontainer.json...

[23485 ms] Start: Run in container: /bin/sh -c composer install && npm install
Installing dependencies from lock file (including require-dev)
Verifying lock file contents can be installed on current platform.
Nothing to install, update or remove
Generating optimized autoload files
> Illuminate\Foundation\ComposerScripts::postAutoloadDump
> @php artisan package:discover --ansi

   INFO  Discovering packages.

  kitloong/laravel-migrations-generator ................................. DONE
  laravel/pail .......................................................... DONE
  laravel/sail .......................................................... DONE
  laravel/tinker ........................................................ DONE
  nesbot/carbon ......................................................... DONE
  nunomaduro/collision .................................................. DONE
  nunomaduro/termwind ................................................... DONE

82 packages you are using are looking for funding.
Use the `composer fund` command to find out more!

up to date, audited 110 packages in 781ms

30 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice
npm notice New major version of npm available! 10.8.2 -> 11.6.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.6.2
npm notice To update run: npm install -g npm@11.6.2
npm notice
任意のキーを押してターミナルを終了します。



root@cf0e05ea7633:/var/www# php artisan --version
Laravel Framework 12.35.0
root@cf0e05ea7633:/var/www# npm run dev

> dev
> vite


  VITE v7.1.12  ready in 306 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: http://172.21.0.6:5173/
  ➜  press h + enter to show help

  LARAVEL v12.35.0  plugin v2.0.1

  ➜  APP_URL: https://localhost
```
</details>

#### ②VScodeの左下がdevcontainer.jsonで指定したnameになっていればOK

```bash
>< 開発コンテナー：Suq CRM DevContainer
```

---

## 微妙な点
#### VSCodeのDevContainerはメインの1つのサービス(app)に入る
→ 他のコンテナ（db・caddy・mail・phpmyadmin）にも入って中を見たり操作することは可能だが、毎回アクセスする必要がある
→ 一気に全部のコンテナに入ることは不可

#### 他のコンテナにアクセスする方法
- ① 左下の「>< Dev Container: Laravel DevContainer」クリック
- ② Attach to Running Container 選択
- ③ 一覧から入りたいコンテナを選択

---

