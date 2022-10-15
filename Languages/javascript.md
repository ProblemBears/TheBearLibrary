<h1 align="center"> JavaScript </h1>

<h2 align="center"> Table of Contents </h2>

[Functions](#Functions)

<h2 align="center"> Functions </h2>

- There are three ways to make functions -
	1. `let someVar = function(params){}`
	2. `let someVar = (params) => {}`  
        * If one parameter, then you don't need paranthesis 
        * If one statement, no return keyword and braces
	3. `function someVar(params){}` 
        * Can be placed anywhere unlike the other two which have to preceed usecase

<h2 align="center"> String Methods </h2>

- `.length`
- `.toUpperCase`
- `.toLowerCase`
- `.slice(e1, e2)` & `indexOf(e)` : read below
- `.trim()` : removes whitespace from the start/end of strings (spaces, newlines, tabs)
- `.padStart(i, c)` : i is the desired padding length; c is the character to be padded
- `.split(s)` & `.join(a)` : make an array based on the cutting string s; rejoin the array to a string w/a joining each string
- `.repeat(i)` : repeat the string i times
- `.includes(val)` : true/false if val is included in the array
- **Code Units** : 16 bit numbers (Strings are encoded as a sequence of code units)
- **UTF-16** : uses ONE CODE UNIT for the most common characters and TWO UNITS for others
    * This causes problems when one thing uses only one unit and another uses two. BUT TWO IS BECOMING MORE COMMON NOW WITH EMOJIS
    * OperaTIONS LIKE .length and [] DEAL ONLY WITH 1 CODE UNITS
    * someString.charCodeAt(index) - gives you a code unit not a full character code
    * someString.codePointAt(index) - does give a full unicode character (BUT THE INDEX STILL SPEAKS IN CODE UNITS SO WE HAVE TO DEAL WITH IT)
    * for(let char of someString) DEALS WITH THE INDEX PROBLEM - it iterates over characters not code units

<h2 align="center"> Array Methods  </h2>

- `.push(arg)`
- `.pop()`
- `.includes(arg)` - check whether arg exists in the array
- `for loops` in Modern JS can also be done with - `for(let someVar of someArray){}`
- `.shift()` - removes from the start of the array
- `.unshift(e)` -  adds to the start of the array
- `.indexOf(e)` - returns the index of the value e or -1 if not found
- `.lastIndexOf(e)` - same as `indexOf(e)` but searches from the back of the array first to the start 
	* Insert a 2nd argument for both to define the starting index
- `.slice(inclusiveIndex, exclusiveIndex)` - It returns a subarray between these parameters
- `.concat(someArray)` - Concatanates two arrays
- FOR NEXT FOUR SEE HIGH ORDER FUNCTIONS :
    - `.filter(f)`
    - `.map(f)`
    - `.reduce(f)`
    - `.foreach(e => {//Do something w/ e possibly}`
    - `.some(e => return true/false)` - Like Locial OR (||) for arrays
    - `.every(e => return true/false)` - Like Logical AND (&&) for arrays

<h2 align="center"> Rest Parameters </h2> 

- `...` operator has **TWO USES** - 
	1. To define a rest parameter -
		```js
		function max(...numbers)
		{
		   for(let i of numbers){} 
		}
		```
		- Notice that `...numbers` treated like an array. That's because arguments at this paramater are put to an array
	2. To spread
		- Spreading in an argument (here we spread at our Rest Paramter) -
			```js
			let numbers = [5,1,7];
			console.log(max(...numbers));  // spreads the elements of the array to act as arguments
			```
		- Spreading in an array
			```js
			["will", ...numbers, "understand"] //Puts elements of the array into this array
			```

<h2 align="center"> Math Object </h2>

- `max`, `min`, `sqrt`, `cos`, `sin`, `tan`, `PI`, `floor`, `random`, `round`, `abs`

<h2 align="center" > Destructuring </h2>

- Sometimes it's neater to have bindings for elements or objects
- **For Arrays** - wrap an array binding with `[]`'s and put names for each element :
	* Instead of `function phi(table)`, we do, `function phi([n00, n01, n10, n11])`
	* So instead of having to use `table[i]` we simply use the names `n00`, `n11`, etc...
- **For Objects** - wrap a binding with `{}` and define names :
	* `let name  = {name: "Faraji", age: 23};`
	* `console.log( {name} )` - Faraji is output

<h2 align="center" > JSON </h2>

- **Serializing** data means converting it to a flat description
- JSON notation is very similar to JS. Except for -
	1. All property names have to be surrounded by double quotes
	2. Only simple data expressions are allowed - no function calls, bindings, or anything that involves computation
	3. Comments are not allowed
- Javascript gives us the functions -
	- `JSON.stringify(e)` - serialize data such as an object
	- `JSON.parse(s)` - convert a JSON string back into data

<h2 align="center" > HIGH ORDER FUNCTIONS  </h2>

- **High order functions** are functions that - 
	1. Have functions as arguments  
	**OR** 
	2. Return a function

- `someArray.filter(e => e.someProp == "someValue")` - 				
	* This should return a new array that was filtered by the old one based on func
- `someArray.map(e => s.name)` - 							
	* Transforms an array by applying a function to all elements; building new Array from each return.

- `someArray.reduce( (curr, incrementer) => curr + incrementer, someStart)` - 
	* Compute a single value from every element in the array (ex. `sum()`)
	* `reduce` array method doesn't need `someStart` if there's atleast 1 element in the array (start by default)

<h2 align="center" > PROTOTYPES </h2>

- Fallback (like inheritance)
- Types of prototypes - 
	* `Object.prototype`, `Array.prototype`, `Function.prototype`
- Gets the protoype of a value - 
	* `Object.getPrototypeOf(someArrObjOrFunc)`
- Assigns a protoype to an instance... If null no protoype
	* `let rabbit = Object.create(protoRabbit)`
- Safeguard against accessing prototype properties
	* `someObject.hasOwnProperty(propertyName)`
	
<h2 align="center"> CLASSES </h2>

- Defining a class -
	```js
	class Rabbit{
		constructor(someParam){}
		someMethod(){} \
		someMethod()
		/*etc...*/}
	```
	* The methods get packed into the `constructor()`

- Constructing a new instance - 
	```js
	let killeRabbit = new Rabbit(someArg);
	```

<h2 align="center"> HASH MAPS & SETS </h2>

- JavaScript comes with a class called `Map` -
	* `let map = new Map();` - To initialize the Hash Map
	* `map.set(key, val);`
	* `map.get(key);`
	* `map.has(key);`
- JavaScript also has a class called - `Set`

<h2 align="center"> POLYMORPHISM </h2>

- When a piece of code is written to work w/ objects that have a certain interface
- **PREREQ** : learn about `Symbols` ( unique strings for object properties )

<h2 align="center"> SYMBOLS </h2>

- Symbols are used to create unique identifiers for properties in objects
- To create a symbol -
	* `let sym = Symbol("name");`
- To assign the symbol as a property - 
	* Rabbit.prototype[sym] = someVal;

<h2 align="center"> ITERATOR INTERFACE </h2>

- Iterators have the properties - `next()`, `value`, `done()`
	```js
	let okIterator = "OK"[Symbol.iterator](); //Symbol.iterator is provided by JS
	```
- We defined a `Matrix` and `MatrixIterator` class:
	* The **Iterator** has a `constructor` and `next()` that **returns** an object w/ `value` & `done` properties
	* Put in Matrix w/: `Matrix.prototype[Symbol.iterator] = function() {return new MatrixIterator(this)};`
	* We can now loop w/ `for/of` - `for(let {x,y,value})`

<h2 align="center"> GETTERS/SETTERS/STATICS </h2>

- Write `get` or `set` **before a method name** to *take away the need for parenthesis* -
	```js
	class Temperature {
		get fahrenheit()
			//or 
		set fahrenheit(val)
			/* TO DO... */
		temp.farenheit
			//or
		temp.farenheit = 55;
		}
	```
- Write `static` **before a method name** to pack a method in to the constructor instead of the prototype property.
	* This allows us to be able to do the following -
		```js
		Temperature.staticMethod(); //a vall via the Class name
		```

<h2 align="center"> INHERITANCE </h2>

- A simple definition of a child class that can inherit properties from a parent class -
	```js
	class SymmetricMatrix extends Matrix {// This class shouldn't be based on default Object prototype, but this super class
		cosntructor(){
			call super(parentParameters) to initialize parents properties
		}
	```
* `someClass instanceof someClass` - to check if the left operand is a child of the right operand

<h2 align="center"> CREATING IMMUTABLE OBJECTS </h2>

- `Object.freeze(someObject)` - So if you try to change values of properties, then they wouldn't change

MODULES:
	-Improvised Modules of the past:
		*Bind the module to a function
		* eval() or Function constructor: which accept paramter list in paramter 1, and the body in param 2

	-CommonJS Modules (Node.js uses this, and many packages in NPM do to):
		*require(someDependencyName) - makes sure the module is loaded and returns is interface
			**The loader WRAPS module code in a function - giving it its local scope (safety from JS global scope)
			**Modules put their interfaces in an object bound to export. EX:
				exports.formatDate = function(params){return;};
		*module.exports can be overwitten to be any value. Like a single value or function instead of an object.
		*The string that require() uses can be a:
			** Relative Path: './somePath'
			** Not Relative: Then Node.js will look for an installed package by that name

	-ECMAScript Modules:
		*Concept of dependencies and interfaces stay the same. Differences are:
			**Notation is integrated into the language:	
				***Importing:		import varName "moduleName"
				***Exporting:		export function formatDate(paramas){}
			**ES modules don't export a single value. It's a set of named bindings
			**To specify a main export binding:	export default someBinding();
			**Change imported binding name:		import {days as dayNames} from "date-names";
		*Import declarations CAN NOT APPEAR IN BLOCKS. They need to be evaulated before scripts. (Dependencies must be in quotes also then)	
	-One single big file is faster than multiple. So use BUNDLERS.
	-MINIFIERS can be used to make code more compact for speedy transfer over networks	

	-You can load ES modules in browsers by giving your script tag (in html) the attribute type="module"	

ASYNCHRONOUS PROGRAMMING:
	-1)APPROACH: CALLBACKS - make functions that perform a slow action take an extra argument, a callback function.
		*Ex: setTimeout() which is available in browsers and Node.js: waits a number of millisecs and then calls a func.
	
	-2)APPROACH: PROMISES - Abstract concepts like aynchronicity can be better represented as values. So instead of arranging
				a callback to be recalled in the future. RETURN AN OBJECT THAT REPRESENTS THIS FUTURE EVENT.
		*Promise class - an asynchronus action that may complete at some point and produce a value. It's able to notify
				 anyone who is interested when it value is available.
			**Promise.resolve-		The easiest way to create a a promises. This function ensures that the value
							you give is wrapped in a promise.
			**somePromise.then(()=>{});-	***To get the result of a promise, use its then(callback(s) method.
							***Returns another promise, which resolves to the value that the 
							   handler function returns OR if that returns a promise, waits and then
							   resolves to its result
		*Think of promises as a device to move values into an asynchronous reality. A normal value is simply there.
		 A promised value is a value that might already be there or might appear at some time in the future. Computations
		 defined in terms of promises are execute as the values become available

	-3)APPROACH: ASYNC FUNCTIONS - Javascript allows you to write pseudo-synchronous code to describe asynchronous computation. An
					async function is a function that implicitly returns a promise and that can, in its body, await
					other promises in a way that looks synchronous.
		*Notations:
			**When an asyc method or function are called they RETURN A PROMISE. So as soon as its body returns the promise is resolved or if an excetion is thrown
			  the promise is rejected
			**Functions:	async function someStorage(){}
			**Methods:	async methodName(){}

		*Inside of an async function AWAIT can be put infront of an expression to wait for a promise to resolve AND ONLY THEN can the function continue to execute.

		*Easier to use than raw Promises. But can be mixed with them.

		*The event loop is what runs the callbacks of async functions. Therefore yielding responses later even if they resolve immidiately
GENERATORS:
	-Another JS function that pauses and resumer (similar to async but without the promises)
	-Generators return an iterator, so it's easier to create iterators with them (as opposed to creating an iterator object like before):
		*EX:	Group.prototype[Symbol.iterator] = function*() {
  								for (let i = 0; i < this.members.length; i++) {
    									yield this.members[i];
  								}
							    };
	-NOTATION:	A * infront of function = RETURNS AN ITERATOR
	-yield functions can only exist in a generator. AND IT SAVES IT LOCAL STATE! THIS IS WHY ASYNC IS A TYPE OF GENERATOR BUT W/ PROMISES

TCP: 	-One computer must be listening for others to talk to it. Each listener on a computer has a number called a PORT. Other computers can form a 
	 connection by using those ports
	-To connect a maching to the world wide web, all you need is to listen on port 80 w/ the http protocol so other computers can ask it for documents
		*Each document is of the form:	protocol//server/path	or 	http://henlo.com/home

HTML and JAVASCRIPT:
	-You can load ES modules in browsers by giving your script tag (in html) the attribute type="module"	
	-Some tag attributes can also contain a JS program. Thr <button> tag has the attribute onclick="" which runs whenever the button is clicked.

THE DOM:
	-The global binding document gives us access to the DOM.
		*its documentElement property refers to the object representing the <html> tag
		*document.documentElement serves as the root of the DOM
	-Nodes for elements(html tags) represent the structure of the document
	-Each DOM node object has a property:	nodeType - which contains a code that identifies the type of node.
		*1 = Node.ELEMENT_NODE
		*3 = Node.TEXT_NODE
		*8 = node.COMMENT_NODE
	-DOM Nodes have property:	*childNodes - which holds an arraylike object that holds children (excludes most regular array methods like slice and map)
					*parentNode - points to this node's parent
					*firstChild/LastChild - point to the first and last childs respectively. null for nodes without children
					*previousSibling/nextSibling - Points to left/right siblings. The first childs prevSibling will always be null (lastChild similar)
					*children - exactly like childNodes but focuses only on elements(tags)
					*nodeValue - Only for text nodes. It holds the string of text that it represents
					*getElementsByTagNAme("a")[0]; - collects all elements with the given tag name that are decendants of this node and return as an
									 array-like object
					*document.getElementById("id") - gets a specific single node by its id attribute
					*getElementsByClassName - like byTagName, but with the class="" attribute
					*remove() - remove this node from their current parent node
					*appenchChild(e) - puts a child node at the end of an element's child array
					*insertBefore(e1, e2) - inserts e1 before e2. so removes e1 from the dom and puts it "behind" e2
					*replaceChild(newNode, nodeToReplace) - used to replace a child node with another one
					*document.createTextNode("someText") - creates a text node
					*document.createElement("someEle") - creates an element node
					*offsetWidth / offsetHeight - give you the space the element takes up in pixels
					*clientWidth / clientHeight - give you the size of the space inside the element ignoring border width
					*getBoundingClientRect() - most effective way to find the precise position of an element on the screen
								   .It returns an object with top, bottom, left, right properties, indicating the
								   pixel positions of the sides of the element relative to the top left of the screen
					*pageXOffset / pageYOffset - The current scroll position (add this to getBoundingClientRect() if you want it relative to the document)
					*style - an object that has properties for all possible style properties. The values of these properties are string, which we can
						 write to to change aspects of an element's style
							**for hypenated styles like font-family we can use camelcase like so style.fontFamily
					*querySelectorAll("someSelectorString") - take CSS selectors and returns an array of elements that match it
					*querySelector - same as above but for the first instance

	-Empty Text Nodes get created by HTML whitespace!!!!
	-HTML allows you to use made up attributes. So use getAttribute and setAttribute to work with those.
		*convention dictates our made up attribute names start with the previc data-
		*For historical reasons class isn't a property, so to acces this attribute we use className OR our getAttribute

HANDLING EVENTS:
	-EVENT HANDLERS:
		--Primitive ways of handling events include: 1)constantly reading a state for a key. 2)Checking a queue of new events (polling)
		--A better mechanism is to actively notify our code when an event occurs. Browsers do this by allowing us to register functions as
	 	 HANDLERS for specific events.
			*EX:
				<p>Click this document to activate the handler.</p>
				<script>
					window.addEventListener("click", () => {
						console.log("You knocked?");
					});
				</script>
			** window binding (built-in object) represents the browser window that contains the document. Calling its addEventListener()
			registers the second arg to be called whenever the event described by first arg occurs

	-EVENTS AND DOM NODES:
		--Event listeners are called only when the event happens in the context they are registered on (EX: We registered on window but we could register on DOM elements)
		--Giving a node something like an onclick attribute also has the same effect. Most type of event can be attached through attribute w/ the name of the event after on.
		  (You can only register 1 handler per node if you choose attributes. addEventListener() can setup multiple)
		--removeEventListener(event, funcNameToRemove) to remove a previously registered event handler.
	
	-EVENT OBJECTS:
		--Event handler functions are passed an argument: the EVENT OBJECT (holds additional info about the event like: which mouse button was pressed through event object's button property)
			*EX: 	
					<button>Click me any way you want</button>
					<script>
					let button = document.querySelector("button");
					button.addEventListener("mousedown", event => {  //HERE. Info stored in the event object differs per type of event. (the type property can tell you its type of event)
						if (event.button == 0) {
						console.log("Left button");
						} else if (event.button == 1) {
						console.log("Middle button");
						} else if (event.button == 2) {
						console.log("Right button");
						}
					});
					</script>

	-PROPOGATION:
		--For most even types, handlers registered on nodes with children will also recieve events that happen in the children.
		--Events PROPOGATE outward, from the node where it happened to that node's parent node and on and on to the root until finally the window gets a chance to respond to the event.
		--AT ANY POINT, an event handler can call the stopPropagation() method on the event object to stop the outward propogation. (Useful for stacked UI: clickable panel has clickable button)
		--Most event objects have a property, target. It can be used to:
				1)ensure you're not handling somthing that propogated up
				2)cast a wide net for a specific type of event 
					*EX: You have a node that contains a long list of buttons. It'd be annoying to register handlers for all of the buttons. So we just register a single one with the outer node using target.
							<button>A</button>
							<button>B</button>
							<button>C</button>
							<script>
								document.body.addEventListener("click", event => {
									if (event.target.nodeName == "BUTTON") {
									console.log("Clicked", event.target.textContent);
									}
								});
							</script>
	
	-DEFAULT ACTIONS:
		--Many events have default actions associated with them. Ex: right click on browser = context menu; down arrow key = scroll the page down; etc...
		--JavaScript event handlers RUN BEFORE default behaviors take place. You can call preventDefault() method of the event object to cancel the default behavior.

	-EVENTS:
		--KEY EVENTS:
				* "keydown" event 		- 	** fires when a key is pressed
											** It fires events as long as its held (so be careful)

				* "keyup" event 		- 	** fires when a key is released

				* event object			-	** .key = a string that for most keys corresponds to the key it types
											** key combinations: shiftKey, ctrlKey, altKey, metaKey are used with AND operators
										   	   and regular key operators to check if these keys are being combined with others.

				* general				-	** key event originates in the DOM node that has focus (tabindex attribute or certain tags have focus)

		--POINTER EVENTS:
			---CLICKS:
				* "mouseup"				- 	** fires when a mouse button is released
				* "mousedown"			-	** fires when a mouse button is clicked/held
				* "click"				-	** fires after the "mousedown" event
											** event originates on the DOM node that is the common anscestor of where 
											   the "mousedown" and "mouseup" originated (these could be different)
				* "dbclick"				-	** fires if two clicks happen close together.

				*event object			-	**.clientX & .clientY: contain the event's coordinates (in pixels) relative to the 
																   top-left corner of the window.
											**.pageX & .pageY:	(like above) top-left corner of the whole document. (scrolling into account)
				* general 				-	** event originates on DOM node below the pointer.
				* EX: Primitive Drawing Program:
								<style>
								body {
									height: 200px;
									background: black;
								}
								.dot {
									height: 8px; width: 8px;
									border-radius: 4px; /* rounds corners */
									background: blue;
									position: absolute;
								}
								</style>
								<script>
								window.addEventListener("click", event => {
									let dot = document.createElement("div");
									dot.className = "dot";
									dot.style.left = (event.pageX - 4) + "px";
									dot.style.top = (event.pageY - 4) + "px";
									document.body.appendChild(dot);
								});
								</script>
			---MOTION:
				* "mousemove"			-	** fires everytime the mouse pointer moves
											** useful for implementing mouse-dragging stuff
				*EX: Draggable bar
							<p>Drag the bar to change its width:</p>
							<div style="background: orange; width: 60px; height: 20px">
							</div>
							<script>
							let lastX; // Tracks the last observed mouse X position
							let bar = document.querySelector("div");
							bar.addEventListener("mousedown", event => {
								if (event.button == 0) {
								lastX = event.clientX;
								window.addEventListener("mousemove", moved);
								event.preventDefault(); // Prevent selection
								}
							});

							function moved(event) {
								if (event.buttons == 0) {
								window.removeEventListener("mousemove", moved);
								} else {
								let dist = event.clientX - lastX;
								let newWidth = Math.max(10, bar.offsetWidth + dist);
								bar.style.width = newWidth + "px";
								lastX = event.clientX;
								}
							}
							</script>

			---TOUCH:
				* "touchstart"			-	** fired when a finger starts toughing the screen
				* "touchmove"			-	** fired when moved while touching
				* "touchend"			-	** fired when it stops touching the screen
				* event object			-	** .touches - since many touchscreens can detect multiple fingers at the same time, this 
														  property holds an array-like object of points, each of which has its own 
														  clientX, clientY, pageX, pageY
				* general				-	probably call prevenDefault()
				*EX: Multiple Touch detection:
							<style>
							dot { position: absolute; display: block;
									border: 2px solid red; border-radius: 50px;
									height: 100px; width: 100px; }
							</style>
							<p>Touch this page</p>
							<script>
							function update(event) {
								for (let dot; dot = document.querySelector("dot");) {
								dot.remove();
								}
								for (let i = 0; i < event.touches.length; i++) {
								let {pageX, pageY} = event.touches[i];
								let dot = document.createElement("dot");
								dot.style.left = (pageX - 50) + "px";
								dot.style.top = (pageY - 50) + "px";
								document.body.appendChild(dot);
								}
							}
							window.addEventListener("touchstart", update);
							window.addEventListener("touchmove", update);
							window.addEventListener("touchend", update);
							</script>
		--SCROLL EVENTS:
				* "scroll" event		-	** fired whenever an element is scrolled
											** preventDefault doesn't prevent scrolling
				*EX: Progress bar for scrolling:
								<style>
								#progress {
									border-bottom: 2px solid blue;
									width: 0;
									position: fixed;
									top: 0; left: 0;
								}
								</style>
								<div id="progress"></div>
								<script>
								// Create some content
								document.body.appendChild(document.createTextNode(
									"supercalifragilisticexpialidocious ".repeat(1000)));

								let bar = document.querySelector("#progress");
								window.addEventListener("scroll", () => {
									let max = document.body.scrollHeight - innerHeight;
									bar.style.width = `${(pageYOffset / max) * 100}%`;
								});
								</script>

		--FOCUS EVENTS:
				* "focus" event			-	**fired when an element gains focus
				* "blur" event			-	**fired when an element loses focus
				* general				-	**These two events do not propogate
				*EX: Form erases when something loses focus, and adds the text when something gains focus

		--LOAD EVENT:
				* "load" event			-	**fires when the page finishes loading
											**fires on the window and body
				* "beforeunload"		-	**fires when a page is closed or navigated away from
											**used to prevent user losing work
				* general				-	** Usually used when the whole document needs to be loaded

	-DEBOUNCING:
		--Debouncing: For events that fire rapidly (like mousemove) you can use setTimeout to make sure you don't do it often.
			*You can catch a timeOut with clearTimeOut(timeOutBinding);
		--Ex: Detecting user typing pauses:
				<textarea>Type something here...</textarea>
				<script>
				let textarea = document.querySelector("textarea");
				let timeout;
				textarea.addEventListener("input", () => {
					clearTimeout(timeout);
					timeout = setTimeout(() => console.log("Typed!"), 500);
				});
				</script>

HTTP AND FORMS:
	-GET /18_http.html HTTP/1.1
		*Our browser sends requests to servers. The first word in a request is the method of the request:
			**GET, DELETE, PUT, POST, DELETE
		*After the method is the path of the resource
		*Finally, the version of the HTTP protocol (done automatically)
		*When we request a html page, if it has dependencies (images, srcs, etc...) those also get requested simultaneously.

	-HTML may include FORMS, which allow users to fill out info and send it to the server.
		*The default action after submitting a form is navigating to the result.
		* GET /example/message.html?name=Jean&message=Yes%3F HTTP/1.1
			** The form info is added after the PATH (after ?)
			** Each field of a form is separated as name/value pairs via &
			** %3F here is the escaped character for "". This is URL encoding.
				*** encodeURIComponent(s)	*** decodeURIComponent(s)
		* A POST method would put the form info into the body of the request

	-FETCH:
		*The interface through which JS can make HTTP requests (uses promises since its new-ish):
			**fetch returns a promise that resolves to a Response object that holds info about the server's response (status & headers)
			**The headers of the Response is a map like object. (Insensitive keys)
		*The first arg is the URL that should be requested (when it doesn't start as w/ a protocol name like http: then it's treated as relative)
		*To get the actual body of a response use the text() method

		* Forms use the <input> tag w/ the attribute type as follows:
			text		A single-line text field
			password		Same as text but hides the text that is typed
			checkbox		An on/off switch
			radio		(Part of) a multiple-choice field
			file		Allows the user to choose a file from their computer
			**<textarea>
			**<select> <option><A/option> <option></option> </select>
			.
		*Whenever a value of a form field changes the "changes" event is hired

	-Tabindex element attributes let us set the order of where our focus shifts when we press tab
	-disable attribute can disable a ll form fields.
	-THE FORM AS A WHOLE:
		*When a field is contained in a <form>, it'll have a property , form , that links back to the form. (Similarly the form has an array elements)
		*The name attribute of a form field 1)determines the way its value will id' when the form is submitted. 2) Act as a key for <form> elements property
		*A button with a type attribute of submit will submit the contents of the form when clicked. PRODUCES EVENT "submit"
			**You can preventDefault()
		*Textfield types have a property VALUE which stores the textfields current content as a string value
			**EVENTS: "change" : fires when you lose focus and the textfield has been changed. "Input": Fires whenever you type on the textfield
	-STORING DATA CLIENT-SIDE:
		-Browser objet: localStorage
			*Properties/Methods:
				** .setItem(key, val)
				** .getItem(key): returns the value or if nothing, null
				** .removeItem(key)
		-A similar object is session storage except everything is forgotten after the session in this case
