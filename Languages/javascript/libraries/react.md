<h1 align="center"> React </h1>

<h2 align="center"> What is React.js? </h2>

- A JavaScript library for building UI (client-side JS library)
- Extra functionality can be added via community packages. For things other than UI (e.g: routing)
- Competitors: 
    * **Angular** - component based UI; uses TypeScript; has more features; overkill for small projects
    * **Vue.js** - less popular than React & Angular
- Outline :
    * Basics & Foundations :
        * Components & Building UI
        * Working w/ Events & Data: "props" and "state"
        * Styling React Apps & Components
        * Intro into "React Hooks"
    * Advanced Concepts :
        * Side Effects, "Refs" & More React Hooks
        * React's Context API & Redux
        * Forms, Http Requests & "Custom Hooks"
        * Routing, Deployment, NextJS, & More
    * Summaries and Refreshers

<h2 align="center"> REACT BASICS & WORKING WITH COMPONENTS </h2>

### Creating a new React Project (with dummy code)
- How				
    1. cd into a directory where you want to create the project folder
    2. `$ npx create-react-app some-directory-name`
    3. `cd` into it and do `npm start`

<h2 align="center"> A (PRETTY DEEP DIVE) INTRODUCTION TO Next.js </h2>

### Creating a New Next.js Project & App:				
- How		
    1. Have NPM installed
    2. Go to a directory where you want to spawn a new project directory
    3. To auto create do : `npx create-next-app@latest`
    4. It prompts you to name the directory

### Adding First Pages				
- Prerequisite	
    * Delete the following :
        * /pages/api directory
        * /styles/Home.module.css
        * /pages/index.js	//We delete this to show Next.js Routing

- What/How:	
    * Next.js reads the `pages directory structure` to define routes :
        * Ex : We put the following files in `'/pages'` and they have routes :
            * `index.js` = domain.com/
            * `news.js` = domain.com/news

    * Then, we can render the page by exporting A React component like usual (index.html):
        ```js
        function HomePage() {
            return <h1>Hello World!</h1>
        }
        export default HomePage;
        ```
- Important	
    * YOU CAN OMIT `'react' imports` in Next.js
    * The code is pre rendered so HTML code is sent giving us the advantage of no initial loading flickers and improved SEO

### Adding Nested Paths &	Pages (Nested Routes)			
- What/How	
    * Folders can be added to '/pages' which in turn makes nested routes. If you want to still get get a route of the folder name, then we add index.js in that folder

### Creating Dynamic Pages (with Parameters)				
- What/How	
    * The  issue of Dynamic Routes (for things like different productId's) arises. We handle it by naming files with square brackets : `[someIdentifier].js`
    * We can alsp have dynamic folders : [someFolderIdentifier]

### Extracting Dynamic Parameter Values				
- What/How	
    * To extract the Paramter Value of our Dynamic Page. We make use of a Next.js special React hook:
        ```js
        import { useRouter } from 'next/router';

        function SomeComponent(){
            const router = useRouter();
            
            const saveParam = router.query.someIdentifier	//the thing we put in the [] in our filename
        }
        ```

### Linking Between Pages				
- What/How	
    * Yes we could use anchor tags to link between pages, but that would send the server new requests and this app would not be a single page app
    * Instead, we do :
        ```js
        import Link from 'next/link';

        //and instead of <a> we use...
        <Link href="/someFolder/someFile"> Text Here </Link>	// where '/' is the pages folder 
        ```
- Note		
    * We know our code asks the server for a new page if when we refresh, we see a cross pop out quickly. With this <Link> that should not happen.
    * Keeping it as a SPA allows us to keep States across pages

### The "_app.js" File & Layout Wrapper				
- What		
    * We use a Wrapper component to "wrap" our Navigation bar onto Next pages we have. Although, if we had many pages, you could see how this could become cumbersome. **Enter the _app.js file**
- IMPORTANT	
    * "_app.js" is a special file in Next.js that resides in "/pages". It acts as a root component that receives destructured props of the form {Component, pageProps} where:
        * `Component` - holds the actual page content that should be rendered. (so it changes)
        * `pageProps` - are specific props any of our pages might be getting
- Conclusion	
    * Therefore, since the Component prop in _app.js contains what is to be rendered. We could simply wrap our Header Component around it

### Using Programmatic (Imperative) Navigation 				
- What/How	
    * In our Component for giving a button some link to it's own ID Page. We can use `useRouter()` imported again from `'next/router'`. Then in our Handler for that React Component we can specify `router.push('/' + props.id);` ( which pushes a new page on the Page Stack based on the ID passed through props )
- Continue	
    * Now we just need to add a Next.js '/page' route at that path of the route pushed
- Convention	
    * Use JSX on '/components' and also style them there so that our '/pages' stay lean. 

### How Pre-rendering Works & Which Problem We Face			
- Problem	
    * We want to pre-render a page with data, but with data for which we have to wait, and we have to tell Next.js when we're done waiting. If we don't then the HTML we yield will not contain our data and therefore be bad for SEO

### Data fetching for Static Pages		
- What/How		
    * Next.js gives us Two Forms of Pre-rendering which can be used to control how pages are rendered:
        1. Static Generation -  
            * A Page Component is pre-rendered when you build the App for deployment.
            * But, if you need to wait for data(fetching). In the Page component where
                you need to do that, you must export a special function:  
                `export function getStaticProps(){};`  
                NextJS Looks for this function, and if it finds it, it executes this
                function during this pre-rendering process. (It gets called before even
                your Page Component).
            * `getstaticProps()` is allowed to be async. It can run any code a server 
                would run (ex: file system, authentication). Simply, because this code
                is executed during the build process, it never executes in a visitor's machine.
                * So you could fetch data from an API here
                * You always have to return an object of the form:
                    ```js
                    {
                    props: {propsObjectToBeUsedInPageComponent}
                    }
                    ```
                    Which makes that build time prop insertion available in
                    Your props passed to your Page Component
        2. Server-side Rendering

### More on Static Site Generation (SSG)		
- Analysis	
    * To build for deployment we'd have to run: `npm run build`
    * After doing that, we get log output that shows we generated static pages, and it shows us which ones.
- Problem	
    * Since the SSG is built at runtime, if we update the data that it feeds to props, then it would not be updated:
        * A FIX (**Incremental Static Generation**): In our return object, add the following :  
            `revalidate: 10` - which means it will be regenerated every 10 seconds on the server
- End Result	
    ```js
    export async function getStaticProps() {
        // fetch data from an API
        return {
            props: {
                meetups: DUMMY_MEETUPS
            },
            revalidate: 10
        };
    }
    ```

### Exploring Server-side Rendering (SSR) with "getServerSideProps"		
- What		
    * If we want to dynamically update the Page not during the build process or every couple of seconds we use `getServerSideProps()`. It's similar to the previous one except it runs always on the server after deployment. You could run any server side code in  that function (even sensitive info stuff)
    * You can also pass in the `"context"` parameter, which gives you access too `context.req` & `context.res`

- Which do we use?		
    * getStaticProps() is faster

### Working with Param for SSG Data Fetching		
- What		
    * You can't get the URL of a Dynamic Parameter with the help of useRouter() because this is a build-time function. Therefore, you use the context parameter.
- How		
    * In the export async function getStaticProps(context), before the return do:
    const id = context.params.someFileIdentifier;

- Continue	
    * But when we set it in our return object. We get the error "getStaticPaths is required"...

### Preparing Paths w/ "getStaticPaths" & Working With Fallback Pages		
- What		
    * `getStaticPaths` - is a function you need to export in a Page Component file if its a dynamic page and you're using `getStaticProps()`
- How		
    * If the conditions are met then export that function like so :
        ```js    
        export async function getStaticPaths() {
            return {
                paths: [
                ]
            }
        }
        ```
        * This returns an object that describes all dynamic segment values for which this page should be pre generated
        * paths keeps an array of objects. One object per version of this dynamic page in the form: `{ params: { meetupId: 'someId' } }`
    * After doing this, you'll solve the first error, but get another about a 'fallback'. To fix it, in the returned object of `getStaticPaths()`, we also have to define the `'fallback' property` which takes a boolean value :
        ```js
        return {
            fallback: false,
            paths: [
            ]
        }
        ```
        it tells Next.js whether your `paths[]` contains all supported parameter values, or just some of them.
        * false = all parameter values defined		
        * true = some  
        We use false which gives us a 404 error when we go to an undefined path.

### Introducing API Routes			
- What		
    * Allows you to do `POST`,`GET`, `SET` requests using the same server as your Next project.
- How		
    1. Add a special folder to you '/pages' called: 'api'		
        * This allows Next to turn any .js files to API routes
        * Turned into endpoints that can be targeted by request and get/return JSON
    2. Add files that correspond to routes. Except in those files there aren't Page Component. There is server-side code.
        * Ex: An API route with file structure /pages/api/example.js 	has the route  api/example
    3. Write the following (convention says the function is named handler): 
        ```js
        function handler(req, res) {
            if( req.method === 'POST' ) {
                const data = req.body;

                const { title, image, address, description } = data;
            }
        }

        export default handler;
        ```

### Working w/ MongoDB				
- How		
    1. Create a cluster (M0 Free Tier)
    2. Give a Network & Database Access
    3. On the Cluster Do Connect > Connect to your Application > Copy Code (of correct Driver)
    4. (For Node.js) `npm install mongodb` (the official driver for Node.js)
    5. In some '/pages/api' file we can do :
        ```js
        import { MongoClient } from 'mongodb';

        //inside the handler function (which can be made async):
            const client = await MongoClient.connect('//pasteConectionString' );
            const db = client.db();

            const meetupsCollection = db.collection('meetups');
											const result = await meetupsCollection.insertOne(data);
            client.close();
            res.status(201).json({message: 'Meetup Inserted!'});
        ```

### Sending HTTP Requests to our API Routes			
- How		
    1. Go to some page where we need to make the request
    2. Set up a `async function addMeetupHandler(enteredMeetupData) {}`
    3. Write :
        ```js
        const response = await fetch('someDomainPath', {	//External: website URL, Internal: abs path
            method: 'POST',
            body: JSON.stringify(someData),
            headers: {
                'Content-Type': 'application/json'
            }
        })
        ```

### Getting Data from the Database		
- How		
    1. One Way: Create an API Route that connects to the DB and gets the data
        1. Then in a Page component's `getStaticProps()` do a `fetch()` (provided in Next.js server-side code). But that
            would be redundant.
    2. Don't create an API Route and just do that code (the MongoDB import and connection) in the Page Component
        * Yes imports can be used in server-side snippets as well. Next.js automatically seperates them into their own bundle imports

- Important	
    * MongoDB returns data with an auto generated ID, which has a strange format and hence we have to map() the return to an acceptable format. Here we do it in our `getStaticProps()`'s return :
        ```js
        return {
            meetups: meetups.map(meetup => ({
                title: meetup.title,
                address: meetup.address,
                image: meetup.image,
                id: meetup._id.toString()
            }) 
        )
        ```

### Getting Meetup Details Data & Preparing Pages			
- What		
    * Previously, for dynamic singular pages, we had to define a `getStaticPaths()` function which returns an object with the 'paths:' property that defines all dynamic paths. Byt with paths based on IDs stored in mongoDB.
- How		
    1. in `getStaticPaths` connect to MongoDB and then do:
        ```js
        const meetups = await meetupsCollection.find({}, {_id: 1}).toArray(); //Fetch all documents and only their id fields
        client.close(); //close your connection
        ```
    2. Now, instead of hardcoding the paths: `[{params: {meetupId: 'm1'}}, {params: {meetupId: 'm2'}}`. We do :
        ```js
        return {
            fallback: false,
            paths: meetups.map(meetup => ( { params: { meetpID: meetup._id.toString() } } ) );
        }
        ```
    3. Now we actually have to render based on that dynamic path. So in `getStaticProps()` we also connect to mongoDB but just query for one ID :
        ```js
        import {MongoClient, ObjectId } from 'mongodb';
        const selectedMeetup = await meetupsCollection.findOne({_id: ObjectId(meetupId) });  //Where meetupId was gotten from context.params.meetupId
        ```
    4. In `getStaticProps` we now return :
        ```js
        return {
            props: {
                meetupData: {
                    id: selectedMeetup._id.toString(),	//Making sure we don't get ObjectId Type
                    title: selectedMeetup.address,
                    //etc...
                }
            }
        }
        ```

### Adding "head" Metadata":			
- What		
    * For any deployed webpage you should consider adding metadata. Inspecting \<Head> shows things are missing.
	* For example titles, and descriptions for Google Searches
- How		
    1. Navigate to some Page Component
    2. `import Head from 'next/head';`
    3. Do
        ```js
        <Fragment>
        <Head>
            <title> React Meetups </ title>
            <meta name='description' content='some example descriiption that Google sees' .?
        </ Head >
        <SomeComponent />
        </Fragment>
        ```
    4. We can do this on all Pages!
    5. For Dyanmic Single Pages we can use the dynamic data from our props that we got from `getStaticProps` :
        `{props.meetupData.title}`
    6. Make sure Page Component `props` isn't hardcoded and recieves the new `getStaticProps` data

### Deploying Next.js Projects			
- How		
    1. Vercel is great for Next.js deploying, it does it via Gtihub repos
    2. IF you don't use Vercel then you would have to do 'npm run build'. Vercel runs this for you
    3. If connecting to other servers via code, make sure you open access on them (MongoDB Atlas 0.0.0.0 IP)
- Problem	
    * We get an error when trying to click a button to a dynamic route. The problem lies with the `"fallback" property` in `getStaticProps()`. We have to give it the value of `'blocking'` or `true`, which generates the page on demand. We use `"blocking"` because it doesn't render anything until there's something to show