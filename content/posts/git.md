---
title:  "Git Tips"
date:   2024-04-12
url: 'git'
---

If you've followed `Confusing git terminology`, follow [this link]({{< ref "confusing-git-terminology" >}}).

`헷갈리는 깃 용어 정리` 한글 번역을 찾아 오셨다면, [이 링크]({{< ref "confusing-git-terminology" >}})를 참조하시기 바랍니다.

---

My 2¢ while using Git.

## Pull

If you face the error below, when you do `$git pull`

```text
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details

	git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

	git branch --set-upstream develop origin/<branch>
```

Reset the `upstream` with below

```bash
$ git branch --set-upstream-to=origin/main main
```

*from `$man git-branch`*

```text
	   -u <upstream>, --set-upstream-to=<upstream>
		   Set up <branchname>'s tracking information so <upstream> is
		   considered <branchname>'s upstream branch. If no <branchname> is
		   specified, then it defaults to the current branch.
```

## Commits

### Removing the Whole commit history (Remote)
[Stack Overflow](https://stackoverflow.com/a/26000395 )

This utilize a new branch which doesn't have any history yet.

**You don't have to remove `.git` directory**

Below, we assume you want to remove all the history under the `main` branch,
but you can use any branch  when 'main' appears.

```bash
$ git checkout --orphan tmp_branch
$ git add -A
$ git commit -am "Init"
$ git branch -D main			# delete the branch you want to purge history
$ git branch -m main			# rename new branch to the target branch name
$ git push -f origin main
```

## Tags
### Rename a tag (Remote)
[Stack Overflow](https://stackoverflow.com/a/5719854)

```bash
git tag new old           # Create a new local tag named `new` from tag `old`.
git tag -d old            # Delete local tag `old`.
git push origin new :old  # Push `new` to your remote named "origin", and delete
						  #     tag `old` on origin (by pushing an empty tag
						  #     name to it).
```

**Note that if you are renaming an annotated tag**, you need to ensure that
the new tag name is referencing the underlying commit and
not the old annotated tag object that you're about to delete.

If the old tag is an annotated tag, or you aren't sure,
**you can use `^{}` to dereference the object** until a non-tag object is found:

```bash
# create a new annotated tag "new" referencing the object
# which the old annotated tag "old" was referencing:
$ git tag -a new old^{}
```

### Archive a `branch` into `tag`
#### Archive

(See: [Stack Overflow 1](https://stackoverflow.com/a/1309934),
[Stack Overflow 2](https://stackoverflow.com/a/4292670) )

Give a 'archive' tag name `archive/new_name` to the branch you want to archive.

```bash
$ git tag archive/new_name old_branch
```

#### Push

If you want to upload this `archive` to Github or Gitlab,
you can do below.

(check your `$ git remove -v` for upstream name, normally it is `origin`)

```bash
$ git push origin archive/new_name
```

#### Remove the old branch (Local)

If you don't want to preserve the old branch, you can delete one.

```bash
$ git branch -D old_branch
```

#### Remove the old branch (Remote)

```bash
$ git push -d origin old_branch
```

#### If you need to check it again, later

**If you need to check the archived branch, later**

You can create temporary `tmp_branch` and checkout to there,
with archived `archive/new_name`(the name which you gave to the archive branch above) tag.

```bash
$ git checkout -b tmp_branch archive/new_name
```

## Submodules
### Re-pull the submodule after cloning the parent repository
[Stack Overflow](https://stackoverflow.com/a/1032653)

**If you fresh `pull`ed the repo or `chckout` to some branch**,
you won't have anything under submodule.

So, use below

```bash
$ git submodule update --init --recursive
```

### Update the submodule

```bash
$ git submodule update --recursive --remote
```
