---
date: "2026-03-11T21:35:56+07:00"
title: "Android Build Variants"
summary: "Master build types, flavors, dimensions, and variants in Kotlin DSL for Android projects"
categories:
  - Code
tags:
  - android
  - gradle
---

{{< gpt >}}

In **Android Gradle**, these four concepts—**Flavor**, **Dimension**, **Build Type**, and **Variant**—work together to generate different versions of your app (for example: free vs paid, dev vs prod). Below is a clear explanation with **Kotlin DSL (`build.gradle.kts`) examples**.

---

# 1. Build Type

A **Build Type** defines **how the app is built** (debugging, optimization, signing).

Android already provides two default types:

- `debug`
- `release`

### Example (Kotlin DSL)

```kotlin
android {
    buildTypes {
        getByName("debug") {
            isDebuggable = true
            applicationIdSuffix = ".debug"
        }

        getByName("release") {
            isMinifyEnabled = true
            isShrinkResources = true
            proguardFiles(
                getDefaultProguardFile("proguard-android-optimize.txt"),
                "proguard-rules.pro"
            )
        }
    }
}
```

### Result

Two builds:

```
app-debug.apk
app-release.apk
```

Typical usage:

- `debug` → development/testing
- `release` → production

---

# 2. Product Flavor

A **Flavor** defines **different versions of the app itself**.

Example:

- free vs paid
- dev vs production server
- googlePlay vs amazon store

### Example

```kotlin
android {
    productFlavors {

        create("free") {
            applicationIdSuffix = ".free"
            versionNameSuffix = "-free"
        }

        create("paid") {
            applicationIdSuffix = ".paid"
        }
    }
}
```

### Result

```
freeDebug
freeRelease
paidDebug
paidRelease
```

Each flavor can have:

- different API URLs
- different features
- different app icons

---

# 3. Flavor Dimension

A **Dimension** groups flavors into **categories**.

Why? Because Gradle allows **multiple flavor categories**.

Example categories:

- `environment`
- `tier`

### Example

```
tier: free / paid
environment: dev / prod
```

### Kotlin DSL

```kotlin
android {

    flavorDimensions += listOf("tier", "environment")

    productFlavors {

        create("free") {
            dimension = "tier"
        }

        create("paid") {
            dimension = "tier"
        }

        create("dev") {
            dimension = "environment"
            applicationIdSuffix = ".dev"
        }

        create("prod") {
            dimension = "environment"
        }
    }
}
```

### Generated combinations

```
freeDevDebug
freeDevRelease
freeProdDebug
freeProdRelease
paidDevDebug
paidDevRelease
paidProdDebug
paidProdRelease
```

This is extremely powerful for **large apps**.

---

# 4. Build Variant

A **Variant = Flavor(s) + Build Type**

Formula:

```
Variant = ProductFlavor(s) + BuildType
```

Example:

| Flavor | Build Type | Variant     |
| ------ | ---------- | ----------- |
| free   | debug      | freeDebug   |
| free   | release    | freeRelease |
| paid   | debug      | paidDebug   |
| paid   | release    | paidRelease |

With dimensions:

```
paidProdRelease
freeDevDebug
```

---

# 5. Folder Structure for Variants

Android lets you override code/resources per flavor or variant.

Example:

```
src/
 ├ main/
 │   └ java/
 ├ debug/
 │   └ java/
 ├ free/
 │   └ java/
 ├ freeDebug/
 │   └ java/
 ├ dev/
 │   └ java/
```

Priority order:

```
variant > flavor > buildType > main
```

Example:

```
src/freeDebug/java
```

overrides everything.

---

# 6. Real Example (API environments)

```kotlin
android {

    flavorDimensions += "environment"

    productFlavors {

        create("dev") {
            dimension = "environment"

            buildConfigField(
                "String",
                "BASE_URL",
                "\"https://dev.api.com\""
            )
        }

        create("prod") {
            dimension = "environment"

            buildConfigField(
                "String",
                "BASE_URL",
                "\"https://api.com\""
            )
        }
    }
}
```

Usage in Kotlin:

```kotlin
val baseUrl = BuildConfig.BASE_URL
```

---

# 7. Visual Summary

```
                    +--------------+
                    |  Build Type  |
                    | debug/release|
                    +--------------+
                           |
                           |
              +----------------------------+
              |        Product Flavor       |
              | free / paid / dev / prod    |
              +----------------------------+
                           |
                           |
                    +-------------+
                    |   Variant   |
                    | freeDebug   |
                    | paidRelease |
                    +-------------+
```

---

# 8. Example Full `build.gradle.kts`

```kotlin
android {

    compileSdk = 34

    defaultConfig {
        applicationId = "com.example.app"
        minSdk = 24
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }

    buildTypes {
        debug {
            applicationIdSuffix = ".debug"
        }

        release {
            isMinifyEnabled = true
        }
    }

    flavorDimensions += "tier"

    productFlavors {

        create("free") {
            dimension = "tier"
        }

        create("paid") {
            dimension = "tier"
        }
    }
}
```

Variants generated:

```
freeDebug
freeRelease
paidDebug
paidRelease
```

---

✅ **Quick Memory Trick**

```
Build Type → HOW the app is built
Flavor → WHAT version of the app
Dimension → CATEGORY of flavors
Variant → FINAL combination
```
