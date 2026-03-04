# Slack to Discord

> Migrate your Slack workspace history into Discord — channels, messages, threads, files, and all.

[![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-blue.svg)](https://www.python.org/downloads/)
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

![Demonstration](demo.png)

## What It Does

Takes a Slack export ZIP file and uses a Discord bot to recreate your workspace — preserving the people, conversations, and context your team built.

### Capabilities

- Complete message history import (tested with 10,000+ messages)
- Private channels (if included in your Slack export)
- Images and file attachments (falls back to Slack-hosted links for oversized files)
- Original usernames and avatars preserved via webhooks
- Emoji and reaction support (custom emojis require manual Discord setup; rename `-` to `_`)
- Threaded conversations maintained as Discord threads
- Pinned messages carried over
- Day separators (`--------YYYY-MM-DD--------`) and per-message timestamps
- Long messages (>2000 chars) automatically split across multiple messages

### Limitations

- Discord timestamps reflect import time, not original send time (mitigated by text timestamps)
- No private/direct messages imported
- Reactions displayed as embeds, not native Discord reactions
- Slack embeds (images, buttons, etc.) and canvases are not preserved
- File uploads require two messages: one for content, one for the attachment

## Getting Started

### Prerequisites

- Python 3.8+
- A Slack export ZIP file
- A Discord bot token with the right permissions

### 1. Export Your Slack Data

Go to https://my.slack.com/services/export and download your workspace export.

### 2. Create a Discord Bot

Follow the [discord.py bot guide](https://discordpy.readthedocs.io/en/latest/discord.html) and grant these permissions:

| Permission | Purpose |
|---|---|
| Manage Channels | Create imported channels and set topics |
| Manage Webhooks | Post as original Slack usernames/avatars |
| Send Messages | Post message content |
| Create Public Threads | Recreate threaded conversations |
| Send Messages in Threads | Post thread replies |
| Embed Links | Display reactions as embeds |
| Attach Files | Upload images and files |
| Manage Messages | Pin messages *(optional)* |

### 3. Install

```bash
pip install slack-to-discord
```

### 4. Run

```bash
slack-to-discord --zipfile <slack_export.zip> --guild "My Server" --token <bot_token>
```

See all options:

```bash
slack-to-discord --help
```

### 5. Wait

Discord rate limits throttle the import to roughly **50 messages/minute**. The program exits when the import is complete.

### 6. Verify and Invite

Inspect the imported history, then invite your users.

## Options

| Flag | Description |
|---|---|
| `-z`, `--zipfile` | Path to Slack export ZIP *(required)* |
| `-t`, `--token` | Discord bot token *(required)* |
| `-g`, `--guild` | Discord server name *(required)* |
| `-c`, `--channels` | Only import specific channels (space-separated) |
| `-s`, `--start` | Start date filter (`YYYY-MM-DD`) |
| `-e`, `--end` | End date filter (`YYYY-MM-DD`) |
| `-p`, `--all-private` | Import all channels as private |
| `-r`, `--real-names` | Use Slack real names instead of usernames |
| `-df`, `--date-format` | Date separator format (default: `%Y-%m-%d`) |
| `-tf`, `--time-format` | Message time format (default: `%H:%M`) |
| `-v`, `--verbose` | Enable debug logging |

## Recommendations

- **Start with a clean Discord server.** The bot posts messages — it can't rewrite history, so existing content gets buried.
- **Private channels** are created with access restricted to the bot and server admins. After import, manually adjust roles/permissions.
- **If something goes wrong**, delete the created channels and re-run. If you fix a bug, please contribute it back!

## Contributing

Contributions are welcome. Fork the repo, make your changes, and open a pull request.

## License

[GPLv3](https://www.gnu.org/licenses/gpl-3.0)

## Acknowledgments

Originally created by [pR0Ps](https://github.com/pR0Ps/slack-to-discord).
