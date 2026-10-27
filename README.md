# Hermes Voice Assistant for Home Assistant

A Home Assistant custom integration that connects your voice PE to [Hermes Agent](https://github.com/ddrayne/hermes-agent) via its OpenAI-compatible API server.

> **Fork note:** this fork of [wunnle/hermes-homeassistant](https://github.com/wunnle/hermes-homeassistant) adds the `X-HA-User` header (see [Speaker identity](#speaker-identity)). To use it, add `brammol/hermes-homeassistant` as a custom repository in HACS (category Integration) instead of the upstream one.

## Features

- Full Hermes agent behind your voice: Spotify, calendar, Linear, camera, smart home, and more
- **`continue_conversation` support** — mic stays open when Hermes asks a follow-up question
- Emoji stripping and TTS character limits for clean voice output
- Simple Bearer token auth
- **Speaker identity** — the Home Assistant user name is sent as an `X-HA-User` header

## Requirements

- Hermes Agent running with the API server enabled
- Home Assistant 2024.1.0+

## Hermes Setup

Add to `~/.hermes/.env`:

```bash
API_SERVER_ENABLED=true
API_SERVER_HOST=0.0.0.0
API_SERVER_PORT=8642
API_SERVER_KEY=your-secret-key
```

Restart the Hermes gateway.

## Installation via HACS

1. In HACS, go to **Integrations** → **Custom repositories**
2. Add `https://github.com/ddrayne/hermes-homeassistant` as an **Integration**
3. Install **Hermes Voice Assistant**
4. Restart Home Assistant

## Manual Installation

Copy `custom_components/hermes/` into your HA config's `custom_components/` folder and restart.

## Configuration

1. Go to **Settings** → **Devices & Services** → **Add Integration**
2. Search for **Hermes**
3. Enter your Pi's IP, port (`8642`), and API key

## Speaker identity

When a request comes from a logged-in Home Assistant user (for example the Companion app or the Assist chat), the integration looks up that user's name and sends it to the Hermes API as a percent-encoded `X-HA-User` header. Requests without a user (such as a satellite without a linked account) omit the header.

A proxy or the agent can use it to tell household members apart, for instance to choose a persona or to key conversations per person. Decode with `urllib.parse.unquote`.

## Voice PE Setup

1. Go to **Settings** → **Voice assistants** → your assistant
2. Set **Conversation agent** to **Hermes**
3. Done — wake word routes to Hermes
