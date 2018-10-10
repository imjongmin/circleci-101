# coinage_auth_server

node -v 10.11.0

# 環境構築手順

## 事前準備
### Docker Community Edition for Macをインストール
https://store.docker.com/editions/community/docker-ce-desktop-mac
Version 18.06.0-ce-mac70 (26399)

### yarnのインストール
1.9.4をインストール
```
$ brew install yarn --without-node
```

- node_modules作成
```
$ yarn
```



### 設定ファイルを設置
```
$ cp config/development.default.json config/development.json
```

## コンテナ起動

### コンテナビルド
```
$ docker-compose build
```

### コンテナ起動
```
$ docker-compose up
```

### migration実行
```
$ docker exec -it auth /bin/sh -c "yarn run migrate"
```

### マスタデータロード
```
$ docker exec -it auth /bin/sh -c "yarn run seed:all"
```

### tsのコンパイル
```
$ docker exec -it auth /bin/sh -c "yarn run compile"
```

## その他
### API通信用ツールのインストール
APIクライアントのPostmanを使用することで、簡単にAPIの実行が可能です。
curlの実行でもAPI実行可能ですが、Postmanを利用する方が簡単です。
https://www.getpostman.com/

指定する環境変数、インポート用のjsonファイルは管理者にお問い合わせ下さい。

### Swagger利用の準備
本プロジェクトではAPI設計書作成にSwagger(https://swagger.io/)を利用している。
Swaggerを利用して、API設計を実施し、その結果をAWSのAPI Gatewayにデプロイすることで、
設計結果がそのままAPIとしてデプロイされます。
なお、Swagger規格の最新バージョンはOpenAPI 3.0だが、
API Gatewayが未対応であるため、Swagger 2.0を利用しています。

#### Swagggerコンテナに関して
swaggerが提供しているdockerを使ってローカルに環境を作成できる。

- コンテナの作成
```
$ docker pull swaggerapi/swagger-editor
```

- コンテナ起動
```
$ docker run -p 8080:8080 swaggerapi/swagger-editor
```

- コンテナ終了
```
⌘ + c
```

#### Swaggerファイルの作成
プロジェクトルートで

```
$ node merge_swagger.js
```
or
```
$ ./build.sh
```
を実行すると、swagger/dist/swagger.ymlが作成されます。
作成したswagger.ymlをSwaggerEditorで読み込む事で、API設計書を確認することができます。

### redis

#### redis GUI (redis commander)
- redisの確認用のguiサービスをdockerに追加
- dockerを起動すれば以下のURLでredisにGUIでアクセスできる

http://localhost:8081/

### 開発環境デプロイ
- rbenv, ruby install(2.5.1)
- bundle install

- 確認
```
cap development deploy:check
```

- deploy (BRANCHのDefault:develop)
```
cap development deploy
cap development deploy BRANCH=branch_name
```
