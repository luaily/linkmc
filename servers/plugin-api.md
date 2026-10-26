# Plugin API

The plugin API is how the LinkMC Bridge plugin or mod talks to LinkMC. It proves your
server's Minecraft identity and reports its player list and status. Use this reference if
you build your own integration.

- **Base URL:** `https://linkmc.app/api/plugin/v2`
- **Requests:** `POST`, with a JSON body, signed with Ed25519.

## Credentials

To create a credential, go to your server's console, open **Identity**, and then create a
credential. LinkMC shows it once:

```yaml
client-id: "lmcsrv_…"     # Public. Identifies your server.
private-key: "lmcsk_…"    # Secret. Signs your requests.
```

The private key is `lmcsk_` followed by the 32-byte Ed25519 seed, encoded as base64url
without padding. LinkMC keeps only the public key, so it can't recover a lost private key.
If you lose it, revoke the credential and create a new one.

## Sign a request

1. Build the string to sign. Join these values with `\n`, with no trailing newline:

   ```
   METHOD
   PATH
   TIMESTAMP
   NONCE
   BODY_HASH
   ```

   | Value | Description |
   |---|---|
   | `METHOD` | `POST`. |
   | `PATH` | The URL path only, for example `/api/plugin/v2/heartbeat`. |
   | `TIMESTAMP` | The current Unix time in seconds. It must be within five minutes of LinkMC's clock. |
   | `NONCE` | A random string of up to 128 characters. Never reuse one. |
   | `BODY_HASH` | The lowercase hex SHA-256 hash of the exact body bytes that you send. |

1. Sign the UTF-8 bytes of that string with your private key.
1. Send the request with these headers:

   | Header | Value |
   |---|---|
   | `X-LinkMC-Client` | Your client ID. |
   | `X-LinkMC-Timestamp` | `TIMESTAMP`. |
   | `X-LinkMC-Nonce` | `NONCE`. |
   | `X-LinkMC-Signature` | The signature, encoded as base64url without padding. |
   | `Content-Type` | `application/json`. |
   | `User-Agent` | A name for your integration, for example `LinkMC-Paper/2.0.0`. |

Java 15 and later support Ed25519. Build the key from the 32-byte seed with
`new EdECPrivateKeySpec(NamedParameterSpec.ED25519, seed)`, and sign with
`Signature.getInstance("Ed25519")`.

## Endpoints

| Endpoint | When to call it | Body |
|---|---|---|
| `/hello` | When the server starts. | `platform` (for example `paper`, `velocity`, or `fabric`), `plugin_version`, `mc_version`, `advertised_host`, and `software`. |
| `/heartbeat` | Every `heartbeat_interval` seconds. `/hello` returns the interval. | `online_players` (`[{uuid, username}]`), `online_count`, `motd`, `max_players`, and `sent_at_ms`. |
| `/software` | When your plugin or mod list changes. | `software`: `[{name, version, type}]`, where `type` is `plugin`, `mod`, `datapack`, `library`, or `other`. |
| `/goodbye` | When the server stops. | `{}` |

The first successful `/hello` verifies your server's Minecraft identity. The plugin API can
only report status. It can't change your server's name, address, or description.

### Player lists

- While `online_count` is 1,000 or fewer, send `online_players`. LinkMC shows this list on
  your server's page.
- Above 1,000 players, send only `online_count`. LinkMC ignores lists sent with a higher
  count.
- If you don't send `online_count`, LinkMC uses the length of `online_players`.

LinkMC counts a visit when a player appears who wasn't in the previous heartbeat. If
heartbeats stop for five minutes, LinkMC clears the player list.

### Uptime

A heartbeat in the last 90 seconds counts as LinkMC's once-a-minute status check, so LinkMC
doesn't ping your server while the plugin reports.

## Errors and limits

| Status | Cause |
|---|---|
| `401 Unauthorized` | The signature is missing or wrong, the timestamp is too old, the nonce was already used, or the credential was revoked. |
| `413 Payload Too Large` | The body is over 256 KB. |
| `429 Too Many Requests` | More than 12 requests a minute, or 400 an hour, from one credential. |
