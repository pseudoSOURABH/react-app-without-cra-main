# react-app-without: create-react-app script(from scratch)

Step by Step
1. Make sure node is installed in your system
Install Node.js in your system and make sure its installed by typing node -v in your terminal.

2. Create project folder and package.json
Create a project folder of any name and navigate to the folder and then use npm init to create a package.json file inside the folder. Navigate to the folder.

3. Install webpack dependencies
npm i --save-dev webpack webpack-cli webpack-dev-server
webpack — Will allow us to bundle all of our code into a final build
webpack-cli — CLI tool for providing a flexible set of commands for developers to increase speed when setting up a custom webpack project. If you’re using webpack v4 or later and want to call webpack from the command line, you need this
webpack-dev-server — Webpack dev server is a mini Node.js Express server.It uses a library called SockJS to emulate a web socket. Will enable us to create a localhost dev environment
4. Install the Babel dependencies
npm i --save-dev babel-loader @babel/preset-env @babel/core 
@babel/plugin-transform-runtime 
@babel/preset-react 
@babel/eslint-parser 
@babel/runtime
@babel/cli
babel-loader — allows transpiling JavaScript files using babel and webpack. exposes a loader-builder utility that allows users to add custom handling of Babel’s configuration for each file that it processes.
@babel/preset-env — allows you to use the latest JavaScript without needing to micromanage which syntax transforms (and optionally, browser polyfills) are needed by your target environment(s). This both makes your life easier and JavaScript bundles smaller!
@babel/core — core package
@babel/plugin-transform-runtime — A plugin that enables the re-use of Babel’s injected helper code to save on codesize.
@babel/preset-react — use react preset when we are using Reactjs. Helps in converting html files to react based file
babel-eslint — parser that allows ESLint to run on source code that is transformed by Babel
@babel/runtime — package that contains a polyfill and many other things that Babel can reference.
@babel/cli — command line interface to use babel
5. Install required Linters and path
npm i --save-dev eslint eslint-config-airbnb-base 
eslint-plugin-jest 
eslint-config-prettier
path
6. Install react and react-dom
npm i react react-dom
7. Create index.html file
Create folder called “public” in the root of the project. Inside that, create index.html.

8. Create App.js file 
Create src folder and inside that create a file called App.js. Add the following code to it:
///
const App = () =>{
    return (
        <h1>
            Welcome to React App thats build using Webpack and Babel separately
        </h1>
    )
}

export default App
///


8. Create index.js file
Create an index.js file at the root of project or anywhere you wish to have. This will act as entry point for our webpack.

Add the following code to it:
///
import React from "react";
import reactDom from "react-dom";
import App from "./src/App"

reactDom.render(<App />, document.getElementById("root"));
///


9. Create webpack.config.js file
Create a file called webpack.config.js at the root of project and add the following code. On a higher note, this file contains configs that takes care of bundling the files into one single file and setting up the dev server.

The comments in the code helps us understand what each line does:
const path = require("path");

/*We are basically telling webpack to take index.js from entry. Then check for all file extensions in resolve. 
After that apply all the rules in module.rules and produce the output and place it in main.js in the public folder.*/

module.exports={
    /** "mode"
     * the environment - development, production, none. tells webpack 
     * to use its built-in optimizations accordingly. default is production 
     */
    mode: "development", 
    /** "entry"
     * the entry point 
     */
    entry: "./index.js", 
    output: {
        /** "path"
         * the folder path of the output file 
         */
        path: path.resolve(__dirname, "public"),
        /** "filename"
         * the name of the output file 
         */
        filename: "main.js"
    },
    /** "target"
     * setting "node" as target app (server side), and setting it as "web" is 
     * for browser (client side). Default is "web"
     */
    target: "web",
    devServer: {
        /** "port" 
         * port of dev server
        */
        port: "9500",
        /** "static" 
         * This property tells Webpack what static file it should serve
        */
        static: ["./public"],
        /** "open" 
         * opens the browser after server is successfully started
        */
        open: true,
        /** "hot"
         * enabling and disabling HMR. takes "true", "false" and "only". 
         * "only" is used if enable Hot Module Replacement without page 
         * refresh as a fallback in case of build failures
         */
        hot: true ,
        /** "liveReload"
         * disable live reload on the browser. "hot" must be set to false for this to work
        */
        liveReload: true
    },
    resolve: {
        /** "extensions" 
         * If multiple files share the same name but have different extensions, webpack will 
         * resolve the one with the extension listed first in the array and skip the rest. 
         * This is what enables users to leave off the extension when importing
         */
        extensions: ['.js','.jsx','.json'] 
    },
    module:{
        /** "rules"
         * This says - "Hey webpack compiler, when you come across a path that resolves to a '.js or .jsx' 
         * file inside of a require()/import statement, use the babel-loader to transform it before you 
         * add it to the bundle. And in this process, kindly make sure to exclude node_modules folder from 
         * being searched"
         */
        rules: [
            {
                test: /\.(js|jsx)$/,    //kind of file extension this rule should look for and apply in test
                exclude: /node_modules/, //folder to be excluded
                use:  'babel-loader' //loader which we are going to use
            }
        ]
    }
}



10. Create .babelrc file
Create a file called .babelrc at the root and add the following code.

This is the configuration file for Babel, and you’ll use it to tell babel to use the plugin and presets defined inside.
{

    /*
        a preset is a set of plugins used to support particular language features.
        The two presets Babel uses by default: es2015, react
    */
    "presets": [
        "@babel/preset-env", //compiling ES2015+ syntax
        "@babel/preset-react" //for react
    ],
    /*
        Babel's code transformations are enabled by applying plugins (or presets) to your configuration file.
    */
    "plugins": [
        "@babel/plugin-transform-runtime"
    ]
}

11. Update package.json file
Add the start and build scripts to it — line number 7 and 8.

The start script asks to run the webpack-dev-server in our current project at port 9500, from the public folder.
The build command asks us to build this package in main.js file. It actually runs all logic in webpack.config.js file.
{
  "name": "new-react-app",
  "version": "1.0.0",
  "description": "A new react app without CRA",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server .",
    "build": "Webpack .",
    "test": "test"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.17.2",
    "@babel/plugin-transform-runtime": "^7.17.0",
    "@babel/preset-env": "^7.16.11",
    "@babel/preset-react": "^7.16.7",
    "@babel/runtime": "^7.17.2",
    "babel-eslint": "^10.1.0",
    "babel-loader": "^8.2.3",
    "eslint": "^8.9.0",
    "eslint-config-airbnb-base": "^15.0.0",
    "eslint-config-prettier": "^8.3.0",
    "eslint-plugin-jest": "^26.1.0",
    "webpack": "^5.68.0",
    "webpack-cli": "^4.9.2",
    "webpack-dev-server": "^4.7.4"
  },
  "dependencies": {
    "@babel/cli": "^7.17.0",
    "path": "^0.12.7",
    "react": "^17.0.2",
    "react-dom": "^17.0.2"
  }
}

12. Final project folder structure has to be like this:
>node_modules
>public : index.html
>src: App.js
.babelrc
index.js
package-lock.json
package.json
webpack.config.js

13. Run “npm run build”
Once added the above code, hit npm run build. it generates main.js file in our public folder. The file is actually over 1 MB in size. This is our development build.
14. Run “npm start”
Start the application by giving npm start from terminal. This will start our dev server.

The final code can be found in the repo link I shared above.

💡Note: If you wish to share and reuse your React components across multiple projects, you can use an open-source toolchain like Bit which lets you create reusable components. You can develop components in isolation and publish them as standalone entities that can be searched, tested, and installed across projects. Find out more here, and here.

How to Create a Composable React App with Bit
In this guide, you'll learn how to build and deploy a full-blown composable React application with Bit. Building a…
bit.dev

Other Key Findings
Changing the build to production
Now we can try changing it to production build. For this you need to make the following change to webpack.config.js file.
mode: "production"
now running npm run build will create main.js file again but the size will be very less (<200kb).
With the optimization from 1000 KB to 200 KB, we might want to use production build always. But, we should use development mode while doing development because the hot reloading is faster in development mode.
Hot Module Replacement
HMR is handled by webpack-dev-server. We can use HMR without page load option too. Setting the required options helps greatly in performance aspect.
Check the below code snippet for various scenarios:
//If you want to use HMR but no live reload then use the below config in webpack.config.js
devServer: {
        hot: true ,
        liveReload:false
    }

//If you want don't want to use HMR but want to use live reload then,
devServer: {
        hot: false ,
        liveReload: true
    },

//If you want don't want to use live reload then,
devServer: {
        hot: false , //this is mandatory to be set to false for this
        liveReload: false
    },
References: these are the links which helped me through-out the development of this project . 
HMR with webpack — https://webpack.js.org/guides/hot-module-replacement/
Different ways to reduce the bundle size — https://blog.jakoblind.no/3-ways-to-reduce-webpack-bundle-size/
Understanding devserver and working in details — https://webpack.js.org/configuration/dev-server/#devserverlivereload
Minimizing the bundle for production — https://webpack.js.org/plugins/mini-css-extract-plugin/#minimizing-for-production
How does production site is built using webpack — https://webpack.js.org/guides/production/
Setting up perfect devpack server — https://linguinecode.com/post/how-to-setup-webpack-dev-server-react-babel
Loaders in details — https://webpack.js.org/concepts/loaders/
Understanding babel-preset-env in details — https://blog.jakoblind.no/babel-preset-env/
