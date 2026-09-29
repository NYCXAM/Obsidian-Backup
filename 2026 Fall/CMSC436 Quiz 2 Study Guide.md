# CMSC436 Tic Tac Toe Quiz Study Guide

**Sources:** `3-TicTacToe.pptx` (content) and `Quiz2KEY_436Spring2026.pdf` (question style). The past quiz is a style reference; its activity lifecycle and SharedPreferences topics are not taught in the supplied Tic Tac Toe slides.

## What to know

### 1. Model, View, Controller, and game state

- A GUI may need to be built in Kotlin when its number or arrangement of views depends on data. The Tic Tac Toe example builds a 3 × 3 button grid in code instead of declaring nine buttons in XML.
- `TicTacToe` is the **Model**. It stores a 3 × 3 `Int` board and the current turn. A cell value of `0` is empty, `1` belongs to player 1 (`X`), and `2` belongs to player 2 (`O`).
- The Model handles moves, game-over checks, results, and resets. The View displays buttons and status. The Controller receives clicks, asks the Model to play, and updates the View.
- Early versions put View and Controller code in `MainActivity`. Version 5 moves the View into a reusable `ButtonGridAndTextView` class that extends `GridLayout`. The slides occasionally reverse the words in this class name; the role of the class matters more than that naming inconsistency.

### 2. Building the grid in Kotlin

- `MainActivity` can pass `this` to `GridLayout(this)` and `Button(this)` because an activity is a `Context`. A `Button` is a `View`; a `GridLayout` is a `ViewGroup`.
- Set `gridLayout.rowCount` and `gridLayout.columnCount`. Add buttons with `gridLayout.addView(buttons[row][col], w, w)`, then use `setContentView(gridLayout)`.
- `Resources.getSystem().displayMetrics.widthPixels` (or `resources.displayMetrics.widthPixels`) gives the screen width in pixels. In the slide example, `w = width / TicTacToe.SIDE` makes square buttons across the screen in portrait orientation.
- A two-dimensional button array can be initialized with `Array(TicTacToe.SIDE) { Array(TicTacToe.SIDE) { Button(this) } }`. The outer array holds rows; each inner array holds buttons in one row. `lateinit` delays assigning the array until the GUI is built.

### 3. Click handling

- The explicit listener pattern has three steps: implement `View.OnClickListener`, instantiate the listener, and register it with `setOnClickListener` on each button.
- `onClick(view: View?)` receives the clicked view. The controller can find its row and column by comparing it with each `buttons[row][col]`.
- `View.OnClickListener` has one abstract method, so a lambda can replace a listener class: `buttons[row][col].setOnClickListener { update(row, col) }`. The click `View` parameter may be omitted when unused.
- The final slides suggest another design: make a custom `GridButton` that stores its own row and column. This could avoid searching the array, but the slides only sketch it.

### 4. Applying the game rules and updating the screen

- The controller calls `ttt.play(row, col)`. If it returns `1`, show `X`; if it returns `2`, show `O`. If neither branch runs, leave that button's text unchanged (for example, an already played cell).
- After a move, check `ttt.isGameOver()`. When the game ends, disable the buttons with `buttons[row][col].isEnabled = false` and show `ttt.result()` in the status view.
- To start a new game, reset both sides: call `ttt.resetGame()` for Model state, then clear and re-enable the buttons and restore the status view. Clearing the labels alone does not reset the rules.

### 5. Status view and GridLayout spans

- Version 3 adds a `TextView` below the three button rows. The grid therefore has `TicTacToe.SIDE + 1` rows.
- `GridLayout.spec(start, size)` defines a starting row or column and how many cells to span. For the status view, use a row spec of `(TicTacToe.SIDE, 1)` and a column spec of `(0, TicTacToe.SIDE)`.
- Pass the **row spec first** and **column spec second** to `GridLayout.LayoutParams(rowSpec, columnSpec)`. Assign the result to `status.layoutParams`, then add the view to the grid.
- In a type name such as `GridLayout.Spec`, the part after the dot names a type nested in the part before it. The slides also use `GridLayout.LayoutParams`, `AlertDialog.Builder`, and `DialogInterface.OnClickListener` this way.
- The status view can change its text, background color, gravity, and text size. The example centers text and changes the background when the game ends.

### 6. Replay dialog and inner class scope

- `AlertDialog.Builder(this)` configures the title, message, and buttons. The example attaches one `DialogInterface.OnClickListener` to YES and NO, calls `create()`, then `show()`.
- In the example listener, the positive button has ID `-1` and restarts the game; the negative button has ID `-2` and exits the activity.
- Inside the `PlayDialog` inner class, `this@MainActivity.finish()` refers to the enclosing activity. Plain `this` refers to the dialog listener object.
- The builder can also set a neutral button, but the slide example uses only positive and negative buttons.
- The dialog appears in the middle by default; the slides show that its `window` can be moved to `Gravity.BOTTOM` after it is shown.

### 7. Reusable View and Controller

- `ButtonGridAndTextView` owns the buttons and status `TextView`. Its constructor receives a `Context`, sizing information, the number of buttons per side, and a `View.OnClickListener` to register on the buttons.
- It exposes methods such as `setButtonText`, `setStatusText`, `setStatusBackgroundColor`, `resetButtons`, `enableButtons`, and `isButton`. These let `MainActivity` work with the View without directly manipulating its private widgets.
- The reusable View uses its own `side` value instead of depending on `TicTacToe.SIDE`. That keeps it independent of the Tic Tac Toe Model.
- In Version 5, `MainActivity` is the Controller. It creates the Model and View, receives clicks, invokes Model methods, and tells the View what to display.

## Practice question bank

Try these before reading the key. The past quiz uses short prompts, several response types, and one-step code completion. These 28 new questions give **four questions to each of the seven slide topics**. For a 10-question timed run, choose at least one question from each topic and three more at random.

### Topic 1: MVC and game state

**1. Short answer.** Which class owns the 3 × 3 array recording whether each square is empty, X, or O? Give the class name only.

**2. Multiple choice.** A cell stores `2`. What does that mean in the slide example?

A. No player has used it.  
B. Player 1 has used it.  
C. Player 2 has used it.  
D. The game has ended.

**3. Yes or no.** Should the View decide whether a move completes a winning line?

**4. Short answer.** A click reaches the controller. Name the layer it should consult before changing a button label.

### Topic 2: Building the grid

**5. Multiple choice.** Why can `MainActivity` pass `this` to `Button(this)`?

A. `Button` extends `MainActivity`.  
B. An activity is a `Context`.  
C. `this` always means the screen width.  
D. `Button` has no constructor parameter.

**6. Complete one Kotlin line.** Set `gridLayout` to have `TicTacToe.SIDE` columns.

```kotlin
gridLayout.__________ = TicTacToe.SIDE
```

**7. Short answer.** What property after `Resources.getSystem().displayMetrics.` gives the screen width in pixels?

**8. Multiple choice.** What is the type of one element `buttons[1]` when `buttons` has type `Array<Array<Button>>`?

A. `Button`  
B. `Array<Button>`  
C. `Array<Array<Button>>`  
D. `IntArray`

### Topic 3: Click handling

**9. Short answer.** Which listener interface does a class implement to receive ordinary button clicks? Give the qualified interface name.

**10. Multiple choice.** A listener object `handler` exists. Which call registers it on `buttons[0][2]`?

A. `handler.addView(buttons[0][2])`  
B. `buttons[0][2].setOnClickListener(handler)`  
C. `buttons[0][2].onClick(handler)`  
D. `setContentView(handler)`

**11. Yes or no.** Can the click `View` parameter be left out of a listener lambda when the body only uses the captured `row` and `col` values?

**12. Short answer.** In the explicit listener version, what is compared with the `view` argument to identify the clicked board position?

### Topic 4: Rules and updates

**13. Multiple choice.** `ttt.play(row, col)` returns `2`. What should the controller put in that button?

A. `X`  
B. `O`  
C. `2`  
D. An empty string

**14. Yes or no.** If `ttt.isGameOver()` becomes true, should the controller disable the buttons and display `ttt.result()` in the status view?

**15. Complete one Kotlin line.** Disable `buttons[row][col]` using the property shown in the slides.

```kotlin
buttons[row][col].__________ = false
```

**16. Short answer.** The user starts a new game. Which Model method must run before fresh moves follow the new game's rules?

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

## Answer key

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

## Fast self-check

From memory, trace one click: `Button` → listener/controller → `ttt.play` → View text → `ttt.isGameOver()` → status and button state. Then trace replay: dialog YES → Model reset → View reset. If you can name the object responsible at each arrow, you understand the main design.
