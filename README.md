# Style guide

## Naming

### View IDs

View ID names (`android:id` in XML) should be all lowercase with underscores separating words.

```xml
<android.support.v7.widget.RecyclerView
    android:id="@+id/dance_moves"
    />
```

When using [Kotlin Android Extensions] to provide synthetic view properties, use the `as` keyword on [imports] to name
properties according to [non-constant names Android Kotlin codestyle]. The name should be [camel case], postfixed with
it's view type:

```kotlin
import kotlinx.android.synthetic.main.scan.dance_moves as danceMovesRecyclerView
```

# License

[![Creative Commons License](https://i.creativecommons.org/l/by/4.0/80x15.png)](http://creativecommons.org/licenses/by/4.0/)

This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).


[Kotlin Android Extensions]: https://kotlinlang.org/docs/tutorials/android-plugin.html
[imports]: https://kotlinlang.org/docs/reference/packages.html#imports
[non-constant names Android Kotlin codestyle]: https://android.github.io/kotlin-guides/style.html#non-constant-names
[camel case]: https://google.github.io/styleguide/javaguide.html#s5.3-camel-case
