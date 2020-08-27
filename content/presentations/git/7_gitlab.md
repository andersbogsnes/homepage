---
weight: 70
outputs: ["Reveal"]
---

{{% section %}}

# The Central repository

When working with others, we want a central repository that we synchronize our local repository with

That way, we can share our changes easily!

---

## The main players

The main companies offering these services are

- Github (bought by Microsoft)
- Gitlab (independent)
- Bitbucket (Atlassian)
- Azure Devops? (old Team Foundation Server)

For this workshop, we will use Gitlab

---

## Setup gitlab.com

Everyone will need a gitlab.com account

- Setup an account at gitlab.com
- Setup ssh keys (under Settings/SSH Keys - follow the instructions to generate new ones)

Pair up two and two (or three). Do the rest of the exercise on one machine

---

## Exercise

- Create a new directory on your machine called `calculator`
- In that directory, create a new file called `calculator.py`
- Define a function `add(a, b)` that returns the sum of a + b
- Run git init, add and commit
- Create a new project in Gitlab called calculator - set it to public and don't click "Initialize repository with a README"

---

## Linking local repo to Gitlab

We need to tell git about our remote repository

```bash
$ git remote add origin git@gitlab.com:andersbogsnes/calculator.git
```

Creates a *label* **origin** :point_right: **my_long_url_i_cant_remember**

---

## Push - Update remote from local

```bash
$ git push -u origin master
```

I want to `push` my changes from my local branch to the branch named `master` at the url specified in `origin` and I want to link these two branches (`-u`)

---

## Set up a develop branch

- We want to practice git flow, so we need a develop branch

```bash
# Create a new branch called develop and switch to it
$ git switch -c develop
# Push local branch develop to origin's develop branch
$ git push -u origin develop  
```

- Go to gitlab and set development as your default branch (Settings/Repository)

---

## Exercise

Add your partner to your repo

{{< figure src=/images/gitlab_add_members.png height=500 >}}

---

## Clone the repo

Your partner should now clone the repo

{{< figure src=/images/gitlab_clone.png height=500 >}}

---

```bash
$ git clone git@gitlab.com:andersbogsnes/calculator.git
```

- `clone` creates a full copy of a repository to have locally
- We are only copying files back and forth - there is no other link!

---

## Exercise - 5 mins

One person

- Create a new feature branch
- add a new function `subtract(a, b)` which subtracts two numbers
- push the new branch to gitlab
- create a merge request

---

## Merge requests

Github calls it a merge request, everyone else calls it a pull request

{{< figure src=/images/create_merge_request.png height=480 >}}

---

## Creating a merge request

{{< figure src=/images/merge_request.png height=480 >}}

---

## Ready for review

{{< figure src=/images/ready_for_review.png height=500 >}}

---

## Exercise - 1 min

One person

- Click on changes
- Make a comment on the code
- Resolve the conversation
- Make another comment on the code
- Resolve the comment in a new issue
- Merge the request

---

## Git pull

There are new changes in the central repository so we need to update our local repository to get the changes

---

## Central vs local repository

- You have two separate copies of the repository, one local and one in the central repository
- `git pull` will asks the central repo if it has any commits that are not present locally
- If it does, git will do a `merge`, merging the new commits into your local commits

{{% /section %}}
