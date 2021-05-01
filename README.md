1. First we create .editorconfig file in the root and put config. Then go to editorconfig.org and click on "Download a Plugin" and then click on your code editor.
It will redirect you to that code editors marketplace and show you the extention needed to be installed for editor config to take effect. Same config can work across multiple teams just by installing that plugin. In VScode code the code from website and press CTRL + P and paste to open extention and then install and reload. 
You can use .editorconfig or prettier or both to maintain consistency across the project.

2. create package.json file and paste content from below link:
    https://gist.github.com/coryhouse/9bdb4b1f06695e913ce6f8a722d4d9fd 

  Since anyone can publish packages on npm and can contain some vulnerabilities in this too. ALthough npm has "audit" feature to check for vulnerabilities but we can provide a additional security check by a 3rd party package "snyk" that scan/checks for vulnerabilities.

  npm audit ===> provide reports for vulnerabilities in our npm packages installed.
