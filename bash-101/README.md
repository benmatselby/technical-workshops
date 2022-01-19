# Bash/Linux/Scripting 101

**Time allocation:** 45 minutes

This was last run on 8th February 2021 and 31 people attended.

This workshop will be conducted in the style of a group code review. We will walk you through various bash scripts and explain what the commands do, and why. We will cover any tips and tricks along the way, such as using shellcheck. Bring your questions, and we will do our best to answer on the spot (no pressure!).

## Notes

- Basic commands (What would these be?)
  - `man` - The manual application. Use this to find information about all the other commands, and options.
  - `df` | `du` - Disk related commands.
  - `ls` | `find` | `grep` | `git ls-files` - File based commands finding, searching in files.
  - `less` | `more` | `tail` | `cat` - Content of files.
  - `ps` | `kill` | `pgrep` | `top` | `stop` - Process management.
  - `which` | `type` - Understanding the type of aliases/functions/commands and where they are installed.
  - `sed` - Sed is really useful for automating changes to files/templates. We extensively use this in our solutions (e.g. creating a new Jenkins Instance, or updating the changelogs for our releases).
    - **e.g.** `sed "s|{INSTANCE}|bobbety|g" jenkins.yaml`
  - `history` - Shows you what you have run before.
    - **TIP**: Add a space to not add to history, to stop passwords being leaked etc.
  - `ctrl r` | `clear` | `ctrl k` - Terminal screen management commands.
  - `echo $?` - Get the exit status of the last command.
  - `!$` - Get the last param of the last command.
  - `!!` - Get the entire last command, so `sudo !!` runs the last command as sudo.
- Pipes
  - Chaining commands together (Single reason idea for commands).
  - Takes the output from one command and **pipes** it into the next command.
  - `env | sort` - Sort all your environments so it's easier to find what you're looking forward. You could even create this as an alias.
  - `history | grep [command]` - Find a command in your history (commands you have run before).
- Output redirection
  - Caring, or not, about output etc.
  - ` ls > /dev/null 2>&1` - Documenting this because I _always_ forget. Redirect all output to "dev null" meaning it goes nowhere.
- `nohup` (no hangup) | `&` background tasks
  - Two different methods, why bother? Shell capture, and no monitoring.
  - `&` will send the command to the "background", therefore freeing up your terminal.
  - `&` will stop if you log out though.
  - `nohup` coupled with `&` will send your command to the background and will continue to run when you log out.
- Scripts
  - Options at the top
  - `set -e` Immediately error if a command failed. But, `trap` is better. See [tracking](https://github.com/emisgroup/tracking-api/blob/develop/scripts/update-deps.sh#L8)
  - `set -o` Set option.. I use for `pipefail` So exits with the first none zero exit code, or zero if all commands successfully work.
- The [shebang line](<https://en.wikipedia.org/wiki/Shebang_(Unix)>)
  - For bash recommend doing `#!/usr/bin/env bash`
- [Shellcheck](https://www.shellcheck.net)
  - Great tool, recommend you install, and use it. Will help you learn bash (I still use it all the time to help).
- Shell expansion (examples in the deploy scripts)
  - [Example](https://github.com/emisgroup/hello-production/blob/develop/scripts/create-release.sh#L39)
  - `for x in {1..5}; do echo $x; done`
- Dotfiles
  - [Ben's](https://github.com/benmatselby/dotfiles)
  - [Paddy's](https://github.com/paddymorgan84/dotfiles)
  - [Jess's](https://github.com/jessfraz/dotfiles)
- Exports / Environment variables
  - Loading them, unsetting them etc.
- Aliases (ftw)
  - oh-my-zsh has some nice defaults
  - Roll your own (makes you faster on your machine, slower on others ¯\_(ツ)\_/¯)
  - `envg aws` | `aliasg fetch`
- Useful projects:
  - [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)
  - [fzf](https://github.com/junegunn/fzf)
  - [Janus](https://github.com/emisgroup/janus)
