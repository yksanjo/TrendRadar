# TrendRadar - English Edition

> AI-powered trending news monitoring tool - Say goodbye to information overload, focus on what matters

[![GitHub Stars](https://img.shields.io/github/stars/sansan0/TrendRadar?style=flat-square&logo=github&color=yellow)](https://github.com/sansan0/TrendRadar/stargazers)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue.svg?style=flat-square)](LICENSE)
[![MCP Support](https://img.shields.io/badge/MCP-AI%20Analysis-FF6B6B?style=flat-square)](https://modelcontextprotocol.io/)

## 🎯 Overview

TrendRadar is an intelligent news aggregation and monitoring system that:

- **Monitors 35+ platforms** - Aggregates trending content from both Chinese platforms (Weibo, Douyin, Zhihu, Bilibili, etc.) and Western platforms (Reddit, Hacker News, GitHub, TechCrunch, BBC, CNN, etc.)
- **AI-powered analysis** - Uses MCP (Model Context Protocol) for trend tracking, sentiment analysis, and similarity search
- **Smart filtering** - Keyword-based filtering with required/exclusion keywords
- **Multi-channel notifications** - Supports Slack, Discord, Telegram, Email, ntfy, and more
- **Multiple modes** - Daily summaries, real-time trending, or incremental monitoring

## 🚀 Quick Start

### Option 1: Docker Deployment (Recommended)

```bash
# Clone the repository
git clone https://github.com/sansan0/TrendRadar.git
cd TrendRadar

# Configure your settings
cp config/config.yaml config/config.yaml.backup
# Edit config/config.yaml with your preferences

# Run with Docker
docker-compose up -d
```

### Option 2: Local Setup

```bash
# Install dependencies
pip install -r requirements.txt

# Configure
# Edit config/config.yaml and config/frequency_words.txt

# Run
python main.py
```

## ⚙️ Configuration

### 1. Platform Configuration

Edit `config/config.yaml` to select platforms. The configuration includes both Chinese and Western platforms:

```yaml
platforms:
  # Chinese Platforms (translated names)
  - id: "weibo"
    name: "Weibo"
  - id: "douyin"
    name: "Douyin (TikTok China)"
  - id: "zhihu"
    name: "Zhihu"
  - id: "bilibili-hot-search"
    name: "Bilibili Hot Search"
  
  # Western Platforms
  - id: "reddit"
    name: "Reddit"
  - id: "hackernews"
    name: "Hacker News"
  - id: "github-trending"
    name: "GitHub Trending"
  # Add more platforms as needed
```

**Note**: Platform IDs depend on the newsnow API. Check https://newsnow.busiyi.world/ for available platforms. You may need to add custom scrapers for platforms not supported by the API.

### 2. Keyword Configuration

Edit `config/frequency_words.txt`. The file includes both Chinese and Western keywords:

```
# Format: One keyword per line
# Prefix with + for required keywords (must appear)
# Prefix with ! for exclusion keywords (must not appear)

# Chinese keywords
华为
Huawei
字节
ByteDance

# Western keywords
OpenAI
ChatGPT
AI
Machine Learning
!gai  # Exclude this
```

### 3. Notification Setup

#### Slack

1. Create a Slack webhook: https://api.slack.com/messaging/webhooks
2. Add to `config/config.yaml`:
```yaml
webhooks:
  slack_webhook_url: "https://hooks.slack.com/services/YOUR/WEBHOOK/URL"
```

#### Discord

1. Create a Discord webhook in your server settings
2. Add to `config/config.yaml`:
```yaml
webhooks:
  discord_webhook_url: "https://discord.com/api/webhooks/YOUR/WEBHOOK/URL"
```

#### Telegram

1. Create a bot with [@BotFather](https://t.me/botfather)
2. Get your chat ID
3. Add to `config/config.yaml`:
```yaml
webhooks:
  telegram_bot_token: "YOUR_BOT_TOKEN"
  telegram_chat_id: "YOUR_CHAT_ID"
```

#### Email

```yaml
webhooks:
  email_from: "sender@example.com"
  email_password: "your_password_or_app_password"
  email_to: "recipient@example.com"
  email_smtp_server: "smtp.gmail.com"  # Optional, auto-detected
  email_smtp_port: "587"  # Optional, auto-detected
```

### 4. Report Modes

Choose your monitoring mode in `config/config.yaml`:

- **`daily`** - Daily summary (all matching news from the day)
- **`current`** - Current ranking (real-time trending)
- **`incremental`** - Incremental monitoring (only new content)

```yaml
report:
  mode: "daily"  # Options: "daily"|"incremental"|"current"
```

## 🤖 AI Analysis (MCP)

TrendRadar includes an MCP server for AI-powered analysis:

- Trend tracking
- Sentiment analysis
- Similarity search
- Platform comparison
- 13+ analysis tools

See [README-MCP-FAQ.md](README-MCP-FAQ.md) for MCP setup instructions.

## 📊 Features

### Smart Filtering

- **Regular keywords**: Match any occurrence
- **Required keywords** (`+keyword`): Must appear in the news
- **Exclusion keywords** (`!keyword`): Exclude news containing this

### Weighted Ranking

News is ranked using:
- 60% ranking position
- 30% frequency
- 10% hotness

### Push Time Windows

Control when notifications are sent:

```yaml
push_window:
  enabled: true
  time_range:
    start: "09:00"
    end: "18:00"
  once_per_day: true
```

## 🐳 Docker Deployment

```bash
# Build and run
docker-compose up -d

# View logs
docker-compose logs -f

# Stop
docker-compose down
```

### Environment Variables

You can override config via environment variables:

```bash
export ENABLE_CRAWLER=true
export REPORT_MODE=daily
export SLACK_WEBHOOK_URL="https://hooks.slack.com/..."
```

## 📝 Platform Support

### Currently Supported (via newsnow API)

Check https://newsnow.busiyi.world/ for the full list of supported platforms.

### Adding Custom Platforms

If a platform isn't supported by the newsnow API, you'll need to:

1. Create a custom scraper
2. Integrate it with the data fetcher
3. Add the platform ID to `config/config.yaml`

## 🔧 Troubleshooting

### HTTP Service Won't Start

1. Check if port 3333 is available:
```bash
# Mac/Linux
lsof -i :3333

# Windows
netstat -ano | findstr :3333
```

2. Try a different port:
```bash
uv run python -m mcp_server.server --transport http --port 33333
```

### No Data Retrieved

1. Check if platforms are correctly configured
2. Verify the newsnow API is accessible
3. Check network/proxy settings

### Notifications Not Working

1. Verify webhook URLs are correct
2. Check notification settings in config
3. Review logs for error messages

## 📄 License

GPL-3.0 License

## 🙏 Acknowledgments

- Original project by [sansan0](https://github.com/sansan0/TrendRadar)
- Data API provided by [newsnow](https://github.com/ourongxing/newsnow)
- This English edition adapted for Western platforms and culture

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📚 Additional Resources

- [MCP FAQ](README-MCP-FAQ.md)
- [Cherry Studio Setup](README-Cherry-Studio.md)
- Original Chinese README: [readme.md](readme.md)

---

**Note**: This is an English adaptation of TrendRadar, configured for Western platforms and culture. The original project focuses on Chinese platforms. Some platform IDs may need to be verified against the newsnow API or custom scrapers may be required.

