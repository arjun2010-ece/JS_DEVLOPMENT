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
    "start": "node buildScripts/srcServer.js",
    "localtunnel": "lt --port 3000",
    "share": "npm-run-all --parallel start localtunnel"
    "share1": "npm-run-all --parallel "name of script 1" "name of script 2"
    },

  The script cmd "share" is running both "start" script and "localtunnel" script too 
