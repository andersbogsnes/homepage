---
weight: 20
outputs: ["Reveal"]
---

{{% section %}}

# Initialize a new repo

To have git track our code, we must tell git that it should create a new *repository*

---

## Git Init

```bash
>>> git init
Initialized empty Git repository in /home/anders/projects/git_demo/.git/
```

Now there should be something in your directory

```bash
>>> ls -a
.  ..  .git
```

---

## Open up the magic directory

Let's have a quick peek under the covers

*Look, don't touch - You will (almost) never need to do anything in here* 

```bash
>>> tree -a .git
.git
├── branches # Any branches are stored here
├── config # Any local configuration is here
├── description # used by gitweb
├── HEAD # points at the HEAD commit
├── hooks # Any scripts you want to run during the git lifecycle
├── info # Contains a local exclude
├── objects # The key-value database
└── refs # Stores the names of references
```

---

## Let's actually do some work and come back to this



{{% /section %}}