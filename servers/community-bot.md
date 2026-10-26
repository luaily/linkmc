# Community bot

The LinkMC bot connects your Discord or Fluxer community to your server's listing on LinkMC.
It proves that the community belongs to you, and it can verify members against their LinkMC
profiles, set Minecraft nicknames, assign region roles, and post live server statistics.
Stoat support is coming.

This page describes the Discord bot first. The Fluxer bot does the same things with prefix
commands; see [Fluxer](#fluxer).

## Add the bot

1. On LinkMC, go to **Servers** > **My servers**, open your server, and then go to
   **Community**.
1. Click **Discord**. Your Discord account must be linked to LinkMC, and you need the
   **Administrator** permission in the Discord server.
1. Add the bot with the invite link that LinkMC shows. The bot asks only for the permissions
   that it uses: Manage Roles, Manage Channels, Manage Nicknames, View Channels, Send
   Messages, Embed Links, and Read Message History.
1. In any channel, run `/lmc link CODE`. The code works once and expires after five minutes.

Your server's console then has a tab for the community.

> [!IMPORTANT]
> In **Server Settings** > **Roles**, drag the bot's role above the `lmc-*` roles and above
> every member that it should rename. The bot can never rename the server owner.

## Settings

Change settings in the community's console tab, or with `/lmc settings` in Discord.

| Setting | What the bot does |
|---|---|
| **Verification: Off** | Doesn't verify anyone. Nicknames and region roles still work. |
| **Verification: Optional** | Members click **Verify** in `#verify` to get the `lmc-verified` role as a badge. Every channel stays visible. |
| **Verification: Strict** | Removes **View Channels** from `@everyone` and gives it to `lmc-verified`. Unverified members only see channels that explicitly allow `@everyone`, like `#verify` and `#rules`. The bot restores the old permission when you turn strict mode off or unlink the community. |
| **Nicknames** | Renames verified members to their Minecraft username, and removes **Change Nickname** from `@everyone`. The bot refreshes names daily, so name changes carry over. |
| **Regions: Roles** | Gives verified members one of `lmc-NA`, `lmc-SA`, `lmc-EU`, `lmc-AS`, `lmc-AF`, or `lmc-OC`. |
| **Regions: Allow or block list** | Strict mode only. Members from countries that aren't allowed can't verify. You choose whether members with an unknown region can. The bot doesn't kick blocked members. |
| **Stats** | Adds a read-only `#server-statistics` channel with a status card that updates every 2.5 minutes. |

A member's region comes from their LinkMC profile, which LinkMC detects when they sign in. A
VPN can change it, so treat region lists as a filter, not a guarantee.

## How members verify

- **Joining:** Members whose Discord account is already linked to LinkMC are verified
  automatically. Everyone else sees `#verify`.
- **Verify button:** The bot looks the member up on LinkMC. If their Discord account isn't
  linked, the bot explains how to link it and shows a **Check again** button.
- **Bypass:** `/lmc bypass @MEMBER` gives `lmc-verified` without a LinkMC profile, for other
  bots or for staff without an account. Bypasses last through resyncs and appear in the
  audit log.

Unlinking Discord from LinkMC doesn't remove `lmc-verified`. Being blocked by a region list
does.

## Commands

| Command | Who can run it | Description |
|---|---|---|
| `/lmc link CODE` | Administrators | Links the community. |
| `/lmc unlink` | Administrators | Unlinks the community and restores `@everyone`. |
| `/lmc status` | Everyone | Shows the linked server and the current settings. |
| `/lmc settings` | Managers | Changes settings. Edit country lists on LinkMC. |
| `/lmc setup` | Managers | Turns on strict verification, after a preview of what stays visible. |
| `/lmc setup-undo` | Managers | Turns off strict verification. |
| `/lmc bypass @MEMBER` | Managers | Verifies a member without a profile. |
| `/lmc recheck @MEMBER` | Managers | Verifies one member again. |
| `/lmc resync` | Managers | Verifies every member again. |
| `/lmc stats [CHANNEL]` | Managers | Turns on the stats channel, optionally in a channel that you choose. |

Managers are members with the **Administrator** permission, plus your server's owner and
admins on LinkMC. The bot gives LinkMC owners and admins the `lmc-server-admin` role and keeps
it in sync with your staff list. To change settings from Discord, you must also be an owner
or admin of the server on LinkMC.

### Commands for everyone

These commands work in any server that has the bot, and in a direct message with the bot.
They don't need a linked community.

| Command | Description |
|---|---|
| `/whois user:@USER` | Shows the LinkMC player card of a Discord user who linked their account. |
| `/whois player:PLAYER` | Shows the player card of a Minecraft username or UUID. |
| `/serverstatus address:ADDRESS` | Pings a Minecraft: Java Edition server once, and shows whether it's online, its player count, version, and MOTD. |

The player card shows the player's Drops avatar, UUID, locator bar color, linked Discord
account, and up to three other accounts from their profile.

`/serverstatus` accepts `host`, `host:port`, or an IP address. It only pings public
addresses, and each person can run it a few times a minute.

## Statistics

The stats card and your server's status page show the same data. For the color key, see
[Uptime](server-listing.md#uptime).

## Fluxer

The Fluxer bot has the same features and settings as the Discord bot. Fluxer doesn't have
slash commands, buttons, or private replies, so the bot works a little differently:

- **Commands** start with `&`, for example `&whois Notch`.
- **Verifying:** Members react with ✅ on the message in `#verify`, or send `&verify`.
- **Private replies:** The bot sends results that only you should see, such as your
  verification result, in a direct message. If your direct messages are closed, it replies in
  the channel and deletes the reply after 30 seconds.
- **Turning on strict verification** takes two steps: `&admin setup` shows what stays
  visible, and `&admin setup confirm` applies it.

### Add the bot to Fluxer

1. On LinkMC, open your server's console, go to **Community**, and click **Fluxer**. Your
   Fluxer account must be linked to LinkMC, and you need the **Administrator** permission in
   the Fluxer community.
1. Add the bot with the invite link that LinkMC shows. Besides the Discord bot's permissions,
   it asks for Add Reactions, Manage Messages, and Attach Files, so that it can manage the
   ✅ reaction in `#verify`, tidy up command messages, and attach server icons.
1. In any channel, send `&link server CODE`.

### Link your Fluxer account with the bot

If you can't connect Fluxer on your dashboard, the bot can link it instead:

1. On your LinkMC dashboard, next to **Fluxer**, click **Use bot**.
1. Open a direct message with the LinkMC bot, and send the command that LinkMC shows:
   `&link profile CODE`. The code works once and expires after five minutes.

> [!WARNING]
> Only send profile codes in a direct message. Anyone who sees the code could link their own
> account to your profile, so the bot cancels any profile code that it sees in a server
> channel.

### Fluxer commands

| Command | Who can run it | Description |
|---|---|---|
| `&link server CODE` | Administrators | Links the community. Same as `&admin link CODE`. |
| `&link profile CODE` | Everyone, in a direct message | Links your Fluxer account to your LinkMC profile. |
| `&status` | Everyone | Shows the linked server and the current settings. |
| `&verify` | Everyone | Verifies you. In a direct message, it checks every linked community that you're in. |
| `&whois [@USER \| PLAYER]` | Everyone | Shows a player card. Without an argument, it shows yours. |
| `&serverstatus ADDRESS` | Everyone | Pings a Minecraft: Java Edition server once. |
| `&help` | Everyone | Lists the commands. |
| `&admin unlink` | Administrators | Unlinks the community and restores `@everyone`. |
| `&admin settings [SETTING VALUE …]` | Managers | Shows or changes settings, for example `&admin settings nicknames on stats off`. Edit country lists on LinkMC. |
| `&admin setup [confirm]` | Managers | Turns on strict verification. |
| `&admin setup-undo` | Managers | Turns off strict verification. |
| `&admin bypass @MEMBER` | Managers | Verifies a member without a profile. |
| `&admin recheck @MEMBER` | Managers | Verifies one member again. |
| `&admin resync` | Managers | Verifies every member again. |
| `&admin stats [#CHANNEL]` | Managers | Turns on the stats channel. |
| `&admin help` | Managers | Lists the admin commands. |
