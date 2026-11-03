# CMSC436 Quiz 3 Study Guide: Mortgage and Duck Hunting

**Scope:** Every topic in the Mortgage app, including activity lifecycle methods, and the Duck Hunting app. Thread concepts and accessing a server are excluded. The Duck Hunting `Timer`/`TimerTask` scheduling API is included because it appears in the app and practice quiz; thread implementation and synchronization are outside this guide.

**How to use this guide:** Read the review, then try the questions without looking at the answer key. The practice quiz uses short Kotlin/XML completions, method names, multiple choice, and small calculations. The questions below use that style but cover more of the stated scope than the sample alone.

**Sources:** `4-Mortgage.pptx`, `6-GraphicsAndAnimation.pptx`, and `CMSC436.md` for content; `Quiz3_Practice_KEY.pdf` as a style reference. Example callback sequences assume the normal activity transitions described in the slides; Android can recreate activities or terminate processes under other circumstances.

## Review: Mortgage app

### 1. Screens, layouts, and the model

- `MainActivity` displays the current mortgage values. `DataActivity` lets the user edit them. The `Mortgage` model stores data and calculates results; the activities handle the UI.
- `TableLayout` directly extends `LinearLayout`. Its `TableRow` children arrange controls in rows; a thin `View` with a background color can act as a separator.
- A `RadioGroup` makes its `RadioButton` children mutually exclusive. `rb15.isChecked = true` selects that button; `if (rb15.isChecked)` tests it.
- Assign an ID in XML with `android:id="@+id/amount"`, then retrieve the view after setting the content view with `findViewById(R.id.amount)`.
- In the Mortgage example, `DataActivity.updateView()` displays model values and selects the appropriate radio button. `updateMortgageObject()` reads input, converts text to numeric values, and updates the model **before** returning. Invalid or blank numeric input can throw `NumberFormatException`.

### 2. Starting an activity and returning

```kotlin
// MainActivity: navigate to the second screen
val intent = Intent(this, DataActivity::class.java)
startActivity(intent)

// DataActivity: update the shared model, then return
updateMortgageObject()
finish()
```

- An explicit `Intent` identifies the current context and target activity class. `startActivity()` places the new activity on top of the usual activity stack; `finish()` closes the current activity and reveals the previous one.
- Declare the second activity inside the `<application>` element of `AndroidManifest.xml`: `<activity android:name=".DataActivity" />`.
- The app shares one in-memory model using a companion object:

```kotlin
class MainActivity : AppCompatActivity() {
    companion object {
        lateinit var mortgage: Mortgage
    }
}
// Access from DataActivity:
val model = MainActivity.mortgage
```

`lateinit` must be initialized before access. A companion object shares an in-memory reference in the current process; it is **not persistent storage** and cannot be relied on after process death. An `Intent` extra is another way to pass supported data.

### 3. Activity lifecycle and returning to MainActivity

| Callback | Role |
| --- | --- |
| `onCreate()` | Initialize a newly created activity instance and its UI. |
| `onStart()` | Activity becomes visible; the Mortgage app refreshes the display here. |
| `onResume()` | Activity enters the resumed/foreground interactive state. |
| `onPause()` | Activity leaves the resumed state. |
| `onStop()` | Activity is no longer visible. |
| `onRestart()` | A stopped activity is about to start again. |
| `onDestroy()` | An activity instance is being destroyed, for example after finishing or during recreation. |

Typical sequences:

```text
First creation:                   onCreate → onStart → onResume
Fully covered / backgrounded:     onPause → onStop
Return to a stopped instance:     onRestart → onStart → onResume
Finish a resumed instance:        onPause → onStop → onDestroy
```

An activity can be paused while still visible, so `onPause()` does not always imply `onStop()`. `onCreate()` runs once **per instance**, not once for the entire lifetime of the app: configuration changes can destroy and recreate an instance. Process termination does not guarantee `onDestroy()` will run.

In the slide's normal return path, MainActivity remains in the stack but was stopped. `onStart()` runs again, so refreshing the values there shows edits made by DataActivity:

```kotlin
override fun onStart() {
    super.onStart()
    updateView()
}
```

### 4. Animations, transitions, and persistent data

- **View animation:** Put XML such as `slide_from_left.xml` in `res/anim/`. Tags include `<translate>`, `<alpha>`, `<scale>`, `<rotate>`, and `<set>`. Call `overridePendingTransition(R.anim.slide_from_left, 0)` immediately after starting or finishing an activity; arguments are **enter animation, then exit animation**. `0` means none.
- **Scene transition:** Put transition XML in `res/transition/`, using tags such as `<transitionSet>`, `<fade>`, `<slide>`, or `<explode>`. Configure window transitions in the theme, start with `ActivityOptions.makeSceneTransitionAnimation(this).toBundle()`, and return with `finishAfterTransition()`.
- These are the two techniques taught in the slides. Some activity-transition APIs are deprecated on newer Android versions; use the lecture syntax for this course's questions.
- `SharedPreferences` stores local key/value data beyond the current activity instance and normal app restarts. `getSharedPreferences(name, Context.MODE_PRIVATE)` opens a named preferences file. `getInt`/`getFloat` return the supplied default when the key is absent.

```kotlin
val pref = getSharedPreferences("mortgage", Context.MODE_PRIVATE)
val years = pref.getInt("years", 30)
val amount = pref.getFloat("amount", 100000f)

val editor = pref.edit()
editor.putInt("years", years)
editor.putFloat("amount", amount)
editor.apply() // or editor.commit()
```

`apply()` updates the in-memory preferences immediately and writes asynchronously; `commit()` writes synchronously and returns a Boolean success result. Calling `put...` without `apply()` or `commit()` does not finish the edit. Private preferences do not require external-storage permission.

## Review: Duck Hunting app

### 5. Custom View and graphics

- The **model** stores the cannon, bullet, duck, positions, speeds, and game state. `GameView : View` draws the game from that state; `MainActivity` coordinates input and updates. The activity and view must refer to the same model.
- `Paint` controls **how** to draw: `color`, `strokeWidth`, `style`, `textSize`, and `isAntiAlias`. `Canvas` supplies operations for **what and where** to draw: `drawCircle`, `drawLine`, `drawRect`, `drawText`, and `drawBitmap`.
- Override `onDraw(canvas: Canvas)` in the custom view. Android supplies the `Canvas`; request a redraw with `invalidate()` on the UI thread or `postInvalidate()` when requesting from elsewhere. A redraw is scheduled, not necessarily immediate.
- The view's coordinate origin is top-left. X increases right and Y increases down. A circle centered at `(0f, height.toFloat())` is centered at the bottom-left corner; only its upper-right quarter is visible.

```kotlin
paint.color = Color.RED
paint.strokeWidth = 10f
paint.isAntiAlias = true
canvas.drawCircle(100f, 200f, 30f, paint)
canvas.drawLine(0f, height.toFloat(), 100f, height - 100f, paint)
```

- Canvas drawing arguments are often `Float` (`30f`); dimensions and `Rect` coordinates are commonly `Int`.
- Screen dimensions from `resources.displayMetrics` and the actual measured view dimensions are not necessarily identical when bars/insets are present. The slides hide system bars or measure visible bounds after layout, then scale game objects relative to the available space.

### 6. Bitmap, Rect, and sprite frames

```kotlin
// duck.png is in res/drawable/
val duck = BitmapFactory.decodeResource(resources, R.drawable.duck)
val duckRect = Rect(50, 100, 250, 180)
canvas.drawBitmap(duck, null, duckRect, paint)
```

- `Rect(left, top, right, bottom)` describes **edges**, so the example is 200 pixels wide and 80 high. It does not take width and height as its last two arguments.
- In `drawBitmap(bitmap, src, dest, paint)`, `src = null` selects the whole bitmap; `dest` chooses its position and drawn size. Preserve the image's width/height ratio by applying the same scale to both dimensions.
- `bitmap.width`, `bitmap.height`, `getPixel(x, y)`, and `setPixel(x, y, color)` access size or pixel data; writing requires a mutable bitmap.
- The flying duck cycles through four displayed frames made from three images: `0, 1, 2, 1`. With a four-element bitmap array, advance with `duckFrame = (duckFrame + 1) % ducks.size`. Movement of its rectangle and cycling its image are different parts of animation.

### 7. Game loop, aiming, and collision

- The lecture schedules a `TimerTask`; its abstract `run()` callback updates the model and requests a view refresh. `timer.schedule(task, delay, period)` uses **milliseconds**, and the parameters are `Long` values in the examples.
- Convert frames/updates per second to period with `1000 / frequency`. For example, 25 updates/s means `40L` ms; `timer.schedule(task, 0L, 40L)` starts without an initial delay.
- Each model update moves the duck, moves a fired bullet, reloads an off-screen bullet, and restarts the duck when appropriate. The view uses model state to draw; use `postInvalidate()` to request the next draw.
- For a cannon centered at `(cx, cy)` with barrel length `L` and angle `θ`, its endpoint is `(cx + L*cos(θ), cy - L*sin(θ))`. The minus is needed because screen Y increases downward. Trigonometric functions use radians.
- To aim using a touch in the same coordinate space: `dx = event.x - center.x`, `dy = center.y - event.y`, then `Math.atan2(dy.toDouble(), dx.toDouble())`. Convert between coordinate spaces when necessary.
- Fire when a bullet is not already active. On a hit, update shot state, play the hit sound, and reload the bullet; the shot duck falls and uses a fixed image until it leaves the screen and a new duck starts.

### 8. Touches, gestures, and SoundPool

- `event.x`/`event.y` give view-local coordinates; `event.rawX`/`event.rawY` give screen coordinates. `ACTION_DOWN`, `ACTION_MOVE`, and `ACTION_UP` distinguish initial touch, movement, and release.
- Register a `View.OnTouchListener` with `view.setOnTouchListener(listener)`. A nullable `MotionEvent?` must be checked before accessing its properties.
- `GestureDetector.SimpleOnGestureListener` is a class with default callbacks. Common callbacks: `onDown`, `onScroll`, `onSingleTapConfirmed`, `onDoubleTap`, and `onDoubleTapEvent`. The last can receive multiple events during a double tap, including down and up; guard against firing repeatedly.
- `GestureDetector.OnDoubleTapListener` has **three** methods: `onSingleTapConfirmed`, `onDoubleTap`, and `onDoubleTapEvent`. Confirmed single tap excludes the first tap of a double tap. Returning `true` from `onDown` helps the detector handle the remaining gesture.

```kotlin
val handler = TouchHandler()
detector = GestureDetector(this, handler)
detector.setOnDoubleTapListener(handler)
// Forward received MotionEvents to detector.onTouchEvent(event).
```

- `SoundPool.Builder().build()` creates the pool. Files in `res/raw/` are referenced as `R.raw.name`, without the extension.
- `load(context, resourceId, priority)` returns a **sound ID**; wait for loading to finish. `play(soundId, leftVolume, rightVolume, priority, loop, rate)` returns a **stream ID**. For `loop`, `0` plays once, `1` twice, and `-1` indefinitely. Rate `1f` is normal; volume `1f` is full. Release the pool when finished.

## Practice questions

Try each independently. For code completions, assume the named variables exist and imports are available unless otherwise stated.

### Mortgage app and lifecycle

**1.** In first-time creation of an activity, list the three callbacks leading to the resumed state in order.

**2.** A fully stopped activity instance returns to the foreground without being recreated. List the three callbacks in order.

**3.** An activity becomes paused. Must it immediately be stopped? Explain in one sentence.

**4.** Name the callback the Mortgage slides use to refresh `MainActivity` when it becomes visible again. Why is using only `onCreate()` insufficient on the ordinary return path?

**5.** In `MainActivity`, write two statements that create an `Intent` named `intent` for `DataActivity` and start it.

**6.** In `DataActivity`, write two statements to update the model by calling `updateMortgageObject()` and then return to the previous activity.

**7.** In which file and inside which XML element should `.DataActivity` be declared? Write the activity element.

**8.** Declare a shared `lateinit var mortgage: Mortgage` inside `MainActivity`. Show how `DataActivity` accesses it.

**9.** Check all true statements about the shared `mortgage` reference:

- A. Both activities can refer to the same object while the process remains alive.
- B. A companion-object property automatically persists after process death.
- C. Accessing a `lateinit` property before initialization fails at runtime.
- D. An `Intent` can carry supported extra data between activities.

**10.** `TableLayout` directly extends which class? What is the purpose of a `TableRow`?

**11.** `rb10`, `rb15`, and `rb30` are radio buttons inside one `RadioGroup`. Write one statement to select `rb15`; write a condition that checks whether it is selected.

**12.** `amountET` is an `EditText`. Obtain its text as a `String`, convert it to `Float`, and assign it to `amount`, assuming the input is valid.

**13.** What exception can `"".toFloat()` throw? Give one way the app can handle invalid input.

**14.** Open a private `SharedPreferences` file named `"loan"` in an activity and assign it to `pref`.

**15.** `pref` is `SharedPreferences`. Read `"years"` as an `Int`, defaulting to 15, and `"rate"` as a `Float`, defaulting to `0.04f`.

**16.** `pref` and an `Int` named `years` exist. Store `years` under `"term"` and complete the edit.

**17.** What is the key difference between `apply()` and `commit()`? Does an uninitialized companion object provide persistence?

**18.** `startActivity()` was just called. The entering animation is in `res/anim/enter.xml`; use no exit animation. Write the next statement.

**19.** Match each resource directory with its approach: `res/anim/` and `res/transition/`. Name one XML tag for each.

**20.** Write the expression that creates the options bundle for `startActivity(intent, options)` using a scene transition. Which method finishes with the corresponding scene transition?

**21.** An activity is destroyed and recreated after a configuration change. Can the new instance's `onCreate()` run even though an earlier instance already ran it? Is `onDestroy()` guaranteed if the OS kills the process?

### Duck Hunting app

**22.** Fill in the blanks: `_____` specifies drawing color and stroke width; `_____` provides `drawCircle()` and `drawBitmap()`.

**23.** Write the complete header for the overridden drawing callback in `GameView`, including the `Canvas` parameter and opening brace.

**24.** A custom view uses a secondary constructor with parameters `context: Context`, `width: Int`, and `height: Int`. Write the constructor header that passes `context` to `View`.

**25.** `paint` already exists. Set its color to blue, stroke width to 12 pixels, and enable antialiasing.

**26.** Draw a circle centered at `(120, 240)` with radius 35, using `canvas` and `paint`.

**27.** The view has height `600`. Where is its bottom-left corner? Which quarter of a circle centered there is visible?

**28.** Write a call to draw a line from `(0, height)` to `(120, height - 80)`, where `height` is an `Int`.

**29.** `robot.png` is in `res/drawable`. Decode it to a bitmap named `robot` inside an activity using `resources`.

**30.** Create `Rect r` with top-left `(20, 30)`, width 160, and height 90.

**31.** `robot` is a `Bitmap`, `r` a destination `Rect`, and `canvas`/`paint` exist. Draw the **whole** bitmap inside `r`.

**32.** The original bitmap is 400 × 200 pixels. If drawn 120 pixels wide without distortion, what height should it have? Show the scaling factor.

**33.** A bitmap array has four elements ordered `[image0, image1, image2, image1]`. Write a statement to advance `frame` cyclically.

**34.** What method should `MainActivity.updateView()` call on `gameView` to request a redraw? Does it call `onDraw()` synchronously?

**35.** Complete the required `GameTimerTask` callback. `activity` supplies `updateModel()` and `updateView()`; update the model first.

**36.** Given `timer` and `task`, schedule the task with a 2-second initial delay at 20 updates per second. State the period in milliseconds.

**37.** `event: MotionEvent` is non-null. Write expressions for the screen Y coordinate, view-local Y coordinate, and the condition for `ACTION_DOWN`.

**38.** `MainActivity` implements `View.OnTouchListener` and `target` is a `View`. Write one statement registering the activity as its touch listener.

**39.** Name the three methods in `GestureDetector.OnDoubleTapListener`. Which callback is appropriate for an unambiguous single tap?

**40.** `TouchHandler` extends `GestureDetector.SimpleOnGestureListener`; `detector` is a `GestureDetector`. Create `handler`, initialize `detector`, and register `handler` for double taps. What should the activity/view do with incoming touch events?

**41.** In the slides, the cannon center is `(0, 600)`, and the touch is `(300, 300)` in the same view coordinates. Calculate `dx` and `dy` used by `atan2(dy, dx)`. Why is `dy` computed as center Y minus touch Y?

**42.** `SoundPool` question: create `pool` with its builder; load `hit.wav` from `res/raw` to `hitId`; after loading, play it once at full volume and normal rate. What do the `load` and `play` return values identify?

**43.** In `SoundPool.play`, what do `loop = -1`, `loop = 0`, and `loop = 1` mean? What does `rate = 1f` mean?

**44.** The duck is hit. Name three model/view/sound actions the app should take. Why should the sound not play repeatedly on every update?

## Answer key

### Mortgage app and lifecycle

**1.** `onCreate()` → `onStart()` → `onResume()`.

**2.** `onRestart()` → `onStart()` → `onResume()`. This assumes the same stopped instance is reused.

**3.** No. For example, it can lose the resumed state while remaining visible; `onStop()` happens when it is no longer visible.

**4.** `onStart()`. On ordinary return to the existing stopped instance, `onCreate()` does not run again, but `onStart()` does.

**5.**
```kotlin
val intent = Intent(this, DataActivity::class.java)
startActivity(intent)
```

**6.**
```kotlin
updateMortgageObject()
finish()
```
Update first so the previous screen can read the new model values.

**7.** `AndroidManifest.xml`, inside `<application>`:
```xml
<activity android:name=".DataActivity" />
```

**8.**
```kotlin
// Inside MainActivity:
companion object {
    lateinit var mortgage: Mortgage
}
// Inside DataActivity:
val model = MainActivity.mortgage
```

**9.** A, C, and D. B is false: an in-memory companion property is not durable storage.

**10.** `LinearLayout`; a `TableRow` groups child views into a row in the table.

**11.** `rb15.isChecked = true`; condition: `if (rb15.isChecked)`. The group unchecks the other radio buttons when one is selected.

**12.**
```kotlin
val amountText = amountET.text.toString()
val amount: Float = amountText.toFloat()
```

**13.** `NumberFormatException`. Check/validate the text, use `toFloatOrNull()`, or catch the exception and handle the error.

**14.** `val pref = getSharedPreferences("loan", Context.MODE_PRIVATE)`.

**15.** `val years = pref.getInt("years", 15)` and `val rate = pref.getFloat("rate", 0.04f)`. Defaults apply if the key is missing.

**16.**
```kotlin
val editor = pref.edit()
editor.putInt("term", years)
editor.apply() // commit() is also acceptable
```

**17.** `apply()` schedules the disk write asynchronously; `commit()` writes synchronously and returns success/failure. No: a companion object exists only in process memory.

**18.** `overridePendingTransition(R.anim.enter, 0)`.

**19.** `res/anim/`: view animation, such as `<translate>` or `<alpha>`. `res/transition/`: scene transition, such as `<slide>` or `<fade>`.

**20.** `ActivityOptions.makeSceneTransitionAnimation(this).toBundle()`; call `finishAfterTransition()` when returning.

**21.** Yes, `onCreate()` runs for the new activity instance. No, process termination does not guarantee `onDestroy()`.

### Duck Hunting app

**22.** `Paint`; `Canvas`. Paint controls appearance, while Canvas drawing calls use it to render.

**23.** `override fun onDraw(canvas: Canvas) {`.

**24.** `constructor(context: Context, width: Int, height: Int) : super(context) {` in a class declaration using secondary-constructor syntax.

**25.**
```kotlin
paint.color = Color.BLUE
paint.strokeWidth = 12f
paint.isAntiAlias = true
```

**26.** `canvas.drawCircle(120f, 240f, 35f, paint)`.

**27.** `(0, 600)`. The visible portion is the circle's upper-right quarter.

**28.** `canvas.drawLine(0f, height.toFloat(), 120f, height - 80f, paint)`.

**29.** `val robot: Bitmap = BitmapFactory.decodeResource(resources, R.drawable.robot)`. Resource names omit `.png`.

**30.** `val r = Rect(20, 30, 180, 120)`: right is `20 + 160`; bottom is `30 + 90`.

**31.** `canvas.drawBitmap(robot, null, r, paint)`. A null source rectangle selects the whole bitmap.

**32.** Scale is `120 / 400 = 0.3`; height is `200 × 0.3 = 60` pixels.

**33.** `frame = (frame + 1) % ducks.size`, assuming `ducks` is the four-element array. If the array is named differently, use that array's size.

**34.** `gameView.postInvalidate()`. No: it requests a future redraw; Android calls `onDraw()` when appropriate.

**35.**
```kotlin
override fun run() {
    activity.updateModel()
    activity.updateView()
}
```

**36.** `1000 / 20 = 50` ms. `timer.schedule(task, 2000L, 50L)`.

**37.** Screen Y: `event.rawY`; view-local Y: `event.y`; condition: `event.action == MotionEvent.ACTION_DOWN`.

**38.** `target.setOnTouchListener(this)`.

**39.** `onSingleTapConfirmed`, `onDoubleTap`, and `onDoubleTapEvent`. Use `onSingleTapConfirmed` for a single tap that is not part of a double tap.

**40.**
```kotlin
val handler = TouchHandler()
detector = GestureDetector(this, handler)
detector.setOnDoubleTapListener(handler)
```
Forward the event stream with `detector.onTouchEvent(event)`, checking null first if the callback's parameter is nullable.

**41.** `dx = 300 - 0 = 300`; `dy = 600 - 300 = 300`. Screen Y increases downward, while the upward direction used for the cannon angle is positive.

**42.**
```kotlin
pool = SoundPool.Builder().build()
hitId = pool.load(this, R.raw.hit, 1)
// Once loading succeeds:
val streamId = pool.play(hitId, 1f, 1f, 1, 0, 1f)
```
`load` returns a sound/sample ID; `play` returns a stream ID for that playback, or zero on failure.

**43.** `-1` loops indefinitely, `0` plays once, `1` plays twice. `rate = 1f` is normal playback speed.

**44.** Mark the duck shot, play the hit sound once, and reload the bullet; then show the fixed duck image and move the duck downward. Play on the state transition to avoid restarting the sound each update while the duck remains shot.
