# Fauna Serverless Framework

`Serverless Framework`と **[Serverless Fauna Plugin](https://github.com/fauna-labs/serverless-fauna)** を利用した Fauna の DB 環境を IaC で管理する。

## 環境

- 実装環境の統一のために VSCode で`devcontainer`を利用する。
- AWSアカウントが必要であり、AWS CLIのcredentialが必要になります。
  - ホスト環境で設定されている場合は`~/.aws`にあるcredential情報をDocker内にマウントするようにしています。

## Setup

- Fauna のアカウントを[こちら](https://dashboard.fauna.com/accounts/register)から取得する。
  - 既にある場合はスキップしてください。
  - アカウントは`Github`と`Netlify`のアカウントからも作れますが、Email と Password での作成をしてくださ。
    - `Github`と`Netlify`のアカウントからだとこのあと CLI からの DB 作成ができなくなります。
- `fauna cloud-login`を実行します。
  - 実行すると対話式で入力を求められるます、CLI の他のコマンドを実行する前にログインしておく必要があります。
  - `The endpoint alias prefix (to combine with a region):`はそのまま Enter
    - 既に`cloud`という名前の Endpoint があると`The endpoint alias already exists. Overwrite?`と聞かれます。
  - `How do you prefer to authenticate? (Use arrow keys)`は`Email and Password`を洗濯してください。
  - `Email address: `はログイン時の Email を入力します。
  - `Password: `はログイン時のパスワードを入力します。
  - `Endpoints created. Would you like to set one of them as default?`と Endpoint が作成されたのでデフォルト Endpoint を選択します。今回は`cloud`を選択します。
  - `Endpoint 'cloud' set as default endpoint.`と表示されたら完了です。
- `fauna create-database fauna-serverless`で DB を作成します。
  - `creating database fauna-serverless`と表示されたら作成完了です。
- `fauna create-key fauna-serverless`でシークレットキーを作成します。
  - `secret: *************`と表示されているのがシークレットキーになります。
- `.env.sample`を`cp .env.sample .env`でコピーします。
- `.env`ファイルが出来るので`FAUNA_SECRET=`に先程のシークレットキーを追記します。

## デプロイ
```
sls deploy [--aws-profile <profile名>]
```
デプロイするとAWSのCloudFormationにスタックが作られます。
今回は`serverless.yml`にAWSリソースを作成するコードは書いてませんが、CloudFormationの状態保存のためにS3が自動作成されます。


## 削除
```
sls remove [--aws-profile <profile名>]
```
リソース削除を行うと、Fauna上のリソースが全て削除されるのと同時にCloudFormationのスタックも削除されます。
このときにS3がちゃんと削除されていることを確認します。



## 参照ページ

https://github.com/fauna-labs/serverless-fauna-example
