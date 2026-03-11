---
date: "2026-03-11T10:52:52+07:00"
draft: false
title: "Understanding Android Architecture: MVVM Pattern"
summary: "A comprehensive guide to the MVVM architecture in Android, focusing on ViewModel, Repository, and DataSource."
categories:
  - Android
  - Architecture
tags:
  - MVVM
  - Android
  - Clean Architecture
---

{{< gpt >}}

In Android (especially with **MVVM architecture**), `ViewModel`, `Repository`, and `DataSource` each have distinct responsibilities. Understanding the separation helps you write clean, testable, and scalable apps.

---

## 1️⃣ ViewModel (UI Logic Layer)

Part of Android Jetpack.

### Responsibility:

- Holds and manages **UI-related data**
- Survives configuration changes (e.g., rotation)
- Calls the Repository to fetch/update data
- Exposes observable state (LiveData / StateFlow)

### Should NOT:

- Access database directly
- Call Retrofit directly
- Contain heavy business logic
- Know where data comes from

### Example:

```kotlin
class UserViewModel(
    private val repository: UserRepository
) : ViewModel() {

    val users = repository.getUsers()

    fun refresh() {
        viewModelScope.launch {
            repository.refreshUsers()
        }
    }
}
```

Think of ViewModel as:

> “What does the UI need?”

---

## 2️⃣ Repository (Data Orchestration Layer)

### Responsibility:

- Acts as **single source of truth**
- Decides where data comes from (network, database, cache)
- Combines multiple data sources
- Contains business logic related to data

It hides implementation details from the ViewModel.

### Example:

```kotlin
class UserRepository(
    private val remote: UserRemoteDataSource,
    private val local: UserLocalDataSource
) {

    fun getUsers(): Flow<List<User>> = local.getUsers()

    suspend fun refreshUsers() {
        val users = remote.fetchUsers()
        local.saveUsers(users)
    }
}
```

Think of Repository as:

> “Where should I get the data from?”

---

## 3️⃣ DataSource (Data Provider Layer)

DataSources are **concrete implementations** that actually fetch/store data.

Usually:

- RemoteDataSource → API (Retrofit)
- LocalDataSource → Room database
- CacheDataSource → In-memory cache

### Example (Remote):

```kotlin
class UserRemoteDataSource(
    private val api: UserApi
) {
    suspend fun fetchUsers(): List<User> {
        return api.getUsers()
    }
}
```

### Example (Local):

```kotlin
class UserLocalDataSource(
    private val dao: UserDao
) {
    fun getUsers(): Flow<List<User>> = dao.getUsers()
    suspend fun saveUsers(users: List<User>) = dao.insert(users)
}
```

Think of DataSource as:

> “Here’s the actual implementation.”

---

# 🔥 Quick Comparison

| Layer      | Knows About | Should Not Know About |
| ---------- | ----------- | --------------------- |
| ViewModel  | Repository  | API, Room             |
| Repository | DataSources | UI                    |
| DataSource | API or DB   | UI, ViewModel         |

---

# 💡 When to Use What?

### Small App?

You _can_ skip DataSource and let Repository talk directly to API/Room.

### Medium/Large App?

Use:

```
UI → ViewModel → Repository → DataSource → API/DB
```

Because:

- Easier testing (mock each layer)
- Easier to scale
- Clear separation of concerns
- Cleaner architecture

---

# 🧠 Real-World Tip

In Clean Architecture:

- ViewModel → Presentation layer
- Repository → Domain/Data boundary
- DataSource → Data layer implementation
