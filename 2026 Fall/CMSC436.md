# Android App Development: Foundations

## Android project structure

An Android Studio project separates Kotlin source code from **resources**, such as layouts, strings, colors, themes, and images.

- `MainActivity.kt` is the default activity class. An **Activity** usually supplies one user-facing screen and acts as the controller.
- `res/layout/activity_main.xml` defines the screen's view hierarchy.
- `res/values/strings.xml`, `colors.xml`, and `themes.xml` hold reusable text, colors, and styles.
- `AndroidManifest.xml` declares app components and requested capabilities. It also configures items such as the app label, icon, theme, and activity orientation.
- Android generates resource IDs. Refer to a resource with `R.type.name`, such as `R.layout.activity_main`, `R.string.app_name`, or `R.id.amount_bill`. Do not edit generated resource files.

### Activity setup

`onCreate` runs when Android creates an activity. After calling the superclass implementation, set the activity's initial view:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

In Model-View-Controller terms:

- **Model:** app data and logic, independent of the GUI. A `TipCalculator` class that stores a bill and tip rate belongs here.
- **View:** the XML layout and its widgets.
- **Controller:** the activity, which reads input, invokes the model, and updates the view.

## XML layouts and resources

XML resembles HTML but uses Android-defined tags and attributes. A non-empty element has opening and closing tags; an empty element can end with `/>`. Layout elements form a nested view tree.

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/message" />
```

- `match_parent`: make the view as large as its parent in that dimension.
- `wrap_content`: make the view only large enough for its content.
- `android:id="@+id/name"`: create an ID for a view. IDs support both layout positioning and access from Kotlin.
- Store displayed text in `strings.xml` and refer to it as `@string/name`; this keeps text reusable and easy to change.
- Define named colors in `colors.xml` and refer to them as `@color/name`.

### Colors, styles, and themes

Android color literals use RGB hexadecimal values:

- `#RRGGBB` for an opaque color.
- `#AARRGGBB` when an alpha/opacity channel is included. `00` is transparent and `FF` is opaque.

A **style** gathers attributes for a kind of view; styles may inherit from a parent. Apply one with `style="@style/Name"`. This keeps layout XML focused on structure rather than repeated visual attributes.

```xml
<style name="TextStyle" parent="@style/TextAppearance.AppCompat">
    <item name="android:textSize">32sp</item>
    <item name="android:padding">10dp</item>
</style>
```

A **theme** applies styling to an activity or the whole app, usually through the `android:theme` attribute in the manifest. Android commonly supplies a separate values resource for night mode.

### Common widgets and layout details

All ordinary GUI components inherit directly or indirectly from `View`.

- `TextView`: displays text.
- `EditText`: accepts user text. `android:inputType="numberDecimal"` requests a decimal-number keyboard.
- `Button`: triggers an action.
- `View`: can also provide a simple divider when given a background color and a small height, such as `5dp`.

`RelativeLayout` positions views relative to its other children. Give a referenced view an ID, then use attributes such as `layout_toRightOf`, `layout_below`, or `layout_alignBottom`. A `ConstraintLayout` can instead express constraints between views.

**Margin** is space outside a view; **padding** is space inside a view around its content. Text-related attributes include `android:textColor`, `android:textSize`, and `android:textStyle`.

## Kotlin essentials

- Kotlin does not require a semicolon at the end of a statement. It supports `//`, `/* ... */`, and documentation comments.
- Common types include `Int`, `Long`, `Double`, `Float`, `Short`, `Byte`, `Char`, `Boolean`, and `String`.
- `var` declares a mutable variable. `val` declares a value that can be assigned once. Type annotations are optional when Kotlin can infer the type.
- `const val` is a compile-time constant. It must be declared outside a local function and initialized with a string or primitive value. A regular `val` can hold a value computed at run time or an object reference.
- Convert values explicitly with functions such as `toInt()`, `toDouble()`, and `toString()`.
- A variable's scope starts at its declaration and ends with its innermost enclosing block.

```kotlin
var total = 0.0
val daysInWeek = 7
const val DAYS_IN_YEAR = 365
val name: String = "Jane"
```

### Arrays, control flow, and functions

Kotlin supplies specialized primitive arrays such as `IntArray`, `DoubleArray`, and `CharArray`. Use `intArrayOf(...)` when the values are known, or a constructor when the size or initial values are computed.

```kotlin
val grades = IntArray(30)                 // 30 zeroes
val scores = intArrayOf(89, 87, 90)
val words = Array(10) { "All Hi" }

val table = Array(4) { i ->
    Array(6) { j -> 10 * i + j }
}
```

`Array<Array<Int>>` represents a two-dimensional array: the outer array holds rows, and each row is another array. The lambda arguments `i` and `j` above are the row and column indices.

- `if`, `else if`, and `else` make selections. `when` fills the role of Java's `switch`.
- `while`, `do while`, and `for` support loops. `for (i in 0..10)` includes both `0` and `10`; `for (item in collection)` visits each item in an array, list, or string.
- `break` leaves a loop and `continue` skips to its next iteration.

A function declares its parameters as `name: Type`. Its return type follows the parameter list; use `Unit` for no meaningful return value. Kotlin also supports default arguments, named arguments, and `vararg` parameters.

```kotlin
fun add(a: Int, b: Int): Int {
    return a + b
}

fun printTotal(total: Double): Unit {
    println(total)
}
```

Exception handling uses `try`, `catch`, and, when needed, `finally`, much like Java. Numeric conversion such as `"341".toInt()` can throw if the string does not contain a valid number.

### Null safety

An ordinary type cannot hold `null`; add `?` when null is valid. Check a nullable value before use. `!!` forces a nullable value to a non-null type, but throws a `NullPointerException` if the value is actually null.

```kotlin
fun lengthOf(text: String?): Int {
    return if (text != null) text.length else 0
}
```

Use `Unit` for a function with no meaningful return value. It is Kotlin's counterpart to Java's `void` and can usually be omitted.

### Classes and inheritance

A class may declare a **primary constructor** in its header and additional constructors in its body. Add `var` or `val` to a primary-constructor parameter when it should become a property. Kotlin creates objects without `new`.

```kotlin
class Person(var name: String, var age: Int) {
    fun incrementAge() {
        age += 1
    }
}

val person = Person("Jane", 20)
```

- A secondary constructor uses the `constructor` keyword. Use it when callers need different construction options.
- Use `this.property` when a parameter hides an instance property.
- Return `this` from a mutating method when method chaining is desired.
- `init` blocks run during construction and can initialize properties. `lateinit var` defers initialization of a non-null reference property, such as a formatter; it cannot be used with primitive types.

```kotlin
class TipCalculator {
    private var amount = 0.0

    fun setAmount(amount: Double): TipCalculator {
        if (amount >= 0.0) this.amount = amount
        return this
    }
}

val calculator = TipCalculator().setAmount(134.56)
```

### Nested classes, data classes, and operators

A nested class has no implicit reference to an instance of its outer class. Mark it `inner` when it must access the outer class's properties.

```kotlin
class Outer {
    class Nested

    var age = 0
    inner class Incrementer {
        fun addOne() {
            age += 1
        }
    }
}

val nested = Outer.Nested()
val incrementer = Outer().Incrementer()
```

Use a `data class` for an object that mainly holds values, for example `data class Student(var age: Int)`. Kotlin generates useful value-oriented operations for data classes. Kotlin can also overload operators such as `+`, `++`, and comparison operators by defining the corresponding `operator` function.

### Inheritance and interfaces

Every class inherits from `Any`, which supplies `equals`, `hashCode`, and `toString`. Classes and members are final by default; mark a class or function `open` to permit inheritance or overriding. The colon after a class name denotes inheritance or interface implementation.

```kotlin
open class Account

class SavingsAccount : Account()

abstract class Shape {
    abstract fun area(): Double
}

class Square(private val side: Double) : Shape() {
    override fun area(): Double = side * side
}
```

An `abstract` class cannot be instantiated. Its abstract members are open by default, and each concrete subclass must implement them with `override`. A class can implement an interface with the same `: InterfaceName` syntax; it must implement the interface's required methods.

## Connecting the view to Kotlin

An activity can locate a view in its inflated layout with `findViewById`:

```kotlin
val billEditText: EditText = findViewById(R.id.amount_bill)
val tipTextView: TextView = findViewById(R.id.amount_tip)
```

For views used across several methods, keep references as activity properties and initialize them in `onCreate`. An `EditText` returns an `Editable`; convert it to a string before numeric conversion:

```kotlin
val bill = billEditText.text.toString().toDouble()
```

`inputType` guides user input but does not remove the need to handle blank or otherwise invalid input before calling `toDouble()`.

### Events and listeners

An event listener responds to user actions. The traditional XML `android:onClick` attribute can point to an activity method, but Kotlin code more commonly registers a listener directly:

```kotlin
button.setOnClickListener {
    calculate()
}
```

For a calculator that updates as the user types, register a `TextWatcher` on each `EditText`. `afterTextChanged` is the appropriate callback when the calculation should use the completed edit.

```kotlin
class TextChangeHandler : TextWatcher {
    override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}
    override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}
    override fun afterTextChanged(s: Editable?) {
        calculate()
    }
}

val handler = TextChangeHandler()
billEditText.addTextChangedListener(handler)
tipEditText.addTextChangedListener(handler)
```

## View binding

View binding provides type-safe references to views with IDs and replaces most `findViewById` calls. Enable it in the app module's Gradle configuration, then sync the project:

```kotlin
android {
    buildFeatures {
        viewBinding = true
    }
}
```

For `activity_main.xml`, Android generates `ActivityMainBinding`. Inflate it in `onCreate`, use its root as the content view, and access IDs through the binding object.

```kotlin
private lateinit var binding: ActivityMainBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = ActivityMainBinding.inflate(layoutInflater)
    setContentView(binding.root)

    binding.amountBill.setText("0.0")
}
```

## Running and debugging

- Run an app on an Android Virtual Device (AVD) from Android Studio's Device Manager or on a connected device with developer options and USB debugging enabled.
- Use **Logcat** to inspect messages. `Log.d`, `Log.i`, `Log.w`, and `Log.e` label messages by level; use a consistent tag such as `"MainActivity"` to filter output.
- Use Android Studio's debugger to set breakpoints, inspect variables, step through code, and resume execution.
- Gradle builds the app package. An APK is the distributable Android application file (`.apk`).
