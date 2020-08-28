---
weight: 30
outputs: ["Reveal"]
---


{{% section %}}

# Committing files

---

## The git workflow

- git status
- git add
- git commit

---

### Git status

`git status` gives us information about the current *state* of git

```bash
$ git status
On branch master

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

Notice it gives us an indication of what our next step might be

---

### Exercise

Create a new file named example.txt and write some text

---

### Git add

Now we have some text - run `git status` again

```bash
$ git status

On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	example.txt

nothing added to commit but untracked files present (use "git add" to track)
```

We have a new *untracked* file - untracked means git has not added it to it's database yet

---

Let's track the file

```bash
$ git add example.txt
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   example.txt
```

---

We are now ready to *commit* our file - aka "Press Save".

We need to provide a commit message

A good commit message is a helper for yourself if you ever need to go back in time!

```bash
$ git commit
```

---

## Aside - Commit messages

A good commit message should have a title - use verbs to describe what this commit will do!

- `Update/Add/Fix/Create` etc

It should also have a descriptive body - a more detailed outline of what is happening

---

Don't be this guy:

[![xkcd](https://imgs.xkcd.com/comics/git_commit.png)](https://xkcd.com/1296/)

---

Where did our file go?

![git staging](/images/git_staging.png)

---

## Update the file

Now that we have a file under version control, let's change it

---

### Exercise

- Update your example.txt with some additional text
- Add your new changes - don't commit
- Run git status
- Add some more text to your file
- Run git status again

---

## What do you think is happening?

```bash
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   example.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   example.txt
```

---

Since Git has the concept of the *staging* area, we can *stage* **changes** as many times as we want before committing.

- A **change** is not the same as a file
- *Staging* means selecting what **changes** we want to include in our next **commit**

---

### Exercise

- Make commit
- Run `git status`
- Add the remaining changes
- Make another commit

---

## Aside - .gitignore

Often we have files we don't want git to keep track of such as editor configuration, large files or temporary files

These can be listed in a special `.gitignore` file in the same location as your .git directory and you should always have one!

A template for a python `.gitignore` file can be found [here](https://github.com/github/gitignore/blob/master/Python.gitignore)


{{% /section %}}
