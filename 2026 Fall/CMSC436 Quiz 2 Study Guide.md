# CMSC436 Quiz 2 Study Guide: Tip Calculator and Tic Tac Toe

**Quiz scope:** Everything in the Tip Calculator and Tic Tac Toe app slides. The instructor's note explicitly excludes Activity lifecycle methods and persistent data from Tuesday's quiz. The older quizzes guide the question style, not the topic list; their questions about multiple activities, `Intent`, `SharedPreferences`, `RadioGroup`, and lifecycle callbacks are outside this stated scope.

**Sources:** `2B-TipCalculator.pptx` and `3-TicTacToe.pptx` (content); `Quiz2Solutions_436Fall2025.pdf` and `Quiz2KEY_436Spring2026.pdf` (style).

## What to know

### Tip Calculator 1. MVC and widgets

- The **Model** is `TipCalculator.kt`: it holds the bill and tip rate and calculates the tip and total. It has no GUI code, so it can be reused with another View.
- The **View** is `activity_main.xml`: `EditText` widgets gather the bill and tip percentage; `TextView` widgets label inputs and display the tip and total. A `Button` triggers calculation in Version 3.
- The **Controller** is `MainActivity.kt`: it reads input from the View, gives values to the Model, gets calculated results, and updates the View.
- `View` is the base GUI class. `EditText` is a subclass of `TextView`; both can display text, but `EditText` accepts user input.

### Tip Calculator 2. XML layout and IDs

- The example changes the generated `ConstraintLayout` to a `RelativeLayout`. Its width and height are `match_parent`; the child labels and fields use `wrap_content` so they take only the space they need.
- `android:id="@+id/amount_bill"` creates an ID. Other views can refer to it in positioning attributes, and Kotlin can later retrieve the view with `findViewById(R.id.amount_bill)`.
- `RelativeLayout` positions one child relative to another. For example, `android:layout_toRightOf="@id/label_bill"` places a field to the right of its label; `android:layout_alignBottom="@id/label_bill"` aligns their bottom edges. `layout_below`, `layout_above`, and edge alignment work similarly.
- `RelativeLayout.LayoutParams` represents layout rules in code. In XML, the corresponding `android:layout_*` attributes arrange children.
- The activity loads `activity_main.xml` with `setContentView(R.layout.activity_main)` before looking up its child views.

### Tip Calculator 3. Input and resources

- `android:inputType="numberDecimal"` presents decimal-oriented input. It does not make conversion safe when an `EditText` is blank; validate input before calling `toDouble()`.
- `android:hint="@string/amount_bill_hint"` shows guidance inside an empty field. A label's visible text can use `android:text="@string/label_bill"`.
- Define reusable text in `res/values/strings.xml`; refer to it with `@string/name`. Use `@+id/name` to create an ID and `@id/name` to reference an existing ID.

### Tip Calculator 4. Appearance, styles, and themes

- A **margin** is outside a view; **padding** is inside it. For example, `android:layout_marginTop` separates a view from its surroundings, while `android:paddingLeft` moves its contents away from its left edge.
- Appearance attributes include `android:background`, `android:textColor`, `android:textSize`, and `android:textStyle`. Color literals can use `#RGB`, `#ARGB`, `#RRGGBB`, or `#AARRGGBB`; a named color resource uses `@color/name`. An empty `View` with a background and small height can serve as a divider.
- A **style** groups attributes for a view: define a `<style>` with `<item>` entries in `res/values/themes.xml`, optionally inherit with `parent`, and apply with `style="@style/InputStyle"`. Changing a shared style updates every view using it.
- A **theme** applies to an activity or the app using `android:theme="@style/ThemeName"` in `AndroidManifest.xml`. A view style and an app theme have different scopes.

### Tip Calculator 5. Clicks and view lookup

- Version 3's XML button can use `android:onClick="calculate"`; the activity handler has the form `fun calculate(v: View)`. The `v` argument is the clicked button.
- `findViewById(R.id.amount_tip)` retrieves the view whose ID was assigned in XML. Store references such as `billEditText: EditText` and `tipTextView: TextView` as activity properties when several methods need them.
- A click listener can instead be built in Kotlin: implement `View.OnClickListener`, create an instance, and register it on the button. Since this interface has one abstract method, `button.setOnClickListener { /* handle click */ }` is another form.
- **Slide correction:** `button.setOnClickListener { null }` registers a lambda that does nothing; the braces mean it is still a listener. To prevent button interaction, the Tic Tac Toe example uses `button.isEnabled = false`. [Android's Kotlin listener explanation](https://developer.android.com/kotlin/common-patterns) confirms that a lambda supplies an `OnClickListener`.

### Tip Calculator 6. Conversion, Model, and output

- `EditText.text` is an `Editable`. Use `.text.toString()` to obtain a `String`, then convert to `Double` only after checking for blank or invalid input.
- The entered tip percentage is a percentage, while the Model's `setTip` receives a fraction: the slides use `tipCalc.setTip(0.01 * newTip)`.
- After setting the bill and tip rate in the Model, ask it for formatted tip and total values and put those strings in the output `TextView`s. Keep arithmetic and formatting in the Model rather than duplicating it in the View.

### Tip Calculator 7. TextWatcher and live updates

- Version 4 removes the Calculate button. A `TextWatcher` observes edits to each `EditText` so changing either input recalculates both outputs.
- `TextWatcher` has `beforeTextChanged`, `onTextChanged`, and `afterTextChanged`. The example leaves the first two empty and calls `calculate()` from `afterTextChanged`.
- Create one `TextChangeHandler` and register it on **both** fields with `addTextChangedListener`. The new `calculate()` takes no `View` argument because it reads both fields regardless of which one changed.

### Tic Tac Toe 1. Model, View, Controller, and game state

- A GUI may need to be built in Kotlin when its number or arrangement of views depends on data. The Tic Tac Toe example builds a 3 × 3 button grid in code instead of declaring nine buttons in XML.
- `TicTacToe` is the **Model**. It stores a 3 × 3 `Int` board and the current turn. A cell value of `0` is empty, `1` belongs to player 1 (`X`), and `2` belongs to player 2 (`O`).
- The Model handles moves, game-over checks, results, and resets. The View displays buttons and status. The Controller receives clicks, asks the Model to play, and updates the View.
- Early versions put View and Controller code in `MainActivity`. Version 5 moves the View into a reusable `ButtonGridAndTextView` class that extends `GridLayout`. The slides occasionally reverse the words in this class name; the role of the class matters more than that naming inconsistency.

### Tic Tac Toe 2. Building the grid in Kotlin

- `MainActivity` can pass `this` to `GridLayout(this)` and `Button(this)` because an activity is a `Context`. A `Button` is a `View`; a `GridLayout` is a `ViewGroup`.
- Set `gridLayout.rowCount` and `gridLayout.columnCount`. Add buttons with `gridLayout.addView(buttons[row][col], w, w)`, then use `setContentView(gridLayout)`.
- `Resources.getSystem().displayMetrics.widthPixels` (or `resources.displayMetrics.widthPixels`) gives the screen width in pixels. In the slide example, `w = width / TicTacToe.SIDE` makes square buttons across the screen in portrait orientation.
- A two-dimensional button array can be initialized with `Array(TicTacToe.SIDE) { Array(TicTacToe.SIDE) { Button(this) } }`. The outer array holds rows; each inner array holds buttons in one row. `lateinit` delays assigning the array until the GUI is built.

### Tic Tac Toe 3. Click handling

- The explicit listener pattern has three steps: implement `View.OnClickListener`, instantiate the listener, and register it with `setOnClickListener` on each button.
- `onClick(view: View?)` receives the clicked view. The controller can find its row and column by comparing it with each `buttons[row][col]`.
- `View.OnClickListener` has one abstract method, so a lambda can replace a listener class: `buttons[row][col].setOnClickListener { update(row, col) }`. The click `View` parameter may be omitted when unused.
- The final slides suggest another design: make a custom `GridButton` that stores its own row and column. This could avoid searching the array, but the slides only sketch it.

### Tic Tac Toe 4. Applying the game rules and updating the screen

- The controller calls `ttt.play(row, col)`. If it returns `1`, show `X`; if it returns `2`, show `O`. If neither branch runs, leave that button's text unchanged (for example, an already played cell).
- After a move, check `ttt.isGameOver()`. When the game ends, disable the buttons with `buttons[row][col].isEnabled = false` and show `ttt.result()` in the status view.
- To start a new game, reset both sides: call `ttt.resetGame()` for Model state, then clear and re-enable the buttons and restore the status view. Clearing the labels alone does not reset the rules.

### Tic Tac Toe 5. Status view and GridLayout spans

- Version 3 adds a `TextView` below the three button rows. The grid therefore has `TicTacToe.SIDE + 1` rows.
- `GridLayout.spec(start, size)` defines a starting row or column and how many cells to span. For the status view, use a row spec of `(TicTacToe.SIDE, 1)` and a column spec of `(0, TicTacToe.SIDE)`.
- Pass the **row spec first** and **column spec second** to `GridLayout.LayoutParams(rowSpec, columnSpec)`. Assign the result to `status.layoutParams`, then add the view to the grid.
- In a type name such as `GridLayout.Spec`, the part after the dot names a type nested in the part before it. The slides also use `GridLayout.LayoutParams`, `AlertDialog.Builder`, and `DialogInterface.OnClickListener` this way.
- The status view can change its text, background color, gravity, and text size. The example centers text and changes the background when the game ends.

### Tic Tac Toe 6. Replay dialog and inner class scope

- `AlertDialog.Builder(this)` configures the title, message, and buttons. The example attaches one `DialogInterface.OnClickListener` to YES and NO, calls `create()`, then `show()`.
- In the example listener, the positive button has ID `-1` and restarts the game; the negative button has ID `-2` and exits the activity.
- Inside the `PlayDialog` inner class, `this@MainActivity.finish()` refers to the enclosing activity. Plain `this` refers to the dialog listener object.
- The builder can also set a neutral button, but the slide example uses only positive and negative buttons.
- The dialog appears in the middle by default; the slides show that its `window` can be moved to `Gravity.BOTTOM` after it is shown.

### Tic Tac Toe 7. Reusable View and Controller

- `ButtonGridAndTextView` owns the buttons and status `TextView`. Its constructor receives a `Context`, sizing information, the number of buttons per side, and a `View.OnClickListener` to register on the buttons.
- It exposes methods such as `setButtonText`, `setStatusText`, `setStatusBackgroundColor`, `resetButtons`, `enableButtons`, and `isButton`. These let `MainActivity` work with the View without directly manipulating its private widgets.
- The reusable View uses its own `side` value instead of depending on `TicTacToe.SIDE`. That keeps it independent of the Tic Tac Toe Model.
- In Version 5, `MainActivity` is the Controller. It creates the Model and View, receives clicks, invokes Model methods, and tells the View what to display.

## Practice question bank

Try these before reading the key. The past quizzes use short prompts, multiple choice, and one-step code completion. This bank has **four questions for each of the 14 topic groups**: 28 on each app. For a 10-minute, 10-question run, choose five from each app and cover different groups.

### Tic Tac Toe questions (1-28)

### Topic 1: MVC and game state

**1. Short answer.** Which class owns the 3 × 3 array recording whether each square is empty, X, or O? Give the class name only. ==TicTacToe==

**2. Multiple choice.** A cell stores `2`. What does that mean in the slide example?

A. No player has used it.  
B. Player 1 has used it.  
==C. Player 2 has used it.==  
D. The game has ended.

**3. Yes or no.** Should the View decide whether a move completes a winning line? ==No==

**4. Short answer.** A click reaches the controller. Name the layer it should consult before changing a button label. ==onClickListener==

### Topic 2: Building the grid

**5. Multiple choice.** Why can `MainActivity` pass `this` to `Button(this)`?

A. `Button` extends `MainActivity`.  
==B. An activity is a `Context`.==  
C. `this` always means the screen width.  
D. `Button` has no constructor parameter.

**6. Complete one Kotlin line.** Set `gridLayout` to have `TicTacToe.SIDE` columns.

```kotlin
gridLayout.__________ = TicTacToe.SIDE
```

**7. Short answer.** What property after `Resources.getSystem().displayMetrics.` gives the screen width in pixels? ==widthPixel==

**8. Multiple choice.** What is the type of one element `buttons[1]` when `buttons` has type `Array<Array<Button>>`? 

A. `Button`  
==B. `Array<Button>`==  
C. `Array<Array<Button>>`  
D. `IntArray`

### Topic 3: Click handling

**9. Short answer.** Which listener interface does a class implement to receive ordinary button clicks? Give the qualified interface name. ==onClickListener==

**10. Multiple choice.** A listener object `handler` exists. Which call registers it on `buttons[0][2]`? 

A. `handler.addView(buttons[0][2])`  
==B. `buttons[0][2].setOnClickListener(handler)`==  
C. `buttons[0][2].onClick(handler)`  
D. `setContentView(handler)`

**11. Yes or no.** Can the click `View` parameter be left out of a listener lambda when the body only uses the captured `row` and `col` values? ==No==

**12. Short answer.** In the explicit listener version, what is compared with the `view` argument to identify the clicked board position? 

### Topic 4: Rules and updates

**13. Multiple choice.** `ttt.play(row, col)` returns `2`. What should the controller put in that button?

A. `X`  
==B. `O`==  
C. `2`  
D. An empty string

**14. Yes or no.** If `ttt.isGameOver()` becomes true, should the controller disable the buttons and display `ttt.result()` in the status view? ==Yes==

**15. Complete one Kotlin line.** Disable `buttons[row][col]` using the property shown in the slides.

```kotlin
buttons[row][col].isEnabled = false
```

**16. Short answer.** The user starts a new game. Which Model method must run before fresh moves follow the new game's rules? resetGame()

### Topic 5: Status view and spans

**17. Short answer.** For a board with `TicTacToe.SIDE` button rows, what should `gridLayout.rowCount` be after adding one status row? Write an expression using `TicTacToe.SIDE`.

**18. Complete one Kotlin line.** Make a `GridLayout.Spec` for a status view starting at row `TicTacToe.SIDE` and spanning exactly one row.

```kotlin
val rowSpec = GridLayout.spec(__________, __________)
```

**19. Multiple choice.** Let `SIDE` be the number of buttons per row. The status view must span all `SIDE` columns, starting at column zero. Which specification does that?

A. `GridLayout.spec(SIDE, 0)`  
B. `GridLayout.spec(0, SIDE)`  
C. `GridLayout.spec(1, SIDE + 1)`  
D. `GridLayout.spec(SIDE, 1)`

**20. Short answer.** In the type name `GridLayout.Spec`, which class contains the nested `Spec` type?

### Topic 6: Replay dialog

**21. Multiple choice.** Which object is used to set a dialog's title, message, and YES/NO buttons before showing it?

A. `GridLayout.Spec`  
B. `AlertDialog.Builder`  
C. `DisplayMetrics`  
D. `ViewGroup`

**22. Short answer.** In the slide example's dialog listener, what `id` denotes the positive YES button?

**23. Complete one Kotlin line.** From inside the `PlayDialog` inner class, finish the enclosing activity.

```kotlin
__________.finish()
```

**24. Short answer.** Which listener interface handles the replay dialog's YES and NO button clicks? Give its qualified name.

### Topic 7: Reusable View and Controller

**25. Short answer.** In Version 5, which class becomes the Controller and holds references to both the Model and reusable View?

**26. Multiple choice.** Why does `ButtonGridAndTextView` receive a side length rather than using `TicTacToe.SIDE` internally?

A. `GridLayout` forbids constants.  
B. The View can work with a different Model or grid size.  
C. The Model must draw the buttons.  
D. Android cannot create a 3 × 3 grid.

**27. Short answer.** Which View method lets the Controller ask whether a clicked `Button` occupies a particular row and column?

**28. Yes or no.** After separating View and Controller, should the Controller directly change the reusable View's private `status` field?

### Tip Calculator questions (29-56)

#### Topic 8: MVC and widgets

**29. Short answer.** Which file contains the calculations for the tip and total but no GUI code? Give the filename.

**30. Multiple choice.** A user types the bill in one widget; the calculated total appears in another. Which pair fits those roles?

A. `TextView` input, `EditText` output  
B. `EditText` input, `TextView` output  
C. `Button` input, `RelativeLayout` output  
D. `GridLayout` input, `Button` output

**31. Yes or no.** Should `TipCalculator` directly call `findViewById` to update the displayed tip?

**32. Short answer.** Which app layer passes the user's values to the Model and puts the results into the View?

#### Topic 9: XML layout and IDs

**33. Complete one XML attribute.** Give a new bill field the ID `amount_bill`.

```xml
android:id="__________"
```

**34. Multiple choice.** A bill field must sit to the right of a label whose ID is `label_bill`. Which attribute expresses that rule inside the `EditText`?

A. `android:layout_toRightOf="@id/label_bill"`  
B. `android:layout_below="@id/label_bill"`  
C. `android:layout_width="@id/label_bill"`  
D. `android:hint="@id/label_bill"`

**35. Short answer.** Which value makes the root layout fill its parent in a dimension: `wrap_content` or `match_parent`?

**36. Yes or no.** Can the same XML ID help position a view and later let the activity retrieve that view?

#### Topic 10: Input and resources

**37. Short answer.** What `android:inputType` value does the example use for decimal entry?

**38. Complete one XML attribute.** Show the empty bill field's hint from the string resource named `amount_bill_hint`.

```xml
android:hint="__________"
```

**39. Multiple choice.** Where is the text for `@string/label_bill` defined in this example?

A. `AndroidManifest.xml`  
B. `res/values/strings.xml`  
C. `MainActivity.kt`  
D. `res/drawable/`

**40. Yes or no.** Does `android:inputType="numberDecimal"` guarantee that converting a blank field with `toDouble()` succeeds?

#### Topic 11: Appearance, styles, and themes

**41. Short answer.** What property puts space **inside** a view, between its edge and its text: margin or padding?

**42. Multiple choice.** Which color expression refers to a named resource instead of a literal color value?

A. `#FF3300`  
B. `#F30`  
C. `@color/darkGreen`  
D. `#AAFF3300`

**43. Complete one XML attribute.** Apply the existing `InputStyle` to an `EditText`.

```xml
style="__________"
```

**44. Short answer.** Which manifest attribute applies a style to the whole app or an activity?

#### Topic 12: Clicks and view lookup

**45. Complete one Kotlin expression.** Retrieve the `EditText` whose XML ID is `amount_bill`.

```kotlin
val billEditText: EditText = __________
```

**46. Short answer.** In an XML click handler declared as `android:onClick="calculate"`, what is the type of the single parameter to `fun calculate(v: ...)`?

**47. Multiple choice.** Assume `fun calculate()` takes no parameters. Which call registers a lambda to run when `calculateButton` is clicked?

A. `calculateButton.setOnClickListener { calculate() }`  
B. `calculateButton.findViewById { calculate() }`  
C. `calculateButton.setContentView { calculate() }`  
D. `calculateButton.addTextChangedListener { calculate() }`

**48. Yes or no.** If `calculate()` needs the input and output widgets in several methods, is it reasonable to store their references as activity properties?

#### Topic 13: Conversion, Model, and output

**49. Short answer.** What type does `tipEditText.text` return before `toString()` is called?

**50. Complete one Kotlin expression.** Convert a checked, nonblank bill field's text to `Double`.

```kotlin
val bill: Double = billEditText.text.toString().__________
```

**51. Multiple choice.** The user enters `18` for an 18% tip. What value should be passed to the Model's `setTip` in the slide example?

A. `18.0`  
B. `1.8`  
C. `0.18`  
D. `0.018`

**52. Short answer.** Which object should calculate and format the tip before the activity assigns it to `tipTextView.text`?

#### Topic 14: TextWatcher and live updates

**53. Short answer.** Which `TextWatcher` callback does the example use to recalculate **after** the text has changed?

**54. Yes or no.** If the user can edit both bill and percentage, should the watcher be registered on both `EditText`s?

**55. Complete one Kotlin line.** Register an existing watcher `tch` on `billEditText`.

```kotlin
billEditText.__________(tch)
```

**56. Multiple choice.** Why does Version 4's `calculate()` take no `View` parameter?

A. It reads both input fields regardless of which one changed.  
B. `TextWatcher` passes a `Button` instead.  
C. `EditText` has no IDs.  
D. The Model cannot accept a `View`.

## Answer key

The `==...==` highlights already present in some Tic Tac Toe questions are study attempts. Check them against this key, especially Questions 4, 7, and 9.

1. `TicTacToe` — its board is Model state.
2. **C** — `2` means player 2 (`O`) claimed the cell.
3. **No** — the Model determines game rules and wins.
4. **Model** — the controller asks it to play before displaying the result.
5. **B** — `MainActivity` is a `Context` through inheritance.
6. `gridLayout.columnCount = TicTacToe.SIDE`.
7. `widthPixels`.
8. **B** — one outer-array element is a row of `Button` objects.
9. `View.OnClickListener`.
10. **B** — the button registers the listener object.
11. **Yes** — the lambda does not need to name an unused click parameter.
12. Each `buttons[row][col]` (the Button references in the array).
13. **B** — return value `2` displays `O`.
14. **Yes** — the controller disables play and uses the Model's result for the status text.
15. `buttons[row][col].isEnabled = false`.
16. `ttt.resetGame()` — it clears Model state for a new game.
17. `TicTacToe.SIDE + 1`.
18. `GridLayout.spec(TicTacToe.SIDE, 1)`.
19. **B** — start at column `0` and span `SIDE` columns.
20. `GridLayout`.
21. **B** — `AlertDialog.Builder` configures the dialog.
22. `-1`.
23. `this@MainActivity.finish()`.
24. `DialogInterface.OnClickListener`.
25. `MainActivity`.
26. **B** — the View then has no dependency on the Tic Tac Toe Model's board constant.
27. `isButton`.
28. **No** — the Controller calls the View's public methods such as `setStatusText`.
29. `TipCalculator.kt` — the Model owns the calculations.
30. **B** — `EditText` accepts input and `TextView` displays output.
31. **No** — the Model has no GUI code; the Controller updates the View.
32. **Controller** (`MainActivity`).
33. `@+id/amount_bill`.
34. **A** — `layout_toRightOf` places the field right of its label.
35. `match_parent`.
36. **Yes** — the ID serves both layout references and `findViewById`.
37. `numberDecimal`.
38. `@string/amount_bill_hint`.
39. **B** — the example's string values are in `res/values/strings.xml`.
40. **No** — blank text cannot be converted by `toDouble()`.
41. **Padding** — margin is outside the view.
42. **C** — `@color/darkGreen` names a color resource.
43. `@style/InputStyle`.
44. `android:theme`.
45. `findViewById(R.id.amount_bill)`.
46. `View`.
47. **A** — `setOnClickListener` registers the click callback.
48. **Yes** — properties let multiple activity methods use the same references.
49. `Editable`.
50. `toDouble()` — only after validating the text.
51. **C** — `18 * 0.01 = 0.18`.
52. The `TipCalculator` Model (`tipCalc`).
53. `afterTextChanged`.
54. **Yes** — either field can trigger recalculation.
55. `addTextChangedListener`.
56. **A** — it does not need to know which field triggered the update.

## Fast self-check

Trace both apps from memory:

- Tip Calculator: `EditText` → Controller converts input → `TipCalculator` updates/calculates → output `TextView`s. In Version 4, either input's watcher triggers that path.
- Tic Tac Toe: `Button` → listener/Controller → `ttt.play` → View text → `ttt.isGameOver()` → status and button state. Dialog YES → Model reset → View reset.
