# TrendRadar - 日本語版

> AI を活用したトレンドニュース監視ツール - 情報過多に別れを告げ、重要な情報に集中

[![GitHub Stars](https://img.shields.io/github/stars/sansan0/TrendRadar?style=flat-square&logo=github&color=yellow)](https://github.com/sansan0/TrendRadar/stargazers)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg?style=flat-square)](LICENSE)
[![MCP Support](https://img.shields.io/badge/MCP-AI%20分析-FF6B6B?style=flat-square)](https://modelcontextprotocol.io/)

## 🎯 概要

TrendRadar は、以下の機能を備えたインテリジェントなニュース集約・監視システムです：

- **35以上のプラットフォームを監視** - 中国、欧米、日本のプラットフォーム（Weibo、Reddit、Twitter、GitHub、NHK、TechCrunch など）からトレンドコンテンツを集約
- **AI による分析** - MCP（Model Context Protocol）を使用したトレンド追跡、感情分析、類似検索
- **スマートフィルタリング** - 必須/除外キーワードによるキーワードベースのフィルタリング
- **マルチチャネル通知** - Slack、Discord、Telegram、メール、ntfy などをサポート
- **複数のモード** - 日次サマリー、リアルタイムトレンド、または増分監視

## 🚀 クイックスタート

### オプション1: Docker デプロイ（推奨）

```bash
# リポジトリをクローン
git clone https://github.com/yksanjo/TrendRadar.git
cd TrendRadar

# 設定を構成
cp config/config.yaml config/config.yaml.backup
# config/config.yaml を編集して設定を変更

# Docker で実行
docker-compose up -d
```

### オプション2: ローカルセットアップ

```bash
# 依存関係をインストール
pip install -r requirements.txt

# 設定
# config/config.yaml と config/frequency_words.txt を編集

# 実行
python main.py
```

## ⚙️ 設定

### 1. プラットフォーム設定

`config/config.yaml` を編集してプラットフォームを選択します。設定には中国、欧米、日本のプラットフォームが含まれます：

```yaml
platforms:
  # 中国のプラットフォーム（英語名に翻訳）
  - id: "weibo"
    name: "Weibo"
  - id: "douyin"
    name: "Douyin (TikTok China)"
  
  # 欧米のプラットフォーム
  - id: "reddit"
    name: "Reddit"
  - id: "hackernews"
    name: "Hacker News"
  - id: "github-trending"
    name: "GitHub Trending"
  
  # 日本のプラットフォーム（利用可能な場合）
  - id: "twitter"
    name: "Twitter/X"
  # その他のプラットフォームを追加
```

**注意**: プラットフォーム ID は newsnow API に依存します。https://newsnow.busiyi.world/ で利用可能なプラットフォームを確認してください。API でサポートされていないプラットフォームには、カスタムスクレイパーが必要な場合があります。

### 2. キーワード設定

`config/frequency_words.txt` を編集します。ファイルには中国、欧米、日本のキーワードが含まれます：

```
# 形式: 1行に1つのキーワード
# + をプレフィックスとして必須キーワード（必ず含まれる必要がある）
# ! をプレフィックスとして除外キーワード（含まれてはいけない）

# 日本のキーワード
AI
人工知能
ChatGPT
OpenAI
ソニー
Sony
トヨタ
Toyota
任天堂
Nintendo

# 中国のキーワード
华为
Huawei
字节
ByteDance

# 欧米のキーワード
OpenAI
ChatGPT
Google
Microsoft
!gai  # これを除外
```

### 3. 通知設定

#### Slack

1. Slack ウェブフックを作成: https://api.slack.com/messaging/webhooks
2. `config/config.yaml` に追加：
```yaml
webhooks:
  slack_webhook_url: "https://hooks.slack.com/services/YOUR/WEBHOOK/URL"
```

#### Discord

1. サーバー設定で Discord ウェブフックを作成
2. `config/config.yaml` に追加：
```yaml
webhooks:
  discord_webhook_url: "https://discord.com/api/webhooks/YOUR/WEBHOOK/URL"
```

#### Telegram

1. [@BotFather](https://t.me/botfather) でボットを作成
2. チャット ID を取得
3. `config/config.yaml` に追加：
```yaml
webhooks:
  telegram_bot_token: "YOUR_BOT_TOKEN"
  telegram_chat_id: "YOUR_CHAT_ID"
```

#### メール

```yaml
webhooks:
  email_from: "sender@example.com"
  email_password: "your_password_or_app_password"
  email_to: "recipient@example.com"
  email_smtp_server: "smtp.gmail.com"  # オプション、自動検出
  email_smtp_port: "587"  # オプション、自動検出
```

### 4. レポートモード

`config/config.yaml` で監視モードを選択：

- **`daily`** - 日次サマリー（その日のすべてのマッチングニュース）
- **`current`** - 現在のランキング（リアルタイムトレンド）
- **`incremental`** - 増分監視（新しいコンテンツのみ）

```yaml
report:
  mode: "daily"  # オプション: "daily"|"incremental"|"current"
```

## 🤖 AI 分析（MCP）

TrendRadar には AI による分析用の MCP サーバーが含まれています：

- トレンド追跡
- 感情分析
- 類似検索
- プラットフォーム比較
- 13以上の分析ツール

MCP のセットアップ手順については [README-MCP-FAQ.md](README-MCP-FAQ.md) を参照してください。

## 📊 機能

### スマートフィルタリング

- **通常のキーワード**: 任意の出現にマッチ
- **必須キーワード** (`+keyword`): ニュースに必ず含まれる必要がある
- **除外キーワード** (`!keyword`): これを含むニュースを除外

### 重み付けランキング

ニュースは以下でランク付けされます：
- 60% ランキング位置
- 30% 頻度
- 10% 人気度

### プッシュ時間ウィンドウ

通知を送信する時間を制御：

```yaml
push_window:
  enabled: true
  time_range:
    start: "09:00"
    end: "18:00"
  once_per_day: true
```

## 🐳 Docker デプロイ

```bash
# ビルドして実行
docker-compose up -d

# ログを表示
docker-compose logs -f

# 停止
docker-compose down
```

### 環境変数

環境変数で設定を上書きできます：

```bash
export ENABLE_CRAWLER=true
export REPORT_MODE=daily
export SLACK_WEBHOOK_URL="https://hooks.slack.com/..."
```

## 📝 プラットフォームサポート

### 現在サポートされている（newsnow API 経由）

https://newsnow.busiyi.world/ でサポートされているプラットフォームの完全なリストを確認してください。

### カスタムプラットフォームの追加

プラットフォームが newsnow API でサポートされていない場合：

1. カスタムスクレイパーを作成
2. データフェッチャーに統合
3. プラットフォーム ID を `config/config.yaml` に追加

## 🔧 トラブルシューティング

### HTTP サービスが起動しない

1. ポート 3333 が利用可能か確認：
```bash
# Mac/Linux
lsof -i :3333

# Windows
netstat -ano | findstr :3333
```

2. 別のポートを試す：
```bash
uv run python -m mcp_server.server --transport http --port 33333
```

### データが取得できない

1. プラットフォームが正しく設定されているか確認
2. newsnow API にアクセスできるか確認
3. ネットワーク/プロキシ設定を確認

### 通知が機能しない

1. ウェブフック URL が正しいか確認
2. 設定の通知設定を確認
3. エラーメッセージのログを確認

## 📄 ライセンス

GPL-3.0 ライセンス

## 🙏 謝辞

- オリジナルプロジェクト: [sansan0](https://github.com/sansan0/TrendRadar)
- データ API: [newsnow](https://github.com/ourongxing/newsnow)
- この日本語版は、中国、欧米、日本のプラットフォームと文化に対応するように適応されています

## 🤝 貢献

貢献を歓迎します！プルリクエストを自由に提出してください。

## 📚 追加リソース

- [MCP FAQ](README-MCP-FAQ.md)
- [Cherry Studio セットアップ](README-Cherry-Studio.md)
- 英語版 README: [README_EN.md](README_EN.md)
- 中国語版 README: [readme.md](readme.md)

---

**注意**: これは TrendRadar の日本語版で、中国、欧米、日本のプラットフォームと文化に対応するように設定されています。一部のプラットフォーム ID は newsnow API で確認するか、カスタムスクレイパーが必要な場合があります。

