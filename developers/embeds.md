# Link previews and oEmbed

When someone shares a LinkMC profile or server link in Discord, on social media, or in any
app that shows link previews, LinkMC provides a card image with the player's or server's
details. You can also use these cards in your own site or app.

## Pages with cards

| Page | Card shows |
|---|---|
| `https://linkmc.app/profile/PLAYER` | Drops avatar, username, UUID, flags, and up to four linked accounts. |
| `https://linkmc.app/servers/SERVER_ID` | Server name, address, players, version, uptime, and tags. |
| `https://linkmc.app/servers/SERVER_ID/status` | Online status, player count, and uptime bars for the last 24 hours and 14 days. |

Server cards exist for listed servers only. Every other page uses a general LinkMC card.

## oEmbed

LinkMC supports [oEmbed](https://oembed.com/) for the pages in the preceding table. Each page
advertises its oEmbed URL in a `<link rel="alternate" type="application/json+oembed">` tag,
so apps that support oEmbed discovery find it automatically.

```
GET /api/oembed?url=PAGE_URL
```

| Parameter | Description |
|---|---|
| `url` | Required. The URL of a profile, server, or status page on `linkmc.app`. |
| `format` | Optional. Only `json` is supported. `xml` returns `501 Not Implemented`. |
| `maxwidth` | Optional. The widest image that you can show, in pixels. |
| `maxheight` | Optional. The tallest image that you can show, in pixels. |

The response is a `photo`:

```json
{
  "version": "1.0",
  "type": "photo",
  "title": "Notch — LinkMC",
  "author_name": "Notch",
  "author_url": "https://linkmc.app/profile/Notch",
  "provider_name": "LinkMC",
  "provider_url": "https://linkmc.app",
  "url": "https://linkmc.app/api/embed/profile/069a79f444e94726a5befca90e38aaf5.png?v=67368d89c2e80af2",
  "width": 1280,
  "height": 640,
  "cache_age": 3600
}
```

A URL that isn't on `linkmc.app`, or a page without a card, returns `404 Not Found`.

## Card images

You can request a card image directly:

| Endpoint | Card |
|---|---|
| `/api/embed/profile/PLAYER.png` | Player card |
| `/api/embed/server/SERVER_ID.png` | Server card |
| `/api/embed/server/SERVER_ID/status.png` | Server status card |
| `/api/embed/site.png` | General LinkMC card |

Cards are 1280 × 640 PNG images. To get a smaller card, add `w` with a width from `200` to
`1280`. The height is always half the width.

### Caching

A card's URL from the oEmbed response or the page's `og:image` tag includes a `v`
parameter. `v` changes whenever anything on the card changes, so you can cache that URL for
as long as you like. Without `v`, or with an old `v`, LinkMC tells caches to keep the
image for five minutes.

## OpenGraph and Twitter cards

Each page also includes OpenGraph and Twitter card tags that point to the same image. On
profile pages, `theme-color` is the player's locator bar color. On server pages, it's the
server's uptime color.
