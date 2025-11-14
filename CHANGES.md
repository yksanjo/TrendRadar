# TrendRadar English Edition - Changes Summary

## Overview

This is an English adaptation of TrendRadar, configured for Western platforms and culture. The original project by [sansan0](https://github.com/sansan0/TrendRadar) focused on Chinese platforms.

## Key Changes

### 1. Platform Configuration

**Chinese Platforms (kept, with English translated names):**
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

**Western Platforms (added):**
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

**Note**: Platform IDs depend on the newsnow API availability. Some platforms may require custom scrapers if not supported by the API.

### 2. Keyword Configuration

**Combined Chinese + Western keywords:**
- **Chinese keywords:** 华为, 字节, 抖音, 腾讯, 阿里巴巴, 百度, 小米, etc. (with English translations)
- **Western keywords:** OpenAI, Google, Microsoft, Apple, etc.
- AI/ML terms (both languages)
- Programming languages and frameworks
- Social media platforms (both Chinese and Western)
- Business and politics (both regions)
- Science and space topics

### 3. Notification Channels

**Added:**
- Slack webhook support
- Discord webhook support

**Existing (kept):**
- Telegram
- Email
- ntfy
- Feishu (Chinese)
- DingTalk (Chinese)
- WeChat Work (Chinese)

### 4. Documentation

- Created `README_EN.md` - Complete English documentation
- Translated configuration file comments
- Updated setup instructions for Western users

### 5. Code Updates

- Added `send_to_slack()` function
- Added `send_to_discord()` function
- Updated `split_content_into_batches()` to support Slack/Discord formats
- Updated notification configuration reading
- Updated notification dispatch logic

## Configuration Files Modified

1. **config/config.yaml**
   - Translated all comments to English
   - Replaced Chinese platforms with Western ones
   - Added Slack and Discord webhook configuration
   - Updated time window defaults (09:00-18:00 instead of 20:00-22:00)

2. **config/frequency_words.txt**
   - Completely replaced with Western-focused keywords
   - Organized by categories (Tech, AI, Programming, etc.)

## Next Steps

### For Users:

1. **Verify Platform Support**: Check if your desired platforms are supported by the newsnow API at https://newsnow.busiyi.world/
2. **Add Custom Scrapers**: If a platform isn't supported, you'll need to create custom scrapers
3. **Configure Notifications**: Set up your preferred notification channels (Slack, Discord, etc.)
4. **Customize Keywords**: Edit `frequency_words.txt` with your specific interests

### For Developers:

1. **Platform Scrapers**: The project uses the newsnow API. To add unsupported platforms:
   - Create a scraper module
   - Integrate with `DataFetcher` class
   - Add platform ID to config

2. **Notification Channels**: Adding new channels follows the pattern:
   - Add config reading in `load_config()`
   - Create `send_to_<channel>()` function
   - Add to `send_to_notifications()` dispatch
   - Update `_has_notification_configured()`

## Known Limitations

1. **Platform Dependency**: Relies on newsnow API which may not support all Western platforms
2. **Language**: Some code comments and print statements are still in Chinese (original codebase)
3. **Time Zones**: Default time handling uses Beijing time - may need adjustment for other timezones

## Contributing

Contributions welcome! Areas that need work:
- More platform scrapers for Western platforms
- Complete translation of all code comments
- Timezone handling improvements
- Additional notification channels
- Better error messages in English

## License

Same as original: GPL-3.0

