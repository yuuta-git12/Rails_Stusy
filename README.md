# Rails 8 App with Docker

Ruby on Rails 8 + PostgreSQL の最新開発環境をDockerで構築したWebアプリケーションです。

## 🚀 技術スタック

- **Ruby**: 3.3.0
- **Rails**: 8.0.2（最新版）
- **データベース**: PostgreSQL 15
- **コンテナ**: Docker & Docker Compose
- **アセットパイプライン**: Propshaft（Rails 8の新機能）
- **JavaScript**: Importmap + Turbo + Stimulus
- **キャッシュ**: Solid Cache
- **キュー**: Solid Queue
- **WebSocket**: Solid Cable
- **デプロイ**: Kamal
- **パフォーマンス**: Thruster

## 📋 セットアップ手順

### 1. 前提条件

以下のソフトウェアがインストールされていることを確認してください：

- Docker
- Docker Compose

### 2. アプリケーションの起動

```bash
# コンテナをビルドして起動
docker compose up --build

# バックグラウンドで起動する場合
docker compose up -d --build
```

### 3. データベースのセットアップ

```bash
# データベースを作成
docker compose exec web bin/rails db:create

# マイグレーションを実行
docker compose exec web bin/rails db:migrate

# シードデータを投入（必要な場合）
docker compose exec web bin/rails db:seed
```

### 4. アプリケーションにアクセス

ブラウザで以下のURLにアクセスしてください：
- **http://localhost:3000**

## 🛠️ 開発用コマンド

### Railsコマンドの実行

```bash
# Railsコンソール
docker compose exec web bin/rails console

# ジェネレーター
docker compose exec web bin/rails generate model User name:string email:string

# マイグレーション
docker compose exec web bin/rails db:migrate

# テスト実行
docker compose exec web bin/rails test

# アセットプリコンパイル
docker compose exec web bin/rails assets:precompile
```

### コンテナの管理

```bash
# コンテナの停止
docker compose down

# コンテナとボリュームを削除
docker compose down -v

# ログの確認
docker compose logs web
docker compose logs db

# コンテナの再ビルド（キャッシュクリア）
docker compose build --no-cache
```

## 📁 プロジェクト構造

```
.
├── app/                    # アプリケーションコード
│   ├── controllers/        # コントローラー
│   ├── models/            # モデル
│   ├── views/             # ビュー
│   ├── assets/            # アセット（CSS、JS）
│   └── javascript/        # JavaScript（Importmap）
├── bin/                   # 実行可能スクリプト
│   ├── rails             # Railsコマンド
│   ├── rake              # Rakeコマンド
│   └── docker-entrypoint # Dockerエントリーポイント
├── config/                # 設定ファイル
│   ├── database.yml       # データベース設定
│   ├── routes.rb          # ルーティング
│   ├── application.rb     # アプリケーション設定
│   ├── importmap.rb       # Importmap設定
│   └── deploy.yml         # Kamalデプロイ設定
├── db/                    # データベース関連
├── docker-compose.yml     # Docker Compose設定
├── Dockerfile             # Dockerイメージ設定（本番用）
├── Gemfile                # Ruby依存関係
└── README.md              # このファイル
```

## 🔧 環境変数

以下の環境変数が設定されています：

- `DATABASE_URL`: PostgreSQL接続URL
- `RAILS_ENV`: Rails環境（development）
- `POSTGRES_DB`: データベース名
- `POSTGRES_USER`: データベースユーザー
- `POSTGRES_PASSWORD`: データベースパスワード

## 🆕 Rails 8の新機能

### Propshaft（新しいアセットパイプライン）
- Sprocketsの後継
- より高速でシンプルなアセット管理

### Solid Cache
- データベースベースのキャッシュ
- Redis不要で簡単なキャッシュ管理

### Solid Queue
- データベースベースのジョブキュー
- Sidekiq不要で簡単なジョブ管理

### Solid Cable
- データベースベースのWebSocket
- Redis不要でリアルタイム通信

### Kamal
- 簡単なデプロイメントツール
- Dockerコンテナの自動デプロイ

### Thruster
- PumaのHTTPアセットキャッシュ/圧縮
- パフォーマンス向上

## 🐛 トラブルシューティング

### ポートが既に使用されている場合

```bash
# 使用中のポートを確認
lsof -i :3000
lsof -i :5432

# 必要に応じてポートを変更
# docker-compose.yml の ports セクションを編集
```

### データベース接続エラー

```bash
# データベースコンテナの状態確認
docker compose ps

# データベースログの確認
docker compose logs db

# データベースコンテナの再起動
docker compose restart db
```

### Railsコマンドが見つからない場合

Rails 8系では`bin/rails`を使用します：

```bash
# 正しいコマンド
docker compose exec web bin/rails console

# 間違ったコマンド
docker compose exec web rails console
```

### キャッシュの問題

```bash
# Dockerキャッシュをクリア
docker compose build --no-cache

# ボリュームを削除
docker compose down -v
```

## 📚 構築時の注意点

### バージョン互換性の問題
- Rails 7.0.8.7とRuby 3.2.2の組み合わせで`LoggerThreadSafeLevel`エラーが発生
- Rails 8系 + Ruby 3.3.0で解決

### Gemfile.lockの管理
- Dockerビルド時に古いGemfile.lockが使用される場合がある
- `--no-cache`オプションで強制再ビルドが必要

### プラットフォーム識別子
- 古い識別子（`mingw`, `x64_mingw`, `mswin`）は非推奨
- 新しい識別子（`windows`）を使用

## 🚀 デプロイ

Kamalを使用したデプロイ：

```bash
# Kamalの初期化
docker compose exec web bin/kamal init

# デプロイ設定の確認
docker compose exec web bin/kamal config

# デプロイ実行
docker compose exec web bin/kamal deploy
```

## 📄 ライセンス

このプロジェクトはMITライセンスの下で公開されています。

## 🙏 謝辞

Rails 8系の最新機能を活用した、モダンな開発環境の構築が完了しました。バージョン互換性の問題を解決し、最新のRails機能を活用できる環境が整いました。
