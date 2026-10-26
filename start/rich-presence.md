# Rich presence

The LinkMC mod shows what you're playing in Minecraft: Java Edition on your LinkMC profile
and as your Discord and Fluxer status. You decide what it shares.

> [!NOTE]
> The LinkMC mod isn't released yet. This page describes how it works so that you can
> plan for it. It's planned for these loaders:
>
> - **Fabric**, which also works on Quilt
> - **NeoForge**
> - **Forge**, for Minecraft 1.20.1 and older

## What the mod shows

| You're… | Your status says |
|---|---|
| On a [large network](large-networks.md), such as Hypixel | **Playing on Hypixel**, with the network's address, icon, and player count |
| On a server [listed on LinkMC](server-listing.md) | **Playing on** and the server's name and icon. Never its address |
| On any other server | **Multiplayer** |
| In a singleplayer world | **Singleplayer**, with the world's name |
| On the title screen | **In the menus**, or nothing |

Your status also shows:

- The dimension you're in, such as The Nether, when you allow it.
- How long you've been playing.
- Your [Drops avatar](skin-api.md) as the small image.

Only large networks and servers listed on LinkMC are ever named. For every other server, the
mod says **Multiplayer** and nothing else. Private servers, LAN worlds, and IP addresses are
never shown, and LinkMC doesn't keep their addresses.

A listed server's owner can turn this off for their server. Players on it then show
**Multiplayer**.

Your Discord and Fluxer status also gets a **View on LinkMC** button that opens the server's
LinkMC page.

## Connect the mod to your account

You connect the mod once on each computer that you play on.

### Connect automatically

1. Start Minecraft with the LinkMC mod installed.
2. The mod signs in to the LinkMC verify server in the background and posts a link in chat.
3. Click the link. It opens linkmc.app.
4. Check that the player shown is you, then click **Connect**.
   If you don't have a LinkMC account yet, accept the Terms of Service to create one.
5. Go back to Minecraft. The mod finishes connecting within a few seconds.

> [!WARNING]
> Don't share the link. For 10 minutes, anyone who opens it can sign in to LinkMC as you.
> If you didn't ask for a link, click **This wasn't me**.

The verify server only accepts IPv6 connections for now. If the mod can't reach it, connect
with a token instead.

### Connect with a token

1. On LinkMC, go to [Settings → Social](https://linkmc.app/settings#rpc).
2. Under **Minecraft rich presence**, enter a device name and click **Create token**.
3. Copy the token and paste it into the mod's settings.

LinkMC shows the token only once. A token can only report what you're playing. It can't
sign in or change your account. If you lose it, remove it and create a new one.

## Choose what to share

You can control what the mod shares in two places. If something is hidden in either place,
it's hidden everywhere: on your profile and in your Discord and Fluxer status.

**On LinkMC**, go to [Settings → Social](https://linkmc.app/settings#rpc) →
**Minecraft rich presence**:

| Setting | When it's off |
|---|---|
| **Use rich presence** | The mod stops reporting, and your current status is cleared |
| **Show on my profile** | Your LinkMC profile doesn't show what you're playing. Your Discord and Fluxer status still do |
| **Show the server** | Every server shows as **Multiplayer** |
| **Show the world** | The dimension and your singleplayer world's name are hidden |

**In the mod**, the **Show server** and **Show world** options do the same. When they're off,
the mod still tells LinkMC that you're playing, but not where, and LinkMC doesn't store it.

### Hide single servers

To keep one server private while still showing the others, hide it:

1. Go to [Settings → Social](https://linkmc.app/settings#rpc) → **Minecraft rich presence**.
2. Under **Hidden servers**, search for the server or network, and then click **Hide**.
   If you're playing on it right now, you can also click **Hide** next to **You're on…**.

A hidden server always shows as **Multiplayer**, and LinkMC doesn't keep which server it was.
To show it again, click **Show** next to it.

## Manage your devices

Each computer that you connect appears under **Devices** in
[Settings → Social](https://linkmc.app/settings#rpc), with when it was last used.
To disconnect a computer, click the remove button next to it. That computer's status
disappears right away, and its mod stops reporting until you connect it again.

You can have up to five devices. Connecting a sixth removes the one you used least recently.

## Your data

While you play, the mod sends LinkMC:

- **Your Minecraft account:** your UUID and username, so that LinkMC knows whose status it is.
- **The server:** the address you typed, its icon, and its player count. LinkMC uses the address
  only to find out whether it's a large network or a listed server, and then forgets it. It
  keeps which server it was, the icon, and the player count only for servers that it names and
  that you haven't hidden.
- **The world:** whether you're in singleplayer, your dimension, and your singleplayer world's
  name, unless you hide them.
- **Times:** when you started playing, and when each update was sent.
- **The mod:** its version, your mod loader, and your Minecraft version.

Your status disappears about two minutes after you stop playing, and LinkMC deletes it soon
after. For more information, see the [privacy policy](privacy.md).
