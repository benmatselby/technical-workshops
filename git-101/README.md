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

## Pushing

## Rebasing

## Logging

## Hooks

## Configuration (Global and repo level)
