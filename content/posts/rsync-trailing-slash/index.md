---
date: "2026-02-12T08:20:43+07:00"
draft: false
title: "Rsync trailing slash"
summary: "How rsync treats trailing slashes — whether the source directory itself is copied or only its contents."
description: "Explains why adding or omitting a trailing slash to the rsync source changes whether the parent folder is included, plus notes on moving vs copying."
categories:
  - Tools
tags:
  - rsync
  - linux
  - chatgpt
---

{{< gpt >}}

It **does not move the parent folder by default** — this trips a lot of people up.

`rsync` copies the **contents** of the source directory, and whether the parent folder itself appears at the destination depends on the **trailing slash**.

### The key rule (trailing slash matters a lot)

#### 1️⃣ With a trailing slash → copies _contents only_

```bash
rsync -av source_dir/ dest_dir/
```

Result:

```
dest_dir/
├── file1
├── file2
└── subdir/
```

`source_dir` itself is **not** created inside `dest_dir`.

---

#### 2️⃣ Without a trailing slash → copies the folder itself

```bash
rsync -av source_dir dest_dir/
```

Result:

```
dest_dir/
└── source_dir/
    ├── file1
    ├── file2
    └── subdir/
```

Here, the **parent folder _is_ included**.

---

### If you mean “move” instead of copy

`rsync` is a copy tool. To _move_, you add:

```bash
--remove-source-files
```

…but that **only removes files, not directories**, so you’d usually follow with:

```bash
find source_dir -type d -empty -delete
```

Or just use:

```bash
mv source_dir dest_dir/
```

if it’s local and simple.

---

### TL;DR

- **Trailing slash = no parent folder**
- **No trailing slash = parent folder included**
- `rsync` copies by default, doesn’t move unless you force it

If you want, tell me what you’re trying to end up with and I’ll give you the exact command 👌
