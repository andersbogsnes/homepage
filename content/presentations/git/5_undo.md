---
weight: 50
outputs: ["Reveal"]
---

{{% section %}}

# Version Controlling

The whole point of a VCS is to be able to navigate between versions and we have a few ways to do that in git

---

## Examine the history

```bash
$ git log
commit 1359962e527f4ab2c15c7703b233fb4e8a0afb83 (HEAD -> master)
Author: Anders Bogsnes <andersbogsnes@gmail.com>
Date:   Mon Aug 10 15:03:13 2020 +0200

    Committing to my branch

commit c26f7174f47f0781a8c4f83229d0c50b79a204c0
Author: Anders Bogsnes <andersbogsnes@gmail.com>
Date:   Mon Aug 10 14:46:27 2020 +0200

    My third commit

commit 90b8e5126da798b6e217f8dbdb3ec4354f534955
Author: Anders Bogsnes <andersbogsnes@gmail.com>
Date:   Mon Aug 10 14:32:05 2020 +0200

    Second commit

commit 722b3172cabe09b98d54ad91d9ceddd4c31e86aa
Author: Anders Bogsnes <andersbogsnes@gmail.com>
Date:   Mon Aug 10 14:19:00 2020 +0200

    Initial commit
```

---

## A shorter version

```bash
$ git log --oneline
1359962 (HEAD -> master) Committing to my branch
c26f717 My third commit
90b8e51 Second commit
722b317 Initial commit
```

- The number on the side is called the `SHA` - it's the `ID` git uses, that we saw before

- We can refer to a commit by it's `SHA` and we only need a few digits, enough to be unique

---

## Timetravel with git

```bash
$ cat example.txt # Look at my file
My test file

Now has a more descriptive body

And some more texto

Fourth line of text

$ git switch -d 722 # Go back in time to SHA 722
$ ls
example.txt # No example2.txt!

$ cat example.txt # Look at the file again
My new text file # What I wrote in my file when I created it

$ git switch - # Go back to newest version of master
```

---

## Restore a file

We can change a file to the way it looked before

```bash
$ git restore example.txt -s 722 # Restore the file from revision with SHA 722
$ ls
example2.txt  example.txt

$ cat example.txt
My new text file

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   example.txt

no changes added to commit (use "git add" and/or "git commit -a")
>>> git restore example.txt # Return the file to newest version of master
```

---

## Undoing a commit

We often want to undo everything from one commit at a time

For example, we want to rollback a new feature which is bugged

---

We can do that in two ways:

- git reset
- git revert

---

## Git revert

Create a new commit that does the opposite of the specified commit

```bash
$ git log --oneline
* 1359962 (HEAD -> master) Committing to my branch
* c26f717 My third commit
* 90b8e51 Second commit
* 722b317 Initial commit

$ git revert 135
Removing example2.txt
[master 0f7f27d] Revert "Committing to my branch"
 2 files changed, 3 deletions(-)
 delete mode 100644 example2.txt

$ ls
example.txt
```

---

Revert is most often used when we have published our changes and don't want to change the history

We will talk more about publishing later

---

## Git reset

Chop out all commits after the specified one. Resets the history as if that revision is the newest

```bash
$ git log
0f7f27d (HEAD -> master) Revert "Committing to my branch"
1359962 Committing to my branch
c26f717 My third commit
90b8e51 Second commit
722b317 Initial commit

$ git reset 1359
Unstaged changes after reset:
M	example.txt
D	example2.txt

$ git log
1359962 (HEAD -> master) Committing to my branch
c26f717 My third commit
90b8e51 Second commit
722b317 Initial commit
```

---

This is a bit harder to undo as our label is now pointing to a different commit - the other commits are *orphaned*

If you are unsure you're doing it right - write down the SHA of the commit you're currently on - that way you can always get back

---

{{< figure src=https://wac-cdn.atlassian.com/dam/jcr:4c7d368e-6e40-4f82-a315-1ed11316cf8b/02-updated.png?cdnVersion=1172 height=640 width=480 >}}

---

## Reflog

We can also see a history of when we changed HEAD (our current location) by using `git reflog`

```bash
$ git reflog
1359962 (HEAD -> master) HEAD@{0}: checkout: moving from 1359962e527f4ab2c15c7703b233fb4e8a0afb83 to master
1359962 (HEAD -> master) HEAD@{1}: checkout: moving from master to 1359962
1359962 (HEAD -> master) HEAD@{2}: reset: moving to 1359962
0f7f27d HEAD@{3}: revert: Revert "Committing to my branch"
1359962 (HEAD -> master) HEAD@{4}: reset: moving to HEAD@{3}
1359962 (HEAD -> master) HEAD@{5}: reset: moving to HEAD@{2}
90b8e51 HEAD@{6}: checkout: moving from master to master
90b8e51 HEAD@{7}: reset: moving to 90b8e51
1359962 (HEAD -> master) HEAD@{8}: checkout: moving from 722b3172cabe09b98d54ad91d9ceddd4c31e86aa to master
722b317 HEAD@{9}: checkout: moving from master to 722b317
1359962 (HEAD -> master) HEAD@{10}: checkout: moving from 722b3172cabe09b98d54ad91d9ceddd4c31e86aa to master
722b317 HEAD@{11}: checkout: moving from master to 722b317
1359962 (HEAD -> master) HEAD@{12}: checkout: moving from 722b3172cabe09b98d54ad91d9ceddd4c31e86aa to master
```

Then we can do `git switch` as normal

---

## Fixing detached state

When we switch to a given commit, git warns us

```bash
You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -
```

---

Git is lettting us know that we are no longer on a branch, so any changes we make won't have a *label* (unless you write down the SHA)

If we want to put a *label* on the commit, we can use `switch -c` to create a new branch or we can get back to labelled territory with 
`git switch -` to take you back to the last branch you were on

---

## Exercise

- Switch to an earlier commit
- Create a new branch from that commit
- Make a change to a file
- Add and commit that change
- Go back to master
- Run `git log --graph --all --oneline`

Where is your new branch coming from?

{{% /section %}}
