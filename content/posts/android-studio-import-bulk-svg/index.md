---
date: "2025-08-05T22:38:19+07:00"
draft: false
title: "Import Bulk SVGs into Android Studio via CLI"
summary: "Quickly convert and import multiple SVGs into Android Studio using SVGO and vd-tool."
categories:
  - Code
tags:
  - android
  - svg
  - tools
---

Efficiently import multiple SVGs into Android Studio using command-line tools **SVGO** and **vd-tool**.

### Step 1: Optimize SVGs with SVGO

Run:

```bash
svgo -f ~/svgs -o ~/svgs_go
```

This optimizes SVGs, reducing size and cleaning up files.

### Step 2: Convert to Vector Drawables

Run:

```bash
vd-tool -c -in ~/svgs_go -out ~/svgs_android
```

This converts optimized SVGs to Android-compatible vector drawables.

### Step 3: Import into Android Studio

Copy the generated files to `app/src/main/res/drawable/` in your project.

### Conclusion

Using SVGO and vd-tool streamlines bulk SVG imports for Android development. Happy coding!
