#Testrail Reporter for Webdriver.io

Pushes test results into Testrail system.

## Precondition

- Add *testrail-reporter* folder to the AT solution

In TestRail:
 - Create a new test suite with test cases that you will automate.
 ![Adding a test suite](/images/test_cases.png)
 - Create a new test run for created test suite and for each run instance. For ex., if you run your tests on Chrome, Edge and Firefox browser, you have to add three runs for each.
  ![Adding a test suite](/images/test_runs.png)
 
## Usage
Ensure that your testrail installation API is enabled and generate your API keys. See http://docs.gurock.com/

- Create wdio.testraildata.json file and set configs for testrail runs:
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
 
 where
 
 **smoke** and **regression**: *string*  names of suites
 
 **runIds** IDs of runs in the testtrail associated with specific instances (you added mentioned runs in *Precondition*)

- Add reporter to wdio.conf.js:

```Javascript
let WdioTestRailReporter = require('../../automation/wdio-testrail-reporter/testrail-reporter/reporter').default;

...

    reporters: [[ WdioTestRailReporter,
    {
      testRailUrl: "yourdomain.testrail.net",
      username: "username",
      password: "password",
      configFilePath: 'configs/wdio/wdio.testraildata.json'
    }]],

```

## Options

**testRailUrl**: *string* domain name of your Testrail instance (e.g. for a hosted instance instance.testrail.net)

**username**: *string* user under which the test run will be created (e.g. jenkins or ci)

**password**: *string* password or API token for user

**configFilePath**: *string* file path to TestRail config file


## Preparing tests

Mark your mocha test names with ID of Testrail test cases. Ensure that your case ids are well distinct from test descriptions.
 
```Javascript
it("C123 Authenticate with invalid user", . . .
it("Authenticate a valid user C321", . . .
```

Only passed or failed tests will be published. Skipped or pending tests will not be published resulting in a "Pending" status in testrail test run.


## Running tests

When you run your tests, please indicate in the test script the name of the suite, where you run your tests. Suite name must match the name in *wdio.testrail.json* file.
For ex. ,

wdio.testrail.json - 

 ```json
 {
  "smoke": {
    "runIds": {
      "Firefox": 11111,
      "Edge": 22222,
      "chrome" : 44444
    }
  },
  "regression": {
    "runIds": {
      "chrome" : 44444
    }
  }
 ```
Test script - 
"test": "wdio configs/wdio/wdio.config.js **--suite smoke**"

