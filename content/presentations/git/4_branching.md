---
weight: 40
outputs: ["Reveal"]
---

{{% section %}}

# Git branching

---

One of the reasons why Git has become popular is the cheap branching

It is very easy to make a branch to work in parallel

---

## Create a new branch

```bash
$ git switch -c my_new_feature # -b means create the branch
Switched to a new branch 'my_new_branch'

$ git status
On branch my_new_branch
nothing to commit, working tree clean
```

---

`git switch -c` will create a new branch based on the branch where you are - e.g `master` in this instance

---

## Exercise

- Add some more text to example.txt
- Add a new file example2.txt with some text
- git `add` and `commit` the new changes

---

## Switch between branches

```bash
$ ls
example2.txt  example.txt

$ git switch master
Switched to branch 'master'

$ ls
example.txt
```

What happened to your files?

---

## Interlude - the .git directory

Git is "just" a key-value store

When we commit - git saves our data and writes down an ID to where it is

When we change branches, git looks up what files it needs to get and simply *replaces your working directory*

---

## We can see for ourselves

```bash
# Look up the ID of my_new_branch
$ cat .git/refs/heads/my_new_branch 
1359962e527f4ab2c15c7703b233fb4e8a0afb83

$ cat .git/refs/heads/master
c26f7174f47f0781a8c4f83229d0c50b79a204c0

$ tree .git/objects # The actual files
.git/objects
├── 10
│   └── ba6b215ed96b95bef8d9e605b45fffe24efa95
├── 13
│   └── 59962e527f4ab2c15c7703b233fb4e8a0afb83 # Here's our branch ID
├── 43
│   └── e5d206cf6e3486c50e0c68329e2f4301cb8454
...
├── af
│   └── b164663e3655ebab9d07129cad85b046db6ae0
├── c2
│   └── 6f7174f47f0781a8c4f83229d0c50b79a204c0 # Here's our master branch
├── fb
│   └── a58de71fd101b35076d9eaa60ea954b139e03b
├── info
└── pack
```

---

![git](https://git-scm.com/book/en/v2/images/commit-and-tree.png)

---

## /Aside

---

## Combining branches

When we are happy with the extra code we wrote on my_new_branch we want to *merge* it into master

```bash
$ git switch master # Switch to master
$ git merge my_new_branch # Merge my_new_branch into master
Updating c26f717..1359962
Fast-forward
 example.txt  | 2 ++
 example2.txt | 1 +
 2 files changed, 3 insertions(+)
 create mode 100644 example2.txt

$ ls
example2.txt  example.txt
```

*Note that my_new_branch is untouched by the merge*

---

## Delete the branch

Now that we are done with the branch, we can delete it

```bash
$ git branch -d my_new_branch
Deleted branch my_new_branch (was 1359962)
```

This only deletes the reference - the file found in .git

```bash
$ ls .git/refs/heads
master
```

The database of files is still intact *(our .git/objects directory)*

{{% /section %}}
