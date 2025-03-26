---
date: '2024-11-23T08:41:00+07:00'
draft: false
title: 'Rhino Cheatsheet'
summary: 'Execute Javascript in Kotlin using Rhino'
categories:
- Code
tags:
- rhino
- javascript
- kotlin
- chatgpt
---

{{< gpt >}}

If you're using **Mozilla Rhino** in a **Kotlin** application, here's a tailored cheatsheet to help you work with Rhino's API:

---

### **Setting Up Rhino in Kotlin**

#### Adding Dependency

Add Rhino to your `build.gradle.kts` file:

```kotlin
dependencies {
    implementation("org.mozilla:rhino:1.7.15") // Use the latest version
}
```

---

### **Basic Rhino Script Execution**

#### Evaluating JavaScript

```kotlin
import org.mozilla.javascript.*

fun main() {
    val context = Context.enter() // Create a Rhino Context
    try {
        val scope = context.initStandardObjects() // Initialize global scope

        // JavaScript code as a string
        val script = "function greet(name) { return 'Hello, ' + name; } greet('Kotlin');"
        val result = context.evaluateString(scope, script, "script", 1, null)

        // Print the result
        println(Context.toString(result)) // Outputs: Hello, Kotlin
    } finally {
        Context.exit() // Exit the Rhino Context
    }
}
```

---

### **Interacting with Java Objects**

#### Pass Kotlin Objects to JavaScript

```kotlin
import org.mozilla.javascript.*

fun main() {
    val context = Context.enter()
    try {
        val scope = context.initStandardObjects()

        // Expose a Kotlin object to JavaScript
        val myList = mutableListOf("Apple", "Banana")
        ScriptableObject.putProperty(scope, "myList", Context.javaToJS(myList, scope))

        // JavaScript code
        val script = """
            myList.add('Cherry');
            myList.size;
        """
        val result = context.evaluateString(scope, script, "script", 1, null)

        // Print the updated Kotlin list and the script result
        println(myList) // [Apple, Banana, Cherry]
        println(Context.toString(result)) // 3
    } finally {
        Context.exit()
    }
}
```

#### Call JavaScript Functions from Kotlin

```kotlin
import org.mozilla.javascript.*

fun main() {
    val context = Context.enter()
    try {
        val scope = context.initStandardObjects()

        // Define a JS function
        val script = """
            function multiply(a, b) {
                return a * b;
            }
        """
        context.evaluateString(scope, script, "script", 1, null)

        // Access the JS function
        val multiply = scope.get("multiply", scope) as Function
        val result = multiply.call(context, scope, scope, arrayOf(5, 3))

        // Print the result
        println(Context.toString(result)) // 15
    } finally {
        Context.exit()
    }
}
```

---

### **Executing External JavaScript Files**

```kotlin
import org.mozilla.javascript.*
import java.io.FileReader

fun main() {
    val context = Context.enter()
    try {
        val scope = context.initStandardObjects()

        // Execute a JS file
        val reader = FileReader("path/to/your/script.js")
        context.evaluateReader(scope, reader, "script.js", 1, null)
        reader.close()
    } finally {
        Context.exit()
    }
}
```

---

### **Error Handling**

```kotlin
import org.mozilla.javascript.*

fun main() {
    val context = Context.enter()
    try {
        val scope = context.initStandardObjects()

        // Invalid JavaScript code
        val script = "function invalid() { return ; }"
        context.evaluateString(scope, script, "script", 1, null)
    } catch (e: RhinoException) {
        println("Error: ${e.message}")
        println("Stack Trace: ${e.scriptStackTrace}")
    } finally {
        Context.exit()
    }
}
```

---

### **Custom Functions in Kotlin**

Define and expose a Kotlin function to JavaScript:

```kotlin
import org.mozilla.javascript.*

fun main() {
    val context = Context.enter()
    try {
        val scope = context.initStandardObjects()

        // Define a custom function
        val kotlinFunction = object : BaseFunction() {
            override fun call(
                cx: Context?,
                scope: Scriptable?,
                thisObj: Scriptable?,
                args: Array<out Any?>
            ): Any {
                return "Hello from Kotlin, ${args[0]}"
            }
        }

        ScriptableObject.putProperty(scope, "kotlinGreet", kotlinFunction)

        // Call the function in JavaScript
        val script = "kotlinGreet('JavaScript');"
        val result = context.evaluateString(scope, script, "script", 1, null)

        println(Context.toString(result)) // Outputs: Hello from Kotlin, JavaScript
    } finally {
        Context.exit()
    }
}
```

---

### **Advanced Features**

#### Access Java/Kotlin Classes in JavaScript

```kotlin
import org.mozilla.javascript.*

fun main() {
    val context = Context.enter()
    try {
        val scope = context.initStandardObjects()

        // Expose a Java/Kotlin class
        ScriptableObject.putProperty(scope, "System", Context.javaToJS(System::class.java, scope))

        // Use the class in JavaScript
        val script = "System.out.println('Hello from JavaScript');"
        context.evaluateString(scope, script, "script", 1, null)
    } finally {
        Context.exit()
    }
}
```

---

### **Debugging**

Enable detailed stack traces and debug options:

```kotlin
val context = Context.enter()
context.optimizationLevel = -1 // Disable JIT for better debugging
context.setGeneratingDebug(true) // Generate debug info
```

### Troubleshooting

Common issues and solutions when working with Rhino:

#### Syntax Errors
- Error: "missing ; before statement (#15)"
  - Cause: Using Kotlin keywords (like `val`) in JavaScript code
  - Solution: Replace Kotlin keywords with JavaScript equivalents (use `var` or `let`)

#### Type Conversion Issues
- Error: "stack overflow: unexpected Object instead of string"
  - Cause: Automatic type conversion failing between JavaScript and Kotlin
  - Solution: Explicitly convert objects to strings using `String()` or `toString()`
  - Example: `String(title)` instead of just `title`

#### Other Tips
- Always check JavaScript syntax is valid
- Use strict mode (`'use strict';`) to catch errors early
- Enable debugging as shown in debugging section above


---

This cheatsheet should cover most use cases of Mozilla Rhino in **Kotlin**. For advanced details, refer to the [official Rhino documentation](https://github.com/mozilla/rhino).
