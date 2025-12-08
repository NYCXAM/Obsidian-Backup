#### HTML:
- `<br>`  
    _Effect:_ Breaks the text to a new line  
    Example:
    ```html
    First line<br>Second line
    ```
    First line  
    Second line
    
- `<strong></strong>`  
    _Effect:_ Bold text  
    Example:
    ```html
    This is <strong>bold</strong> text.
    ```
    This is **bold** text.
    
- `<em></em>`  
    _Effect:_ Emphasized (italic) text  
    Example:
    ```html
    This is <em>italic</em> text.
    ```
    This is _italic_ text.
    
- `<h1></h1>`  
    _Effect:_ Creates a large header (like `# Heading` in markdown)  
    Example:
    ```html
    <h1>This is a heading</h1>
    ```
    
- `<div></div>`  
    Container element  
    _Function:_ Groups block-level content without any inherent styling.
    
- `<span></span>`  
    Inline container element  
    _Function:_ Groups inline content for styling or manipulation without affecting layout.
    
- `<p></p>`  
    Paragraph  
    _Effect:_ Defines a block of text with spacing before and after  
    Example:
    ```html
    <p>This is a paragraph.</p>
    ```
    
- `<pre></pre>`  
    Preformatted text block  
    Example:
    ```html
    <pre>
      Code block or text with
      preserved formatting.
    </pre>
    ```
    
- `<a>`  
    _Function:_ Creates a hyperlink.  
    Example:
    ```html
    <a href="https://example.com">Visit Example</a>
    ```
    
- `<img>`  
    _Function:_ Embeds an image.  
    Example:
    ```html
    <img src="image.jpg" alt="A description">
    ```
    
#### Additional Tags for Structuring HTML in the `<body>` Section
**Tables (Define tables):**
- `<table>`  
    _Function:_ Container for table data.
- `<tr>`  
    _Function:_ Defines a table row.
- `<th>`  
    _Function:_ Defines a table header cell.
- `<td>`  
    _Function:_ Defines a table data cell.

**Lists:**
- `<ol></ol>`  
    _Function:_ Creates an ordered (numbered) list.
- `<ul></ul>`  
    _Function:_ Creates an unordered (bulleted) list.
- `<li></li>`  
    _Function:_ Defines a list item within ordered or unordered lists.

**Forms (Define forms):**
- `<form></form>`  
    _Function:_ Encloses form elements. (Essential for reset functionality.)
- `<input type="text">`  
    _Function:_ Creates a text field.
- `<button>`  
    _Function:_ Creates a clickable button.
- `<input type="reset">`  
    _Function:_ Creates a reset button to clear form data.
- `<input type="submit">`  
    _Function:_ Creates a submit button to send form data.

- **Block vs. Inline HTML Elements**
    - **Block elements:**  
        Occupy the full available width, always start on a new line (e.g., `<div>`, `<p>`, `<h1>`).
    - **Inline elements:**  
        Only take up as much width as necessary and do not force line breaks (e.g., `<span>`, `<a>`, `<em>`).
        
- **CSS Properties (Responsible Only For):**
    - **color:** Sets the text color.
    - **font-size:** Specifies the size of the text.
    - **background-color:** Defines the background color of an element.
    - 
- **Including CSS in HTML:**
    - **Inline CSS:**  
        Applied directly within an element’s `style` attribute.
        
        html
        
        CopyEdit
        
        `<p style="color: red; font-size: 16px;">Text</p>`
        
    - **Internal CSS:**  
        Defined within a `<style>` block inside the `<head>` section.
        
        html
        
        CopyEdit
        
        `<style>   p { color: blue; font-size: 14px; } </style>`
        
    - **External CSS:**  
        Linked via a separate file using the `<link>` tag.
        
        html
        
        CopyEdit
        
        `<link rel="stylesheet" href="styles.css">`
        
- **CSS Selectors (Responsible Only For):**
    - **ID selectors:**  
        Target elements with a specific `id` (e.g., `#header { ... }`).
    - **Class selectors:**  
        Target elements with a specific `class` (e.g., `.menu { ... }`).
    - **Descendant selectors:**  
        Target elements nested inside other elements (e.g., `div p { ... }`).
        
- **Pseudo-classes for Links:**
    - **a:link:** Applies to unvisited links.
    - **a:visited:** Applies to links that have been visited.
    - **a:hover:** Applies when the pointer is over a link.
    - **a:active:** Applies when the link is being clicked.
    
- **Type Selectors:**  
    Target HTML elements by their tag name. For example:
    
    - `h1`, `h2` for headings
    - `p` for paragraphs
    - `table` for tables
    - `ol`, `ul` for ordered and unordered lists
      
- **Writing HTML Corresponding to an Image:**  
    You should be able to analyze a provided layout image (e.g., one with a table and a list) and then construct HTML using the appropriate tags (like `<table>`, `<tr>`, `<td>`, `<ol>`, `<ul>`, `<li>`, etc.).
    
- **Web Servers:**  
    Programs that serve web pages to clients over HTTP/HTTPS by hosting HTML, CSS, and JavaScript files.
    
- **URLs and URIs:**
        - **URL (Uniform Resource Locator):**  
        A specific type of URI that not only identifies a resource but also provides a means of locating it (includes protocol, domain, port, path).
    - **URI (Uniform Resource Identifier):**  
        A string that identifies a resource, which may be a URL or a URN.
        
- **HTTP POST and GET:**
    - **GET:**  
        Requests data from a server; parameters are sent via the URL query string.
    - **POST:**  
        Sends data to a server within the request body, often used for submitting form data.
        
- **Port Number in a URL:**  
    A numerical identifier that specifies the endpoint on a server for communication (e.g., `:80` for HTTP or `:443` for HTTPS). It is appended to the domain in a URL.
    
- **XAMPP and htdocs:**
    - **XAMPP:**  
        A local web server environment that packages Apache, MySQL, PHP, and Perl for development.
    - **htdocs:**  
        The folder in XAMPP where you place your web files so that they can be served by the Apache server.
        
- **Localhost and 127.0.0.1:**
    - **Localhost:**  
        A hostname that refers to the local computer.
    - **127.0.0.1:**  
        The loopback IP address for the local machine; both terms are used interchangeably in web development.
        
- **index.html vs. index.shtml:**
    - **index.html:**  
        A static HTML file that typically serves as the default landing page.
    - **index.shtml:**  
        Similar to index.html but processed for server-side includes (the “s” indicates server-side), allowing for dynamic content insertion before the page is sent to the client.
        
- **Server Side Includes:**  
    A technique used by web servers to dynamically insert the content of one file into another before delivering it to the client. It’s often used for shared components like headers or footers across multiple pages.

#### JavaScipt equations:
- **`==` (Abstract Equality)**
    - **Behavior:**  
        Compares two values for equality after performing type coercion.
    - **Example:**
        ```js
        0 == false;      // true, because false is coerced to 0
        "5" == 5;        // true, because "5" is coerced to the number 5
        null == undefined; // true, special coercion rule
        ```
        
    - **Pitfalls:**  
        Implicit type conversion can lead to unexpected results.
        
- **`===` (Strict Equality)**
    - **Behavior:**  
        Compares two values for equality without type coercion. Both the value and the type must be identical.
    - **Example:**
        
        ```js
        0 === false;     // false, because the types differ (number vs. boolean)
        "5" === 5;       // false, because one is a string and the other is a number
        null === undefined; // false
        ```
        
    - **Best Practice:**  
        Generally preferred to avoid ambiguity from type conversion.
        
- **Other Related Operators:**
    
    - **`!=` (Abstract Inequality):**  
        Works like `==` but checks for inequality with type coercion.
        
        ```js
        0 != false; // false, because 0 == false is true
        ```
        
    - **`!==` (Strict Inequality):**  
        Works like `===` but checks for inequality without type coercion.
        
        ```js
        0 !== false; // true, because strict equality fails due to different types
        ```
    
#### JavaScipt functions:
**Array:**
1. map -> same as ocaml map, but will return the updated map instead of updating the original map
   `const map1 = array1.map((x) => x * 2)`
2. forEach -> same as ocaml map
   `array.forEach(i=>document.writeln(i.name + "<br>"))`
3. filter -> pass in a boolean function, return the list of all element that is true from that function
   `const result = words.filter((word) => word.length > 6)`
3. reduce -> same as ocaml fold
```js
const sumWithInitial = array1.reduce(
  (accumulator, currentValue) => accumulator + currentValue,
  initialValue,
);
   ```
4. every -> tests if all elements pass the passed in function, return boolean
   `array1.every((currentValue) => currentValue < 40)`
5. some -> test if at least one element pass the function

**Set:**
1. add() -> add an element into a set
   `mySet.add(150).add(12)`
2. has() -> boolean of whether the element is in the set
   `mySet.has(150)`
3. size -> return the size of the set
   `let size = mySet.size`
4. delete() -> delete this element
   `mySet.delete(150)`
5. clear() -> clear all element
   `mySet.clear()`
5. Set based on array -> build a set based on the content in an array
```js
let arr = ["C", "JavaScript", "Pascal", "Pascal", "C", "Nava"];  
document.writeln(`Array: ${arr}<br>`);  
const setFromArray = new Set(arr);
//elements = C, JavaScript, Pascal, Pascal, C, Nava. Not just the array object
```

**Map (Similar to HashTable):**
1. set() -> creating association 
   `map.set("Mike", 88)`
2. get() -> finding value for key  
   `map.get("NotInMap")`
3. has() -> a key has a mapping? 
   `map.has("Mike")`
4. delete() -> remove a mapping (returns true if mapping found)
   `map.delete("Rose")`
5. size (property)
   `map.size`
6. keys() -> returns iterable for values in the map
```js
for (const key of map.keys()) {  
    document.writeln(key + "<br>");  
}
//Mike, Rose, Kelly, ...
```
7. values() -> returns iterable for values in the map
```js
for (const value of map.values()) {  
    document.writeln(value + "<br>");  
}
//120, 41, 101, ...
```
8. entries() -> returns iterable over (key, value) pairs
```js
for (const entry of map.entries()) {  
    document.writeln(`${entry[0]}, ${entry[1]}<br>`);  
}
```
**Weak map:**
1. **Key Type Restrictions:**
    - **WeakMap:**
        - Only allows objects as keys. Primitive values like strings or numbers cannot be used.
    - **Map:**
        - Accepts keys of any type, including primitives and objects.
2. **Garbage Collection:**
    - **WeakMap:**
        - Holds keys weakly. This means if there are no other references to an object used as a key, that key-value pair can be garbage collected. This is useful for caching or storing metadata without preventing the object from being cleaned up when it’s no longer needed.
    - **Map:**
        - Maintains strong references to keys. Even if the key object isn’t used elsewhere, the Map still holds a reference to it, which can prevent garbage collection.
3. **Enumeration and Size:**
    - **WeakMap:**
        - Does not support iteration methods (like `keys()`, `values()`, or `entries()`) and doesn’t have a `size` property. The non-enumerability is due to its design, which permits keys to be garbage collected at any time.
    - **Map:**
        - Provides full iteration capabilities and a `size` property, allowing you to easily loop through keys, values, or entries.

**Errors:**
Same java style try/catch:
```js
try {  
    alert(`About to access method`);  
    window.terps();  
} catch (error) {  
    alert(`Problem: ${error.message}`);  
}
```



## Exam2:

**Topics**

The exam will include all the material covered in lecture, and projects (#1, #2, #3 and #4) including the following topics:

- [ ] How to write the HTML that goes in the `<body></body>` section. You don't need to know the tags associated with the header (only the `<title>` tag).

- [ ] You are only responsible for HTML tags that allow us to:
    - [ ] Define tables.
    - [ ] Define ordered and unordered lists.
    - [ ] Define forms using text fields, buttons, reset, and submit buttons. You don't need to know any other form elements (e.g., radio buttons, etc.).
    - [ ] Other tags you should be familiar with:
        - [ ] `<br>`
        - [ ] `<strong></strong>`
        - [ ] `<em></em>`
        - [ ] `<h1></h1>`
        - [ ] `<div></div>`
        - [ ] `<span></span>`
        - [ ] `<p></p>`
        - [ ] `<pre></pre>`
        - [ ] `<img>`
        - [ ] `<a></a>`
        - [ ] `<img>`

- [ ] You are only responsible for the following CSS properties:
    - [ ] color
    - [ ] font-size
    - [ ] background-color

- [ ] You are only responsible for the following CSS selectors:
    - [ ] id selectors
    - [ ] class selectors
    - [ ] type (e.g., `h1`, `h2`, `p`, `table`) selectors

- [ ] **JavaScript**
    - [ ] Comments
    - [ ] Variable declarations using `let`, `const`, `var`
    - [ ] Primitive types
    - [ ] Reference types
    - [ ] Type conversions using `Number()`
    - [ ] Reading data using `prompt()`
    - [ ] Display results using `document.writeln/write` and `alert()`
    - [ ] Mathematical, logical and relational expressions
    - [ ] Conditional statements
    - [ ] Loop constructs - while, do while, for loops
    - [ ] `break`, `switch` statements
    - [ ] `"use strict";`
    - [ ] Only `console.log()` (ignore `console.info()`, `console.error()`, `console.warn()`, `console.table()`)
    - [ ] `isNaN()`
    - [ ] window object
    - [ ] Function definition (including arrow (lambda) functions)
    - [ ] One-dimensional array
    - [ ] How to use the String length property to determine the length of a string
    - [ ] String methods - how to use the following methods: `includes`, `trim`, `indexOf`, `toLowerCase`, `toUpperCase`, `chatAt`, `split`, `localeCompare`
    - [ ] Ternary Operator (`?:`) (e.g., `x > 1 ? true : false`)
    - [ ] `typeof`
    - [ ] `instanceof`
    - [ ] `for...of`
    - [ ] `for...in`
    - [ ] `null`
    - [ ] `undefined`
    - [ ] `NaN`
    - [ ] `Math.sqrt()`
    - [ ] `Math.random()`
    - [ ] Two-dimensional arrays
    - [ ] Template literals
    - [ ] Sorting
    - [ ] Default parameters
    - [ ] Rest operator
    - [ ] Spread operator
    - [ ] Destructuring
    - [ ] What is ECMAScript
    - [ ] What is a JavaScript engine
    - [ ] Function execution context
    - [ ] JavaScript sets and maps
    - [ ] Freezing objects
    - [ ] Sealing objects
    - [ ] Preventing objects from been extended
    - [ ] Chaining (?) Operator
    - [ ] `&&`, `||`, and `??` operators
    - [ ] JSON - how to use `JSON.parse()` and `JSON.stringify()`. You are not responsbile for writing JSON that corresponds to a provided object description.
    - [ ] Array Methods: how to use the following methods: `join`, `pop`, `push`, `reverse`, `forEach`, `find`, `findIndex`, `map`, `every`, `some`, `reduce`
    - [ ] IIFE
    - [ ] Custom Type Definition - Be prepare to define "classes" using the "Default Pattern for Custom Type Definition" (you cannot use **`class, extends, super`**). The following is an example of defining custom types: [DefaultPatternCustomTypeDefinition.html.txt](https://www.cs.umd.edu/class/spring2025/cmsc335/prot/exams/exam2/DefaultPatternCustomTypeDefinition.html.txt).
    - [ ] Defining classes using **`class, extends, super`**. The following is an example: [SportsCar.html.txt](https://www.cs.umd.edu/class/spring2025/cmsc335/prot/exams/exam2/SportsCar.html.txt).
    - [ ] You should be prepare to draw diagrams (like we did in lecture) that shows the relationship between constructor function objects and prototype objects
    - [ ] JavaScript closures
    - [ ] Currying
    - [ ] Context object and arrow functions
    - [ ] How to use the following methods: `call`, `apply` and `bind`
    - [ ] Accessing function arguments using the **`arguments`** object
    - [ ] Errors
        - [ ] How to throw and catch errors
        - [ ] How to define error classes
    - [ ] How to use the following methods: `parseInt`, `parseFloat`
    - [ ] Remember that a function can return different kinds of values (e.g., it can return a number or a string)

- [ ] `fetch` (only GET requests)

- [ ] How to use `await` and `async` when using `fetch()`

- [ ] HTTP post and get

- [ ] What is localhost and `127.0.0.1`

- [ ] Event-driven programming

- [ ] Recognizing the `onclick` and `submit` events. The submit event as seen when validating form data.

- [ ] DOM functions - `document.getElementById()`, `document.querySelector()`

- [ ] How to retrieve a value from a text field

- [ ] Associating a function with an event (i.e., adding listeners using `addEventListener`, `onclick=`)

- [ ] Accessing/modifying attributes using `getAttribute( )` and `setAttribute()`

- [ ] Modifying a page using `innerHTML`

- [ ] Loading JavaScript from a file (i.e., `<script src="">`) and the defer attribute

- [ ] `setInterval`, `setTimeout` and `clearInterval` functions

- [ ] How to use `window.onsubmit` to control submission of form data to a server

- [ ] Event-Loop

- [ ] Recursion (Important: You will get 0 credit if we ask for a recursive implementation and you use any kind of loops in the implementation)

- [ ] `window.confirm()`

- [ ] **The exam will NOT cover the following topics:**

- [ ] Ajax

- [ ] Local Storage

- [ ] Canvas

- [ ] Chrome Debugging (debugger and debugger statement)

- [ ] PHP

- [ ] XAMMP - What is htdocs

- [ ] Server side includes

- [ ] FileReader API

- [ ] Sound generation

- [ ] Node.js

- [ ] Express

- [ ] SQL

- [ ] CORS

- [ ] Event Propagation Phases (capturing and bubbling)

- [ ] Weakmaps

- [ ] Sessions

- [ ] Cookies
      

```js
function WirelessSpeaker(diameter, maxDb, range){
	Speaker.call(this, diameter, maxDb);
	this.range = range;
}

WirelessSpeaker.prototype = new Speaker();
WirelessSpeaker.prototype.constructor = Speaker;
WirelessSpeaker.prototype.getRange = function(){
	return this.range;
}

WirelessSpeaker.prototype.info = function() {
	return Speaker.prototype.info.call(this) + ", Range:" this.range;

```


```js
class Sign{
	static #totalSigns = 0;
	#message;
	constructor(message){
		this.#message = message;
		this.#totalSigns++;
	}

	[Symbol.toPrimitive()] {
		return `Message: ${this.#message}`;
	}
	set message(newMessage){
		this.#message = newMessage;
	}

	get message(){
		return this.#message;
	}

	static getTotalSigns(){
		return Sign.#totalSigns;
	}
}


class LedSign extends Sign {
	#ledNumber;
	constructor(message, ledsNumber){
		super(message);
		this.#ledNumber = ledNumber;
	}

	[Symbol.toPrimitive](){
		return super[Symbol.toPrimitive]() + `. LedNumbers: ${this.#ledsNumber}`
	}
}

	get ledsNumer(){
		return this.#ledNumbers;
	}
}
		
```


Spring 2023
```json
{"gate" = "21 A", "nonstop" = "true", "duration" = "1.5"}
```

```js
function Bed(model, weight){
	this.model = model;
	this.weight = weight;
}

Bed.prototype = {
	constructor = Bed,

	setModel: function(model="BASIC"){
		this.model = model;
	}

	info: function(){
		return 'Model: ${this.model}, Weight: ${this.weight}`;
	}
}

function WaterBed(model, weight, gallons){
	Bed.call(this, model, weight);
	this.gallons = gallsons;
}

WaterBed.prototype = new Bed();
WaterBed.prototype.constructor = WaterBed;
WaterBed.prototype.getGallons = function(){
	return this.gallons;
}
```


```js
class Door{
	static #totalDoors = 0;
	#make;
	#area;
	
	constructor(make, area){
		this.#make = make;
		this.#area = area;
		this.#totalDorrs ++;
	}

	info(){
		document.writeln(`Make: ${this.#make}, Area: ${this.#area}`);
	}

	[Symbol.toPrimitive](){
		return this.#make + "," + this.#area;
	}

	set make(newMake){
		this.#make = newMake;
	}

	get make(){
		return this.#make;
	}

	static getTotalDoor(){
		return Door.#totalDoors;
	}
}

class ElectricDoor extends Door{
	#voltage;
	constructor(make, area, voltage){
		super(this, make, area);
		this#voltage = voltage;
	}

	info(){
		super.info();
		doc.writeln(...)
	}
}

```
