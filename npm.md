npm commands and overview:
==========================
Concept is similar to pip for python

* *package.json* - This is the metadata file that tells us what we have installed
   * *scripts* -
      * start - Give startup command so `npm start` can work
      * test - Give test command to kick off automated tests
      * server - Can do `npm run server`.  Not really sure about this vs start, I think start is just some convention so npm allows you to skip the "run" portion of the command.  Otherwise you can list any label under scripts and do `npm run <whatever>` and it will do wwhatever you define
   * *dependencies* - list all libraries you need for your apps
   * *devDependencies* - like dependencies, but for dev environment only, ignored if you use --production flag

# Initialize a new npm file aka package.json file
```
npm init
```

# Add something to package.json
--save flag means download package AND put it in package.json file  
--save-dev is similar to --save, but for devDependencies
```
npm install <libname> --save
```

# Get all libraries from an existing package.json file, ie when moving to new server:
```
npm install
OR
npm install --production
```

# Define version in package.json
* "*^xx.yy.zz*" - look for latest minor version + patch, keep major version  
* "*~xx.yy.zz*" - only look for latest patch, no new major or minor version  
* "*xx.yy.zz*" - no smart match, just get exact version match  
* "*\**" - Just get latest version of whatever (not recommended, try to stick to same major version)

# Check npm root, aka where stuff is installed
```
npm root -g
```
# Check Installed Libraries:
```
npm list
OR to avoid recursive dependencies:
npm list --depth 0|1
```

# Uninstall Library:
```
npm remove <lib> -g
```


Common libraries I should learn
=================================
* live-server - simple webserver to test apps
* gulp - Streaming builds (?)
* lodash - Easy to work with arrays and objects in javascript (not sure in what way)
* browserify - Make it easy to load dependencies files from browser env using "require" syntax
* grunt-cli - CLI interface for grunt, which can be kind of like "make" for javascript projects i guess.  Minify, compile, test, lint, etc via grunt
