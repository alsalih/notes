# Git

Git is a version control system. Basically - keep track of versions of files (like backups, but git is more efficient).

This is especially useful when developing software collaboratively (esp. asynchronously, non-linearly, etc.)

## Setup

Check if git is installed; prompt to install if it isn't: git --version

Install git on:
- Windows (WSL) / Linux (Ubuntu): 'sudo apt install git-all'
- macOS: 'brew install git'; or alternatively, install Xcode and its "Command Line Tools" package, which includes Git.

## Usage

### Manual

To read the manual: 'man git'

Manual shortcuts:

- 'q': quit
- 'j', 'k': one line down/up
- 'd', 'u': half-page down/up

- '/term': search for 'term'
- 'n', 'N': next/previous search term

### Commands

In Git, commands are divided into high-level ("porcelain") and low-level ("plumbing") commands.

99% of the time, you'll be using porcelain/high-level commands. 

#### Porcelain Commands:

(Assuming a 'git' before the command)

- status
- add
- commit
- push
- pull
- log


#### Plumbing Commands:

(Assuming the same as above)

- apply
- commit-tree
- hash-object

