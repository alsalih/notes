<!--
Author: Haidar Alsalih
Source(s): Boot.dev Git Course
-->

# Git Notes

## Methodology

(START SECTION - NOT YET APPROVED):

In addition to the general methodology for notes, for the Git notes:

- Directly under each level 3 heading, add a list of commands and/or actions described under that heading
    - this allows for easy browsing when someone just wants to find the notes for a specific command

(END SECTION - NOT YET APPROVED)

## Setup and Basics

Check git version with ```git --version```, which prompts to install git if you don't have it.

Otherwise install with:

- for WSL or Ubuntu Linux: ```sudo apt install git-all```
- for macOS: ```brew install git```

### Manual

check the git manual page with ```man git```

some shortcuts for the manual are:

q: Quits the manual
j: One line down
k: One line up
d: Half page down
u: Half page up
/(term): Search for "term"
n: Next search term
N: Previous search term

When reading commands in manual or other:

- arguments in angle brackets (<>) are mandatory and must be provided when running the command.
- arguments in square brackets ([]) are optional and can be included if needed.

### Porcelain and Plumbing

In Git, commands are divided into high-level ("porcelain") and low-level ("plumbing") commands.

99% of the time, you'll be using porcelain/high-level commands. 

Porcelain Commands:

(Assuming a ```git``` before the command)

- status
- add
- commit
- push
- pull
- log

Plumbing Commands:

(Assuming a ```git``` before the command)

- apply
- commit-tree
- hash-object

### Config

Commands: ```git config```

Whenever code changes, Git tracks who made the change. 
To ensure you get proper credit (or more likely, blame) for all the code you write, you need to set your name and email.

Git comes with a configuration both at the global and the repo (project) level. Most of the time, you'll just use the global config.

use the command ```git config --get``` followed by ```user.name``` or ```user.email``` to get that config info

if not set yet, set them with ```git config --add --global``` followed by ```user.name``` or ```user.email``` to set them
it is recommended to use your GitHub username and email here

Set a default branch by using:
```git config --global init.defaultBranch``` followed by the branch name (e.g. ```master```). Git's default is 'master'.

the ~/.gitconfig file stores your global Git configuration

view it by ```cat ~/.gitconfig```

## Repositories

Commands: ```git init```, ```git status```, 

Create a new folder and move inside it (cd folder_name), then type:

```git init```

to create a new git repository. This will create a new hidden .git file in that directory. ```ls -a``` to confirm this.

A file can be in one of several states in a Git repository. Here are a few important ones:

    untracked: Not being tracked by Git
    staged: Marked for inclusion in the next commit
    committed: Saved to the repository's history

The ```git status``` command shows you the current state of your repo. It will tell you which files are untracked, staged, and committed.

### Staging

When a file is created or modified, that file or its changes are 'untracked' at first. We need to stage it (add it to the "index") with the ```git add``` command before committing it later.

Without staging, every file in the repository would be included in every commit, but that's often not what you want.

Here's the command:

```git add <path-to-file | pattern>```

### Commit

After staging a file, we can commit it.

A commit is a snapshot of the repository at a given point in time. It's a way to save the state of the repository, and it's how Git keeps track of changes to the project. A commit comes with a message that describes the changes made in the commit.

Here's how to commit all of your staged files:

```git commit -m "your message here"```

*Note.* If you screw up a commit message, you can change it with the --amend flag. For example:

\# Change the last commit message
```git commit --amend -m "A: add contents.md"```

### Half of git

You've learned half of Git.

...well, not really. But kind of.

Half of your workflow as a developer will just be 3 simple commands:

```git status```
```git add```
```git commit```

It's most of what you need to work effectively as a solo developer. Another 40% of Git is about collaborating and storing your work on a remote server.

The last 10% is mostly about fixing mistakes, rolling back changes, and other advanced topics. Don't worry, we'll cover those, too.

### Git Log

A Git repo is a (potentially very long) list of commits, where each commit represents the full state of the repository at a given point in time.

The git log command shows a history of the commits in a repository. This is what makes Git a version control system. You can see:

- Who made a commit
- When the commit was made
- What was changed

#### A commit hash

Each commit has a unique identifier called a "commit hash". This is a long string of characters that uniquely identifies the commit. Here's an example:

```5ba786fcc93e8092831c01e71444b9baa2228a4f```

For convenience, you can refer to any commit or change within Git by using the first 7 characters of its hash. For mine, that's 5ba786f.

run `git log` to see the comits in a repository (or "repo").
run `git log` followed the ```-n``` option followed by a number x to limit it to the last x commits
run `git log` followed by the ```--no-pager``` option to run without the interactive pager
you can always run those options together by adding them one followed by the other as well.

## Internals

Even though commits can both have the same content in their repositories, they will have different commit hashes. 
While commit hashes are derived from their content changes, there's also some other stuff that affects the end hash. For example:

- The commit message
- The author's name and email
- The date and time
- Parent (previous) commit hashes

All this to say that hashes are (almost) always unique, and because they're generated automatically for you, you don't need to worry too much about what goes into them right now.

*Note.* Hash = SHA. Git uses a cryptographic hash function called SHA-1 to generate commit hashes. We won't go into the details of how SHA-1 works in this course, but it's important to know because you might also hear commit hashes referred to as "SHAs".

### The Plumbing

So far we've been using Git in a "porcelain" manner. But to sate our insatiable curiosity, let's take a look at some of the "plumbing".

#### It's just files all the way down

All the data in a Git repository is stored directly in the (hidden) .git directory. That includes all the commits, branches, tags, and other objects we'll learn about later.

Git is made up of objects that are stored in the .git/objects directory. A commit is just a type of object.

**Try it:**

- Use git log -n 10 to find your commit hash again.
- Use ls -al to list the contents of the .git/objects directory.
- Look for a directory that suspiciously matches the first two characters of your commit hash.
- Use ls -al to list the contents of that directory

After the steps above, you would see a .git/objects/xx/xxxxx... where x represents some alphanumeric character

Let's take a look at what's inside this suspicious commit object file.

---

**Try it** (Hint: It's the file you used in the previous "Try it" exercise):

- Try to cat the contents of the commit object file (which will interpret the contents as text).

cat path/to/file/from-before

Nope... it's a mess. The contents have been compressed to raw bytes!

- Try again with the xxd command to print the contents of the file in hexadecimal format:

xxd path/to/file/from-before

Note: you may need to install xxd.

- Do that again, but redirect the output to a temporary file so the tests can read it:

xxd path/to/file/from-before > /tmp/commit_object_hex.txt

**Cat File:**
Luckily, Git has a built-in plumbing command, cat-file, that allows us to see the contents of a commit without needing to futz around with the object files directly.

```git cat-file -p <hash>```

**Try it: (Tip: Use git log -1 to get the hash of your last commit.)**

- Use the cat-file command to view the contents of your last commit.
- For the CLI to be able to check your output, do it again, but redirect the output to a temporary file:

```[catfile command here] > /tmp/catfileout.txt```

#### Trees and Blobs

Now that we understand some of our plumbing equipment, let's get into the pipes. Here are some terms to know:

    tree: git's way of storing a directory
    blob: git's way of storing a file

Here's what I got when I inspected my last commit:

```> git cat-file -p 5ba786fcc93e8092831c01e71444b9baa2228a4f```
```tree 4e507fdc6d9044ccd8a4a3061324c9f711c4667d```
```author ThePrimeagen <the.primeagen@aol.com> 1705891256 -0700```
```committer ThePrimeagen <the.primeagen@aol.com> 1705891256 -0700```
```A: add contents.md```

Notice that we can see:

    The tree object
    The author
    The committer
    The commit message

However, we cannot see the contents of the contents.md file itself! That's because the blob object stores it.

**Try it:**

- Use git cat-file -p again, but this time with the hash of the tree object instead of the commit hash. You should see a blob object with its own hash.
- Use cat-file again to view the contents of the blob object.
- Run that same command, but this time redirect the output to a temporary file: /tmp/blobfile.txt.

Reminder of command syntax:

    git log (q to exit, arrow keys to scroll)
    git cat-file -p <hash>

Note: log is a porcelain command, while cat-file is a plumbing command. You'll use log much more often when working on coding projects, but cat-file is useful for us in this course to understand Git's internals.

**Try it:**

Create a second file called titles.md and add the following text:

<!-- 
# Titles

- A River Runs Through It
- Fight Club
- 12 Years a Slave
- The Big Short
- 12 Monkeys 
-->

- Save, stage, and commit the file with any commit message you like as long as it starts with B:. For example, B: add titles.
- Use (git cat-file) to inspect the commit you just made. You should notice one extra field in the commit object that wasn't present in the first commit.

#### Storing Data

Git stores an entire snapshot of files on a per-commit level. This was a surprise to me! I always assumed each commit only stored the changes made in that commit.
Optimization

While it's true that Git stores entire snapshots, it does have some performance optimizations so that your .git directory doesn't get too unbearably large.

- Git compresses and packs files to store them more efficiently.
- Git deduplicates files that are the same across different commits. If a file doesn't change between commits, Git will only store it once.

**Try it:**

- Use git cat-file -p to view the hash of the blob for the titles.md file in your last commit. Save that hash somewhere in your notes.
- Add a new directory to your project called quotes. Inside, add two files:

<!-- 
starwars.md:

- "May the Force be with you"
- "I find your lack of faith disturbing"
- "I am your father"
- "Do or do not. There is no try"
- "I've got a bad feeling about this"

dune.md:

- "May thy knife chip and shatter"
- "A Great Man Doesn't Seek To Lead. He's Called To It."
- "An Animal Caught In A Trap Will Gnaw Off Its Own Leg To Escape. What Will You Do?"
- "When Is A Gift Not A Gift?" 
-->

- Stage and commit both files in a single commit with a message starting with C:, for example, C: add quotes.
- Use the command cat-file to view the hash of the blob for the titles.md file again in the latest commit. You should notice that it still has the same hash because the file hasn't changed.

## Config

Git stores author information so that when you're making a commit it can track who made the change. Here's how you might update your global Git configuration:

```git config --add --global user.name "ThePrimeagen"```
```git config --add --global user.email "the.primeagen@aol.com"```

Let's take the command apart:

    ```git config```: The command to interact with your Git configuration.
    ```--add```: Flag stating you want to add a configuration.
    ```--global```: Flag stating you want this configuration to be stored globally in your ~/.gitconfig. The opposite is "local", which stores the configuration in the current repository only.
    ```user```: The section.
    ```name```: The key within the section.
    ```"ThePrimeagen"```: The value you want to set for the key.

You can actually store any old data in your Git configuration. Granted, only certain keys are used by Git, but you can store whatever you want.

Set the following useless key/value pairs in your local Git configuration for the Webflyx repository (omit the --global flag to set them locally):

- ```webflyx.ceo``` "ThePrimeagen"
- ```webflyx.cto``` "TheLaneagen"
- ```webflyx.valuation``` "mid"

Git has a command to view the contents of your config:

git config --list --local

You can also just view the contents of your local config file directly:

cat .git/config

Tip: Git version 2.46.0 added git config list to replace git config --list, though both still work.

### Get

We've used --list to see all the configuration values, but the --get flag is useful for getting a single value.

```git config --get <key>```

Keys are in the format ```<section>.<keyname>```. For example:


- ```user.name```
- ```webflyx.ceo```

### Unset

The ```--unset``` flag is used to remove a configuration value. For example:

```git config --unset <key>```

### Duplicates

Typically, in a key/value store, like a Python dictionary, you aren't allowed to have duplicate keys. Strangely enough, Git doesn't care.

#### Unset All

The ```--unset-all``` flag is useful if you ever really want to purge all instances of a key from your configuration. Conversely, the ```--unset``` flag only works with a single instance of a key.

```git config --unset-all example.key```

Try It:

While only the most unhinged of projects operate this way, some projects, including WebFlyx, allow for multiple CEOs. Let's add a few more CEOs to the WebFlyx configuration:

```git config --add webflyx.ceo "Warren"```
```git config --add webflyx.ceo "Carson"```
```git config --add webflyx.ceo "Sarah"```

Take a look at your new cursed configuration:

```git config --list --local```

Remove all the CEOs from the WebFlyx configuration at once using the --unset-all flag.

#### Remove a section

As I pointed out before, the webflyx section is nonsensical because Git doesn't use it for anything. While we can store any key/value pairs we want in our Git configuration, that doesn't mean we should.

The --remove-section flag is used to remove an entire section from your Git configuration. For example:

git config --remove-section section


Try It: Remove the entire webflyx section from your local Git config. You might notice that the default "core" section is still there, that's okay.

#### Locations

There are several locations where Git can be configured. From more general to more specific, they are:

- system: ```/etc/gitconfig```, a file that configures Git for all users on the system
- global: ```~/.gitconfig```, a file that configures Git for all projects of a user
- local: ```.git/config```, a file that configures Git for a specific project
- worktree: ```.git/config```.worktree, a file that configures Git for part of a project

In my experience, 90% of the time you will be using --global to set things like your username and email. 
The other 9% of the time you will be using --local to set project-specific configurations. 
The last 1% of the time you might need to futz with system and worktree configurations, but it's extremely rare.

##### Overriding

If you set a configuration in a more specific location, it will override the same configuration in a more general location. 
For example, if you set user.name in the local configuration, it will override the user.name set in the global configuration.

![alt text](media/images/git_location_diagram.png)

# Work-in-progress sections:

## Branching

## Merge

## Rebase

## Reset

## Remote

## GitHub

## Gitignore