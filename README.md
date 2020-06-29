# README

# 必要なファイル
- Dockerfile
- docker-compose.yml
- Gemfile
- Gemfile.lock
リポジトリ内のファイルをそのまま使って下さい

# Rails新規プロジェクト作成

ターミナルで以下を実行

```
docker-compose run app bundle exec rails new . --api --force --database=mysql --skip-bundle
```

# database.rbの修正
プロジェクトを作成すると、たくさんのファイルが自動生成される。
そのうちの、database.ymlを以下のように修正する。
### config/database.yml
```

︙
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password # docker-compose.ymlのMYSQL_ROOT_PASSWORDで指定した
  host: db # docker-compose.ymlのサービス名
︙
```

#docker上でbundle install
ターミナルで以下のコマンドを実行。
```
docker-compose run app bundle install
```
「bundle updateをしてくれ」というメッセージが出たら,
```
docker-compose run app bundle update
```
を続けて実行

#docker関係のコマンド
docker-compose build コンテナをビルド。dockerのコンテナを新たに作る。
docker-compose up コンテナを起動する
docker-compose stop コンテナを一旦停止する
docker-compose ps 作ったコンテナの状態を確認する
docker-compose rm -v コンテナをすべて削除する

#その他メモ
herokuでmysqlを使う場合、
```
heroku addons:create jawsdb:kitefin
```
を実行すること

database.ymlのproductionも、以下のように変更すること
```
production:
  <<: *default
  url: <%= ENV['JAWSDB_URL']&.sub('mysql://', 'mysql2://') %>
```
