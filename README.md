# x-webdriverio-intro

Exercise to introduce E2E (end-to-end) testing with [WebdriverIO](http://webdriver.io/).

## Steps

### Build

Based on the previous exercises.

- Move the code of the `chess-queen` exercise into a `src` folder
- Change the extension of your styles to `scss`
- Create a `package.json` and
  - Install the developent dependencies (node-sass, mkdirp, cp, browserify, uglify-js, jshint)
  - Add the build scripts (for the the JS and SCSS, like in the loader simulation exercise)
  - Add a dependency to your `array-matrix` (under `dependencies` not `devDependencies`!)
    You can do so like: `npm i --save <your-gh-username>/<name-of-your-gh-repository>`


### Using your array matrix library

- In your terminal enter the following command: `npm i -D wdio` to install WebdriverIO as a development dependency
- `./node_modules/.bin/wdio` to start the WebdriverIO setup wizard and answer to the questions
  - Where do you want to execute your tests?
    On my local machine
  - Which framework do you want to use?
    mocha
  - Shall I install the framework adapter for you?
    Yes
  - Where are your test specs located?
    ./test/specs/**/*.js
  - Which reporter do you want to use?
    spec
  - Shall I install the reporter library for you?
    Yes
  - Do you want to add a service to your test setup?
    selenium-standalone and static-server
  - Shall I install the services for you?
    Yes
  - Level of logging verbosity
    silent
  - In which directory should screenshots gets saved if a command fails?
    ./errorShots/
  - What is the base url?
    http://localhost:9090
- Move the `wdio.conf.js` file that the wizard created at the root of the project into the test folder and change it to __make sure__ to have the following code in it:
  
  ````js
  // ...
  capabilities: [
    // {
    //     // maxInstances can get overwritten per capability. So if you have an in-house Selenium
    //     // grid with only 5 firefox instances available you can make sure that not more than
    //     // 5 instances get started at a time.
    //     maxInstances: 5,
    //     //
    //     browserName: 'firefox'
    // },
    {
        // maxInstances can get overwritten per capability. So if you have an in-house Selenium
        // grid with only 5 firefox instances available you can make sure that not more than
        // 5 instances get started at a time.
        maxInstances: 5,
        //
        browserName: 'chrome',
        chromeOptions: {
            // args: ['--headless', '--disable-gpu', '--window-size=1280,800']
        }
    }
    ],
    //
    // ===================
    // Test Configurations
    // ===================
    // Define all options that are relevant for the WebdriverIO instance here
    //
    // By default WebdriverIO commands are executed in a synchronous way using
    // the wdio-sync package. If you still want to run your tests in an async way
    // e.g. using promises you can set the sync option to false.
    sync: true,
    //
    // Level of logging verbosity: silent | verbose | command | data | result | error
    logLevel: 'silent',
    //
    // Enables colors for log output.
    coloredLogs: true,
    //
    // If you only want to run your tests until a specific amount of tests have failed use
    // bail (default is 0 - don't bail, run all tests).
    bail: 0,
    //
    // Saves a screenshot to a given path if a command fails.
    screenshotPath: './errorShots/',
    //
    // Set a base URL in order to shorten url command calls. If your url parameter starts
    // with "/", then the base url gets prepended.
    baseUrl: 'http://localhost:9090',
    //
    // Default timeout for all waitFor* commands.
    waitforTimeout: 10000,
    //
    // Default timeout in milliseconds for request
    // if Selenium Grid doesn't send response
    connectionRetryTimeout: 90000,
    //
    // Default request retries count
    connectionRetryCount: 3,
    //
    // Initialize the browser instance with a WebdriverIO plugin. The object should have the
    // plugin name as key and the desired plugin options as properties. Make sure you have
    // the plugin installed before running any tests. The following plugins are currently
    // available:
    // WebdriverCSS: https://github.com/webdriverio/webdrivercss
    // WebdriverRTC: https://github.com/webdriverio/webdriverrtc
    // Browserevent: https://github.com/webdriverio/browserevent
    // plugins: {
    //     webdrivercss: {
    //         screenshotRoot: 'my-shots',
    //         failedComparisonsRoot: 'diffs',
    //         misMatchTolerance: 0.05,
    //         screenWidth: [320,480,640,1024]
    //     },
    //     webdriverrtc: {},
    //     browserevent: {}
    // },
    //
    // Test runner services
    // Services take over a specific job you don't want to take care of. They enhance
    // your test setup with almost no effort. Unlike plugins, they don't add new
    // commands. Instead, they hook themselves up into the test process.
    services: ['selenium-standalone','static-server'],
    seleniumLogs: './selenium.log',
    staticServerFolders: [
    {
      mount: '/',
      path: './docs'
    }
    ],
    staticServerPort: 9090,
    // ...
    ````


### Add WebdriverIO

Use the wizard provided