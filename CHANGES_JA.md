# TrendRadar 日本語版 - 変更履歴

## 概要

これは TrendRadar の日本語版で、中国、欧米、日本のプラットフォームと文化に対応するように設定されています。オリジナルプロジェクトは [sansan0](https://github.com/sansan0/TrendRadar) によるものです。

## 主な変更点

### 1. プラットフォーム設定

**中国のプラットフォーム（保持、英語名に翻訳）：**
- Toutiao (Today's Headlines) - 今日头条
- Baidu Hot Search - 百度热搜
- Weibo - 微博
- Douyin (TikTok China) - 抖音
- Zhihu - 知乎
- Bilibili Hot Search - bilibili 热搜
- Wall Street CN - 华尔街见闻
- The Paper - 澎湃新闻
- Cailian Press Hot - 财联社热门
- Phoenix News - 凤凰网
- Baidu Tieba - 贴吧

**欧米のプラットフォーム（追加）：**
- Reddit
- Hacker News
- Product Hunt
- GitHub Trending
- Dev.to
- TechCrunch
- The Verge
- Wired
- BBC News
- CNN
- The New York Times
- Wall Street Journal

**日本のプラットフォーム（追加）：**
- Twitter/X
- Reddit (r/newsokur など)
- GitHub Trending
- その他（newsnow API で利用可能な場合）

**注意**: プラットフォーム ID は newsnow API の可用性に依存します。一部のプラットフォームは、API でサポートされていない場合、カスタムスクレイパーが必要な場合があります。

### 2. キーワード設定

**中国 + 欧米 + 日本のキーワードを統合：**
- **中国のキーワード:** 华为, 字节, 抖音, 腾讯, 阿里巴巴, 百度, 小米 など（英語翻訳付き）
- **欧米のキーワード:** OpenAI, Google, Microsoft, Apple など
- **日本のキーワード:** ソニー, トヨタ, 任天堂, LINE, AI, 人工知能 など
- AI/ML 用語（複数言語）
- プログラミング言語とフレームワーク
- ソーシャルメディアプラットフォーム（中国、欧米、日本）
- ビジネスと政治（各地域）
- 科学と宇宙のトピック

### 3. 通知チャネル

**追加：**
- Slack ウェブフックサポート
- Discord ウェブフックサポート

**既存（保持）：**
- Telegram
- メール
- ntfy
- Feishu（中国）
- DingTalk（中国）
- WeChat Work（中国）

### 4. ドキュメント

- `README_JA.md` を作成 - 完全な日本語ドキュメント
- 設定ファイルのコメントを翻訳
- 日本のユーザー向けにセットアップ手順を更新

### 5. コード更新

- `send_to_slack()` 関数を追加
- `send_to_discord()` 関数を追加
- `split_content_into_batches()` を更新して Slack/Discord 形式をサポート
- 通知設定の読み取りを更新
- 通知ディスパッチロジックを更新

## 変更された設定ファイル

1. **config/config.yaml**
   - すべてのコメントを英語に翻訳
   - 中国、欧米、日本のプラットフォームを追加
   - Slack と Discord のウェブフック設定を追加
   - 時間ウィンドウのデフォルトを更新（09:00-18:00）

2. **config/frequency_words.txt**
   - 中国、欧米、日本のキーワードを完全に統合
   - カテゴリ別に整理（テック、AI、プログラミング、日本の文化など）

## 次のステップ

### ユーザー向け：

1. **プラットフォームサポートを確認**: https://newsnow.busiyi.world/ で希望するプラットフォームがサポートされているか確認
2. **カスタムスクレイパーを追加**: プラットフォームがサポートされていない場合、カスタムスクレイパーを作成する必要があります
3. **通知を設定**: 希望する通知チャネル（Slack、Discord など）を設定
4. **キーワードをカスタマイズ**: `frequency_words.txt` を編集して特定の興味に合わせる

### 開発者向け：

1. **プラットフォームスクレイパー**: プロジェクトは newsnow API を使用します。サポートされていないプラットフォームを追加するには：
   - スクレイパーモジュールを作成
   - `DataFetcher` クラスに統合
   - プラットフォーム ID を config に追加

2. **通知チャネル**: 新しいチャネルの追加は次のパターンに従います：
   - `load_config()` で設定読み取りを追加
   - `send_to_<channel>()` 関数を作成
   - `send_to_notifications()` ディスパッチに追加
   - `_has_notification_configured()` を更新

## 既知の制限事項

1. **プラットフォーム依存性**: newsnow API に依存しており、すべての日本のプラットフォームをサポートしていない可能性があります
2. **言語**: 一部のコードコメントと print 文はまだ中国語のまま（オリジナルコードベース）
3. **タイムゾーン**: デフォルトの時間処理は北京時間を使用 - 他のタイムゾーンでは調整が必要な場合があります

## 貢献

貢献を歓迎します！作業が必要な領域：
- 日本のプラットフォーム用のより多くのスクレイパー
- すべてのコードコメントの完全な翻訳
- タイムゾーン処理の改善
- 追加の通知チャネル
- より良い英語/日本語のエラーメッセージ

## ライセンス

オリジナルと同じ: GPL-3.0

