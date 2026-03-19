---
date: "2026-03-20T00:05:34+07:00"
title: "Bash wildcard"
summary: "Bash wildcard behavior in Bash"
categories:
  - Code
tags:
  - bash
  - wildcard
---

**if you put `*` inside quotes, it does NOT work as a wildcard in Bash.**

### Why?

In Bash, wildcard expansion (called _globbing_) happens **before** quotes are processed. But if you quote something, you’re telling Bash:

> “Treat this literally — don’t expand it.”

### Examples

#### ✅ Works (unquoted wildcard)

```bash
ls *.txt
```

→ Expands to:

```bash
ls file1.txt file2.txt
```

#### ❌ Does NOT work (quoted wildcard)

```bash
ls "*.txt"
```

→ Bash treats `"*.txt"` as a literal filename
→ It will look for a file literally named `*.txt`

---

### Partial quoting nuance

```bash
ls file"*.txt"
```

This still **does NOT expand**, because part of the pattern is quoted.

---

### How to safely use wildcards with variables

If you're using variables:

```bash
pattern="*.txt"
ls $pattern     # expands
ls "$pattern"   # does NOT expand
```

---

### Rule of thumb

- **Unquoted → wildcard expands**
- **Quoted → wildcard is literal**

---

If you want, I can show you tricks like safely combining wildcards with spaces or using `find` instead (which behaves differently).

-
