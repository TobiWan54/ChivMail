# ModMail
A Modmail bot for Discord.

I originally made this as a custom project for the official Chivalry 2 Discord server, but it is now server-agnostic.
Although it is not a fork of the widely-used Modmail bot made by Kyber: https://github.com/kyb3r/modmail, the experience will be very similar.

Message me on Discord if you have any questions, or if you need any help with using it.

## Features
When a user messages the bot, a new channel is created in a category in your server, representing a ticket. As you would expect from a Modmail bot, 
all the messages in the ticket are logged when the ticket is closed. You can also blacklist users for misuse, and set pre-defined snippets which can 
be sent with a single command.

However, this bot has a few additional features that make it unique:

- **Discussion Threads:**  Each ticket channel has a thread automatically created which is also logged when the ticket is closed.
This allows mods to discuss freely, without the risk of accidentally sending a rude message to the user! From v.1.1.0 onwards there is also an
option to send messages only with the commands !reply and !areply (anonymous).

- **!search:** Allows you to retrieve the logs of a user's previous tickets, and to search for specific phrases within them.

- **!send:** Creates a new ticket and sends an anonymous message to a user that does not already have a ticket open.

Once you have the bot running, the !help command will show you a list of all the available commands and their sub-commands.

## Setup

To use this bot, you will have to create a bot account for it on the [Discord Developer Portal](https://discord.com/developers)
and host the script yourself. Oracle Cloud and Google Cloud both have free tiers that provide sufficiently-resourced instances 
(virtual machines) for hosting.

The script requires Python 3.10 or higher and the packages listed under Dependancies.

### Configuration
Fill out [config.json](https://github.com/TobiWan54/ModMail/tree/main/templates/config.json) with your own values, and put it in the same 
directory as `modmail.py`. Then run the script, and your bot will be online!

Snippets are stored in `snippets.json`, the blacklist is stored in `blacklist.json` and ticket logs are indexed in the SQLite database `logs.db`.
Along with `counter.txt` these are automatically created by the script, so do not delete them.

#### config.json

- `token` is your bot account's token from the Discord Developer Portal.
- `guild_id` is your server's ID.
- `category_id` is the ID of the category that tickets to be created in. You will have to create this yourself.
- `log_channel_id` is the ID of the channel that ticket logs will be sent in.
You will have to create this yourself. If it is the tickets category, make sure that it has no topic, or it will break the bot.
- `attachment_log_channel_id` is the ID of the channel that attachments will be logged in. It can be the same as log_channel_id above.
Set to 0 to enable legacy logging, which makes attachments only viewable for up to 24 hours but tickets close much faster.
- `error_channel_id` is the ID of the channel that you want error messages to be sent in.
This can also be the log channel if you want.
- `helper_role_id` is the ID of your server's helper or trainee role, which can use everything except the blacklist.
If you do not have a helper role, set this to the same value as mod_role_id below.
- `mod_role_id` is the ID of your server's moderator role, which can use everything.
- `bot_owner_id` is the ID of the user that error tracebacks will be DM'd to. Access to the !eval command, which allows you to run code,
is given to the bot owner(s) in the Discord Developer Portal, not this user (although they will likely be the same user).
- `prefix` is the bot's prefix.
- `open_message` is the text that users will receive under "Ticket Created" when they open a ticket.
- `close_message` is the text that users will receive under "Ticket Closed" when a mod closes their ticket.
- `anonymous_tickets` (true/false) names ticket channels anonymously, rather than using the name of the user.
- `send_with_command_only` (true/false) only allows messages to be sent using !reply and !areply.

The !refresh command will make the script re-read the config file, so you can change these values without restarting the bot.
It also resets a few things behind the scenes which may help fix some issues.

### Dependancies

The required/working versions of these packages are listed in `requirements.txt`. To install them, use `pip install -r requirements.txt`.

[bleach](https://github.com/mozilla/bleach)

[discord.py](https://github.com/Rapptz/discord.py)

[aiohttp](https://github.com/aio-libs/aiohttp) (this is installed with discord.py)
