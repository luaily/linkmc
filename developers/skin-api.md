# Skin renders

LinkMC serves skin textures and renders for every Minecraft: Java Edition player. Each
endpoint accepts a username or a UUID in place of `PLAYER`.

Skin changes appear on LinkMC within about five minutes.

## Textures

| Endpoint | Returns |
|---|---|
| `/api/profiles/PLAYER/skin/raw` | The skin texture exactly as Mojang serves it. |
| `/api/profiles/PLAYER/skin/processed` | The texture as a 64×64 skin, with older 64×32 skins converted and stray transparent pixels cleaned up. |
| `/api/profiles/PLAYER/skin/cape` | The player's cape texture, or `404 Not Found` if they have no cape. |

## Renders

| Endpoint | Default height | Render |
|---|---|---|
| `/api/profiles/PLAYER/skin/2d/face` | 128 px | Front of the head, with the hat layer. Square. |
| `/api/profiles/PLAYER/skin/2d/bust` | 256 px | Front view from the waist up. |
| `/api/profiles/PLAYER/skin/2d/full` | 256 px | Full body, front view. |
| `/api/profiles/PLAYER/skin/3d/head` | 256 px | Isometric head. |
| `/api/profiles/PLAYER/skin/3d/bust` | 256 px | Isometric view from the waist up. |
| `/api/profiles/PLAYER/skin/3d/full` | 256 px | Isometric full body. |
| `/api/profiles/PLAYER/skin/drops` | 256 px | The chunky Drops-style avatar. See [Drops avatar](#drops-avatar). |

### Parameters

| Parameter | Values | Description |
|---|---|---|
| `height` | `16` to `4096` | Output height in pixels. The width scales to match. |
| `format` | `png`, `jpg`, `svg` | Output format. Defaults to `png`. |
| `lossless` | No value | Compresses PNG output losslessly. Has no effect on other formats. |

Examples:

```
/api/profiles/Notch/skin/2d/full?height=512
/api/profiles/Notch/skin/3d/head?format=svg
/api/profiles/Notch/skin/2d/face?height=64&lossless
```

If the 3D service is busy, a 3D endpoint can return the matching 2D render instead.

## Drops avatar

```
/api/profiles/PLAYER/skin/drops
```

The Drops avatar is a stylized, front-facing figure built from the player's skin.

| Parameter | Values | Description |
|---|---|---|
| `format` | `png`, `svg` | Defaults to `png`. |
| `height` | `23` to `2048` | PNG only. The image is square, and the figure is centered in it. |
| `overlay` | `true`, `false` | Whether to include the outer skin layers. Defaults to `true`. |

For most uses, request the SVG. It scales to any size without blurring, so one file covers
every size you need.

## SVG output

An SVG render draws each skin pixel as a square, so it stays sharp at any size and is often
smaller than a large PNG. To keep the edges crisp when you scale a PNG render in a browser,
add `image-rendering: pixelated` to the image's CSS.

## Output size

2D body renders are two pixels wider than the skin's body, so slim and classic skins have
different widths at the same height:

| Variant | Arm width | Native width × height |
|---|---|---|
| Classic (Steve) | 4 px | 18 × 32 |
| Slim (Alex) | 3 px | 16 × 32 |

Renders scale from these sizes to the `height` that you request.

## Skin variant

The profile response includes the skin's model in `linkmc.skin.variant`: `CLASSIC` or
`SLIM`. LinkMC uses the model that Mojang reports. If Mojang doesn't report one, LinkMC
detects it from the texture.
