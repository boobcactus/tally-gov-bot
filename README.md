# Tally Governance Discord Bot

A Discord bot that monitors governance proposals for projects using Tally, and sends rich alerts to a designated channel.

## Features
- Monitors governance proposals in real time via the Tally API (with built‑in rate limiting)
- Announces new ACTIVE proposals to Discord, with optional role pings and direct Tally links
- Rich embeds: title, proposer profile link, voting window, status, auto-generated abstract, and vote progress bars
- Keeps embeds in sync until proposals reach a final status, re-publishing if the original message is missing
- SQLite tracking prevents duplicate announcements and survives restarts; configurable sync interval via `.env`
- Admin slash command `/clear_db` to reset the tracking database when needed

## Requirements
- Python 3.10+ 
- Discord bot token with Message Content intent enabled
- A Discord server with appropriate permissions
- Tally API key

## Installation

### 1. Discord Bot Setup
1. Create a Discord application at https://discord.com/developers/applications
2. Navigate to the "Bot" section and create a bot
3. Copy the bot token (you'll need this for the `.env` file)
4. Under "Privileged Gateway Intents", enable:
   - Message Content Intent (enables commands)
5. Generate an invite link with these permissions:
   - Send Messages
   - Embed Links
   - Read Message History
6. Invite the bot to your server using the generated link
7. Create a channel for governance alerts and copy the channel ID (requires Developer Mode)

### 2. Project Setup
1. Clone the repository:
```bash
git clone <repository-url>
cd tally-gov-bot
```

2. Create and activate a virtual environment:
```bash
python -m venv venv
source venv/bin/activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Copy `.env.example` to `.env`:
```bash
cp .env.example .env
```

5. Configure your `.env` file:
```
# Discord Bot Configuration (Required)
DISCORD_TOKEN=your_discord_bot_token_here
DISCORD_GUILD_ID=your_discord_guild_id_here
PROPOSALS_CHANNEL_ID=your_channel_id_here

# Tally API Configuration (Required)
# Get your API key from https://www.tally.xyz/user/settings
TALLY_API_KEY=your_tally_api_key_here
# Target Tally organization ID for the DAO/governor you want to monitor
TALLY_ORG_ID=your_tally_org_id_here

# Bot Configuration (Optional)
# How often to check for new proposals (in minutes, default: 5)
SYNC_INTERVAL_MINUTES=5
```

### 3. Run the Bot
```bash
python tally_bot.py
```

The bot will:
- Create an `announced_proposals.db` file to track proposals (if `LIVE_MODE=false`)
- Start checking for new governance proposals every `SYNC_INTERVAL_MINUTES`
- Post alerts to your configured channel when proposals become active
- Update existing alerts as proposal statuses change

## Discord Commands
- `/clear_db` - Clear the announced proposals database (User must have Administrator permissions)

## Database
The bot uses a SQLite database (`announced_proposals.db`) to track which proposals have been announced. This database is automatically created on first run.

## Troubleshooting
- **Bot not responding**: Check that the bot token is correct and the bot is invited to your server
- **No alerts**: Verify the channel ID is correct and the bot has permissions to send messages
- **Tally details missing**: Ensure your Tally API key is valid and properly set in `.env`
- **Rate limit errors**: The bot implements automatic rate limiting, but excessive requests may still cause issues

## Disclaimer
This bot is provided as-is, with no warranty of any kind. Use at your own risk.

Designed with Claude 4 Opus + Sonnet, using Windsurf IDE.