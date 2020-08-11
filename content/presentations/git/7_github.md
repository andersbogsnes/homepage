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

## Exercise - 2 mins

- Create a new directory on your machine called `calculator`
- In that directory, create a new file called `calculator.py`
- Define a function `add(a, b)` that returns the sum of a + b
- Run git init, add and commit

---

## Exercise - 3 mins

- Setup an account at gitlab.com
- Setup ssh keys (under Settings/SSH Keys - follow the instructions to generate new ones)
- Create a new project in Gitlab called calculator - set it to public and don't click "Initialize repository with a README"

---

## Linking local repo to Gitlab

We need to tell git about our remote repository

```bash
>>> git remote add origin git@gitlab.com:andersbogsnes/calculator.git
```

Creates a *label* **origin** :point_right: **my_long_url_i_cant_remember**

---

## Push - Update remote from local

```bash
>>> git push -u origin master
```

I want to `push` my changes from my local branch to the branch named `master` at the url specified in `origin` and I want to link these two branches (`-u`)

---

## Exercise 10 min

- Pair up two and two


- pull request
- git pull
- git push
- review


{{% /section %}}


