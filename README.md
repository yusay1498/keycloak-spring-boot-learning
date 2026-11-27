keycloak-spring-boot-learning
================================================================================

Keycloak と Spring Boot の学習用プロジェクト

環境要件
--------------------------------------------------------------------------------

- Java 25
- Docker と Docker Compose

技術スタック
--------------------------------------------------------------------------------

- API サーバー: Java, Spring Boot, Spring Security
- 認証サーバー: Keycloak
- 認証データベース: PostgreSQL

開発向け起動手順
--------------------------------------------------------------------------------

手順 1. 依存サーバー群を起動

```bash
docker compose up -d
```

初回起動時、`keycloak/export` の realm とユーザーの設定がインポートされます。
Keycloak の起動には時間がかかる場合があります。状態は次のコマンドで確認できます。

```bash
docker compose ps
docker compose logs -f keycloak
```

手順 2. Keycloak 管理画面にアクセス

- URL: http://localhost:8000
- 初期管理者: `admin` / `admin`
- 学習用 realm: `learning`

管理者は初回の空の DB に作成される一時管理者です。継続して使用する場合は、管理画面で恒久的な管理者を作成してください。
`keycloak/export` には `user01` と `user02` が含まれます。パスワードは export 時点の状態に従います。
この設定と認証情報はローカル学習用であり、本番環境には使用しないでください。

手順 3. アプリケーションを起動

TODO: アプリケーションの実装後に、ビルド・起動・Keycloak 連携の手順を追記する。

終了する場合は以下を実行します。コンテナを削除して新たに起動すると、DB は初期化され、export されていない変更は引き継がれません。

```bash
docker compose down
```

Keycloak Export 方法
--------------------------------------------------------------------------------

1. Keycloak 管理画面でユーザーや client などを設定します。
2. Keycloak を停止し、realm とユーザーを別のディレクトリに export します。

```bash
mkdir -p keycloak/export-new
docker compose stop keycloak
docker compose run --rm --no-deps \
  --entrypoint /opt/keycloak/bin/kc.sh \
  -v "$PWD/keycloak/export-new:/tmp/export" \
  keycloak export --dir /tmp/export --realm learning --users different_files
docker compose start keycloak
```

export したファイルの内容を確認してから `keycloak/export` に反映してください。ファイルにはユーザーの認証情報や client secret が含まれる場合があります。
既存の realm は起動時インポートで上書きされないため、JSON の変更は既存 DB には自動反映されません。
