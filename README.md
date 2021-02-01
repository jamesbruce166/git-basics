# Git Tutorial

## Understanding Git

### Why use Git?

-   Collabration - Git allows you to work on the same codebase with other members of your team, or other teams.
-   Safe code updates - Changes are separated into 'branches', no one member will be working on the same _version_ of code at any one time. Changes are also assesed before being approved and merged into other branches.
-   Version history - Using version history, you can view changes from the past, and even revert back to a previous version with ease.

### What is Branching?

![Branching](./img/branches.png 'Branching')

### Working Areas of Git:

![Working Areas](./img/workingareas.png 'Working Areas')

### More Detail:

![Zoo of Areas](./img/zoo.png 'Working Zoo')

## Setup

```bash
git config --global user.name “Your Name”
git config --global user.email “you@example.com”
git config --global color.ui auto
```

-   `--global` is global to the current user.
-   `--system` is global to all users.
-   `--local` is specific to a given repository, this is the default. Unless you are in a git directory, this will fail.

## Day-To-Day Work

### Creating a Repo

```bash
git init myrepo
cd myrepo
```

Or, if you are already inside a directory:

```bash
git init
```

Everything git tracks will now be stored in a hidden `.git/` folder.

### The Basics

```bash
touch add.sh multiply.sh
```

This is the most common git command you will ever run in the CLI.
You can see there are no changes in the working directory at this point in time.

```bash
git status
git add --all
git status
git commit -m "Initial Commit"
git status
```

Let's make some code changes!

```bash
vim add.sh
---
#!/bin/bash

RES=$(( $1 + $2 ))
echo $RES
```

```bash
bash add.sh 2 3
>> 5
```

-   Run `git status` again to see that git has detected a change in the code.
-   This refers back to the working tree shown earlier.
-   If you are happy with the changes you have made, you can then add these to the staging area.

```bash
git add .
```

-   The period (`.`) will tell git to add _all_ of the files that have changed.
-   You can also add folders, and individual files.
-   We can now check the status and see that these files have been added to the _staging area_. You can stage as many new changes as you like.

Before committing, make another change to the file.

```bash
vim add.sh
---
#!/bin/bash

# This will add together two arguments
RES=$(( $1 + $2 ))
echo $RES
```

```bash
git diff .
```

<center>_(exit out of this by pressing 'q')_</center>

-   Assume that we have now decided these changes are not required, we can easily revert them back with:

```bash
git checkout -- .
```

-   Verify this with `git status`.
-   Now we are back to its original state, with our changes in the staging area, we can commit these with:

```bash
git commit -m "Addition complete"
```

-   Note how commits require a message, you can ommit the `-m` to be presented with a window to enter your message instead.
-   Running `git status` now, will show us that our working directory is up-to-date.
-   These changes are now commited and ready to be pushed to a remote repository.

## Creating Branches

Note how right now we are in the master branch, usually this wouldnt be the case - as explained earlier, the master branch (or the production branch) is used to retain a working release of the code.

In reality, nobody would ever be working on the master branch, so let's make a different one.

```bash
git checkout -b feature/your-name
```

The `-b` flag will create a new branch.

Branches can have different prefixes other then `feature`, you usually see `bugfixes`, `hotfixes`, and `release` branches - but the branching model will probably change depending on the project.

```bash
git branch
```

## Stashing

If we refer back to the zoo of working areas from ealier, there was a `stash` area, lets take a look at when you'd typically use this.

One scenario is to imagine you realise you have forgotten something in your previous commit, but have already started working on our next commit.

```bash
vim multiply.sh
---
#!/bin/bash

RES=$(( $1 * $2 ))
echo $RES
```

Go ahead and commit this change:

```bash
git add .
git commit -m "Multiplication complete"
```

Assume we now work on a new feature

```bash
touch divide.sh
git add .
```

-   At this point, we realise we forgot something with our previous commit - the comment :(
-   We can `stash` our current changes.

```bash
git stash
git stash list
git stash show 0
```

-   Make some changes that are part of your previous commit.
-   Add them as follows.

```bash
git add .
git commit --amend
```

We still have our code stashed, we can bring back these changes with `pop`.

```bash
git stash pop
git stash list
```

## Merging

```bash
git restore --staged .
rm -f divide.sh
```

We can now merge our changes to the `master` branch.

```bash
git checkout master
git merge feature/multiply
```

## Inspection

There are a few ays to inspect your git. I will show you `git log`.

```bash
git log
```

We can see thats quite long, we can shorten that with:

```bash
git log --oneline # compress to oneline
git log --oneline --graph # show graph
git log --author='James Erringham-Bruce' # filter by author
git log -p # see diff
```
