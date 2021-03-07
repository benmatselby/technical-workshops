# Git

**Time allocation:** 30 minutes

This workshop will be a high level overview of the basic commands you will use for git daily. It's an entry level workshop, so if you're comfortable with git on the CLI, this probably isn't for you.

- Cloning
- Fetching
- Pulling/Pushing
- Branching
- Logging
- Rebasing

If we have time (intermediate)

- Hooks
- Configuration (Global and repo level)

## Notes

> Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.
> I’ve used Zip files, email, CVS, Subversion, and Git.

## Disclaimer

There are different ways to achieve the same thing. So cheatsheets:

- [Atlassian](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet)
- [GitLab](https://about.gitlab.com/images/press/git-cheat-sheet.pdf)

## Cloning

Git is distributed, so doesn’t need a server. You can clone from another machine on a network, GitHub, GitLab, your own GitServer.

`git clone https://github.com/emisgroup/welcome.git`

This gets the codebase from somewhere, and keeps a copy on your machine. You have “cloned” the repo.

How do you know where the original end point is? “Remotes”

`git remote show`

This shows you `origin`.

`git remote show origin`

This gives you all the details.

But, you can have many, many origins.

```shell
git remote add github https://github.com/emisgroup/welcome.git
git remote show
git remote show github
```

Which brings us onto

## Fetching

Git is a distributed version control system. So you have the full repo locally, but changes are being made elsewhere. You need to `fetch` those changes.

Benefits, you can work on a train and commit code. You do not need an internet connection to commit code!!!

`git fetch origin`

This pulls the latest information from the `origin` remote. Which mainly for EMIS Engineers is currently GitHub.

But,

`git fetch github`

## Pulling

So, we have cloned, and fetched. Now we need to pull and push.

`git pull origin main`

This pulls the code/assets from the `origin` remote, and the `main` branch. You can change both of these values.
At this point, you are “up to date” with what is on the server, wherever that server is.

## Branching

You have a clone, you have the code, now you want to change it.

`git checkout -b workshop origin/main`

So `git checkout` is the command. `-b` is the option to create a branch. `workshop` is the name of the new branch. `origin/main` is what I am branching from. It can be from any remote, or even locally.

## Committing

You're on your branch, and now need to commit some code.

`git add .` ? Risky, if not overly disciplined.

or

`git add [file] [file]` Better

This adds files to the "index".

How do you know what to commit?

- `git diff` shows you want is not in the index, that has changed.
- `git diff --cached` shows you want has changed, and is in the index, ready to commit.
- `git diff` shows you the diff, without whitespace changes.

`git status`

This shows you what is and is not in the index, and what will be committed.

`git commit -a` ? Risky, if not overly disciplined.

or

`git commit`

Other options:

- `git commit -n` Very naughty. The `-n` means naughty ;-)
- `git commit -m "This is my commit message"`

Soap box:

- [Logically atomic commits](https://benmatselby.dev/post/logical-commits/)
- Please be liberal in information in your commit history. It's the only thing that stays with us.

## Pushing

Committing code, does not make it available for everyone, since git is distributed. So, we need to push the changes

`git push origin workshop`

So `git push` is the command. `origin` is location of the server we want to push to. `workshop` is the branch I am pushing to.

## Rebasing

So you now commit all day long, but at the end of the day, you want to make [logically atomic commits](https://benmatselby.dev/post/logical-commits/). This means you need to learn rebasing.

I always think of rebasing as "replying your work over the top of the latest code".

But you can also do much more with an "interactive rebase".

Normal rebase:

`git rebase origin/main`

Interactive rebase:

`git rebase -i origin/main`

Interactive rebase provides you a menu of options, such as squashing, picking, editing, dropping.

## Configuration (Global and repo level)

`git config --list` will show you all your configuration items.

You can break that down

`git config --list --local`
`git config --list --global`

So to add config

`git config --add user.email engineer@gmail.com`

This sets configuration locally to the repo you are in, and overrides global configuration.

To unset configuration

`git config --unset-all user.email`

Or to add configuration globally

`git config --global --add user.email engineer@gmail.com`

You can also edit `~/.gitconfig` for global configuration, and `.git/config` for the local repo configuration.

Let's say you want to have version controlled git configuration, but you need different email addresses (e.g. one for work, one for home), you can set

```ini
[include]
  path = ~/.gitconfig.local
```

```shell
❯ cat ~/.gitconfig.local
[user]
    name = Ben Selby
    email = engineer@gmail.com
```

## Logging

Logging is what it is, bit here are some useful ones

- `git log` - Show the log.
- `git log --stat` - Shows the log and the files changed.
- `git log --stat -p` - Shows the log and the files changed, and the diff!
- `git log --graph` - Shows the log and graphs how the commits came in.
- `git log --oneline --decorate` - Shows the title of the commits, and curates tags/branches. This is why useful git commits are important.
- `git shortlog` - Commits by engineer.
- `git shortlog -ns` - Commit totals by engineer, in order.

## Hooks

The [book](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks).

Note that there are server and client side hooks. If you maintain your own git server, you can have both. If you use GitHub, you have "GitHub Actions" as the server hooks.

Normally, we deal with local hooks, which are stored in `.git/hooks`.

The files:

- Need to have a shebang line.
- Need to have the correct permissions.

## Tidbits

- `master` is being moved away from. If you `git init` a new repo now, git explains this. GitHub is also moving away from `master` in favour of `main`.
- `git init` creates a new repo.
- `git show [commit]` will show you the contents of the commit.
- `git ls-files` will give you a list of files in git.
