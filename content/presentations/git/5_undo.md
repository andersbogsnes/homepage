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
>>> git log
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
>>> git log --oneline
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
>>> cat example.txt # Look at my file
My test file

Now has a more descriptive body

And some more texto

Fourth line of text
>>> git switch -d 722 # Go back in time to SHA 722
>>> ls
example.txt # No example2.txt!
>>> cat example.txt # Look at the file again
My new text file # What I wrote in my file when I created it
>>> git switch - # Go back to newest version of master
```

---

## Restore a file

We can change a file to the way it looked before

```bash
>>> git restore example.txt -s 722 # Restore the file from revision with SHA 722
>>> ls
example2.txt  example.txt
>>> cat example.txt
My new text file
>>> git status
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
>>> git log --oneline
* 1359962 (HEAD -> master) Committing to my branch
* c26f717 My third commit
* 90b8e51 Second commit
* 722b317 Initial commit
>>> git revert 135
Removing example2.txt
[master 0f7f27d] Revert "Committing to my branch"
 2 files changed, 3 deletions(-)
 delete mode 100644 example2.txt
>>> ls
example.txt
```

---

Revert is most often used when we have published our changes and don't want to change the history

We will talk more about publishing later

---

## Git reset

Chop out all commits after the specified one. Resets the history as if that revision is the newest

```bash
>>> git log
0f7f27d (HEAD -> master) Revert "Committing to my branch"
1359962 Committing to my branch
c26f717 My third commit
90b8e51 Second commit
722b317 Initial commit
>>> git reset 1359
Unstaged changes after reset:
M	example.txt
D	example2.txt
>>> git log
1359962 (HEAD -> master) Committing to my branch
c26f717 My third commit
90b8e51 Second commit
722b317 Initial commit

```
This is a bit harder to undo 
q
- git log
- git reflog
