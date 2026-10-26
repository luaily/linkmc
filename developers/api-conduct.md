# API code of conduct

LinkMC is a free service run by one person. This code of conduct explains how to use the API
so that it stays fast and free for everyone. It's part of the [API terms of use](api-terms.md).

## Identify yourself

- Send a `User-Agent` that names your app and links to it, for example
  `AppName/1.0.0 (+https://example.com)`.
- Keep the link current, so that LinkMC can reach you if something goes wrong.

## Be gentle with requests

- Cache responses. Profiles rarely change from minute to minute, and skins change at most
  every few minutes.
- Use UUIDs instead of usernames where you can.
- Load skin renders lazily when a page shows more than five of them.
- Spread out background jobs instead of sending bursts of requests.
- If you get `429 Too Many Requests`, wait before you try again, and wait longer each time
  that it happens.

## Respect players

- Show players' data the way that they chose to show it on LinkMC. Don't reveal accounts or
  servers that they hid.
- Don't use LinkMC data to track players across services, or to contact them without their
  consent.
- Don't use the API to find, harass, or target any player or community.

## Respect server owners

- Use a server's own credentials only on that server. Never share a plugin private key.
- Don't report false player lists, statuses, or other data through the plugin API.

## Report problems

If you find a security problem or a bug that exposes data, email `development@linkmc.app`
instead of using it or posting it in public.
