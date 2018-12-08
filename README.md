# DiscordWarningBot

This is a C#/.NETCore based Discord bot.

Created by mcmonkey4eva for use on my own Discord groups, though made available to the public for usage elsewhere or forking.

## Support Notice

I made this largely for usage on servers I control and may not have documented everything super thoroughly or made it as customizable as possible. If you're unsure how to use it or you want better customization options, please feel free to contact me to ask - either post an issue on GitHub, or send a DM to me (`mcmonkey#6666`) on Discord (GitHub issues are preferred for project-related contact).

## How It Works

- A Discord bot user, controlled by this program, will sit idly in your Discord guild.
- At any time, a 'helper' ranked user (a role to give to moderators, name can be changed in config) can issue a `warn` or `listwarnings` command on any other user.
    - To issue a warning, a helper can use the following format: `@Bot warn @User normal Did a bad thing!` where `@Bot` is a mention of this bot, and `@User` is the user to warn, and `normal` is any of the following levels:
        - `Minor`: Not very significant warning.
        - `Normal`: Standard warning. Counted towards automatic muting.
        - `Serious`: More significant than normal warning. Counted extra towardsd automatic muting.
        - `InstantMute`: Extremely significant warning. Induces an immediate automatic muting.
    - To list the warnings of another user, a helper can type: `@Bot listwarnings @User` where `@Bot` is a mention of this bot, and `@User` is the user to list the warnings for.
- At any time, a user with the `botcommander` role (create a role with this exact name if needed) can issue a `restart` command to restart the bot.
- At any time, a user may use the commands `help` or `hello` for general information, or `listwarnings` to see their own active warnings.
- When a user receives a warning:
    - That warning is recorded permanently, including metadata about it (timestamp, helper giving it, etc).
    - Depending on severity that warning can cause the bot to mute a user (if `InstantMute` is used, or if multiple `Normal` or `Serious` warnings were issued within a few days long period).

## Tips

- Create a mute role that has `Send Messages` and `Add Reactions` both disabled in all relevant text channels, and possibly `Speak` disabled in voice channels.
- Give the mute role a clear name (like `Muted`) and a different-than-normal color (probably a darker one) to clearly indicate the user is muted.
- You may want to add a text channel for handling issues. Make this channel only visible to muted users and moderators (and not visible to general users), and allow muted users to post in it. That way they can plead their case, request explanation of the mute, or whatever else as relevant - separated from the public channels that they've been muted in. Depending on preference, you may want to block `View Message History` permission from muted users in this channel (so they can't look at past issues) or just clear out the channel occasionally.

## Setup

Gathering the files note:
- In addition to a `git clone` of this repository, you need to clone the sub-repository, using a command like `git submodule update --init --recursive` (the `start.sh` script will do this for you by default).

The `start.sh` file is used by the `restart` command and should be maintained as correct to the environment to launch a new bot program instance... points of note:
- It starts with a `git pull` command and then a `git submodule update...` command to self-update. If this is not wanted, remove it. Be careful what repository this will pull from (a fork you own vs. the original repository vs. some other one...)
- It uses a `screen` command to launch the bot quietly into a background screen. The `screen` program must be installed for that to work. Alternately, replace it with some other equivalent background terminal program.
- The restart command will run this script equivalently to the following terminal command: `bash ./start.sh 12345` where `12345` is the ID number for the channel that issued a restart command.

To configure the bot:
- Create directory `config` within this bot's directory.
- Within the `config` directory, create file `token.txt` containing only the Discord bot token without newlines or anything else.
    - To create a bot token, refer to official Discord documentation. Alternately, you can follow the bot-user-creation parts of https://discord.foxbot.me/docs/guides/getting_started/intro.html (ignore the coding parts, just follow the first bits about creating a bot user on the Discord application system, and getting the token).
- Within the `config` directory, create file `config.fds` (a FreneticDataSyntax file) with the following options (See also the full file text sample below):
    - `helper_role_name` set to the name of the role for helpers (who can issue warnings).
    - `mute_role_name` set to the name of the role for muted users (given automatically by the bot).
    - `attention_notice` set to text to append to a mute notice. You can use Discord internal format codes, including `<@12345>` where `12345` is a user's ID to create a Discord `@` mention (helpful to auto-mention an admin).
    - `incidents_channel` set to a channel ID for where to post about a muting incident. Can be set to a list, and the first ID that's valid for any given guild will be used (useful for a bot operating across multiple Guilds).

`config.fds` sample text content (the mention code is my own user ID, `mcmonkey#6666`):
```
helper_role_name: helper
attention_notice: (Attn: <@105458332365504512>)
mute_role_name: muted
incidents_channel: 493100185665142795
```

To start the bot up:
- Run `./start.sh` while in the bot's directory (You made need to run `chmod +x ./start.sh` first).

To view the bot's terminal:
- Connect to the screen - with an unaltered `start.sh` file, the way to connect to that is by running `screen -r DiscordWarningBot`.

## Copyright/Legal Info

The MIT License (MIT)

Copyright (c) 2018 Alex "mcmonkey" Goodwin

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
