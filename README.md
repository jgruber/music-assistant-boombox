# Boombox — public YouTube Music for Music Assistant

A [Music Assistant](https://music-assistant.io) custom music provider that
reads YouTube Music's public web catalog — the browse, search and playlist
pages any logged-out visitor to `music.youtube.com` sees — and plays the
underlying audio through `yt-dlp`. No Google account needed.

Think of it as a complement to the official `ytmusic` provider, intended for
people who don't have a Google / YouTube Music account. It offers none of the
Premium-tier features (library sync, personalized recommendations, higher-
bitrate masters, official support). If you have Premium, use the official
provider.

## What it is

- Anonymous InnerTube catalog access via [`ytmusicapi`](https://github.com/sigma67/ytmusicapi).
- Playback resolved through [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) using
  the `web_music` client — the only client that still returns opus/m4a formats
  for logged-out requests.
- Companion sidecar add-on supplies the GVS PO Token that YouTube now requires
  of every yt-dlp client (see **Dependencies**).

## What it is not

- Not a bypass of a Premium subscription. The free YouTube Music tier serves
  ads; this provider does not strip them.
- Not shipped, listed, linked to or endorsed by Music Assistant upstream.
- Not a replacement for the official `ytmusic` provider; it coexists under a
  separate domain (`ytmusic_anon`).

## Install

### A. As a Music Assistant custom provider (requires upstream support)

Once Music Assistant core ships the `custom_providers/` loader (currently
proposed in [`feat/custom-providers`](https://github.com/jgruber/server/tree/feat/custom-providers)),
drop the `ytmusic_anon/` directory from this repo into:

```
<storage_path>/custom_providers/ytmusic_anon/
```

Install the Python dependencies into Music Assistant's venv (see
**Dependencies** below), then restart Music Assistant.

### B. As a Home Assistant add-on (current working path)

Use the [Boombox + Mixtape add-on repo](https://github.com/jgruber/music-assistant-boombox-mixtape-sources) —
it bakes both providers plus all dependencies into the Music Assistant image.
This is the recommended install today; it will remain supported after the
custom-provider loader lands.

## Dependencies

Pin versions; unpinned requirements are ignored by Music Assistant's loader.

```
ytmusicapi==1.12.3
yt-dlp>=2025.09.26
bgutil-ytdlp-pot-provider==2.0.1
deno==2.9.5
```

Also required alongside Music Assistant on the same host:

- A **PO Token provider sidecar** — the `ytm_po_token_generator` add-on from
  the Music Assistant repo, running on host network. YouTube returns
  image-only formats to anonymous callers without the GVS PO Token, which
  surfaces as "Requested format is not available" errors.

Dropping any one of `yt-dlp`, `bgutil-ytdlp-pot-provider`, `deno`, or the
sidecar → playback fails.

## Config

The provider has no mandatory configuration — leave the defaults and it will
browse and play. The config entries live under `provider.ytmusic_anon.*` in
Music Assistant's settings page.

## Safety

A custom provider runs with Music Assistant's full privileges — this one too.
The server does not sandbox it. Only install custom providers from sources
you trust.

## License

Apache License 2.0 — see `LICENSE`.
