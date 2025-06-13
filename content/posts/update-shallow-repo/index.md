
---
date: '2025-06-13T10:59:01+07:00'
draft: false
title: 'How to Update a Shallow Git Repository'
summary: 'Learn how to update a shallow-cloned Git repository with the latest commit or a specific branch, and how to convert it to a full clone.'
categories:

- Git
- Code
tags:
- git
- chatgpt

---

{{< gpt >}}

You're asking how to **update a shallow clone** (a repo cloned with limited history using `--depth`) with the **latest commit** from the remote. Here's how you do that.

---

### ✅ Update a shallow repo with the latest commit

If you've done a shallow clone like:

```bash
git clone --depth 1 https://github.com/example/repo.git
```

You can pull the latest commit like this:

```bash
cd repo
git fetch --depth=1
git pull --ff-only
```

This fetches only the newest commit and updates your shallow repo without downloading the full history.

---

### 🔄 Update to a **specific branch or commit**

To update a specific branch (e.g., `main`):

```bash
git fetch --depth=1 origin main
git checkout main
git pull --ff-only origin main
```

This ensures you’re on the correct branch and fast-forwards to the latest commit, keeping the history shallow.

---

### 🧼 Limitations and how to get full history

**Note:** In a shallow repo, operations like rebase, cherry-pick, or merge may not work due to missing history. To convert your shallow clone into a full clone:

```bash
git fetch --unshallow
```

This command downloads the entire history, removing the depth limitation.

---
