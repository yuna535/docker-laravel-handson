# docker-laravel-handson
## build & up
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


## appコンテナ内ミドルウェアのバージョン確認(コンテナに入ってコマンド実行)
作成したappコンテナの中に入ってPHP, Composerのバージョン、インストール済みの拡張機能を確認します。

`docker compose exec app bash`

- phpのバージョン確認

`php -v`

- composerのバージョン確認

`composer -V`

- インストール済みの拡張機能の一覧

`php -m`