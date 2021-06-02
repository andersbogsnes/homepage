---
title: "CI/CD in a nutshell"
description: "A short introduction to CI/CD"
outputs: ["Reveal"]
date: 2021-06-01T09:00:00+02:00
reveal_hugo.theme: "black"
reveal_hugo.highlight_theme: "nord"
---

# CI/CD in a nutshell

---

{{< slide background-image=/images/ci_cd/ci-cd-flow.png background-size=contain >}}

---

{{% section %}}

## Continuos Integration

Integration means adding new code to the shared codebase 

What does it mean to do that continously?

---

## Version Control

- The code needs to be in version control.

<span class="fragment">These days that means git</span>

<ul>
    <li class="fragment">Roll back when something is wrong</li>
    <li class="fragment">Work independently on a shared codebase</li>
    <li class="fragment">A record of who has done what</li>
</ul>

---

## Test

<ul>
    <li class="fragment">All tests are run automatically</li>
    <li class="fragment">New code can't break tests</li>
    <li class="fragment">You still need to write the tests! 😉</li>
</ul>

---

## Lint

It is common to have a linter setup - a utility that checks codestyle, formatting etc.

This ensures that the code is formatted consistently across multiple developers

---

## Review

*After* tests are green and linters have passed, another developer gives a code review.

{{< figure src=/images/ci_cd/pull_request_diff.png >}}

---

Reviews are focused on logic and implementation

---

## Build

Take the approved code and package it into an *artifact*, pushing it to some artifact storage


{{< figure src=/images/ci_cd/dockerfile-icon.jpg >}}

<span class="fragment">
    This usually means a Docker Image, though it could be a binary, a JAR or a Python package
</span>


---

## Continous Integration

Continous Integration has three goals

<ul>
<li class="fragment">Quick feedback</li>
<li class="fragment">Rapid integration of new code into the main codebase</li>
<li class="fragment">...while ensuring high quality through automatic testing and linting</li>
</ul>

{{% /section %}}

---

{{% section %}}

## Continous Delivery

Continous Delivery is making sure that code is always in a releasable state. 

The CD part of the pipeline is in charge of taking the *artifact* and *deploying* to an *environment*

---

## Quick Aside - Definitions

---

### Artifact

The packaged code that is the result of our CI step

--- 

### Deploying

Releasing the *artifact* into a given *environment*

---

### Environment

A set of services and their configuration. Isolated from each other, so they don't share resources

---

### Configuration

A setting that changes how the *artifact* operates. 

---

In one environment, the GDPR masking setting might be turned on and another might have it turned off.

---

Same *artifact* - different behaviour depending on configuration

---

Read more at https://12factor.net/

---

## Deployment vs Delivery

<ul>
    <li class="fragment"><strong>Continous Deployment</strong> is having code that's always shippable</li>
    <li class="fragment"><strong>Continous Delivery</strong> is always shipping code</li>
</ul>


---

Most implement *Continous Deployment* - a pipeline that is able to deploy a given artifact to a given environment

---

## Deploying

What deploying means is different from project to project

<ul>
    <li class="fragment">Upload a binary to a homepage</li>
    <li class="fragment">Publish a package to a package index</li>
    <li class="fragment">Copy code to a webserver</li>
    <li class="fragment">Create a Kubernetes release</li>
</ul>

{{% /section %}}

---

{{% section %}}

# CICD Services

There are many vendors in this space

---

## Jenkins

{{< figure src=/images/ci_cd/jenkins.png >}}

- Is open source
- Written in Java
- Tons of plugins
- Groovy syntax

---

## Gitlab CI/CD

{{< figure src=/images/ci_cd/gitlab-cicd.png >}}

- Pioneered Repo -> CICD integration
- Tied to Gitlab
- YAML syntax

---

## Github Actions

{{< figure src=/images/ci_cd/github-actions.png height=50% width=50% >}}

- The new kid on the block
- Uses Node.js / Docker containers
- Tied to Github
- YAML syntax

---

## Bamboo

{{< figure src=/images/ci_cd/bamboo.png >}}

- Integrated with Bitbucket
- Written in Java
- Has a limited subset in YAML

{{% /section %}}

---

{{% section %}}

# Git workflow

To best use CI/CD, we often combine it with a standardized git workflow.

This helps write CI/CD pipelines that match the intentions when using git

---

## Git flow

{{< figure src=/images/ci_cd/git_flow.svg >}}


---

Each type of branch has a meaning

---

### Main branch

Is always in a releasable state - should only be merged into when ready to release

---

### Develop

Contains the newest features being worked on - the basis for new features

---

### Feature branch

When starting work on a new feature we create a feature branch starting from `develop`.

---

This is the main unit of work - every bit of new code starts as a feature branch and is merged back into `develop` when done, through a Pull Request.

When the feature branch is merged, it should be deleted

---

### Release

When we are ready to release, we create a release branch and run through the release checklist.

for example: 
- Update versions
- Update any version references

---

- Merge into master and develop.
- Tag master with the version number and push tags

CI/CD is often set up to run deployment if it's a tagged commit

{{% /section %}}

---
# Resources

- https://docs.gitlab.com/ee/ci/introduction/
- https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow