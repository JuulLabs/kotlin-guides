# Style guide

Code styles shall be adhered to in the following order (whereas their precedence follows the order listed and subsequent
guides can be used as fall back if the previous does not cover a specific topic):

1. [Android Kotlin Guides](https://android.github.io/kotlin-guides/style.html)
2. [JetBrain's Kotlin Coding Conventions](https://kotlinlang.org/docs/reference/coding-conventions.html)
3. This Guide

## Naming

### Variables

Variable names should be named to remove ambiguity. In general, including a variable's type in it's name is overly
verbose, unless it provides necessary clarity. For example, a list of locations (`List<Location>`) could appropriately
be named `locations` (by virtue of being plural, it implies that the variable is likely a `Collection`).

Conversely, a property of type `LiveData<List<Location>>` would be appropriately named to include it's type in it's
name (`locationsLiveData`). This helps to avoid confusion, as `val locations` is an unchanging reference to a collection
of objects of type `Location`, while it would be useful to differentiate a property of type `LiveData<List<Location>>`
as _also_ being an unchanging reference _but_ able to emit multiple `List<Location>` collections.

```kotlin
val colors = listOf(Color.RED, Color.GREEN, Color.BLUE) // Okay
```

```kotlin
val color: LiveData<Color> // Discouraged

val colorLiveData: LiveData<Color> // Okay
```

```kotlin
val color: Observable<Color> // Discouraged

val colorObservable: Observable<Color> // Okay
```

Abbreviations in variable names are strongly discouraged. Acronyms may be used if they are standard nomenclature
(commonly used in place of their longhand counterparts).

```kotlin
val bookTitle = "Programming 101" // Okay

val bookTtl = "Programming 101" // WRONG!
```

```kotlin
val ipAddress = "127.0.0.1" // Okay

val ipAddr = "127.0.0.1" // WRONG!
```

### View IDs

View ID names (`android:id` in XML) should be all lowercase with underscores separating words (snake_case).  Do your best to create a name that contains an underscore so it is distinguishable from local variables. 

```xml
<android.support.v7.widget.RecyclerView
    android:id="@+id/dance_moves"
    />

<Button
    android:id="@+id/login"
    />
```

When using [Kotlin Android Extensions] to provide synthetic view properties, use the `as` keyword on [imports] to name
properties that do not have an underscore in their name, and append `_view` to the end


```kotlin
import kotlinx.android.synthetic.main.scan.dance_moves
import kotlinx.android.synthetic.main.scan.login as login_view
```

# License

[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/80x15.png)](http://creativecommons.org/licenses/by/4.0/)

This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).


[Kotlin Android Extensions]: https://kotlinlang.org/docs/tutorials/android-plugin.html
[imports]: https://kotlinlang.org/docs/reference/packages.html#imports
[non-constant names Android Kotlin codestyle]: https://android.github.io/kotlin-guides/style.html#non-constant-names
[camel case]: https://google.github.io/styleguide/javaguide.html#s5.3-camel-case
