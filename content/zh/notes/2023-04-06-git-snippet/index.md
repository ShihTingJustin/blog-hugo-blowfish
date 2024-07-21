---
title: "Git Snippet"
summary: "Git config and commands that I use frequently"
categories: ["note"]
tags: ["Git", "Snippet", "Command"]
#externalUrl: ""
# showSummary: true
date: 2023-04-06
draft: false
---

## Config

### Get repository or global options

```sh copy
git config --list
```

```sh copy
git config -l
```

### Identity setup

```sh copy
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

you can also use `--local` to use this identity only in the directory

```sh copy
git config --local user.name "John Doe"
git config --local user.email johndoe@example.com
```

## Remote

###

```sh copy
git remote set-url origin <repo url>
```

### Auto setup remote branch
```sh copy
git config --global --add --bool push.autoSetupRemote true
```

## Tag

Push branch and tag

```sh copy
git push --follow-tags
```

Delete origin tag

```sh copy
git push --delete origin v0.1.3
```

Delete local tag

```sh copy
git tag -d v0.1.3
```

## Commit

### Remove latest commit

```sh copy
git reset HEAD^ --hard
```

## Branch

### Reomve local branch

```sh copy
git branch -D <branch>
```

### Reomve remote branch

```sh copy
git push --delete <remote> <branch>
```

## Git Aliases

```sh copy
git config --global alias.br branch
git config --global alias.co checkout
git config --global alias.cp cherry-pick
git config --global alias.sp stash pop
git config --global alias.st stash
git config --global alias.pl pull
git config --global alias.ps push
```

## Reset author information for all commits

for all branch

```sh copy
git filter-branch -f --env-filter '
    GIT_AUTHOR_NAME='ShihTingJustin'
    GIT_AUTHOR_EMAIL='justinhuang777@gmail.com'
    GIT_COMMITTER_NAME='ShihTingJustin'
    GIT_COMMITTER_EMAIL='justinhuang777@gmail.com'
' -- --all
```

you could also use `HEAD`

```sh copy
git filter-branch -f --env-filter '
    GIT_AUTHOR_NAME='ShihTingJustin'
    GIT_AUTHOR_EMAIL='justinhuang777@gmail.com'
    GIT_COMMITTER_NAME='ShihTingJustin'
    GIT_COMMITTER_EMAIL='justinhuang777@gmail.com'
' HEAD
```

src: https://git-scm.com/docs/git-filter-branch

---

### Why are my contributions not showing up on my profile?

make sure the email setting in git config is as same as GitHub email

```sh copy
git config user.email
```

or read [GitHub Doc](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-github-profile/managing-contribution-graphs-on-your-profile/why-are-my-contributions-not-showing-up-on-my-profile)
