## Day 2 — 24 June 2026

- Created CloudWatch alarm on cpu_usage_idle metric, threshold >=20%, evaluation period 1x5min
- Understood difference between cpu_usage_idle (CWAgent metric) vs CPUUtilization (native EC2 metric)
- Learned what consecutive periods mean in alarm evaluation and why it prevents false alarms
- Created Telegram bot via BotFather, obtained bot token and chat ID
- Built Make scenario: Custom Webhook → Telegram Bot
- Debugged SNS subscription confirmation — root cause was mailhook vs custom webhook module mismatch, and missing https:// prefix in webhook URL
- Understood SNS concepts: topics, subscriptions, confirmation handshake and why it exists
- Wired CloudWatch alarm action → SNS topic → Make webhook → Telegram bot
- Full pipeline confirmed working via manual curl test
