# 📦 How to Build Your Electron Application

This guide will walk you through the process of building and bundling a Vue-powered Electron application using Electron Forge.

---

## 🛠️ Step 1: Build the Vue Web App

1. **Navigate into the Vue project directory**:
   ```bash
   cd src/vue-project
   ```

2. **Install all of the dependencies** (your node modules):

    ```bash 
    npm install
    ```

3. **Fix TypeScript declaration error**
When attempting to run the build of our vue app right away, you might notice the following error popping up:

    ```
    error TS7016: Could not find a declaration file for module './components/FullScreenMessage.vue'. It implicitly has an 'any' type.

    3 import FullScreenMessage from './components/FullScreenMessage.vue';
    ```

To fix this issue and to make sure that our _TypeScript building process_ can read our vue files with the correct types, we need to do the following:

- Create a __shims-vue.d.ts__ file in the _src/_ directory of our vue project:

    ```ts
    declare module '*.vue' {
    import type { DefineComponent } from 'vue'
    const component: DefineComponent<{}, {}, any>
    export default component
    }
    ```

    
- This file should also should then be included in both our __tsconfig.node.json__ and __tsconfig.app.json__ files:

    ```json
    {
        "include": [
            "...",
            "shims-vue.d.*"
    ]
    }
    ```


4. **Build the Vue App**
Now with all of that ready, we can run the following command to build our project:

    ```bash 
    npx vite build --base ./
    ```

This will generate our hostable web app in our _dist/_ directory, which includes our __index.html__ file that we can load in our Electron app.

> [!NOTE]
> The ```--base ./``` flag that we include is to make sure that all of our CSS and JS files are loaded in our __index.html__ from it's relative path, so that _file://_ loading works properly. This is essentially to avoiding that white screen when trying to open our __index.html__ file which is caused by this error:

> [!CAUTION]  
> Access to CSS stylesheet at 'file:///.../src/vue-project/dist/assets/index-BHK2zz_v.css' from origin 'null' has been blocked by CORS policy: Cross origin requests are only supported for protocol schemes: chrome, chrome-extension, chrome-untrusted, data, http, https, isolated-app.


Congratulations! You just built your vue application 🎉 Halfway there!

<hr>

## ⚡ Step 2: Bundle the Electron App

1. Navigate back to the _root_ directory of your electron app:

    ```bash 
    cd ../../
    ```

2. Install all of the Electron dependencies (node modules):

    ```bash 
    npm install
    ```

3. Update your `main.js` file to load the index.html file of your built Vue application:

    ```js
    // ❌ Remove the dev server reference
    // mainWindow.loadURL('http://localhost:5173/');

    // ✅ Load the built Vue app
    mainWindow.loadFile(path.join(__dirname, 'vue-project/dist/index.html'));
    ```

4. Next, we need to initialise our Electron application to use Electron Forge to make our app:

    ```bash
    npm install --save-dev @electron-forge/cli  

    npx electron-forge import
    ```
    This will install all of the required dependencies and add a `forge.config.js` file, as well as add new scripts to our `package.json` file.

5. You can run your Electron application to make sure your Vue application is displaying correctly:

    ```bash
    npm run start
    ```

6. And finally, we can run the following command to package and make our Electron app:

    ```bash
    npm run make
    ```

    After the script runs, you should see an _out_ folder containing both the distributable and a folder containing the packaged application code. The distributable in the _out/make_ folder should be ready to launch! 

    If you get the following error:

    ```DeprecationWarning: Calling promisify on a function that returns a Promise is likely a mistake.```

    Don't panic, it's not a critical error and can be ignored for now.

<hr>

### ✅ Success!

Your Electron & Vue app is now built and bundled. You should see your application in the _out/make_ directory, ready for distribution.

🎉 Congratulations! You've built a full-stack desktop app. Time to celebrate! 🎉

---

> [!TIP]
> Just a reminder, that even though Electron allows for cross-platform building, you will need to generate an `.exe`/`.msi` on a Windows computer, and an `.app` on MacOS.