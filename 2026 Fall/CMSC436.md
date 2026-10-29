# Android App Development: Foundations & Midterm Notes (CMSC436)

## 1. Android Project Structure & Foundations (`1-HelloAndroid`)

### Android project structure
An Android Studio project separates Kotlin source code from resources, such as layouts, strings, colors, themes, and images.
- `MainActivity.kt` is the default activity class (located under `app/src` or shown under `kotlin+java` in Android Studio). An `Activity` usually supplies one user-facing screen and acts as the controller.
- `res/layout/activity_main.xml` defines the screen's view hierarchy.
- `res/values/strings.xml`, `colors.xml`, and `themes.xml` hold reusable text, colors, and styles.
- `res/mipmap/` stores launcher icons (for example, `@mipmap/hi` configured via `android:icon` and `android:roundIcon` in the manifest; default `mdpi` launcher icon size is `48 × 48 dp`).
- `AndroidManifest.xml` (in `manifests/`) declares app components (activities) and requested capabilities/permissions (such as file system or Internet access). It also configures the app label (`@string/app_name`), icon (`@mipmap/...`), round icon, theme (`android:theme`), and activity orientation (`android:screenOrientation`).
- Android generates integer resource IDs (stored in generated files such as `R.txt` and `R.jar` under `app/build/intermediates/...`). Refer to a resource in Kotlin with `R.type.name`, such as `R.layout.activity_main`, `R.string.app_name`, or `R.id.amount_bill`. **Do not edit generated resource files.**
- For the XML-based projects used in this course, create an **Empty Views Activity**. `MainActivity.kt` extends `AppCompatActivity` (in `androidx.appcompat.app`, a subclass of `Activity`); `activity_main.xml` supplies its initial view.

### Activity setup and Model-View-Controller (MVC)
`onCreate` runs automatically when Android creates an activity. After calling the superclass implementation, set the activity's initial view using `setContentView(layoutResID: Int)` or `setContentView(view: View)`:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
}
```

*(Note: Android Studio templates may also include `enableEdgeToEdge()` and `ViewCompat.setOnApplyWindowInsetsListener(...)` for SDK 35+; these can be commented out or deleted.)*

In **Model-View-Controller (MVC)** terms:
- **Model**: App data and business logic, completely independent of the GUI (could be written as a plain Kotlin class and reused in another app). Examples include `TipCalculator`, `TicTacToe`, `Mortgage`, and `Hangman`.
- **View**: What the user sees and interacts with—either an XML layout (`activity_main.xml`) and its widgets, or a custom `ViewGroup` subclass built in Kotlin (such as `ButtonGridAndTextView`).
- **Controller**: The middleman between the View and the Model (`MainActivity`, `DataActivity`). It captures user events from the View, updates/queries the Model, and updates the View with results.

### XML layouts and resources
XML (eXtensible Markup Language) resembles HTML but uses Android-defined tags and attributes. A non-empty element has opening and closing tags (`<Tag attr="val">content</Tag>`); an empty element can self-close with `/>` (`<Tag attr="val" />`). Layout elements form a nested view tree.

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/message" />
```

- `match_parent`: Make the view as large as its parent container in that dimension.
- `wrap_content`: Make the view only large enough to wrap its content.
- `android:id="@+id/name"`: Create/assign a resource ID for a view so other XML elements can position relative to it and Kotlin code can retrieve it via `findViewById(R.id.name)`. When referencing an already-defined ID in XML, `@id/name` (or `@+id/name`) is used.
- **String resources (`res/values/strings.xml`)**: Declared inside `<resources>` as `<string name="message">HI THERE</string>` and referenced in XML as `@string/message` (note: singular `string`, no `s`).
- **Color resources (`res/values/colors.xml`)**: Declared inside `<resources>` as `<color name="lightGreen">#40F0</color>` and referenced in XML as `@color/lightGreen`.

### Colors, styles, and themes
Android color literals use RGB hexadecimal values (`00` = `0` to `FF` = `255` per channel, supporting 16.7 million RGB colors):
- `#RRGGBB` for an opaque color (for example, `#00FF00` is opaque green; `#FF0000` is opaque red).
- `#AARRGGBB` (or `#TTRRGGBB`) when an alpha/transparency channel is included (`00` is fully transparent, `FF` is fully opaque).
- `#RGB` and `#ARGB` are shorthand forms (for example, `#00F` and `#F00F` are opaque blue; `#FF00` is opaque red).

A **style** gathers visual and layout attributes for a kind of view in `res/values/themes.xml` (or `styles.xml`). Styles may inherit from a parent style via `parent="..."` (creating a style hierarchy similar to CSS) and are applied to a widget with `style="@style/Name"`:

```xml
<style name="TextStyle" parent="@style/TextAppearance.AppCompat">
    <item name="android:layout_width">wrap_content</item>
    <item name="android:layout_height">wrap_content</item>
    <item name="android:textSize">32sp</item>
    <item name="android:padding">10dp</item>
</style>

<style name="ButtonStyle" parent="TextStyle">
    <item name="android:background">@color/darkGreen</item>
</style>
```

A **theme** applies styling to an entire activity (`<activity android:theme="@style/..." />`) or the whole app (`<application android:theme="@style/Theme.AppName" />`) in `AndroidManifest.xml`. Android supplies a second `themes.xml` resource for night/dark mode (managed programmatically via `Configuration` and `AppCompatDelegate`). Switching the parent theme from `Theme.Material3.DayNight.NoActionBar` to `Theme.Material3.DayNight` displays the top `ActionBar` showing `@string/app_name`.

### Common widgets and layout details
All GUI components inherit directly or indirectly from `View` (`android.view.View`), while most widgets and layouts reside in `android.widget`:
- **Inheritance hierarchy**:
  - `View` → `ImageView`, `KeyboardView`, `TextView`, `ViewGroup`
  - `TextView` → `Button`, `EditText` (`EditText` → `AutoCompleteTextView`)
  - `Button` → `CompoundButton` → `CheckBox`, `RadioButton`, `Switch`, `ToggleButton`
  - `ViewGroup` → `GridLayout`, `LinearLayout` (→ `TableLayout`, `TableRow`), `RelativeLayout`, `FrameLayout`, `ConstraintLayout`
- **Common widgets**:
  - `TextView`: Displays static or dynamic text (label).
  - `EditText`: Accepts user keyboard input. Use `android:inputType="numberDecimal"` to constrain the soft keyboard to decimal numbers, and `android:hint="@string/..."` to display gray prompt text when empty.
  - `Button`: Triggers an action when clicked.
  - `View`: Can act as a simple colored divider line when given a small height and background color:
    ```xml
    <View
        android:layout_width="match_parent"
        android:layout_height="5dp"
        android:background="#FF00" />
    ```
- **Positioning and spacing**:
  - `RelativeLayout` positions children relative to sibling views (by ID) or the parent container using `RelativeLayout.LayoutParams` XML attributes:
    - Sibling-relative: `android:layout_toRightOf`, `android:layout_toLeftOf`, `android:layout_below`, `android:layout_above`, `android:layout_alignLeft`, `android:layout_alignRight`, `android:layout_alignTop`, `android:layout_alignBottom`.
    - Parent-relative: `android:layout_alignParentLeft`, `android:layout_alignParentRight`, `android:layout_alignParentTop`, `android:layout_alignParentBottom`, `android:layout_centerHorizontal="true"`.
  - `ConstraintLayout` is the default layout generated by Android Studio and expresses constraints between views.
  - **Margin** is extra space *outside* a view (`android:layout_marginTop`, `android:layout_marginBottom`, `android:layout_marginLeft`, `android:layout_marginRight`; method `setMargins(int, int, int, int)`).
  - **Padding** is space *inside* a view between its border and its content (`android:padding`, `android:paddingTop`, `android:paddingBottom`, `android:paddingLeft`, `android:paddingRight`; method `setPadding(int, int, int, int)`).
  - **Text attributes**: `android:textColor`, `android:textSize` (use `sp` = scale-independent pixels), `android:textStyle` (`bold`, `italic`), `android:background`. Layout dimensions use `dp` (density-independent pixels).

### Running and debugging
- Run an app on an **Android Virtual Device (AVD)** created via Android Studio's **Device Manager**, or on a physical Android device with **Developer Options** and **USB Debugging** enabled (`Settings -> System -> Developer Options -> USB debugging`).
- To uninstall an app on the emulator, drag up from the bottom search bar to open the app drawer, then drag the app icon to **Uninstall**.
- **Logcat**: Use `android.util.Log` static methods `Log.d` (debug), `Log.e` (error), `Log.i` (info), `Log.v` (verbose), and `Log.w` (warning), all with signature `fun w(tag: String?, message: String): Int`. Filter output in the Logcat pane using `package:mine TagName` (for example, `Log.w("MainActivity", "Inside onCreate")`).
- **Debugger**: Click left of a line number to set a breakpoint (red/orange circle), click the Debug icon (bug) to pause execution at breakpoints, inspect the `Threads & Variables` pane, step over lines, and click Resume.
- **Gradle & APK**: Gradle compiles the project and packages it into an **Android Application Package (`.apk`)**, located at `projectName/app/build/intermediates/apk/debug/app-debug.apk`.

---

## 2. Kotlin Essentials (`2A-KotlinBasics`)

### Variables, constants, and basic types
- Kotlin does not require semicolons (`;`). It supports single-line (`//`), multi-line (`/* ... */`), and doc (`/** ... */`) comments.
- **Numeric types**: `Short`, `Byte`, `Int`, `Float`, `Double`, `Long` (all inherit from `Number`). Other basic types: `Char`, `Boolean`, `String`.
- `var` declares a mutable variable; `val` declares a read-only (run-time constant) variable that can be assigned once (may hold an object reference or runtime-computed value). Type annotations (`var name: String = "Jane"`) are optional when Kotlin can infer the type (`var grade = 94`).
- `const val` declares a **compile-time constant**. It cannot be local to a function and can only be initialized with a `String` or primitive/basic data type (`const val DAYS: Int = 365`).
- Kotlin has **no `?:` ternary operator**; use an `if` expression instead (`val len = if (text != null) text.length else 0`).
- Use `print(...)` or `println(...)` for console output.
- **Explicit type conversion**: Use `.toInt()`, `.toDouble()`, `.toFloat()`, `.toChar()`, `.toString()` (for example, `"341".toInt()`).
- **Variable scope**: Starts at the point of declaration and ends at the closing brace `}` of the innermost enclosing block.

```kotlin
var total = 0.0
val daysInWeek = 7
const val DAYS_IN_YEAR = 365
val name: String = "Jane"
```

### Arrays, control flow, functions, and exceptions
Kotlin supplies specialized primitive arrays (`IntArray`, `DoubleArray`, `FloatArray`, `CharArray`) as well as generic `Array<T>` objects:

```kotlin
val grades = IntArray(30)                 // 30 zeroes
val scores = intArrayOf(89, 87, 90)       // hardcoded contents
val words = Array<String>(10, { i -> "All Hi" }) // generic Array constructor

// 2D Array: 4 rows, 6 columns, where cell (i, j) = 10 * i + j
val table = Array<Array<Int>>(4, { i ->
    Array<Int>(6, { j -> 10 * i + j })
})
```

- `Array<Array<Int>>` represents a 2D array (an array of rows, where each row is an `Array<Int>`). The constructor takes `(size, { index -> initialValue })`.
- **Selection**: `if`, `else if`, `else`, and `when` (Kotlin's counterpart to Java's `switch`).
- **Loops**: `while`, `do while`, and `for`.
  - Inclusive range: `for (i in 0..10)` includes **both `0` and `10`** (so iterating over indices of a string or array uses `for (i in 0..word.length - 1)` or `for (i in 0..arr.size - 1)`).
  - Collection/item iteration: `for (item in collection)` iterates over an array, list, or `String`.
  - `break` exits a loop; `continue` skips to the next iteration.
- **Functions**: Declare parameters as `name: Type` and place the return type after the parameter list. `Unit` is Kotlin's counterpart to Java's `void` and is optional:
  ```kotlin
  fun add(a: Int, b: Int): Int {
      return a + b
  }

  fun printTotal(total: Double): Unit {
      println(total)
  }
  ```
- **Exception handling**: Uses `try`, `catch (e: ExceptionClass)`, and `finally`. Numeric parsing such as `"abc".toInt()` or `"".toDouble()` throws a `NumberFormatException`.

### Null safety (`?` and `!!`)
- By default, Kotlin types are **non-nullable** (`var s0: String = null` does **not** compile).
- Append `?` to a type (`String?`, `View?`, `MotionEvent?`) to allow `null`. You must check for `null` (`if (s != null)`) before accessing properties/methods on a nullable reference, or pass it only to parameters that accept `T?`.
- The non-null assertion operator `!!` (`s2!!`) forces a nullable variable to be treated as non-null ("trust me, it's not null"). It compiles, but if the value is `null` at runtime, it throws a `NullPointerException` (NPE).

### Classes, constructors, `init`, and `lateinit`
- Kotlin instantiates objects **without** the `new` keyword (`val p = Person("Jane", 20)`).
- Every non-null property of a basic type or `String` must be initialized at declaration, in a constructor, or in an `init` block.
- For non-primitive object references (such as `TextView`, `NumberFormat`, `TicTacToe`, or `Array<Boolean>`) that will be assigned later (for example, in `onCreate`), declare them with `lateinit var`:
  ```kotlin
  private lateinit var nf: NumberFormat
  ```
  *(Note: `lateinit` cannot be used with primitive types like `Int`, `Double`, or `Boolean`.)*
- **Primary constructor**: Declared in the class header (`class Person(var name: String, var age: Int)`). Adding `var` or `val` automatically creates instance properties. Because a primary constructor cannot contain code, use one or more `init { ... }` blocks to run initialization logic during construction.
- **Secondary constructors**: Declared inside the class body using the `constructor` keyword (`constructor(newAmount: Double, newTip: Double) { ... }`). Constructors can be overloaded.
- **`this` and method chaining**: Use `this.property` when a parameter shadows an instance variable. Return `this` from a mutating method (`fun setAmount(amount: Double): TipCalculator { ...; return this }`) so callers can chain calls left-to-right (`tipCalc.setTip(0.22).setAmount(134.56)`).

### Nested classes, inner classes, data classes, and operator overloading
- **Nested class** (`class B` inside `class A`, without `inner`): Equivalent to a `static` nested class in Java. It has **no reference** to an outer instance and cannot access outer instance variables. Instantiated as `A.B()`.
- **Inner class** (`inner class D` inside `class C`): Holds an implicit reference to the enclosing `C` instance (`this@C`) and can directly access outer instance variables and methods. Instantiated via an outer instance as `C().D()`.
- **Data class**: Declared with `data class Student(var age: Int)`. Acts as a value holder (like a Java `record`), useful when reading data from files or databases.
- **Operator overloading**: Kotlin allows overloading standard operators (`+`, `++`, `<`, `==`) on custom classes (like C++, unlike Java).

### Inheritance, abstract classes, and interfaces
- Every Kotlin class implicitly inherits from `Any` (Kotlin's root class, equivalent to `Object` in Java), which provides `equals`, `hashCode`, and `toString`.
- Classes and methods are **`final` by default** (cannot be subclassed or overridden). Mark a class or method `open` to allow inheritance or overriding.
- Use a colon `:` for both class inheritance (calling the superclass constructor with `()`) and interface implementation (no parentheses):
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
- An `abstract` class cannot be instantiated; `abstract` classes and `abstract` methods are `open` by default, and concrete subclasses must implement them using the mandatory `override` keyword.
- When implementing an interface, a concrete class must implement **all** of the interface's abstract methods.

---

## 3. Tip Calculator & View Binding (`2B-TipCalculator` & `Bindings`)

### Connecting the View to Kotlin (`findViewById`)
An activity locates a widget from its inflated XML layout using `findViewById`:

```kotlin
val billEditText: EditText = findViewById(R.id.amount_bill)
val tipTextView: TextView = findViewById(R.id.amount_tip)
```

For views accessed across multiple methods, declare them as `private lateinit var` properties of the Activity and initialize them in `onCreate` after `setContentView(...)`.
- Reading an `EditText`: `editText.text` returns an `Editable`; convert it with `.toString()` before parsing to a number:
  ```kotlin
  val bill: Double = billEditText.text.toString().toDouble()
  ```
- Even when `android:inputType="numberDecimal"` is set in XML, `toDouble()` or `toFloat()` will still throw a `NumberFormatException` if the user leaves the `EditText` empty! Wrap conversions in a `try / catch (e: NumberFormatException)` block.

### Events and listeners (`OnClickListener` & `TextWatcher`)
Generally, event handling in code follows **three steps**:
1. Write an event handler class implementing the listener interface.
2. Instantiate an object of that listener class.
3. Register the listener object on one or more GUI components.

**Four ways to handle events in the Tip Calculator:**
1. **XML `android:onClick="calculate"`**: Add `android:onClick="calculate"` to the `<Button>` in XML and define `fun calculate(v: View)` in `MainActivity` (where `v` is the clicked `View`).
2. **Explicit `View.OnClickListener` inner class**:
   ```kotlin
   inner class ButtonHandler : View.OnClickListener {
       override fun onClick(v: View?) {
           calculate()
       }
   }
   val bh = ButtonHandler()
   button.setOnClickListener(bh)
   ```
3. **Lambda expression (for Functional / SAM interfaces)**:
   - Because `View.OnClickListener` is a **functional interface** (it has **only 1 abstract method**, `onClick(v: View?)`), you can combine steps 1, 2, and 3 into a single lambda expression:
     ```kotlin
     button.setOnClickListener { v -> calculate() }
     // or simply:
     button.setOnClickListener { calculate() }
     ```
   - To remove/disable a click listener on a button: `button.setOnClickListener(null)`.
4. **Live key/text updates with `TextWatcher` (`android.text.TextWatcher`)**:
   - Because `EditText` inherits from `TextView`, you can register a `TextWatcher` on each `EditText` using `addTextChangedListener(watcher)` so the app recalculates automatically after every keystroke without needing a Button.
   - `TextWatcher` has **3 abstract methods**: `beforeTextChanged`, `onTextChanged`, and `afterTextChanged`. Because it has 3 methods (not 1), **all 3 must be implemented** (a lambda cannot be used directly), with the first two left as empty "do-nothing" methods:
     ```kotlin
     inner class TextChangeHandler : TextWatcher {
         override fun beforeTextChanged(s: CharSequence?, start: Int, count: Int, after: Int) {}
         override fun onTextChanged(s: CharSequence?, start: Int, before: Int, count: Int) {}
         override fun afterTextChanged(e: Editable?) {
             calculate() // or this@MainActivity.calculate()
         }
     }

     val tch = TextChangeHandler()
     billEditText.addTextChangedListener(tch)
     tipEditText.addTextChangedListener(tch)
     ```

### View Binding (`Bindings.pptx`)
View binding generates a type-safe binding class for each XML layout file, replacing `findViewById`.
1. Enable view binding in `build.gradle` (Module) and sync the project with Gradle files:
   ```groovy
   android {
       buildFeatures {
           viewBinding = true
       }
   }
   ```
2. For a layout named `abc_xyz.xml`, Android automatically generates `AbcXyzBinding` (so `activity_main.xml` generates `ActivityMainBinding`, imported from `com.example.projectName.databinding.ActivityMainBinding`).
3. Inflate the binding in `onCreate` using the activity's `layoutInflater`, pass `binding.root` (the top-level layout view) to `setContentView`, and access any widget with an `android:id` directly via `binding.idOfElement`:
   ```kotlin
   private lateinit var binding: ActivityMainBinding

   override fun onCreate(savedInstanceState: Bundle?) {
       super.onCreate(savedInstanceState)
       binding = ActivityMainBinding.inflate(layoutInflater)
       setContentView(binding.root)

       binding.amountBill.setText("0.0")
   }
   ```

---

## 4. Tic-Tac-Toe: Building a Dynamic GUI in Code (`3-TicTacToe`)

### When to build a GUI by code
Build views programmatically in Kotlin when the number or arrangement of widgets is dynamic (for example, based on the length of a word in Hangman or data from a file/server) or when a 2D array of widgets (`Array<Array<Button>>`) is much cleaner to manipulate in loops than 9 separate XML elements.

### Game model and board view (`GridLayout` & `DisplayMetrics`)
- **Model (`TicTacToe`)**: Holds a 3 × 3 `Array<Array<Int>>` (`0` = empty, `1` = Player 1 / `X`, `2` = Player 2 / `O`), tracks whose turn it is, and provides `play(row, col)`, `isGameOver()`, `result()`, and `resetGame()`. Contains zero Android GUI code.
- **Screen dimensions (`DisplayMetrics`)**: To size each square button to 1/3 of the device screen's width in portrait orientation, retrieve the device's `DisplayMetrics`:
  ```kotlin
  // Using the Activity's resources property:
  val dm: DisplayMetrics = resources.displayMetrics
  val width: Int = dm.widthPixels

  // Or using the global system Resources:
  val width: Int = Resources.getSystem().displayMetrics.widthPixels
  val w: Int = width / TicTacToe.SIDE
  ```
- **Constructing the `GridLayout` and 2D `Button` array**:
  Because `MainActivity` inherits from `AppCompatActivity` → `Activity` → `Context`, `this` is a `Context`:
  ```kotlin
  val gridLayout = GridLayout(this)
  gridLayout.rowCount = TicTacToe.SIDE
  gridLayout.columnCount = TicTacToe.SIDE

  buttons = Array<Array<Button>>(TicTacToe.SIDE, { i ->
      Array<Button>(TicTacToe.SIDE, { j -> Button(this) })
  })

  for (i in 0..buttons.size - 1) {
      for (j in 0..buttons[i].size - 1) {
          gridLayout.addView(buttons[i][j], w, w)
      }
  }
  setContentView(gridLayout)
  ```
  Here `addView(child: View, width: Int, height: Int)` (inherited from `ViewGroup`) adds each `Button` (`View`) to the `GridLayout` with width `w` and height `w`.

### Click handling and game state
- Using an inner class `ButtonHandler : View.OnClickListener`, `onClick(view: View?)` loops through `buttons` to find `(row, col)` where `view == buttons[row][col]` and calls `update(row, col)`.
- Alternatively, registering a lambda inside the nested loop captures `i` and `j` directly:
  ```kotlin
  buttons[i][j].setOnClickListener { update(i, j) }
  ```
- Inside `update(row: Int, col: Int)`:
  ```kotlin
  fun update(row: Int, col: Int) {
      val play: Int = ttt.play(row, col)
      if (play == 1)
          buttons[row][col].text = "X"
      else if (play == 2)
          buttons[row][col].text = "O"

      if (ttt.isGameOver()) {
          status.setBackgroundColor(Color.RED)
          enableButtons(false)
          status.text = ttt.result()
          showNewGameDialog()
      }
  }
  ```
- To enable or disable a `Button` (inherited from `TextView`), set `buttons[row][col].isEnabled = enabled` (or call `setEnabled(enabled)`).

### Adding a spanning status row (`GridLayout.Spec` & `GridLayout.LayoutParams`)
To add a `TextView` below the 3 × 3 board that spans all 3 columns of row 3 (the 4th row, 0-indexed):
1. Set `gridLayout.rowCount = TicTacToe.SIDE + 1`.
2. Create vertical (row) and horizontal (column) spans using the static method `GridLayout.spec(start: Int, size: Int): GridLayout.Spec`:
   ```kotlin
   val rowSpec: GridLayout.Spec = GridLayout.spec(TicTacToe.SIDE, 1)    // starts at row 3, spans 1 row
   val columnSpec: GridLayout.Spec = GridLayout.spec(0, TicTacToe.SIDE) // starts at col 0, spans 3 cols
   val lp = GridLayout.LayoutParams(rowSpec, columnSpec)                // (rowSpec first, columnSpec second!)

   status = TextView(this)
   status.layoutParams = lp
   status.width = width
   status.height = w
   status.gravity = Gravity.CENTER
   status.setBackgroundColor(Color.GREEN)
   status.text = ttt.result()
   gridLayout.addView(status)
   ```

### Play-again dialog (`AlertDialog.Builder` & `DialogInterface.OnClickListener`)
When the game ends, use `AlertDialog.Builder(context: Context!)` to pop up a dialog box with positive (`YES`), negative (`NO`), or neutral buttons:

```kotlin
fun showNewGameDialog() {
    val builder = AlertDialog.Builder(this)
    builder.setTitle("This is fun")
    builder.setMessage("Play again?")
    val playAgain = PlayDialog()
    builder.setPositiveButton("YES", playAgain)
    builder.setNegativeButton("NO", playAgain)
    val alert: AlertDialog = builder.create()
    alert.show()
}
```

*(Optional: To move the dialog to the bottom of the screen, use `alert.window?.setGravity(Gravity.BOTTOM)`.)*

The listener implements `DialogInterface.OnClickListener`, whose single method `onClick(dialog: DialogInterface?, id: Int)` receives an `id` identifying which button was clicked:
- `id == -1` (`DialogInterface.BUTTON_POSITIVE` / `"YES"`): Reset the game model and view.
- `id == -2` (`DialogInterface.BUTTON_NEGATIVE` / `"NO"`): Close the activity via `this@MainActivity.finish()`.

```kotlin
inner class PlayDialog : DialogInterface.OnClickListener {
    override fun onClick(dialog: DialogInterface?, id: Int) {
        if (id == -1) { // YES button
            ttt.resetGame()
            enableButtons(true)
            resetButtons()
            status.setBackgroundColor(Color.GREEN)
            status.text = ttt.result()
        } else if (id == -2) { // NO button
            this@MainActivity.finish()
        }
    }
}
```

### Separating the View from the Controller (Tic-Tac-Toe Version 5)
To make both the Model and the View reusable, Version 5 extracts all GUI construction and display methods into `ButtonGridAndTextView`, a subclass of `GridLayout`:

```kotlin
class ButtonGridAndTextView(
    context: Context,
    width: Int,
    newSide: Int,
    listener: View.OnClickListener
) : GridLayout(context) { ... }
```

| Part | Owns | Example Responsibility |
| :--- | :--- | :--- |
| `TicTacToe` (Model) | Board array and current turn | Decide whether `play(row, col)` is valid; check `isGameOver()`; report `result()` |
| `ButtonGridAndTextView` (View) | `buttons` array, `status` `TextView`, `GridLayout` | Display `"X"`/`"O"` or status (`setButtonText`, `setStatusText`, `setStatusBackgroundColor`, `resetButtons`, `enableButtons`); identify a clicked button via `isButton(b, row, col)` |
| `MainActivity` (Controller) | References to `ttt` (Model) and `tttView` (View) | Handle clicks in `ButtonHandler` and `PlayDialog`, call `ttt.play(row, col)`, and update `tttView` |

- Note that `ButtonGridAndTextView` uses its own `side` parameter (`newSide`) rather than `TicTacToe.SIDE`, keeping the View completely independent of the Model.
- To avoid looping through the 2D array to find which button was clicked, you can subclass `AppCompatButton` (for example, `class GridButton : AppCompatButton`) to store its `row` and `col` indices as instance variables.

---

## 5. Multi-Activity Apps, Lifecycle, Transitions & Persistent Data (`4-Mortgage`)

### TableLayout, TableRow, RadioGroup, and RadioButton
The Mortgage app uses two screens (`MainActivity` with `activity_main.xml` and `DataActivity` with `activity_data.xml`):
- **Screen 1 (`TableLayout`)**:
  - `TableLayout` inherits from `LinearLayout` → `ViewGroup` and arranges children into rows and columns.
  - Each row is defined by a `<TableRow>` element containing child widgets (for example, two `<TextView>` elements per row for label and value):
    ```xml
    <TableRow
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">
        <TextView
            android:text="@string/label_amount"
            android:padding="10dp" />
        <TextView
            android:id="@+id/amount"
            android:text="@string/amount" />
    </TableRow>
    ```
  - A `<View android:layout_height="5dp" android:background="#FF0000" />` between rows draws a red horizontal separator line.
  - The final `<TableRow android:gravity="center" android:paddingTop="50dp">` holds a `<Button android:onClick="modifyData" />`.
- **Screen 2 (`RelativeLayout` with `RadioGroup` & `RadioButton`)**:
  - Placing `<RadioButton>` elements inside a `<RadioGroup>` makes them **mutually exclusive** (selecting one automatically unchecks the others):
    ```xml
    <RadioGroup
        android:layout_toRightOf="@+id/label_years"
        android:layout_alignLeft="@+id/data_rate"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <RadioButton
            android:id="@+id/ten"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/ten" />
        <!-- 15 and 30 year RadioButtons -->
    </RadioGroup>
    ```
  - In Kotlin, read or set a `RadioButton`'s state via its `isChecked` property (`if (rb10.isChecked)` or `rb10.isChecked = true`).
  - Before wiring navigation, you can test Screen 2 in the emulator by temporarily changing `setContentView(R.layout.activity_data)` in `MainActivity.onCreate`.

### Navigating between Activities and the Activity Stack
- Android organizes running activities in a **stack** (Last-In, First-Out). The user always sees and interacts with the Activity at the **top of the stack**.
- **Starting a second Activity (pushes onto the stack)**:
  Create an `Intent(context: Context!, cls: Class<*>!)` specifying the current activity (`this`) and the target activity class (`DataActivity::class.java`), then call `startActivity(intent)`:
  ```kotlin
  fun modifyData(v: View) {
      val myIntent = Intent(this, DataActivity::class.java)
      this.startActivity(myIntent)
  }
  ```
- **Returning to the prior Activity (pops off the stack)**:
  In `DataActivity`, call `finish()` to close `DataActivity` and pop it off the stack, revealing `MainActivity` underneath:
  ```kotlin
  fun goBack(v: View) {
      updateMortgageObject()
      this.finish()
  }
  ```
- **Declaring the second Activity in `AndroidManifest.xml`**:
  Every activity in an app **must** have an `<activity>` element inside `<application>` in `AndroidManifest.xml`:
  ```xml
  <application ...>
      <activity android:name=".MainActivity" ...>
          ...
      </activity>
      <activity android:name=".DataActivity">
      </activity>
  </application>
  ```

### Activity Lifecycle
An `Activity` transitions through seven lifecycle methods that Android calls automatically as the activity enters the foreground, goes to the background, or finishes:

| Lifecycle Method | When Android Automatically Calls It |
| :--- | :--- |
| `onCreate` | Called **once** when the activity is first created (pushed onto the stack). Not called when returning to the activity. |
| `onStart` | Called after `onCreate` (and after `onRestart`) when the activity becomes **visible** to the user. |
| `onResume` | Called after `onStart` when the activity enters the foreground and the user **starts interacting** with it. |
| `onPause` | Called first when Android starts or resumes another activity (current activity loses foreground focus). |
| `onStop` | Called after `onPause` when the activity becomes **completely invisible** (in the background or finishing). |
| `onRestart` | Called **only** when a stopped activity is about to restart (returning to the top of the stack); always followed by `onStart()` → `onResume()`. **Never** called immediately after `onCreate()`. |
| `onDestroy` | Called after `onStop` when the activity **ends** (for example, popped off the stack via `finish()`). Not called when merely backgrounded. |

**Key Lifecycle Sequences:**
- **Activity first launches**: `onCreate()` → `onStart()` → `onResume()`
- **Another activity starts on top (or Home pressed — activity goes to background)**: `onPause()` → `onStop()`
- **Activity is popped off the stack (`finish()` called)**: `onPause()` (first) → `onStop()` (second) → `onDestroy()` (third)
- **User returns to a stopped activity (comes back to top of stack)**: `onRestart()` → `onStart()` → `onResume()`

Because `onStart()` runs both on initial creation and whenever the user returns to `MainActivity` from `DataActivity`, override `onStart()` in `MainActivity` to refresh the GUI (`updateView()`):

```kotlin
override fun onStart() {
    super.onStart()
    updateView()
}
```

### Sharing data between Activities (`companion object`)
Ways to pass or share data between activities include:
1. **`companion object` (Kotlin's equivalent of `public static`)**: The simplest way to share a Model object across activities in the same app:
   ```kotlin
   class MainActivity : AppCompatActivity() {
       companion object {
           lateinit var mortgage: Mortgage
       }
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           mortgage = Mortgage(this)
           ...
       }
   }
   ```
   Inside `DataActivity` (or any other class), access the shared model using `MainActivity.mortgage`:
   ```kotlin
   var mortgage: Mortgage = MainActivity.mortgage
   ```
2. **`Intent.putExtra(...)`**: Pass primitive types, `String`s, arrays, or `Serializable` objects inside an `Intent`.
3. **Singleton class**: Restrict `Mortgage` to a single shared instance.
4. **Local file or SQLite database**: Overkill for simple object sharing, though used for persistence.

### Screen Animations and Transitions (`res/anim` & `res/transition`)
Android supports two XML-based approaches for animated transitions between activities:

**Approach 1: View Animations in `res/anim/` with `overridePendingTransition`**
- Place animation XML files in `res/anim/`.
- Supported XML tags (and classes): `<set>` (`AnimationSet`), `<alpha>` (`AlphaAnimation` — fade in/out via `fromAlpha`, `toAlpha`), `<rotate>` (`RotateAnimation`), `<scale>` (`ScaleAnimation` — `fromXScale`, `toXScale`), and `<translate>` (`TranslateAnimation` — `fromXDelta`, `toXDelta`, `duration` in ms).
- Call `overridePendingTransition(enterAnim: Int, exitAnim: Int)` immediately after `startActivity(...)` or `finish()` (pass `0` for no animation):
  ```kotlin
  // Inside MainActivity.modifyData:
  this.startActivity(myIntent)
  overridePendingTransition(R.anim.slide_from_left, 0)

  // Inside DataActivity.goBack:
  this.finish()
  overridePendingTransition(R.anim.fade_in_and_scale, 0)
  ```

**Approach 2: Scene Transitions in `res/transition/` with `ActivityOptions`**
- Place transition XML files in `res/transition/` wrapped in `<transitionSet xmlns:android="http://schemas.android.com/apk/res/android">`.
- Supported XML tags (and classes): `<transitionSet>` (`TransitionSet`), `<fade>` (`Fade` — `android:fadingMode="fade_in"`), `<explode>` (`Explode`), and `<slide>` (`Slide` — `android:slideEdge="left"`, `android:duration="3000"`).
- Add transition items to the app theme in `res/values/themes.xml`:
  ```xml
  <item name="android:windowEnterTransition">@transition/fade_in</item>
  <item name="android:windowExitTransition">@transition/slide_from_left</item>
  ```
- Launch with `ActivityOptions.makeSceneTransitionAnimation(this).toBundle()` and exit with `finishAfterTransition()`:
  ```kotlin
  // Inside MainActivity:
  this.startActivity(
      intent,
      ActivityOptions.makeSceneTransitionAnimation(this).toBundle()
  )

  // Inside DataActivity:
  updateMortgageObject()
  this.finishAfterTransition()
  ```

### Managing Persistent Data with `SharedPreferences`
To remember data across app restarts, store persistent data locally using the `SharedPreferences` interface, which organizes data as a **list/set of key/value pairs** (like a hash table).

1. **Obtain a `SharedPreferences` reference** via `Context.getSharedPreferences(name: String!, mode: Int): SharedPreferences!`:
   ```kotlin
   // Since Activity inherits from Context, pass `this` or a Context parameter:
   val pref: SharedPreferences = context.getSharedPreferences(
       context.packageName + "_preferences",
       Context.MODE_PRIVATE
   )
   ```
   *(Note: On exam questions, check the documentation sheet—it often specifies passing a literal string such as `"XYZ"` as the first argument alongside `Context.MODE_PRIVATE`.)*

2. **Read persistent data (`getDataType`)**:
   Use `pref.getInt(key: String, defaultValue: Int): Int`, `pref.getFloat(key: String, defaultValue: Float): Float`, or `pref.getString(...)`. If the key does not exist (such as the very first time the app runs), `defaultValue` is returned:
   ```kotlin
   constructor(context: Context) {
       val pref: SharedPreferences = context.getSharedPreferences(
           context.packageName + "_preferences", Context.MODE_PRIVATE
       )
       setAmount(pref.getFloat(PREFERENCE_AMOUNT, 100000.0f))
       setYears(pref.getInt(PREFERENCE_YEARS, 30))
       setRate(pref.getFloat(PREFERENCE_RATE, 0.035f))
   }
   ```

3. **Write persistent data (`SharedPreferences.Editor`)**:
   Call `pref.edit()` to obtain a `SharedPreferences.Editor`, call `putInt`, `putFloat`, `putString`, etc. (which return `SharedPreferences.Editor` and can be chained), and **always call `editor.commit()`** (or `apply()`) to persist the changes:
   ```kotlin
   fun setPreferences(context: Context) {
       val pref: SharedPreferences = context.getSharedPreferences(
           context.packageName + "_preferences", Context.MODE_PRIVATE
       )
       val editor: SharedPreferences.Editor = pref.edit()
       editor.putFloat(PREFERENCE_AMOUNT, amount)
       editor.putInt(PREFERENCE_YEARS, years)
       editor.putFloat(PREFERENCE_RATE, rate)
       editor.commit() // DO NOT FORGET!
   }
   ```
   *(In `AndroidManifest.xml`, the slides also include `<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />`.)*

---

## 6. Touch Events and Moving Views (`5-Touches` Slides 1–49)

*(Exam Scope Note: Slides 1–49 covering `View.OnTouchListener`, `MotionEvent`, and moving a `View` with a finger are **included** on the midterm. Slides 50–72 covering `GestureDetector`, swipes, and taps are **excluded**.)*

### Implementing and registering `View.OnTouchListener`
Handling touch events follows the standard 3-step event model:
1. Implement the `View.OnTouchListener` interface (a public static inner interface of `View`) and override its single callback method `onTouch`:
   ```kotlin
   abstract fun onTouch(v: View?, event: MotionEvent?): Boolean
   ```
   - `v`: The `View` where the touch event originated.
   - `event`: A `MotionEvent` object containing details about the touch (coordinates, action type, timestamps).
   - **Return value (`Boolean`)**:
     - Return `true` → the touch event is **consumed** here.
     - Return `false` → the touch event is **propagated** to views underneath the current view.
2. Either write an inner class (`inner class TouchHandler : View.OnTouchListener`) or have `MainActivity` implement `View.OnTouchListener` directly so `this` is the listener:
   ```kotlin
   class MainActivity : AppCompatActivity(), View.OnTouchListener {
       override fun onTouch(v: View?, event: MotionEvent?): Boolean {
           // process touch event
           return true
       }
   }
   ```
3. Register the listener on any `View` using `v.setOnTouchListener(listener)`:
   ```kotlin
   v.setOnTouchListener(this)
   ```
   To capture touches across the entire screen, retrieve the top-level content view using `android.R.id.content`:
   ```kotlin
   val view: View = findViewById(android.R.id.content)
   view.setOnTouchListener(this)
   ```

### The `MotionEvent` class: coordinates and actions
| `MotionEvent` Method / Property | Return Type | Description |
| :--- | :--- | :--- |
| `getRawX()` / `event.rawX` | `Float` | X coordinate of the touch within the **entire screen** |
| `getRawY()` / `event.rawY` | `Float` | Y coordinate of the touch within the **entire screen** |
| `getX()` / `event.x` | `Float` | X coordinate of the touch within the **View `v` that originated the event** |
| `getY()` / `event.y` | `Float` | Y coordinate of the touch within the **View `v` that originated the event** |
| `getAction()` / `event.action` | `Int` | Action code: compare against `MotionEvent.ACTION_DOWN`, `MotionEvent.ACTION_MOVE`, or `MotionEvent.ACTION_UP` |

- When the touch listener is registered on the full-screen content view (`android.R.id.content`), `rawX` equals `x` and `rawY` equals `y`. When registered on a smaller child widget (like a movable label), `rawX`/`rawY` give screen coordinates while `x`/`y` give coordinates relative to that child widget's top-left corner.
- **`MotionEvent` action constants**:
  - `MotionEvent.ACTION_DOWN`: The user first touched the screen.
  - `MotionEvent.ACTION_MOVE`: The user is moving/dragging their finger on the screen.
  - `MotionEvent.ACTION_UP`: The user lifted their finger off the screen.
- When a user touches the screen, drags their finger, and lifts it up, Android generates **one `ACTION_DOWN` event**, followed by **many discrete `ACTION_MOVE` events**, and finally **one `ACTION_UP` event**:
  ```kotlin
  override fun onTouch(v: View?, event: MotionEvent?): Boolean {
      if (event != null) {
          val action: Int = event.action
          if (action == MotionEvent.ACTION_DOWN)
              Log.w(MA, "DOWN: v = " + v + "; event = " + event)
          else if (action == MotionEvent.ACTION_MOVE)
              Log.w(MA, "MOVE: v = " + v + "; event = " + event)
          else if (action == MotionEvent.ACTION_UP)
              Log.w(MA, "UP: v = " + v + "; event = " + event)
      }
      return true
  }
  ```

### Moving a `TextView` with a finger (Touches Version 1)
To drag a `TextView` (`tv`) across the screen with a finger:
1. **Place the `TextView` inside a `RelativeLayout` using `RelativeLayout.LayoutParams`**:
   - `RelativeLayout.LayoutParams(w: Int, h: Int)` inherits from `ViewGroup.MarginLayoutParams` → `ViewGroup.LayoutParams`.
   - Specify the view's initial X and Y position using its `leftMargin` and `topMargin` fields (it also has `rightMargin` and `bottomMargin`), then add the view and register the touch listener on `tv`:
     ```kotlin
     private lateinit var tv: TextView
     private lateinit var params: RelativeLayout.LayoutParams
     private var startX: Int = 0
     private var startY: Int = 0
     private var startTouchX: Int = 0
     private var startTouchY: Int = 0

     override fun onCreate(savedInstanceState: Bundle?) {
         super.onCreate(savedInstanceState)
         val rl = RelativeLayout(this)
         params = RelativeLayout.LayoutParams(300, 200) // width = 300, height = 200
         params.leftMargin = 50                         // initial X coordinate
         params.topMargin = 150                         // initial Y coordinate

         tv = TextView(this)
         tv.setBackgroundColor(Color.BLUE)
         rl.addView(tv, params)
         setContentView(rl)

         tv.setOnTouchListener(this)
     }
     ```
2. **Update position in `onTouch` on `ACTION_DOWN` and `ACTION_MOVE`**:
   - On `ACTION_DOWN`: Save both the label's starting position (`startX`, `startY`) and the finger's starting screen position (`startTouchX`, `startTouchY`).
   - On `ACTION_MOVE`: Compute the finger's displacement `(event.rawX.toInt() - startTouchX)` and add it to `startX` (and similarly for Y), then re-assign `tv.layoutParams = params` (which calls `setLayoutParams`):
     ```kotlin
     override fun onTouch(v: View?, event: MotionEvent?): Boolean {
         if (event != null) {
             val action: Int = event.action
             if (action == MotionEvent.ACTION_DOWN) {
                 startX = params.leftMargin
                 startY = params.topMargin
                 startTouchX = event.rawX.toInt()
                 startTouchY = event.rawY.toInt()
             } else if (action == MotionEvent.ACTION_MOVE) {
                 params.leftMargin = startX + (event.rawX.toInt() - startTouchX)
                 params.topMargin = startY + (event.rawY.toInt() - startTouchY)
                 tv.layoutParams = params
             }
         }
         return true
     }
     ```
   - Because `setOnTouchListener(this)` is registered on `tv` only, touching inside `tv` drags it smoothly, while touching outside `tv` does nothing.

---

## 7. Toast Notifications & WebView (`Toast` & `WebView`)

### Toast notifications
A `Toast` displays a small, temporary pop-up message over the current screen.
- Create a `Toast` using the static method `Toast.makeText(context: Context!, text: CharSequence!, duration: Int): Toast`.
- Duration constants: `Toast.LENGTH_LONG` and `Toast.LENGTH_SHORT`.
- Call `.show()` on the `Toast` instance to display it:

```kotlin
var t: Toast = Toast.makeText(this, "HI", Toast.LENGTH_LONG)
t.show()
```

### WebView (`WebView.pptx`)
A `WebView` is a `View` subclass that runs an embedded web browser inside your activity:

```kotlin
val webView: WebView = WebView(this)
val settings: WebSettings = webView.settings // or webView.getSettings()
settings.javaScriptEnabled = true            // JavaScript is disabled by default
webView.loadUrl("https://www.yahoo.com")
setContentView(webView)
```

To allow the app to load web pages, add the `INTERNET` permission inside `<manifest>` in `AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

---

## 8. Custom Graphics, Animation & Sound (`6-GraphicsAndAnimation`)

*Source: `6-GraphicsAndAnimation.pptx`, slides 1–120. This section follows the Duck Hunting app, independently of the earlier midterm scope notes.*

### Custom View, Canvas, and Paint

**`GameView : View`** handles drawing. Override `onDraw(canvas: Canvas)`; Android supplies the `Canvas` when the view needs to render. **Canvas describes what/where to draw; Paint describes how to draw**, including color, stroke width, fill/stroke style, text size, and antialiasing.

```kotlin
class GameView(context: Context) : View(context) {
    private val paint = Paint().apply {
        color = Color.BLACK
        strokeWidth = 20f
        style = Paint.Style.FILL
        isAntiAlias = true
    }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)
        canvas.drawCircle(0f, height.toFloat(), height / 5f, paint)
        canvas.drawLine(0f, height.toFloat(), height / 5f,
            height - height / 5f, paint)
    }
}
// In the Activity: setContentView(GameView(this))
```

- The custom view constructor must call the `View` constructor with a `Context`. The lecture also accepts width/height constructor parameters to size the game.
- **Coordinates:** Origin `(0,0)` is the view's upper-left corner; x increases right, y increases downward. Drawing coordinates are pixels; many Canvas methods take `Float` values.
- A circle centered at `(0, height)` lies at the bottom-left corner, so only its upper-right quarter is visible. A line from that point forms the cannon barrel.
- Other Canvas methods include `drawRect`, `drawOval`, `drawArc`, `drawText`, `drawLines`, and `drawBitmap`. Configure `Paint` before drawing and reuse it rather than recreating it each frame.

**Available dimensions:** The lecture reads `resources.displayMetrics.widthPixels/heightPixels` and scales game objects relative to the screen. For actual drawing, distinguish full-screen size from the custom view's measured `width/height` and system-bar insets; a pixel coordinate from one space is not automatically valid in another.

To hide system bars in the lecture's full-screen game:

```kotlin
WindowCompat.getInsetsController(window, window.decorView)
    .hide(WindowInsetsCompat.Type.systemBars())
```

If retaining the status bar, the lecture uses `window.decorView.getWindowVisibleDisplayFrame(rect)` and `rect.top` after layout, in `onWindowFocusChanged`, because measurements in `onCreate` may not be ready. Use the intended landscape orientation when calculating dimensions; emulator-specific sizes are not universal constants.

### Bitmap resources and proportional scaling

Put `duck.png` in `res/drawable/`, then decode it once:

```kotlin
val duck = BitmapFactory.decodeResource(resources, R.drawable.duck)
val scale = viewWidth.toFloat() / (5f * duck.width)
val duckHeight = (duck.height * scale).toInt()
val duckRect = Rect(viewWidth - viewWidth / 5, 0, viewWidth, duckHeight)
// Inside onDraw:
canvas.drawBitmap(duck, null, duckRect, paint)
```

- **`Rect(left, top, right, bottom)`** takes edge coordinates, not `(x, y, width, height)`. Width is `right-left`; height is `bottom-top`.
- `drawBitmap(bitmap, src, dest, paint)`: `src` selects a source crop; `null` uses the entire image. `dest` positions and scales it on the Canvas.
- In this example, the drawn width is one-fifth of the view width. Multiplying both original image dimensions by the **same scale** preserves its aspect ratio; arbitrary width/height stretching distorts it.
- `bitmap.width/height` report image dimensions. `getPixel(x,y)` reads a pixel color; `setPixel(x,y,color)` writes one when the bitmap is mutable.

### MVC and the timer-driven game loop

| Component | Responsibility in Duck Hunting |
| --- | --- |
| `Game` model | Cannon position/angle; bullet position, speed, firing state; duck rectangle, speed, alive/shot state; movement and collision logic |
| `GameView` | Read the model and draw the cannon, bullet, and current duck image |
| `MainActivity` controller | Set up the shared model/view, handle input, and coordinate updates |

The Activity and GameView must reference the **same `Game` object**, not separate game states. The lecture creates the model through GameView and exposes it to the Activity. Dimensions, hunting bounds, speeds, and the update interval are configured relative to the available view size.

**One tick:** Update model state, then request a redraw. `Timer` schedules repeated work; `TimerTask` is abstract, so implement `run()`:

```kotlin
class GameTimerTask(private val activity: MainActivity) : TimerTask() {
    override fun run() {
        activity.updateModel()
        activity.updateView()
    }
}

// Activity setup; deltaTimeMs is a Long, for example 50L:
val gameTimer = Timer()
gameTimer.schedule(GameTimerTask(this), 0L, deltaTimeMs)

// Activity method:
fun updateView() {
    gameView.postInvalidate()
}
```

- `schedule(task, delay, period)` uses **milliseconds**. A 100 ms period targets 10 updates/s; 50 ms targets 20 updates/s. Movement uses speed and elapsed/update time in matching units.
- **Do not call `onDraw` yourself.** `postInvalidate()` requests a future UI-thread redraw; it is not an immediate synchronous call or a guarantee of exactly one rendered frame per tick. `invalidate()` is the UI-thread form. [View API](https://developer.android.com/reference/android/view/View#invalidate())
- `TimerTask.run()` runs on a timer thread. Posting a redraw does not make simultaneous model updates automatically thread-safe; coordinate model access with drawing and touch handling. Stop/cancel the timer when the game is no longer active.
- The update loop moves the duck, restarts it when off-screen, and moves a fired bullet or reloads it when it leaves the hunting area. `onDraw` reads the resulting positions instead of containing all game rules.

### Sprite frames, cannon geometry, and bullets

**Sprite animation:** Three duck images can form a four-frame wing cycle `0, 1, 2, 1`. Decode resources into an `Array<Bitmap>` once, draw `ducks[duckFrame]`, and cycle the index:

```kotlin
canvas.drawBitmap(ducks[duckFrame], null, game.getDuckRect(), paint)
duckFrame = (duckFrame + 1) % ducks.size
```

This is the lecture's frame-cycling approach. Duck position changes and image changes are separate: the model moves its rectangle while the image sequence animates its wings.

**Cannon endpoint:** With center `(cx,cy)`, barrel length $L$, and angle $\theta$ measured upward from the positive x-axis:

$$endX=cx+L\cos\theta,\qquad endY=cy-L\sin\theta$$

Subtract the vertical component because screen y increases downward. Kotlin/Java trigonometric functions use **radians**.

To aim at a touch in the same coordinate space as the cannon:

```kotlin
val dx = event.x - game.getCannonCenter().x
val dy = game.getCannonCenter().y - event.y
val angle = Math.atan2(dy.toDouble(), dx.toDouble()).toFloat()
game.setCannonAngle(angle)
```

`atan2(dy, dx)` handles quadrants and vertical aiming without dividing by zero. Forward events in the GameView's coordinate space, or convert Activity/screen coordinates before using this calculation.

**Bullet state:** Fire only when no bullet is already active. Keep its firing angle in the model, advance its position each tick, and draw it with `drawCircle` using model coordinates/radius. Reload when off-screen or after a hit; firing and moving are different operations.

### GestureDetector and event forwarding

`GestureDetector.SimpleOnGestureListener` supplies default implementations for gesture and double-tap callbacks, so subclass it and override only the needed methods.

| Callback | Game action |
| --- | --- |
| `onDown(event)` | Return `true` to accept the gesture sequence |
| `onSingleTapConfirmed(event)` | Aim after confirming the tap was not a double tap |
| `onScroll(e1, e2, distanceX, distanceY)` | Aim using the current event `e2`; returns `Boolean` |
| `onDoubleTap(event)` | Fire once for the recognized double tap |
| `onDoubleTapEvent(event)` | Receives individual down/move/up events within the double-tap gesture |

```kotlin
inner class TouchHandler : GestureDetector.SimpleOnGestureListener() {
    override fun onDown(event: MotionEvent): Boolean = true

    override fun onSingleTapConfirmed(event: MotionEvent): Boolean {
        updateCannon(event)
        return true
    }

    override fun onScroll(e1: MotionEvent?, e2: MotionEvent,
        distanceX: Float, distanceY: Float): Boolean {
        updateCannon(e2)
        return true
    }

    override fun onDoubleTap(event: MotionEvent): Boolean {
        if (!game.isBulletFired()) game.fireBullet()
        return true
    }
}
```

Create one listener, pass it to `GestureDetector(this, handler)`, and register it with `detector.setOnDoubleTapListener(handler)`. Forward the touch stream from an Activity/View callback using `detector.onTouchEvent(event)`; otherwise the detector never sees the events. Return `true` when handled, or delegate unhandled input to the superclass.

**API clarification:** The slides also fire through `onDoubleTapEvent`; that callback may run multiple times, so guard against repeated firing. Returning `true` from `onDown` is important because the default `false` can suppress subsequent gesture callbacks. [Gesture guide](https://developer.android.com/develop/ui/views/touch-and-input/gestures/detector), [callback definitions](https://developer.android.com/reference/android/view/GestureDetector.SimpleOnGestureListener)

### SoundPool and hit-state changes

**SoundPool** loads and plays short game sound effects. Use `SoundPool.Builder().build()` rather than the deprecated multi-argument constructor. Store sound files such as `cannon_fire.wav` in `res/raw/` and reference them without extensions:

```kotlin
val pool = SoundPool.Builder().build()
val fireSoundId = pool.load(this, R.raw.cannon_fire, 1)
// Later, once loading has completed, when the bullet is fired:
val streamId = pool.play(fireSoundId, 1f, 1f, 1, 0, 1f)
```

- `load(context, resourceId, priority)` returns a **sound ID**. Wait for successful loading, using `setOnLoadCompleteListener` when needed, before playing.
- `play(soundId, leftVolume, rightVolume, priority, loop, rate)` returns a **stream ID** for that playback. Sound IDs identify loaded samples; stream IDs control individual playbacks with methods such as `pause`/`resume`.
- Volumes range from `0f` to `1f`. `loop=0` plays once, `1` plays twice, `-1` repeats indefinitely. Playback rate `1f` is normal, with range `0.5f`–`2f`. Release the pool when finished. [SoundPool API](https://developer.android.com/reference/android/media/SoundPool)

**Hit handling:** The model detects a bullet/duck collision. On the transition to “shot,” play the hit sound once, set the duck's shot state, and reload the bullet. The shot duck falls downward and displays `ducks[0]` rather than cycling wing frames. When it leaves the screen, clear the shot state and create/reposition the next duck. Do not replay the hit sound on every tick while the duck remains shot.
