# Soulmask Chat Bridge

A desktop tool for bridging Soulmask server chat with Discord.

The tool watches one or more Soulmask server log files, forwards in-game chat to Discord, can forward Discord messages back to all configured servers via RCON, and can post player join/leave events and welcome messages.

## Features

- Forward in-game chat messages to Discord via webhook
- Forward Discord channel messages to all RCON-enabled Soulmask servers
- Show the Discord bot as online while the bridge is running
- Support multiple servers, for example `EU-1` and `EU-2`
- Forward chat between configured servers through RCON
- Send an in-game welcome message every time a player joins
- Post a Discord first-join welcome only once per player and server
- Post normal join and leave events to Discord
- Keep track of known and welcomed players locally
- Tabbed interface with separate sections for overview, settings, servers, and logs
- Local configuration through `config.json`

## Requirements

- Windows
- Python 3.10 or newer
- A Discord webhook URL for server-to-Discord messages
- A Discord bot token and Discord channel ID if you want Discord-to-server messages
- RCON enabled on your Soulmask server if you want the tool to send messages back into the game

## Basic setup

### 1. Discord webhook

Create a Discord webhook in the target Discord channel and paste the webhook URL into the tool under:

```text
Settings → Discord Webhook URL
```

This is used for:

- in-game chat to Discord
- first-join welcome messages
- join/leave event messages

### 2. Discord bot for Discord-to-server messages

To send Discord messages back into the Soulmask servers, configure:

```text
Settings → Discord → Server via RCON
```

Required fields:

- Discord bot token
- Discord channel ID
- command prefix
- RCON command template

The bot ignores bot/webhook messages to prevent message loops.

Discord messages are sent to all servers where RCON is enabled.

### 3. Server configuration

Add or configure your servers under:

```text
Server
```

For each server, configure:

- server prefix, for example `EU-1`
- path to the Soulmask log file
- display color
- RCON enabled/disabled
- RCON host
- RCON port
- RCON password

RCON is required for:

- Discord-to-server messages
- in-game welcome messages
- cross-server chat forwarding

### 4. Welcome and join/leave messages

Configure these under:

```text
Settings → Join Welcome / Join & Leave
```

Available placeholders:

```text
{player}
{server}
{id}
```

Behavior:

- in-game welcome message is sent on every join
- Discord first-join welcome is sent only once per player and server
- Discord join message is sent on every join when enabled
- Discord leave message is sent on every leave when enabled

## Local data files

The tool creates and updates these files next to the Python file or EXE:

### `config.json`

Stores local settings such as webhook URL, bot token, server paths, RCON settings, and templates.

### `welcomed_players.json`

Stores players who already received the Discord first-join welcome.

This prevents first-join messages from being posted repeatedly.

### `known_players.json`

Stores known player names by player ID.

This helps the tool display useful leave messages even when the leave log line only contains the player ID.

## Updatin

Recommended update process:

1. Download the latest `SoulmaskChatBridge.exe` from github.
2. Replace only the `.exe` file.
3. You will keep your local `config.json`, `welcomed_players.json`, and `known_players.json`.

## Security notes

Never publish or commit these values:

- Discord webhook URL
- Discord bot token
- RCON password
- `config.json`

If a token or webhook is accidentally shared, revoke it immediately and create a new one.

## Troubleshooting

### Discord messages do not appear in-game

Check that:

- Discord-to-server bridge is enabled
- bot token is correct
- channel ID is correct
- RCON is enabled for each target server
- RCON host, port, and password are correct
- the server firewall allows the RCON connection

### In-game welcome messages do not appear

Check that:

- welcome messages are enabled
- the correct server log file is selected
- RCON is enabled for the server
- RCON settings are correct

### Join or leave events do not appear in Discord

Check that:

- join/leave Discord posting is enabled
- Discord webhook URL is set
- the selected server log file contains join/leave lines
- the tool is running

## License

```text
Copyright (c) 2026 Moonpie

All rights reserved.

This software is proprietary. You may download and use the compiled application for personal or server administration purposes.

You may not copy, modify, reverse engineer, decompile, redistribute, sell, sublicense, or publish this software or any part of it without prior written permission from the copyright holder.

This software is provided "as is", without warranty of any kind.
```
