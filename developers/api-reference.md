# API reference

The LinkMC API returns public data about Minecraft players and listed Minecraft servers. You
can call it from a server, a bot, a mod, or directly from a web page.

To try any endpoint in your browser, use the
[interactive API explorer](https://linkmc.app/api/docs).

By using the API, you agree to the [API terms of use](api-terms.md) and the
[API code of conduct](api-conduct.md).

## Before you begin

- **Base URL:** `https://linkmc.app`
- **Format:** Responses are JSON unless an endpoint returns an image.
- **Player identifiers:** Every player endpoint accepts a username or a UUID, with or without
  dashes. Prefer UUIDs: they never change, and they don't need a username lookup.
- **Browsers:** Any website can call the API from the browser. Requests from other sites
  are sent without cookies, so they only see public data.

### Identify your app

Send a `User-Agent` header that names your app and links to it:

```
User-Agent: AppName/1.0.0 (+https://example.com)
```

Requests with a missing or generic user agent get `403 Forbidden`. This includes the
default user agents of curl, Python Requests, Java, OkHttp, and Go. Browsers don't need to
set anything.

```bash
curl -A "AppName/1.0.0 (+https://example.com)" https://linkmc.app/api/profiles/Notch
```

### API keys

API keys are optional. You can create keys on the **API keys** page while signed in, but
no endpoint requires one.

### Use the API responsibly

LinkMC is free to use, and these practices help keep it that way:

- Cache responses where you can, especially skin renders.
- If a page shows more than five skin renders, load them lazily, for example with
  `loading="lazy"` on each `<img>`.
- Load large lists of profiles in pages rather than all at once.

## Errors

Errors return an HTTP status code and a JSON body with a `detail` message:

```json
{ "detail": "Player not found." }
```

| Status | Meaning |
|---|---|
| `400 Bad Request` | A parameter is missing or invalid. |
| `403 Forbidden` | Your request has no `User-Agent`, or a blocked one. |
| `404 Not Found` | The player, server, or route doesn't exist. |
| `422 Unprocessable Entity` | A query parameter is out of range. |
| `429 Too Many Requests` | You sent too many requests. Wait, and then try again. |
| `503 Service Unavailable` | A dependency, such as the skin service, is temporarily down. |

## Profiles

### Get a profile

```
GET /api/profiles/PLAYER
```

Replace `PLAYER` with a username or UUID. Works for every Minecraft: Java Edition player.
Players who haven't registered on LinkMC have `"registered": false` and no linked accounts.

```json
{
  "username": "Notch",
  "uuid": "069a79f4-44e9-4726-a5be-fca90e38aaf5",
  "uuid_raw": "069a79f444e94726a5befca90e38aaf5",
  "timestamp": 1791956072,
  "region_country": "SE",
  "locator_bar_color": "#b3adb2",
  "linkmc": {
    "registered": true,
    "verified": true,
    "badge": null,
    "style": { "gradient": { "start": "#4ade80", "end": "#22d3ee" } },
    "linked_accounts": {
      "discord": { "username": "notch", "display_name": "Notch", "uid": 123456789012345678 },
      "github": { "handle": "notch" }
    },
    "primary_im": "discord",
    "socials": { "modrinth": "notch" },
    "card_links": [{ "platform": "discord", "handle": "notch", "display_name": "Notch" }],
    "server_memberships": [
      { "id": 2, "name": "Example SMP", "icon_url": null, "role": "member", "joined_at": "2026-10-12 05:11:24" }
    ],
    "skin": {
      "variant": "CLASSIC",
      "2d": { "face": "/api/profiles/069a…/skin/2d/face", "bust": "…", "full": "…" },
      "3d": { "head": "…", "bust": "…", "full": "…" },
      "raw": "/api/profiles/069a…/skin/raw",
      "processed": "/api/profiles/069a…/skin/processed",
      "cape": "/api/profiles/069a…/skin/cape"
    }
  },
  "live_server": null
}
```

| Field | Description |
|---|---|
| `registered` | `true` if the player has signed in to LinkMC. |
| `verified` | `true` if LinkMC staff verified the player. Players can be verified without registering. |
| `badge` | The ID of the player's badge, or `null`. See [Badges](#badges). |
| `linked_accounts` | Accounts the player linked and chose to show. |
| `card_links` | Up to four accounts that the player chose for their player card. |
| `region_country` | Two-letter country code, or `null`. |
| `locator_bar_color` | The player's color on the Minecraft locator bar. |
| `live_server` | The listed server that the player is on right now, or `null`. |

> [!WARNING]
> Discord's `uid` is a 64-bit integer. In JavaScript, numbers this large lose precision when
> you parse them. Read the value as a string, or use a JSON parser that supports big integers.

### Search for players

```
GET /api/profiles/search?q=QUERY
```

Finds LinkMC players whose usernames contain `QUERY`. Searches include registered and
verified players only. Matching ignores case, spaces, and symbols.

| Parameter | Description |
|---|---|
| `q` | At least three characters. A full UUID returns that player. |

Results are ordered exact match first, then prefix matches, then other matches. The response
contains up to five `results`. When `complete` is `true`, `results` holds every match, so
you can narrow a longer query on your side without another request.

```json
{
  "query": "not",
  "count": 1,
  "total": 1,
  "complete": true,
  "results": [
    {
      "username": "Notch",
      "uuid": "069a79f4-44e9-4726-a5be-fca90e38aaf5",
      "uuid_raw": "069a79f444e94726a5befca90e38aaf5",
      "badge": null,
      "verified": true,
      "registered": true,
      "skin": { "face": "/api/profiles/069a…/skin/2d/face?height=64" }
    }
  ]
}
```

### Skins

For skin textures and renders, see [Skin renders](skin-api.md).

## Servers

These endpoints return listed servers only. A server that isn't listed returns
`404 Not Found`.

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/servers` | Listed servers, featured first, then by most recent bump. |
| `GET` | `/api/servers/SERVER_ID` | One server's listing, staff, and linked communities. |
| `GET` | `/api/servers/SERVER_ID/status` | The server's latest status. |
| `GET` | `/api/servers/SERVER_ID/uptime` | Uptime and player history. |
| `GET` | `/api/servers/SERVER_ID/members` | Players who joined the server on LinkMC. |

### List servers

| Parameter | Description |
|---|---|
| `q` | Optional. Text to search for in names and descriptions. |
| `tags` | Optional. Comma-separated tags. |
| `limit` | Optional. Number of results. |
| `offset` | Optional. Number of results to skip. |

### Get status and uptime

LinkMC checks each listed server about once a minute. The `/uptime` response contains the
latest check, 48 hourly buckets, and 14 daily buckets. Times are in UTC.

```json
{
  "now": { "online": true, "players": 12, "max": 100, "version": "Paper 1.21.4", "checked_at": "2026-10-14 05:40:03" },
  "hours": [{ "t": "2026-10-14T05", "samples": 60, "up": 60, "uptime": 1.0, "avg": 11, "peak": 14 }],
  "days": [{ "t": "2026-10-14", "samples": 340, "up": 340, "uptime": 1.0, "avg": 9, "peak": 21 }],
  "uptime_24h": 0.998,
  "uptime_7d": 0.991,
  "peak_24h": 21,
  "peak_7d": 34
}
```

`uptime` values are fractions from `0` to `1`, or `null` for buckets with no checks.

## Link previews

For card images and oEmbed, see [Link previews and oEmbed](embeds.md).

## Sign in with LinkMC

To let players sign in to your app with their LinkMC account, see
[Sign in with LinkMC](oauth.md).

## Site information

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/meta/badges` | Every badge, with its name, description, color, and icon URL. |
| `GET` | `/api/meta/badges/BADGE_ID` | One badge. |
| `GET` | `/api/meta/version` | The current LinkMC version. |
| `GET` | `/api/costs` | LinkMC's running costs and donation goal. |

### Badges

Badge icons are SVG files at `/brand/badges/BADGE_ID.svg`.

## Brand assets

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/design-resources/` | A JSON index of logos, fonts, and other brand files. |
| `GET` | `/design-resources/FILENAME` | Download one brand file. |

Before you use LinkMC's brand, read [Branding and attribution](branding.md).
