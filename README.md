#Testrail Reporter for Webdriver.io

Pushes test results into Testrail system.

## Precondition

In TestRail:
 - Create new test suite with test cases, that you will be automate.
 ![Adding a test suite](/images/test_cases.png)
 - Create new test run for each instance. For ex., if you run your tests on Chrome, Edge and Firefox browser, you have to add three runs for each.
  ![Adding a test suite](/images/test_runs.png)
  
In solution:

 
## Usage
Ensure that your testrail installation API is enabled and generate your API keys. See http://docs.gurock.com/

Create wdio.testraildata.json file and set configs for testrail runs:
 ```json
 {
  "smoke": {
    "runIds": {
      "Firefox": 11111,
      "Edge": 22222,
      "Chrome" : 33333
    }
  },
  "regression": {
    "runIds": {
      "chrome" : 44444
    }
  }
 ```
 
 where:
 **smoke** and **regression**: *string*  names of suites,
 **runIds** ids of runs in testrail related to defined instances.


Add reporter to wdio.conf.js:

```Javascript
let WdioTestRailReporter = require('./packages/wdio-testrail-reporter/lib/wdio-testrail-reporter');

...

    reporters: [[ WdioTestRailReporter,
    {
      testRailUrl: "yourdomain.testrail.net",
      username: "username",
      password: "password",
      configFilePath: 'configs/wdio/wdio.testraildata.json'
    }]],

```


Mark your mocha test names with ID of Testrail test cases. Ensure that your case ids are well distinct from test descriptions.
 
```Javascript
it("C123 Authenticate with invalid user", . . .
it("Authenticate a valid user C321", . . .
```

Only passed or failed tests will be published. Skipped or pending tests will not be published resulting in a "Pending" status in testrail test run.

## Options

**testRailUrl**: *string* domain name of your Testrail instance (e.g. for a hosted instance instance.testrail.net)

**username**: *string* user under which the test run will be created (e.g. jenkins or ci)

**password**: *string* password or API token for user

**configFilePath**: *string* file path to TestRail config file

## Running tests

When you run your tests, please specify in test script the suite name, where you run your tests. Suite name should be equal to name in *wdio.testrail.json* file.
For ex.,

wdio.testrail.json - 

 ```json
 {
  "smoke": {
    "runIds": {
      "Firefox": 11111,
      "Edge": 22222,
      "Chrome" : 33333
    }
  },
  "regression": {
    "runIds": {
      "chrome" : 44444
    }
  }
 ```
Test script - 
**"test":** "wdio configs/wdio/wdio.config.js --suite smoke"

