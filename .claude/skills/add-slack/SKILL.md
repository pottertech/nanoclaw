---
name: add-slack
description: Add Slack as a channel for NanoClaw. Enables Socket Mode bot that responds to DMs and @mentions.
---

# Add Slack Channel

This skill adds Slack support to NanoClaw using Socket Mode (no public URL needed).

## Prerequisites

### 1. Create a Slack App

1. Go to https://api.slack.com/apps
2. Click **Create New App** → **From scratch**
3. Enter app name (e.g., "NanoClaw") and select your workspace
4. Click **Create App**

### 2. Configure OAuth & Permissions

1. Go to **OAuth & Permissions** in the left sidebar
2. Scroll to **Scopes** → **Bot Token Scopes**
3. Add these scopes:
   - `app_mentions:read`
   - `chat:write`
   - `im:history`
   - `im:read`
   - `im:write`
   - `users:read`
   - `users:read.email`

### 3. Enable Socket Mode

1. Go to **Socket Mode** in the left sidebar
2. Toggle **Enable Socket Mode**
3. Generate an app-level token:
   - Click **Generate Token and Scopes**
   - Name it "nanoclaw-socket"
   - Add `connections:write` scope
   - Copy the token (starts with `xapp-`)

### 4. Install App to Workspace

1. Go to **Install App** in the left sidebar
2. Click **Install to Workspace**
3. Allow the requested permissions
4. Copy the **Bot User OAuth Token** (starts with `xoxb-`)

### 5. Update .env File

Add environment variables to your NanoClaw configuration:

```bash
# Channel selection
NANOCLAW_CHANNEL=slack

# Slack tokens from steps above
SLACK_BOT_TOKEN=xoxb-your-bot-token-here
SLACK_APP_TOKEN=xapp-your-app-token-here
```

### 6. Restart NanoClaw

```bash
npm run build
npm start
```

---

## Usage

### Direct Messages (DM)

Simply DM the bot - it will respond to all messages in a DM.

### Channels

In channels, the bot only responds when:
- @mentioned (@nanoclaw ...)
- Sent as a DM to the bot

### Obtaining Channel ID

To register a channel:
1. Right-click the channel name → **Copy Link**
2. Extract the ID or use this format: `slack:<channel-id>`
3. For DMs, the ID is in the format: `slack:D<user-id>`

---

## Configuration Options

| Variable | Default | Description |
|----------|---------|-------------|
| `NANOCLAW_CHANNEL` | `whatsapp` | Channel to use (`whatsapp` or `slack`) |
| `SLACK_BOT_TOKEN` | (none) | Bot User OAuth Token (starts with `xoxb-`) |
| `SLACK_APP_TOKEN` | (none) | App-level token for Socket Mode (starts with `xapp-`) |

---

## Comparison with WhatsApp

| Feature | WhatsApp | Slack |
|---------|----------|-------|
| Setup | QR code pairing | App + tokens |
| Public URL | Not needed | Not needed (Socket Mode) |
| Typing indicator | ✅ Yes | ❌ No |
| Message limit | 4096 chars | 4000 chars (auto-chunked) |
| Group names | Yes | Yes |
| Threading | Yes | Via message history |

---

## Troubleshooting

**Bot not responding:**
- Check logs for token errors
- Ensure app is installed to workspace
- Verify Socket Mode is enabled
- Check bot is invited to channel (for channels, not DMs)

**Messages not showing:**
- Run with `DEBUG=pino` for verbose logging
- Check token scopes are correct
- Verify `NANOCLAW_CHANNEL=slack` is set

---

*Based on upstream contribution by Joel Helbling (PR #176)*
