# docker-laravel-handson
## php
### build & up
- appコンテナの作成
- PHPのバージョン確認
- Laravelで必要なPHP拡張機能の確認

`docker compose up -d --build`

- docker compose コマンドは docker-compose.yml があるディレクトリで実行します。
- docker compose up は docker-compose.yml に定義したサービスを起動します。
- -d 「デタッチド」モードでコンテナを起動します。
  - デフォルトは「アタッチド」モードで全てのコンテナログを画面上に表示
  - 「デタッチド」モードではバックグラウンドで動作
- --build コンテナの開始前にイメージを構築します
  - 特に変更がない場合はキャッシュが使用されます。


### appコンテナ内ミドルウェアのバージョン確認(コンテナに入ってコマンド実行)
作成したappコンテナの中に入ってPHP, Composerのバージョン、インストール済みの拡張機能を確認します。

`docker compose exec app bash`

- phpのバージョン確認

`php -v`

- composerのバージョン確認

`composer -V`

- インストール済みの拡張機能の一覧

`php -m`

## nginx
### build & up
`docker compose up -d`

nginxのバージョン確認

`docker compose exec web nginx -v`

### webコンテナの確認
- webコンテナの動作確認
- HTMLとPHPが表示されるか

`mkdir backend/public`

`echo "Hello World" > backend/public/index.html`

`echo "<?php phpinfo();" > backend/public/phpinfo.php`

http://127.0.0.1:8080/index.html
「Hello World」が表示されたらwebサーバーが正しく動作している。

http://127.0.0.1:8080/phpinfo.php
phpinfoの情報が表示されることを確認する。
webサーバーがappサーバーへphpを実行させ結果を返してくれることを確認。

## Laravelをインストール
`docker compose exec app bash`

`composer create-project --prefer-dist "laravel/laravel=8.*" .`

`php artisan -V`

http://127.0.0.1:8080/


## mysqlコンテナ作成
### mysqlバージョン確認
`docker compose exec db mysql -V`


## 再構築方法
`docker compose exec app bash`

`composer install`

127.0.0.1:10080

composer install 時は .env 環境変数ファイルは作成されないので、 .env.example を元にコピー

`cp .env.example .env`

`php artisan key:generate`

public/storage から storage/app/public へのシンボリックリンクを張る
システムで生成したファイル等をブラウザからアクセスできるよう公開するためにシンボリックリンクを張る

`php artisan storage:link`

storage, bootstrap/cache はフレームワークからファイル書き込みが発生するので、書き込み権限を与える必要がある。

`chmod -R 777 storage bootstrap/cache`

welcome画面

`php artisan migrate`


## Laravelのログをコンテナに表示する
backend/.env

`LOG_CHANNEL=stderr`

backend/routes/web.php

Route::get('/', function () {
    logger('welcome route.');
    return view('welcome');
});

$ docker compose logs
-f でログウォッチ
$ docker compose logs -f
サービス名を指定してログを表示
$ docker compose logs -f app

## mysqlクライアントツール
ports:
      - 33060:3306