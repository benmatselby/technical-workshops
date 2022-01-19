# WSL

Cross posted to [dev.to](https://dev.to/paddymorgan84/getting-started-with-wsl-2-ja)

**Time allocation:** 60 minutes

This was last run on 22nd March 2021 and 31 people attended.

This workshop will be a high level overview of installing, configuring and using WSL. It's an entry level workshop, so if you're comfortable with WSL and already use it, this probably isn't for you.

What we will cover:

- [What is WSL](#what-is-wsl)
- [Installation](#installation)
- [WSL cli](#wsl-cli)
- [Using with VS Code](#using-with-vs-code)
- [Using with Docker](#using-with-docker)
- [Using a standalone terminal](#using-a-standalone-terminal)
- [Making it your own](#making-it-your-own)
- [Why I like it](#why-i-like-it)
- [Tidbits](#tidbits)
- [References](#references)

## Before we start

- This workshop is going to talk about the use of WSL 2 rather than WSL 1.
- There will be some minor disucssion around the differences, but the focus of this workshop will be around WSL2

## What is WSL

- A compatibility layer to allow you run Linux distributions on Windows
- You can download a Linux distro from the Microsoft Store as an app
- Once installed, you can run from a Linux terminal just like you would from a real Linux distribution

## Installation

- Windows version pre-requisite:

  - **x64**: Windows version 1903 or higher, with Build 18362 or higher.
  - **ARM64**: Windows version 2004 or higher, with Build 19041 or higher.
  - Use `winver` to find this out

- Installation is pretty straightforward:

  - Enable WSL

    ```powershell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    ```

  - Enable virtual machine feature

    ```powershell
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```

  - Download the [linux kernel package](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

  - Set WSL 2 as your default

    ```powershell
    wsl --set-default-version 2
    ```

  - [Choose your distro](https://aka.ms/wslstore)
    - Once installed, you'll need to create your user account

## WSL CLI

- Once installed, you can interact with WSL using the `wsl.exe`

- You can use `wsl --list -v` to list all of your available distros

  ```powershell
  > wsl --list -v

    NAME                   STATE           VERSION
  * docker-desktop-data    Running         2
    Ubuntu-18.04           Running         2
    docker-desktop         Running         2
  ```

- You can change the WSL version of your distro with `wsl --set-version`

  ```powershell
  > wsl --set-version Ubuntu-18.04 1
  > wsl --list -v

    NAME                   STATE           VERSION
  * docker-desktop-data    Running         2
    Ubuntu-18.04           Running         1
    docker-desktop         Running         2
  ```

- You can import and export distros, meaning that you can actually run more than one on your machine

  ```powershell
  > wsl --import debian C:\my-distros\debian ./debian.tar
  > wsl --list -v

    NAME                   STATE           VERSION
  * docker-desktop-data    Running         2
    Ubuntu-18.04 (Default) Running         2
    docker-desktop         Running         2
    debian                 Running         2
  ```

  ```powershell
  > wsl --export Ubuntu-18.04 ./ubuntu.tar
  ```

- When you have multiple distros, you have a default so that when you run `wsl` on the command line, it knows which distro you want to run in

  ```powershell
  > wsl --setdefault debian

    NAME                   STATE           VERSION
  * docker-desktop-data    Running         2
    Ubuntu-18.04           Running         2
    docker-desktop         Running         2
    debian (Default)       Running         2
  ```

- If you want to delete a distro, you can use `wsl --unregister`

  ```powershell
  > wsl --unregister debian

    NAME                   STATE           VERSION
  * docker-desktop-data    Running         2
    Ubuntu-18.04           Running         2
    docker-desktop         Running         2
  ```

## Using with VS Code

- Once you have WSL installed, you can use it with VS Code
- All you need is the [Remote WSL extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl)
- - We'll add my [dotfiles](https://github.com/paddymorgan84/dotfiles) repo as an example so I can show you what I mean
    ```bash
    git clone https://github.com/paddymorgan84/dotfiles.git
    ```
- This is where things get fun - From here you can basically develop exclusively within Linux.
- Don't "halfway house" on the filesystem, work from Linux, not the mounted Windows directories.
- Some extensions will prompt you to re-install them in WSL, do it!

## Using with Docker

- As we mentioned earlier, WSL 2 changed their fundamental architecture to introduce a full Linux kernel.
- This means that you can run your containers natively on Linux, rather than through emulation.
- Solves issues around [mounting volumes](https://github.com/docker/for-win/issues/2151) that you would experience in WSL 1.

### Installing Docker Desktop

- You'll need to make sure you have [Docker Desktop Stable 2.3.0.2](https://hub.docker.com/editions/community/docker-ce-desktop-windows/) or later to have WSL integration features.
- Go to "Settings -> General", make sure that you have `Use the WSL 2 based engine` enabled.
- Go to to "Settings -> Resources", make sure you have WSL integration confirmed for your distro.
- Try out some commands in your terminal

```bash
> docker ps -a

CONTAINER ID   IMAGE                                                        COMMAND                  CREATED        STATUS                    PORTS     NAMES
324412169e5f   vsc-workface-exporter-16cd14d118257fd98e818bcacc7022d0-uid   "/bin/sh -c 'echo Co…"   19 hours ago   Up 19 hours               80/tcp    workface-exporter
2c52e64e8e7d   vsc-hello-production-4b0fccdf5f0ac555655732482f1d2cb2-uid    "/bin/sh -c 'echo Co…"   2 days ago     Exited (255) 2 days ago             code-hello-production
```

## Using a standalone terminal

- I predominantly use the terminal in VS Code.
- There are times when it's useful to be able to use more of your screens real estate, especially in light of the fact that [VS Code doesn't currently support dual screens](https://github.com/microsoft/vscode/issues/34442).
- You can log into WSL from a Windows terminal using the following command (as an exmaple):

  ```powershell
  wsl ~ -d Ubuntu-18.04
  ```

- I started with Powershell, but I didn't really enjoy the experience, mainly for aesthetic reasons and how I present my terminal in WSL.

- I then moved onto [Hyper](https://hyper.is/)

  - Small amount of configuration to get it up and running with WSL2
  - Access Hypers settings with `ctrl + ,`
  - Example of settings can be found in my [dotfiles](https://github.com/paddymorgan84/dotfiles/blob/master/hyper/.hyper.js)

- I've ended up with [Windows Terminal](https://github.com/Microsoft/Terminal). It supports multiple shells and is also highly configurable.

  - Again, a small amount of configuration to get started, `ctrl + ,` for settings
  - Example of settings can be found in my [dotfiles](https://github.com/paddymorgan84/dotfiles/blob/master/windows-terminal/settings.json)

## Making it your own

There's two key things I use to personalise my WSL experience for me.

### Ansible

- Get the software you need with [Ansible](https://www.ansible.com/)
- Ansible is an open source automation engine.
- You can use it for things like provisioning servers, managing configuration, deploying applications etc.
- I use Ansible to provision all the software I require as an engineer:
  - AWS CLI
  - Azure CLI
  - Git
  - GitHub CLI
  - Go
  - Node
  - Terraform
  - Terragrunt
  - etc...
- You run Ansible playbooks that describe what you want Ansible to do
- Playbooks should be idempotent.
- We have a repo for this thanks to inner source: [Janus](https://github.com/emisgroup/janus)

### Dotfiles

- Personalise and keep your experience consistent with [dotfiles](https://github.com/paddymorgan84/dotfiles)
- Dotfiles are essentially configuration files, typically with a leading `.`
- These dotfiles hold any customisations you may have to your environment, such as `.gitconfig`, `.vimrc`, `.zshrc`
- Generally, people keep their dotfiles in a repo.
- You can use a script to copy or symlink these files to the approriate location.

### Productivity

- By using these two features in tandem, you can essentially create your entire developemnt environment from scratch, on any machine.

## Why I like it

- Efficiency, working from the terminal pretty much exclusively
- Aligned with our own technical direction (Linux containers, bash scripting)
- Better aligned with your team in multi-OS scenarios

## Tidbits

- Symantec is OUT. Solves big problems previously had with WSL2 around apt updates

## References

- [Installing WSL2](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
- [WSL 1 vs WSL 2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)
- [WSL cli reference](https://docs.microsoft.com/en-us/windows/wsl/reference)
- [VS Code - Developing in WSL](https://code.visualstudio.com/docs/remote/wsl)
- [Docker Desktop - WSL2 Backend](https://docs.docker.com/docker-for-windows/wsl/)
- [Hyper Terminal](https://hyper.is/)
- [Windows Terminal](https://github.com/Microsoft/Terminal)
- [Getting started with Ansible](https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html)
