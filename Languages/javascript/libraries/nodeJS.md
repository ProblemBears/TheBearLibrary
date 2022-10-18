<h1 align="center"> NodeJS </h1>

<h2 align="center"> TABLE OF CONTENTS </h2> 

<h2 align="center"> IMPROVED DEVELOPMENT WORKFLOW & DEBUGGING </h2>

### Understanding NPM Scripts			
- Process		
    1. `npm init`
    2. Go to `package.json`
    3. Insert a key/value in `"scripts"`
    4. Finally, you can do `npm run <key>` where the key is the name of your script you specified in `"scripts"`
- Creating a `"start"` script is an exception where you don't have to use `"run"` so you could simply do `npm start`
- They key/values should not have whitespace
- You should wrap in double quotes

### Installing 3rd Party Packages			
- Process		
    1. Go to the root of your App
    2. `npm install <package>`
        * Ex 1 -  `npm install nodemon --save-dev` 	
            * `--save-dev` since this is only needed for easy save/restart during development
- Flags
    * `--save` - to install as a production dependency
    * `--save-dev` - to install for development	(Creates devDependncies in your package.json)
    * `-g` - install to your machine

### Using Nodemon for Autorestarts		
- Why?			
    * We would have to stop/rerun our server code manually otherwise
- Process		
    1. Create a `"start"` script with the value `"nodemon app.js"`
    2. Use `npm start` to run that script, which runs nodemon
    3. Now when we save a project, the server automatically reruns
- Notes			
    * You can not use nodemon in terminal if you install it as a local dependency. You would have to install it as a global dependency to use it in terminal
- IMOPORTANT 
    * Restart nodemon if you install a new package

<h2 align="center"> WORKING WITH EXPRESS.JS </h2>

### What is Express.js?				
- Why use Express.js?			
    * Server logic is complex! 
    * It's a framework that does the heavylifting
- Alternatives to Express.js		
    * Express.js (is the most popular)
    * Vanilla Node.js 
    * Adonis.js (Laravel inspired from PHP)
    * Koa
    * Sails.js
- Installing Express.js:				
    * Process
        1. `npm install --save express`
        2. `const express = require('express');`  
            `const http = require('http');` (Ctrl + Click to see the code)
        3. `const app = express();`			(Since the express module exports a function we have to execute for initializing a manager)
        4. `const server = http.createServer(app);`	//app is a valid requestHandler
        5. `server.listen(3000);`

### Adding Middleware				
- **Middleware** - an incoming request is "funneled" through functions by Express.js	
    * Instead of having one request handler, there's a possibility of hooking up multiple functions that the request will go through until it is sent as a response
- Process -		
    1. To put a function in the Middleware -
        * `app.use(param1);`	
            * **Param 1** - A function that receives three arguments - `(req, res, next) => {}`
                * `next`: Is a function that will be passed to this `param1` function by Express.js. Used to move on to the next middleware
    2. To allow the request to travel on to the `next` middleware -
        * Inside the `app.use() Parameter 1 function` write `next()`
        * So if we have another `app.use()` along the way the `next()` allows it to "funnel" to the next Middleware

### How Middleware Works:				
- What			
    * We can now send a response with Express.js (which is easier than in vanilla w/ a utility function "`.send()`"
- Process		
    1. In the last middleware function do:
        * `res.send('<h1> Hello from Express! </h1>)`
- The Response Headers are automatically set by Express 
    * Although we could set the response manually with `res.setHeader()` 	

### Express.js - BTS				
- Important		
    * Instead of http.createServer(app).listen(3000). We can use express to do this instead - `app.listen(3000);`
        * Which means we also don't need the ('http') import
    * The official code can be seen in `Github -> Express -> express/lib/application.js`

### Handling Different Routes			
- What			
    * We want to handle request for different routes.
- How			
    * There are actually **4 overloads** of app.use(), we were using the one with one argument.*** With 2 arguments*** we can specify a path - `app.use('/', (req, res, next) => {} );`
- Problems		
    * Express reads the path `'/'` as a match even if there may be a more specific path, like `'/app-js'`. Therefore, in code, we can make the actual `'/app-js'` and put it above the `'/'` code with no `middleware.next()`, so that it matches the actual path first and isn't able to travel to the next. THIS IS MADE THIS WAY SO THAT WE CAN FUNNEL THINGS TO MULTIPLE PAGES IF WE NEED IT LIKE -
        * Putting `'/'` above a `'/app-js'` above another `'/'` Where the first `'/'` console logs ("this always runs")

### Parsing Incoming Request				
- How to handle a POST request:
	1. Have a page that has a form (In this case we set it up through routes) :
		```js 
		app.use('/add-product', (req, res, next) => {
			res.send('<form action="/product"><input type="text" name="title> <button type="submit"> Add Product </button> </form>');
		});
		```

	2. Add a middleware that parses the request (this should usually be the first middleware for all routes) :		
		1. `npm install --save body-parser`
		2. `const bodyParser = require('body-parser');`
		3. `app.use(bodyParser.urlencoded(extended:false));` - Where this function yields a middleware function that parses and even calls next

	3. Now make a root based on the "action" of the form from `(i)` :
		app.use('/product', (req, res, next) => {
			console.log(req.body
			res.redirect('/'); //To redirect to the home page
		});

	* Now since the `req` is parsed using `(i)`, `(ii)` is able to use it

- Issues to deal with next			
	* This route currently would listen to any request, but we only want to use it for a POST request...

### Limiting Middleware Execution to POST Requests			
- To do `POST` requests for a route:
	```js
	app.post('/',()=>{}); //instead of app.use()
	```
	* To do `GET` requests use: `app.get`
	* Others are : `delete`, `patch`, `put`

### Using Express Router				
- What is Express Router?			
	* Typically, we want to put our routing code throughout different files and import in the main app.js file. Express.js actually gives us a way of outsourcing Express logic to other files
- Process		
	1. Convention says we put route logic in a **"/routes" directory**
	2. Add files like : 
		* "admin.js" - where we'll store routes only accessible to admins
		* "shop.js" - routes that the user can see
	3. For each of these files follow this structure :
		```js
		const express = require('express');
		const router = express.Router(); // This is like a mini-Express app pluggable into the main one
		router.use('/someRoute', (req, res, next) =>{} ) //Can still replace `use` w/ get, post, etc.
		module.exports = router;
		```
	4. In your main file (app.js) :
		```js
		const adminRoutes = require('./routes/admin.js');

		app.use(adminRoutes) 	// because adminRoutes is a valid middleware function 
								// just as before, the order still matters
		```
- Note - `get`, `post` , etc. Do **exact matching** for the path in contrast to 'use'

### Adding a 404 Error Page			
- What			
	* Previously we swicthed all our routes to have matching paths with get or post. We want the power of 'use' for error pages when we go to invalid routes
- How			
	* At the end of your main app.js do :
		```js
		app.use((req, res, next) => {
			res.status(404).send('<h1>Page not found</h1>');
		});
		```

### Filtering Paths				
- What/How		
	* Sometimes route paths have common starting paths like '/admin/add-product'. We could add this path to every route but that would be tedious. Instead, we could let external routes have normal route names, and in the main App.js, we can use a **"filter path"** in our `app.use('/admin', ()=>{})`

### Serving HTML Files				
- Process		
	1. `const path = require('path');` - A NodeJS core module that helps generate paths
	2. In your routes do : 
		```js
		res.sendFile( path.join( __dirname, 'views', 'somePage.html')
		```	
		* We can not use absolute paths because '/' refers to the device root. Instead, we make use of NodeJS's GLOBAL VARIABLE: `__dirname` which generates an absolute path to the current file directory, and the `path.join()` simply generates a path with concatenated parameters ASWELL as generates valid paths for both Windows and Linux systems
- ModelViewController conventions creates a directory called "View" where we put html files
### Using a Helper Function for Navigation			
- What/How		
	* We don't want to tediously generate a root path by manipulating `__dirname`. Instead we generate a root path:
		1. Create a **'/util' directory**, and create a `'path.js' file` in it.
		2. In that file write:
			```js
			const path = require('path');

			module.exports = path.dirname(process.mainModule.filename);	   //where process is globally available via Node
			```
		3. In any file you want to use it, import it like so:
			```js
			const rootDir = require('pathToIt');
			```
- `process.mainModule.filename` may be deprecated, instead possibly use:  
	` module.exports = path.dirname(require.main.filename);`

### Serving Files Statically				
- What			
	* We want to serve CSS files that connect to HTML files that we also serve. So we need to be able to serve files 'statically'
- How		
	1. Create a 'public' directory.
	2. In `app.js` we plug-in one of Express's built-in middleware (after our parser middleware/ before our page routes):
		```js
		app.use( express.static( pathToFolderWeWantToServeStatically ) );
		```
	3. Make sure the html files have \<style> tags with path links relative to the 'public' folder we served statically (since Express forwards file request to the static folder)
- Notes			
	* Up until now the info we send has been private. Meaning if our route specified a file being served, it would yield a 404 error.


<h2 align="center"> THE MODEL VIEW CONTROLLER (MVC) </h2>

### What is the MVC?:
- Definitions		
	* **Model** - Represent your data in your code; work with your data (save, fetch, etc.)
	* **View** - What the user sees; decoupled from you Application code.
	* **Controller** - Connects your Models and Views.
	* **Routes** - Decide what Controller logic to execute

### Adding Controllers				
- What	
	* We don't want to put our Controller Logic into the files in our "/routes" directory, because the files there would quickly get large. 
- How			
	1. Instead, we separate each file into a "/controllers" directory with respective files
	2. We export those functions using : `export.someFuncName = (req, res, next) => {}`
	3. We import those exports with : `const productsController = require('pathToController');`
	4. We use the references of the functions with : `productsController.funcReference` not execution

### Adding a Product Model				
- Code			
	* In a '/models' directory .js file we write :
		```js
		const products = [];

		module.exports = class Product {
			constructor(t) {
				this.title = t;
			}

			save() {
				products.push(this);
			}

			static fetchAll() {
				return products;
			}
		}
		```
	* Then in the some '/controller' .js we import the Model in

### Storing Data in Files via the Model			
- What			
	* Previously, we stored data into an array called 'products'. Instead, we may want to store the data into a file
- How			
	1. Delete the previous `products[]` and import this instead:
		```js
		const fs = require('fs');
		const path = require('path');
		```
	2. In `save()` do this instead:
		```js
		//contstruct a path to the file (it must exist and conventionally reside in '/data')
		fs.readFile(path, (err, fileContent) => {
			let products = [];
			if(!err) {
				products = JSON.parse(fileContent);
			}
			products.push(this);
			fs.writeFile(path, JSON.stingify(products), (err) => {} );
		});
		```
- Despite this we still will get an error which we will solve next...

### Fetching Data from Files Via the Model			
- IMPORTANT		
	* We were getting an error previously because doing file operations is usually asynchronous. Therefore, we can solve this by sending a function argument into the Model which the model will execute by also giving its own argument (the products array). This is a callback.
- How 			
	* In `contollers/product.js` :
		```js
		Product.fetchAll( products => {
			res.render(//Code)
		}
		```
	* In `models/product.js` :
		```js
		static fetchAll(cb) {
			const p = //Get Path Code;

			fs.readFile(p, (err, fileContent) => {
				if(err) cb([]);
				else cb( JSON.parse(fileContent) );
			}
		}
		```

DYNAMIC ROUTES & ADVANCED MODELS:
	-Adding the Product ID to			-How:			* We place an ID property into our Model
	 the Path:								* In some element of some View we create some route with the unique ID of some product.
	
	-Extracting Dynamic Parameters:			-What:			* We must now create that Route in our '/routes'

							-How:			1) In the Express Router Path, We can make a path of the form '/products/:productId' where the colon specifies a
										   variable section in a path (keep in mind all Routes that have '/product/something' should go before routes with
										   variable sections, otherwise they'd never be touched)

										2) In some '/controllers' file we export a function like so:
											exports.getProduct = (req, res, next) => {
												const prodId = req.params.productId;	//WHICH CORRESPONDS TO OUR :productId above	
												//Use the id
											}					

	-Loading Product Detail Data:			-How:			1) In a '/model' file we insert a static function in our Model class called findById(id, cb)
										2) We pass the dynamic route's id that we inserted via the ':' syntax

	-Rendering the Product Detail			-How:			1) In the findById()'s callback we can render our View based on the argument (the ID) attached to the 
	 View:									   callback. We do this with
											res.render('pathToView', {product: product})	//Where the product property is used throughout the View
																	// and the product value is the product w/ the ID from our query

	-TBC? : A lot of EJS
