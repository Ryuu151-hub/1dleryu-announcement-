# 1dleryu Announcement Server

Static JSON config endpoints for the **1dleryu X TikTok** Chrome extension, deployed on Vercel.

Live at: `https://1dleryo-annoncement.vercel.app`

## Files

| File | Purpose |
|---|---|
| `announcement.json` | Popup banner shown on extension open (header, message, cards, buttons, theme). Set `"enabled": false` to turn it off. |
| `version.json` | Latest version + `blockedVersions` list used to hard-block broken releases. |
| `config.json` | Feature flags and links (Discord, TikTok upload URL, size-warning threshold). |
| `changelog.json` | Version history, one entry per release. |
| `vercel.json` | Adds CORS headers so `fetch()` from the extension's `chrome-extension://` origin isn't blocked, and disables caching on all `.json` responses. |

## Updating

Edit the relevant `.json` file directly on GitHub (pencil icon → edit → commit to `main`). Vercel auto-deploys on push, usually live within ~30 seconds. The extension fetches with `cache: "no-store"`, so there's no need to bump a cache-busting query param.

## Schema notes

`announcement.json`:
```json
{
  "enabled": true,
  "blockedVersions": [],
  "announcement": {
    "badge": "string",
    "header": { "title": "string", "subtitle": "string" },
    "content": { "title": "string", "message": "string" },
    "cards": [{ "title": "string", "description": "string" }],
    "buttons": [{ "text": "string", "action": "open|close", "url": "string", "primary": true }],
    "theme": { "primary": "#hex" },
    "footer": { "text": "string" }
  }
}
```

`buttons[].action` only supports `"open"` (opens `url` in a new tab) and `"close"` (dismisses the banner) — the extension ignores any other value.

## Safety

The extension only ever reads plain strings/booleans/arrays from these files and renders them into a fixed UI template (`textContent`, never `innerHTML`). No field here can inject scripts, markup, or arbitrary styling beyond a validated hex color.
