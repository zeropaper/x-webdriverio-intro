# x-webdriverio-intro

Exercise to introduce
- [BEM CSS](https://css-tricks.com/bem-101/) technique.
- E2E (end-to-end) testing with [WebdriverIO](http://webdriver.io/).

As usual, make a commit (and push it) after each step.

## Steps

### Build

Based on the previous exercises.

- Move the code of the `chess-queen` exercise into a `src` folder
- Change the extension of your styles to `scss`
- Create a `package.json` and
  - Install the developent dependencies (node-sass, mkdirp, cp, browserify, uglify-js, jshint)
  - Add the build scripts (for the the JS and SCSS, like in the loader simulation exercise)
  - In your `prebuild` script, use `mkdirp` to create a folder `docs/image-diffs`

__Note:__ You can add additional scripts using `nodemon` (have a look at its help) to avoid to run the commands manually each time something changes and therefore ease development.

### Replace the "table" with "div"

If the usage of a `table` element was OK for the first part of the exercise but it might not be suitable for the following steps (because of the way table elements are rendered by the browser).
This step is meant to exercise the [BEM CSS](https://css-tricks.com/bem-101/) technique.

- Add the following code just after the opening `body`
  ````xml
  <header>
    <label><input type="checkbox" name="tilt"> Tilt chessboard</label>
  </header>
  ````
- Replace the `table` element of your `src/index.html` with
  ````xml
  <div class="chessboard">
    <div class="chessboard__figures"></div>
    <div class="chessboard__table"></div>
  </div>
  ````
- Change the `renderChessboard` function to generate something that looks like the following code
  ````xml
  <div class="chessboard__table">
    <div class="chessboard__row">
      <div class="chessboard__cell"></div>
      <div class="chessboard__cell chessboard__cell--black"></div>
      <div class="chessboard__cell"></div>
      <div class="chessboard__cell chessboard__cell--black"></div>
      <div class="chessboard__cell"></div>
      <div class="chessboard__cell chessboard__cell--black"></div>
      <!-- ... -->
    </div>
    <div class="chessboard__row">
      <div class="chessboard__cell chessboard__cell--black"></div>
      <div class="chessboard__cell"></div>
      <div class="chessboard__cell chessboard__cell--black"></div>
      <div class="chessboard__cell"></div>
      <div class="chessboard__cell chessboard__cell--black"></div>
      <div class="chessboard__cell"></div>
      <!-- ... -->
    </div>
    <!-- ... -->
  </div>
  ````
- Make the necessary changes to your `src/styles.scss`. In the end, you should have a CSS outline similar to:
  ````css
  *,
  *:before,
  *:after { /* ... */ }

  body { /* ... */ }

  .chessboard { /* ... */ }
    .chessboard--tilted { /* ... */ }
    .chessboard__table { /* ... */ }
    .chessboard__row { /* ... */ }
    .chessboard__cell { /* ... */ }
      .chessboard__cell:hover { /* ... */ }
      .chessboard__cell--black { /* ... */ }
        .chessboard__cell--black:hover { /* ... */ }
      .chessboard__cell--highlight:after { /* ... */ }
  ````
  __Note:__ have a close look at what the [`&` sign does in SASS](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#Referencing_Parent_Selectors_____parent-selector) and how the above selectors are indented.


### Work on styles

Go to https://github.com/zeropaper/xt-sc-chess-queen/tree/master/sc, have a look at the different pictures (the ones wich have a name starting with `highlight-`) and change your `src/style.scss` to get as close as possible of the design.  
The slightly green is where a click shows how the `:hover` style should look like.

<details>
  <summary>Screenshot</summary>
  
![screenshot](https://github.com/zeropaper/xt-sc-chess-queen/blob/master/sc/highlight--iphone-6-p.png?raw=true)

</details>


### Using your array matrix library

When you write code, you want to stay DRY (__D__on't __R__epeat __Y__ourself), so now that you have a nice, tested and bug free library (from the [Mocha intro](//github.com/zeropaper/x-mocha-intro) exercise) it would be good to re-use it.

- Add a dependency to your `array-matrix` in the `package.json` (in `dependencies` not `devDependencies`!)
  You can do so like: `npm i --save <your-gh-username>/<name-of-your-gh-repository>`
- In your `src/index.js` add
  ````js
  var arrayMatrixLib = require('array-matrix');
  var createMatrix = arrayMatrixLib.createMatrix;`
  ````


### Add WebdriverIO

- You will need to install something a bit special (called cairo)... to do so, refer to https://www.npmjs.com/package/canvas#installation for your operating system (__NOT__ `npm install canvas` ;) , the table which is below)
- You will also have to install Java (under Ubuntu `sudo apt-get install default-jre`)
- In your terminal enter the following command: `npm i -D wdio zeropaper/xt-sc-chess-queen` to install WebdriverIO as a development dependency
- `./node_modules/.bin/wdio` to start the WebdriverIO setup wizard and answer to the questions
  - Where do you want to execute your tests?
    "On my local machine"
  - Which framework do you want to use?
    "mocha"
  - Shall I install the framework adapter for you?
    "Yes"
  - Where are your test specs located?
    ./test/*.spec.js
  - Which reporter do you want to use?
    "spec"
  - Shall I install the reporter library for you?
    "Yes"
  - Do you want to add a service to your test setup?
    "selenium-standalone" and "static-server"
  - Shall I install the services for you?
    "Yes"
  - Level of logging verbosity
    "silent"
  - In which directory should screenshots gets saved if a command fails?
    "./errorShots/"
  - What is the base url?
    "http://localhost:9090"
- Move the `wdio.conf.js` file that the wizard created at the root of the project into the `test` folder and change it to __make sure__ to have the following code in it:
  
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
- Change your `package.json` `test` script to `npm run build && wdio test/wdio.conf.js`


### Create your _spec_

Create a file `test/chessboard-test.spec.js` with the following code.

````js
const assert = require('assert');
const checkStyle = require('xt-sc-chess-queen');

function matrixFillArray(matrix, array) {
  var e = 0;
  matrix.forEach(function(row, rowNum){
    row.forEach(function(col, colNum){
      matrix[rowNum][colNum] = array[e];
      e++;
    });
  });
  return matrix;
}

describe('Chessboard', function() {
  var elements;
  var matrix = createMatrix(8, 8);

  before(function() {
    browser.url('http://localhost:9090');
  });

  it('has 64 td elements', function() {
    elements = browser.elements('td').value;
    assert(elements.length, 64);
  });

  it('highlights the possible moves when a td is clicked', function() {
    //
    // Look at the documentation of WebdriverIO (in the API section of the site)
    // and write some code to make a click on the cell at the 4th row, 5th column of the chessboard
    //

    var highlightedElements = browser.elements('.chessboard__cell--highlight').value;
    assert(highlightedElements.length, 28);
  });


  describe('normal design', function() {
    Object.keys(checkStyle.resolutions).forEach(key => {
      it('has the right styles for ' + key.split('-').join(' '), function () {
        checkStyle(browser, key, 'highlight', './docs/image-diffs', 5);
      });
    });
  });


  describe.skip('tilted design', function() {
    it('can be toggled', function() {
      // 
      // Look at the documentation of WebdriverIO (in the API section of the site)
      // and write some code to:
      // - Make a click on the checkbox
      // - Pause the browser slightly longer than the transition duration
      // - Also verify that the chessboard get the chessboard--tilted class
      // 
    });

    Object.keys(checkStyle.resolutions).forEach(key => {
      it('has the right styles for ' + key.split('-').join(' '), function () {
        checkStyle(browser, key, 'tilted', './docs/image-diffs', 5, 600);
      });
    });
  });
});
````
