# Style guide

Code styles shall be adhered to in the following order (whereas their precedence follows the order listed and subsequent
guides can be used as fall back if the previous does not cover a specific topic):

1. This Guide
2. [Android Kotlin Guides](https://android.github.io/kotlin-guides/style.html)
3. [JetBrain's Kotlin Coding Conventions](https://kotlinlang.org/docs/reference/coding-conventions.html)

## Naming

### Variables

Variable names should be named to remove ambiguity. In general, including a variable's type in it's
name is overly verbose, unless it provides necessary clarity. For example, a list of locations
(`List<Location>`) would appropriately be named `locations` (by virtue of being plural, it implies
that the variable is likely a `Collection`).

#### Data Streams

Data streams (e.g. `Flow`, `Channel` or `LiveData`) shall be named to describe the data flowing
through them. For example, a data stream of singular color values (e.g. `Flow<Color>`) should be
named `color`. For data streams of `Collections` (e.g. `Flow<List<Color>>`) they should take the
plural form (e.g. `colors`).

| Type                       | Naming   | Example Type        | Example Naming |
|----------------------------|----------|---------------------|----------------|
| Single value variable      | Singular | `Color`             | `color`        |
| Collection                 | Plural   | `List<Color>`       | `colors`       |
| Data flow of single values | Singular | `Flow<Color>`       | `color`        |
| Data flow of collections   | Plural   | `Flow<List<Color>>` | `colors`       |

Instances where naming would conflict (when you have a property to hold a single value of the same
type as a property referencing a data flow, e.g. `Color` and `Flow<Color>`) the naming precedence
shall follow the ordering of the table above, with the lower precedence being postfixed with it's
type.

When you have naming conflicts with multiple data flow properties, their naming precedence is as
follows:

- `Flow`
- `Channel`
- `LiveData`

```kotlin
interface Example {
    val color: Color // Single value property takes precedence over data flow properties.
    val colorFlow: Flow<Color> // Name postfixed with type to resolve naming conflict.
}
```

```kotlin
interface Example {
    val color: Flow<Color> // Flow takes precedence over LiveData.
    val colorLiveData: LiveData<Color> // Naming of LiveData property is postfixed to resolve naming conflict.
}
```

```kotlin
interface Example {
    val color: Color
    val colorFlow: Flow<Color>
    val colorLiveData: LiveData<Color>
}
```

#### Abbreviations

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

#### Feature Flags
Boolean Feature Flags configured remotely should be named such that `true` coresponds to the feature being `enabled` and `false` is `disabled`.

```kotlin
val featureAbc = ExampleRemoteBoolean("feature_abc", true) // Okay, defaults to enabled

val featureAbc = ExampleRemoteBoolean("feature_abc", false) // Okay, defaults to disabled

val featureAbc = ExampleRemoteBoolean("feature_abc_disabled", true) // WRONG! true would result in feature being enabled
```

### View IDs

When Kotlin extensions are used (and view IDs are made available via `import`), view ID names
(`android:id` in XML) should be all lowercase with underscores separating words (snake_case). It is
required that every name have at least one underscore to distinguish it from local variables. In
instances were it proves difficult, then name may be postfixed with the component type
(e.g. `_button` or `_view`).

```xml
<android.support.v7.widget.RecyclerView
    android:id="@+id/dance_moves"
    />
```


```kotlin
import kotlinx.android.synthetic.main.scan.dance_moves
import kotlinx.android.synthetic.main.scan.login_button
import kotlinx.android.synthetic.main.scan.root_view
```

## Nullability

| Operation          | Usage ("When the value...")     | Throws                         |
|--------------------|---------------------------------|--------------------------------|
| [`!!`]             | is "known" to be non-`null`.    | [`KotlinNullPointerException`] |
| [`checkNotNull`]   | is "expected" to be non-`null`. | [`IllegalStateException`]      |
| [`requireNotNull`] | "must" not be `null`.           | [`IllegalArgumentException`]   |

### `!!`

The use of [`!!`] (double bang) operator is allowed but should be used sparingly. It should _only_
be used when the value is **known** to never be `null` (but the compiler is unable to ascertain
nullness), such as:

- Type provided by Java code does not have nullability markers
- Smart casting is not possible when accessing variable from another module

When the reason for the `!!` usage is not immediately obvious, then a comment should be added with
rationale for the `!!` usage.

### `checkNotNull`

`checkNotNull` should be used when a variable can be `null` but you are **not expecting** it to be.
An example is having a nullable `var` that is inialized to `null` but is assigned a value at a later
time. When accessing the `var` (and expecting the value to no longer be `null`) then `checkNotNull`
should be used.

### `requireNotNull`

When you are provided a value (for example, as an argument of a function) to which it's nullness is
unknown (e.g. Java type that is missing nullability markers) but value **must** not be `null`, then
`requireNotNull` should be used.

## Coroutines

### `suspend` vs `CoroutineScope`

Per [Kotlin Coroutines in Practice by Roman Elizarov], the following conventions are recommended:

| Paradigm                  | Usage                                                              |
|---------------------------|--------------------------------------------------------------------|
| `suspend`                 | Does something long & waits for it to complete without blocking.   |
| `CoroutineScope` function | Launches new Coroutines & quickly returns, does not wait for them. |

```kotlin
// Does not return until it (and it's children) are done.
suspend fun downloadContent(location: Location): Content

// Launches Coroutines and returns immediately.
fun CoroutineScope.processReferences(...)
```

# License

[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/80x15.png)](http://creativecommons.org/licenses/by/4.0/)

This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).


[Kotlin Android Extensions]: https://kotlinlang.org/docs/tutorials/android-plugin.html
[imports]: https://kotlinlang.org/docs/reference/packages.html#imports
[non-constant names Android Kotlin codestyle]: https://android.github.io/kotlin-guides/style.html#non-constant-names
[camel case]: https://google.github.io/styleguide/javaguide.html#s5.3-camel-case
[`!!`]: https://kotlinlang.org/docs/reference/null-safety.html#the--operator
[`KotlinNullPointerException`]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-kotlin-null-pointer-exception/
[`checkNotNull`]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/check-not-null.html
[`IllegalStateException`]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-illegal-state-exception/#kotlin.IllegalStateException
[`requireNotNull`]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/require-not-null.html
[`IllegalArgumentException`]: https://kotlinlang.org/api/latest/jvm/stdlib/kotlin/-illegal-argument-exception/#kotlin.IllegalArgumentException
[Kotlin Coroutines in Practice by Roman Elizarov]: https://youtu.be/a3agLJQ6vt8?t=2160
