# tsundoku-web-app
ユーザーが積読を登録・共有できるアプリです。

## 開発環境構築

### コンテナを立ち上げる
```
cd laradock
cp .env.example .env
docker-compose up
```

### laravel をインストールし、初期設定を行う（初回のみ）
```
# laravel と依存ライブラリをインストールする
cd laradock
docker-compose exec workspace composer install

# laravel の環境設定ファイルを作成する
cd ../laravel
cp .env.example .env

# laravel のアプリケーションキーを生成する（生成したキーは .env に記載される）
# 参考: https://laravel.com/docs/6.x/installation#configuration
# TODO: 共同開発で使うアプリケーションキーを決める
cd ../laradock
docker-compose exec workspace php artisan key:generate

# laravel/storage への進歩リックリンクを laravel/public/storage に作成する
# 参考: https://laravel.com/docs/6.x/structure#the-storage-directory
docker-compose exec workspace php artisan storage:link
```

### ブラウザから `localhost:${NGINX_PORT}` にアクセスする
エラーが発生しないことを確認する。

## トラブルシューティング

### `/var/www/html/storage/logs/laravel.log` の書き込み権限がない
ブラウザからアプリにアクセスした際に、以下の権限エラーが発生した場合は、`laravel/storage` と `laravel/bootstrap/cache` のオーナーを `www-data:www-data` に変更する。

エラー
```
UnexpectedValueException
The stream or file "/var/www/html/storage/logs/laravel.log" could not be opened in append mode: failed to open stream: Permission denied
```

オーナー変更
```
cd laravel
sudo chown -R www-data:www-data storage
sudo chown -R www-data:www-data bootstrap/cache
```

参考: https://laravel.com/docs/6.x/installation#configuration
