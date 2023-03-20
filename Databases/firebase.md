<h1 style="text-align: center;"> Firebase </h1>

- If you don't already have projects [you can download quick samples here](https://firebase.google.com/docs/samples)
<h2 style="text-align: center;"> Get Started with Firebase </h2>

-  [Multiple technologies you can setup Firebase with.](https://firebase.google.com/docs/web/setup)
### Adding Firebase to your Javascript project
1. **Create a Firebase project and register your app** - 
    * Before you can add Firebase to your JS app, you **need to create a Firebase project and register your app with that project**. 
    * When your register your app with Firebase, you'll get a **Firebase configuration object** that you'll use to connect your app with your Firebase project resources
- [Create a Firebase Project](https://firebase.google.com/docs/web/setup#create-project)
- [Register your App](https://firebase.google.com/docs/web/setup#register-app)

2. **Install the SDK and initialize Firebase**
    * The following workflow uses `npm` and requires module bundlers for JS framework tooling because the version 9 Firebase JS SDK is optimized to work w/ [module bundlers](https://firebase.google.com/docs/web/module-bundling) to eliminate unused code and decrease SDK size
        1. Install **Firebase** using **npm**:
            ```npm
            npm install firebase
            ```
        2. Initialize Firebase in your app and create a `Firebase App` object:
            ```js
            import { initializeApp } from 'firebase/app';

            // TODO: Replace the following with your app's Firebase project configuration
            const firebaseConfig = {
            //...
            };

            const app = initializeApp(firebaseConfig);
            ```
            * A `Firebase App` is a container-like object that stores common configuration and shares authentication across Firebase services. After you initialize a Firebase App object in your code, you can add and start using Firebase services
3. **Access Firebase in your app**
    * Firebase services (like `Cloud Firestore`, `Authentication`, `Realtime Database`, `Remote Config`, and more) are available to import within individual sub-packages.  
    The example below shows how you could use the `Cloud Firestore Lite SDK` to retrieve a list of data
        ```js
        import { initializeApp } from 'firebase/app';
        import { getFirestore, collection, getDocs } from 'firebase/firestore/lite';
        // Follow this pattern to import other Firebase services
        // import { } from 'firebase/<service>';

        // TODO: Replace the following with your app's Firebase project configuration
        const firebaseConfig = {
        //...
        };

        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);

        // Get a list of cities from your database
        async function getCities(db) {
        const citiesCol = collection(db, 'cities');
        const citySnapshot = await getDocs(citiesCol);
        const cityList = citySnapshot.docs.map(doc => doc.data());
        return cityList;
        }
        ```
4. **Use a module bundler (webpack/Rollup) for size reduction**
    * The Firebase Web SDK is designed to work with module bundlers to remove any unused code (tree-shaking). It's strongly recommended using this approach for production apps.
    * Tools such as the Angular CLI, Next.js, Vue CLI, or Create React App automatically handle module bundling for libraries installed through npm and imported into your codebase
    * See [Using module bundlers with Firebase](https://firebase.google.com/docs/web/module-bundling)
5. **Available Firebase services for web**
    * Now that you're setup to use Firebase, you can start adding and using any of the following [available Firebase services in your web app](https://firebase.google.com/docs/web/setup#available-libraries)
    * The commands in the link show how to import Firebase libraries installed locally with `npm`. For alternative import options, See the [available libraries documentation](https://firebase.google.com/docs/web/learn-more#available-libraries)

### Add the Firebase Admin SDK to your server
- The Admin SDK is a set of libraries that lets you interact with Firebase from privileged environments to perform actions like:
    * Read and write Realtime Database data with full admin privileges
    * Programmatically send Firebase Cloud Messaging messages using a simple, alternative approach to the Firebase Cloud Messagin server protocols
    * Generate and verify Firebase auth tokens
    * Access Google Cloud resources like Cloud Storage buckets and Cloud Firestore databases associated with your Firebase projects
    * Create your own simplified admin console to do things like up user data or change a user's email address for authentication
1. **Set up a Firebase project and service account**
    * To use the `Firebase Admin SDK`, you'll need the following:
        * A Firebase project
        * A Firebase Admin SDK service account to communicate with Firebase. The service account is created automatically when you create a Firebase project or add Firebase to a Google Cloud project
        * A configuration file with your service account's credentials
    * If you don't already have a Firebase project, you need to create one in the [Firebase console](https://console.firebase.google.com/u/0/). Visit [Understand Firebase Projects](https://firebase.google.com/docs/projects/learn-more) to learn more about Firebase projects
    * [Create a Firebase project](https://firebase.google.com/docs/admin/setup#set-up-project-and-service-account)
2. **Add the SDK**
    * If you are setting up a new project, you need to install the SDK for the language of your choice
    * For `Node.js` the process is as follows:
        1. The `Firebase Admin Node.js SDK` is available on `npm`. If you don't already have a `package.json` file, create one via `npm init.`
        2. Next, install the `firebase-admin` npm package and save it to your `package.json`:
            ```js
            npm install firebase-admin --save
            ```
        3. To use the module in your application, `require` it from any JavaScript file:
            ```js
            const { initializeApp } = require('firebase-admin/app');
            ```
            If you are using ES2015, you can `import` the module:
            ```js
            import { initializeApp } from 'firebase-admin/app';
            ```
3. **Initialize the SDK**
    * Once you have created a Firebase project, you can initialize the SDK with Google Application Default Credentials. Because default credentials lookup is fully automated in Google environments, with no need to supply environment variables or other configuration, this way of initializing the SDK is strongly recommended for applications running in Google environments such as Cloud Run, App Engine, and Cloud Functions.

        To optionally specify initialization options for services such as Realtime Database, Cloud Storage, or Cloud Functions, use the `FIREBASE_CONFIG` environment variable. If the content of the `FIREBASE_CONFIG` variable begins with a `{` it will be parsed as a JSON object. Otherwise the SDK assumes that the string is the path of a JSON file containing the options.
        * Note: The `FIREBASE_CONFIG` environment variable is included automatically in Cloud Functions for Firebase functions that were deployed via the Firebase CLI.
    * Once it is initialized, you can use the `Admin SDK` to accomplish the following types of tasks:
        * [Implement custom authentication](https://firebase.google.com/docs/auth/admin)
        * [Manage your Firebase Authentication users](https://firebase.google.com/docs/auth/admin/manage-users)
        * [Read and write data from the Realtime Database](https://firebase.google.com/docs/database/admin/start)
        * [Send Firebase Cloud Messaging messages](https://firebase.google.com/docs/cloud-messaging/admin/send-messages)
    * The Admin SDK also provides a credential which allows you to authenticate with a `Google OAuth2` refresh token: 
        ```js
        const myRefreshToken = '...'; // Get refresh token from OAuth2 flow

        initializeApp({
        credential: refreshToken(myRefreshToken),
        databaseURL: 'https://<DATABASE_NAME>.firebaseio.com'
        });
        ```
        `OAuth 2.0` refresh tokens are bot supported for connecting to Cloud Firestore

<h2 style="text-align: center;"> Managing your Firebase Projects </h2>

<h3 style="text-align: center;"> Understanding Firebase projects </h3>

### Relationship between Firebase projects, apps, and products
- A `Firebase project` is the top-level entity for Firebase
    * In a project, you can register your Apple, Android, or web apps
    * After your register your apps with Firebase, you can add the `Firebase SDKs` for any number of [Firebase products](https://firebase.google.com/products/), like Analytics Cloud Firestore, Performance Monitoring, or Remote Config.
- Understanding the hierarchy of Firebase projects
    <div align="center"><img width="400px" height="400px" src="https://firebase.google.com/static/docs/projects/images/firebase-projects-hierarchy_projects-apps-resources.png" /> </div>

    * A `Firebase project` - is like a container for all your apps and any resources and services provisioned for the project
    * A Firebase project can have one or more `Firebase Apps` registered to it (for example, both the iOS and Android versions of an app, or both the free and paid versions of an app)
    * All Firebase Apps registered to the same Firebase project `share and have access to all the same resources and services provisioned for the project`. Here are some examples:
        * All the Firebase Apps registered to the same Firebase project share the same backends like Firebase Hosting, Authentication, Realtime Database, Cloud Firestore, Cloud Storage, and Cloud Functions
        * All Firebase Apps registered to the same Firebase project are associated with the same Google Analytics property, where each Firebase App is seperate data stream in that property

### Relationship between Firebase projects and Google Cloud