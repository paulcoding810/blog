---
date: "2026-02-21T23:27:22+07:00"
draft: false
title: "Android File Resolver"
summary: "Understanding Android FileProvider and file_paths.xml for secure file sharing"
categories:
  - Code
tags:
  - fileprovider
  - android
---

{{< gpt >}}

This XML is typically used with **`FileProvider`** in Android to safely share files between apps.

It defines which directories your app is allowed to share via a `content://` URI instead of exposing raw file paths (`file://`), which is required starting from Android 7.0+.

---

## 📁 What This File Is

This is usually saved as:

```
res/xml/file_paths.xml
```

And referenced in your `AndroidManifest.xml` inside a `<provider>` tag using:

```xml
android.support.FILE_PROVIDER_PATHS
```

(or `androidx.core.content.FileProvider` if using AndroidX)

---

## 🔍 Explanation of Each Part

### 1️⃣ XML Header

```xml
<?xml version="1.0" encoding="utf-8"?>
```

Standard XML declaration.

---

### 2️⃣ Root Element

```xml
<paths>
```

Container for all shareable file path definitions.

---

### 3️⃣ External Path

```xml
<external-path
    name="download_path"
    path="." />
```

#### ✅ What it means:

- Allows sharing files from **external storage**
- `path="."` means **root of external storage**
- `name="download_path"` is just an internal alias

#### 📂 This maps to:

```
/storage/emulated/0/
```

So your app can share files like:

```
/storage/emulated/0/Download/example.pdf
```

⚠️ Be careful: `path="."` gives access to the entire external storage root.

---

### 4️⃣ Cache Path

```xml
<cache-path
    name="cache"
    path="." />
```

#### ✅ What it means:

- Allows sharing files from your app’s **internal cache directory**
- `path="."` means the root of:

```
/data/data/your.package.name/cache/
```

This is safer and app-private.

---

## 🔐 Why This Is Needed

From Android 7.0 (API 24), Android blocks sharing `file://` URIs and throws:

```
FileUriExposedException
```

Instead, you must use:

```java
FileProvider.getUriForFile(...)
```

Which generates:

```
content://your.package.name.provider/...
```

The XML tells FileProvider what directories it is allowed to expose.

---

## 🧠 Summary

| Tag               | Allows Sharing From | Typical Location            |
| ----------------- | ------------------- | --------------------------- |
| `<external-path>` | External storage    | `/storage/emulated/0/`      |
| `<cache-path>`    | App cache folder    | `/data/data/package/cache/` |

---

## ⚠️ Security Note

Using:

```xml
path="."
```

means **everything inside that directory is shareable**.

For better security, you should restrict it like:

```xml
<external-path
    name="downloads"
    path="Download/" />
```

This limits access only to the Downloads folder.
