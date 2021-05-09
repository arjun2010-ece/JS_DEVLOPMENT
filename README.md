1. First we create .editorconfig file in the root and put config. Then go to editorconfig.org and click on "Download a Plugin" and then click on your code editor.
It will redirect you to that code editors marketplace and show you the extention needed to be installed for editor config to take effect. Same config can work across multiple teams just by installing that plugin. In VScode code the code from website and press CTRL + P and paste to open extention and then install and reload. 
You can use .editorconfig or prettier or both to maintain consistency across the project.

2. create package.json file and paste content from below link:
    https://gist.github.com/coryhouse/9bdb4b1f06695e913ce6f8a722d4d9fd 

  Since anyone can publish packages on npm and can contain some vulnerabilities in this too. ALthough npm has "audit" feature to check for vulnerabilities but we can provide a additional security check by a 3rd party package "snyk" that scan/checks for vulnerabilities.

  npm audit ===> provide reports for vulnerabilities in our npm packages installed.

3. Choose Development web servers:
    choices are: 
    http-server, very light weight for normal purposes.
    Express for development of nodejs
    Browsersync - most interesting one--just need to share the URL and Anyone will see the live changes on any device , be it mobile, laptop, desktop on same LAN connections, integrates well wit webpack and express too.
    webpack-dev-server - Its built into webpack, serves from memory, includes hot reloading.

    These cannot be used for production hosting.

    We will use express server for this project purposes.
    Create a buildScripts folder and srcServer.js file inside.
    Also create sec/index.html too.

    Sharing work in progress:: all punches a hole in firewall and share on a URL.
    a. localtunnel, most easiest to work on.
        steps::
            1. npm i localtunnel -g
            2. start your app, e.g npm start
            3. in the terminal:: lt --port 3000
  
    b. ngrok, little bit extra step.
        steps::
            1. Sign up
            2. Install ngrok
            3. Install authtoken
            4. start your app
            5. ./ngrok http 80
    c. Vercel: Quickly deploys NodeJs to the cloud
        steps::
            1. npm i -g vercel
            2. Create start script
            3. Vercel
    d. Surge
        steps::
            1. npm i -g surge
            2. surge

At this stage we tried localtunnel for demo purposes.


4. Automation : Automating the dev environment build process today.

    3 popular choices are::
      Grunt, Gulp, npm scripts

    we use npm scripts. Inside scripts in package json file for running one service is okay. 
    But if we want to run more than one script then we need a package "npm-run-all" from npm.

  "scripts": {
    "prestart": "node buildScripts/startMessage.js",
    "start": "node buildScripts/srcServer.js",
    "localtunnel": "lt --port 3000",
    "share": "npm-run-all --parallel start localtunnel"
    "share1": "npm-run-all --parallel "name of script 1" "name of script 2"
    },

  The script cmd "share" is running both "start" script and "localtunnel" script too 

  We can put a "prestart" script that will run before everytime we fire "npm start" cmds.

5. Transpilers : 
    These are tools that read source code written in one programming language (JS), and produce the equivalent code in another language (which older browsers can process and understand).
  Languages that convert to JS: More than 100 languages.
  https://github.com/jashkenas/coffeescript/wiki/List-of-languages-that-compile-to-JS


  e.g : Babel and typescript.
  We will use Babel transpilers here.

  Since babel is made of presets so learning about them is important and full power of babel is provided with presets through a config file:

  @babel/preset-env : allows us to use latest es6/es7/es8+ features which currently many browsers does not allow support.

  @babel/preset-react : allows us to write react code, since browsers only understand
    javascript so this presets, breaks down JSX react code into javascript and HTML code.
  
  @babel/preset-typescript : Allows us to write typescript code.

  For using babel, we need to put a dedicated config file ".babelrc" in the root of the project or use it in package.json files.

  By default JS use commonJS "require" statement to import.

  Go to buildScripts/startMessage.js, and use "import" statement as in ReactJS.
  and go to "prestart" cmd in package.json file and change node to "babel-node".
  With babel-node, it will be translated much before even node uses it.

  same way we can do in other files also.
  

6. Bundling : Module bundlers are tools frontend developers used to bundle JavaScript modules into a single JavaScript files that can be executed in the browser.

Browser does not support module system.

ex:
 webpack, parcel, browserify, rollup, Snowpack.

 Why Webpack:
  1. Much more than just JS:
    -  CSS
    -  Images
    -  Fonts
    -  HTML
  2. Bundle Splitting
  3. Hot Module reloading

We use webpack here.
Create a file "webpack.config.dev.js" for bundling purposes.
If you see the object there, then "devtool: eval-source-map", for debug or compile our code.
"entry" -- webpack will need an entry point, which then recursively other js files inside that.
"output" -- In development mode, no actual files are generated. Files are served from memory, but still need to write there.

"plugins" -- It enhances the webpacks power.
"module" -- Here, we tell webpack, how to handle diff file types.

 module: {
    rules: [
      { test: /\.js$/, exclude: /node_modules/, use: ["babel-loader"] },
      { test: /\.css$/, use: ["style-loader", "css-loader"] },
    ],
  }

  why write more rules here than javascript ?
  So that we can import them like we import in JS, here css files also.

  get initial webpack dev file from this link:
  https://gist.githubusercontent.com/coryhouse/47c7f268553b4a4ce84c861595d909eb/raw/e64a0742f82eb4ade2c4090600cfa1ccdb6e40a8/webpack.config.dev.js

  After creating webpack.config.dev.js configs, then go to srcServer.js file and integrate these codes to make nodejs use webpack:::::

import webpack from "webpack";
import config from "../webpack.config.dev";

const compiler = webpack(config);

app.use(require("webpack-dev-middleware")(compiler, {
  publicPath: config.output.publicPath
}))
==============

7. Linters : Avoid mistakes and enforce consistency.
  
  ex: ESLint

decisions to make for eslint:
1. which config files to use. currently use 5 formats:
https://eslint.org/docs/user-guide/configuring/configuration-files#configuration-file-formats

2. Which rules to enable ?
 Setup a team meeting to decide which rules to enable since there are dozens of rules we can configure.

3. Which rules justify warnings and which rules justify errors ?
  with warning, can continue development but errors breaks the builds.

4. Eslint for a Framework ?
We can configure eslint for react with eslint-plugin-react packages.
We can do similar things in nodejs or any framework of out choice, not only react.

5. Use a presets ?
We can configure eslint either from scratch or we can use recommended rules and tweak as when we want something.


Since Eslint does not automatically watch files for errors, so there are 2 solutions/plugins to enable that.
eslint-loader or eslint-watch

eslint-loader : tied to webpack, and re-lints all the files upon save.
eslint-watch: not tied to webpack and do file watch too. Better to use this, separate npm packages.

Eslint does not lint experimental JS features, if we want that then we need babel-eslint. and we can see those features in below link::


Why Lint via an AUtomated Build process when we can have linting in code editors ?
Bcoz we want all our Linting decisions in one place for code quality.
Universal config. Part of CI server.

Lets create a .eslintrc.json file in the root, since extention is reqd nowadays as withoud extention its deprecated, so json there.
 and paste the recommended settings from these links::
 https://gist.github.com/coryhouse/61f866c7174220777899bcfff03dab7f

"root": true, tells us that that its root eslint file.
"extends": [
    "eslint:recommended", // we using recommended rules here.
    "plugin:import/errors",
    "plugin:import/warnings"
  ],

"parserOptions": {
    "ecmaVersion": 7, // it tells js version we are using.
    "sourceType": "module"
  },

And rules section below for overriding.
"rules": {}
 e.g:
 "rules": {
   "no-console": 1
 }

 we are over-riding consoles, meaning, we do not want consoles and number values tells whether its warning(1) or errors(2) or disable completely(0).

if consoles are there then show warnings(1).

"type of rules": "show warning/error if present".

COme to package.json and create a script for linting.

esw is executable for eslint-watch in package.json files.

"lint": "esw webpack.config.* src buildScripts"

esw watches for linting rules in webpack config file, src folder and buildScripts folder.

If some rules we cannot fix then go to the files and put this:

for disabling the rule in entire file::
/* eslint-disable no-console */   

for disabling the rule in only one line::
// eslint-disable-line no-console


If no errors/warnings then it will come clean.

For watching all the time::
"lint:watch": "npm run lint -- --watch",  extra set of dash is reqd for args purposes.

If we want linting to work when we start out app then use this commands:

"start": "npm-run-all --parallel open:src lint:watch",

with npm-run-all pckg we running 2 scripts here.


7. Deployment:
  Host the backend on AWS/Google cloud/Heroku.
  Host frontend on Netlify/Github pages/ Surge

Paths to production:
npm start (development)
npm run build (production build)
npm run deploy (deploy production)

WEBSITE TO FIND REACTJS/NEXTJS/GATSBY boilerplates::

https://www.javascriptstuff.com/react-starter-projects/



