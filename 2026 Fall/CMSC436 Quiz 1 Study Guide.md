
Scope: `1-HelloAndroid`, `2A-KotlinBasics`, and `3-TipCalculator`.

## Expected question style

The Spring 2026 Quiz 1 used 10 short questions: check-all-that-apply, one correct option, and one-line Kotlin or XML completion. Practice writing exact syntax without relying on autocomplete.

## High-yield facts

### Android and XML

- `MainActivity.kt` is the controller. `activity_main.xml` is the view. `TipCalculator.kt` is the model.
- Set the initial layout in `onCreate` with `setContentView(R.layout.activity_main)`.
- Resource references use `@string/name`, `@color/name`, `@style/name`, `@layout/name`, and `R.id/name`.
- Create an XML ID with `android:id="@+id/name"`.
- `match_parent` fills the parent in that dimension. `wrap_content` fits the content.
- Valid color forms include `#RRGGBB`, `#AARRGGBB`, `#RGB`, and `#ARGB`.
- `RelativeLayout` places views relative to IDs. Know `layout_toRightOf`, `layout_below`, and `layout_alignBottom`.
- Use `android:inputType="numberDecimal"` for decimal input and `android:hint="@string/name"` for input guidance.
- A style applies to a view; a theme applies to an activity or app through `android:theme` in the manifest.

### Kotlin

- `var` can change; `val` cannot be reassigned; `const val` is a compile-time primitive or string constant outside local functions.
- A function header follows `fun name(parameter: Type): ReturnType`.
- `0..10` includes both endpoints. Kotlin uses `when` instead of Java `switch`; it has no ternary operator.
- A nullable type ends in `?`. `!!` forces a non-null value and can throw `NullPointerException`.
- Kotlin creates objects without `new`: `val p = Person("Jane", 20)`.
- Classes are final by default. Use `open` to allow inheritance. `class A : B()` means A inherits from B.
- `Array<Array<Int>>` is a two-dimensional array. `IntArray(30)` creates 30 zeroes.

### Tip calculator and Android events

- `findViewById(R.id.amount_bill)` retrieves a view by ID. View binding replaces most such calls with `binding.amountBill`.
- A click listener can use `button.setOnClickListener { calculate() }`.
- XML click handling uses `android:onClick="calculate"` and `fun calculate(v: View)` in the activity.
- `TextWatcher` requires `beforeTextChanged`, `onTextChanged`, and `afterTextChanged`. Register it with `addTextChangedListener` on both input fields.
- `EditText.text` is `Editable`; use `.text.toString().toDouble()` only after handling blank or invalid input.
- Logcat uses a tag and message, for example `Log.w("MainActivity", "Inside onCreate")`.

## Practice Quiz

### 1. Check all valid color definitions for blue.
1 2
- `<color name="blue">#00F</color>`
- `<color name="blue">#0000FF</color>`
- `<color "blue">#00F</color>`
- `<color name="blue">@blue</color>`

### 2. In `activity_main.xml`, set a `TextView`'s text from a string resource named `welcome`. One line only.

```xml
android:text = "@string/welcome"
```

### 3. Choose the controller for the TipCalculator app.
2
- `TipCalculator.kt`
- `MainActivity.kt`
- `activity_main.xml`
- `strings.xml`

### 4. Complete the ID assignment for a view whose ID must be `amount_bill`. One line only.

```xml
android:id = "@+id/amount_bill"
```

### 5. Check all correct statements about Kotlin null safety.
123
- `String?` may hold `null`.
- A `String` value may be passed directly wherever `String?` is expected.
- `!!` safely converts `null` to an empty string.
- `!!` can throw `NullPointerException`.

### 6. Declare the header of `calculateTip`, which accepts `bill: Double` and returns `Double`. One line only.

```kotlin
fun calculateTip(bill: Double): Double
```
### 7. Which expression gives a `TextView` the full width of its parent?
2
- `android:layout_width="wrap_content"`
- `android:layout_width="match_parent"`
- `android:layout_width="parent"`
- `android:layout_width="screen"`

### 8. Complete the Logcat warning call that uses tag `MainActivity` and message `ready`. One line only.

```kotlin
Log.w("MainActivity", "ready")
```

### 9. Check all valid ways to update a calculator when the user edits either input field.
123
- Implement `TextWatcher` and override `afterTextChanged`.
- Register the same `TextWatcher` on both `EditText` fields.
- Use `addTextChangedListener` on the input fields.
- Put `android:onClick` on the root layout and expect it to run for every keystroke.

### 10. Which Kotlin class header means `SavingsAccount` inherits from `Account`?
3
- `class Account : SavingsAccount()`
- `class SavingsAccount extends Account`
- `class SavingsAccount : Account()`
- `class SavingsAccount, Account`

## Answer key

1. First and second options.
2. `android:text="@string/welcome"`
3. `MainActivity.kt`
4. `android:id="@+id/amount_bill"`
5. First, second, and fourth options.
6. `fun calculateTip(bill: Double): Double`
7. `android:layout_width="match_parent"`
8. `Log.w("MainActivity", "ready")`
9. First, second, and third options.
10. `class SavingsAccount : Account()`

## Before the quiz

- Reproduce the five one-line syntax patterns in this guide from memory.
- Explain MVC using the TipCalculator files without looking at notes.
- Trace the input-to-output path: `EditText` -> Kotlin conversion -> model -> `TextView`.


### 1. XML elements

Check all valid XML elements.

- `<TextView android:text="Hi" />`
- `<TextView android:text="Hi"></TextView>`
- `<TextView android:text="Hi">`
- `<View />`

### 2. Activity setup

Inside `onCreate`, set the activity’s screen to the layout file `activity_main.xml`. One line only.

```
_setContentView(R.layout.activity_main)___________________________
```

### 3. Relative layout

An `EditText` must appear to the right of a `TextView` whose ID is `label_bill`. Complete the attribute value.

```
android:layout_toRightOf="@id/label_bill________________"
```

### 4. Margins and padding

Check all correct statements.

- Margin is space outside a view.
- Padding is space inside a view.
- `android:paddingLeft` adds space outside the view.
- `android:layout_marginTop` adds space above the view.

### 5. Applying a style

A style named `InputStyle` already exists in `themes.xml`. Apply it to an `EditText`. One line only.

```
__android:style = "@style/InputStyle" __________________________
```

### 6. Applying a theme

Complete the manifest attribute that applies the style `Theme.TipCalculator` to the whole app.

```
android:theme = "style/Theme.TipCalculator"____________________________
```

### 7. `var`, `val`, and reassignment

Check all statements that compile.

```
var score = 80
val days = 7
```

- `score = 90`
- `days = 8`
- `println(score)`
- `println(days)`

### 8. Compile-time constants

Check all valid `const val` declarations.

- Top level: `const val MAX_USERS = 50`
- Inside a function: `const val MAX_USERS = 50`
- Inside an `object`: `const val MAX_USERS = 50`
- `const val message = StringBuilder("Hi")`

### 9. Loops

What values does this loop print?

```
for (i in 2..4) {
    println(i)
}
```

- `2, 3`
- `2, 3, 4`
- `3, 4`
- `2, 3, 4, 5`

### 10. Arrays

Which declaration creates a two-dimensional array of `Int` values?

- `IntArray(4, 6)`
- `Array<Int>(4) { 6 }`
- `Array(4) { IntArray(6) }`
- `Int[4][6]`

### 11. Property initialization

Check all class declarations that compile.

```
class A {
    private var count: Int = 0
}
```

```
class B {
    private var count: Int
}
```

```
class C {
    private lateinit var name: String
}
```

```
class D {
    private lateinit var count: Int
}
```

### 12. Primary-constructor properties

Given:

```
class Student(name: String, val uid: Int)
```

Check all correct statements.

- `uid` is a property of each `Student`.
- `name` is a property of each `Student`.
- Code outside the class can access `student.uid`.
- Code outside the class can access `student.name`.

### 13. Abstract classes

Check all correct statements.

```
abstract class Shape {
    abstract fun area(): Double
}
```

- `val shape = Shape()` compiles.
- A concrete subclass must implement `area()`.
- `area()` may have no body in `Shape`.
- `Shape` can be inherited by another class.

### 14. `TextWatcher`

A class implements `TextWatcher`. Which methods must it override?

- `beforeTextChanged`
- `onTextChanged`
- `afterTextChanged`
- `onClick`

### 15. View binding

Given `activity_main.xml`, choose the generated binding class.

- `ActivityMain`
- `ActivityMainViewBinding`
- `ActivityMainBinding`
- `activity_mainBinding`

### 16. View-binding setup

Put these statements in the correct order inside `onCreate`.

```
A. setContentView(binding.root)
B. binding = ActivityMainBinding.inflate(layoutInflater)
C. super.onCreate(savedInstanceState)
```

### 17. User input conversion

Check all correct statements.

```
val bill = billEditText.text.toString().toDouble()
```

- `billEditText.text` is an `Editable`.
- `toString()` converts the `Editable` to a `String`.
- `toDouble()` can fail for blank or invalid text.
- `inputType="numberDecimal"` guarantees `toDouble()` can never fail.

### 18. Event handling choices

Which is best for recalculating the tip immediately whenever the user edits either input field?

- A `TextWatcher` registered on both `EditText` views.
- `android:onClick` on the root layout.
- A `Button` with no listener.
- Calling `calculate()` only in `onCreate`.

## Answer key

1. First, second, and fourth
2. `setContentView(R.layout.activity_main)`
3. `@id/label_bill`
4. First, second, and fourth
5. `style="@style/InputStyle"`
6. `android:theme="@style/Theme.TipCalculator"`
7. First, third, and fourth
8. First and third
9. `2, 3, 4`
10. `Array(4) { IntArray(6) }`
11. A and C
12. First and third
13. Second, third, and fourth
14. First, second, and third
15. `ActivityMainBinding`
16. C, B, A
17. First, second, and third
18. A `TextWatcher` registered on both `EditText` views