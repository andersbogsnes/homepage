---
weight: 60
outputs: ["Reveal"]
---

{{% section %}}

# Git Flow

Git is a flexible tool, and many different workflows have built up around how to use git when working in teams

Git flow is one such workflow which has become very popular.

---

## The branches

Git flow has five main types of branches

- master
- develop
- feature
- release
- hotfix

---

### Master

- Always contains the code that is in production 
- This is the branch we deploy to production

---

### Develop

- Where the newest features are included, but is not yet released
- Should generally be ready to release
- The branch we merge into when our features are done
- Where features branch from

---

### Feature

- Represents a new unit of work we want to do
- Should be short-lived and contain only the code necessary to implement the new feature
- One issue/feature per feature branch
- Merged into develop

---

### Release

- Created to release a new version
- Represents a release
- Generally only bump version number
- Merge into master and develop

---

### Hotfix

- Only for critical bugs that can't wait for a new release
- Based off of `master` and not `develop`
- Is merged into `master` and `develop`

---

{{< figure src=https://nvie.com/img/git-model@2x.png height=640 width=480 link=https://nvie.com/posts/a-successful-git-branching-model attr="*Vincent Driessen@nvie.com*" >}}

---

## Benefits

- Feature workflow makes it easy to do code reviews
- Having a dedicated production branch makes it easy to see what was in production when
- Easy to collaborate on new features

---

## Negatives

- Lots of branching
- Only enforced by convention

---

## Additional resources

- [The original article](https://nvie.com/posts/a-successful-git-branching-model/)

- [Atlassian's example](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

- [An (opinionated) Datascience take on branching](https://github.com/dslp/dslp/blob/main/branching/branch-types.md)

{{% /section %}}