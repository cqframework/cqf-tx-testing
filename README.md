# cqf-tx-testing
This is a node testing tool for running and reporting on Postman collections using the Postman Collection Runner, Newman. This app contains a reporter that returns results in a json file to either a specified output directory or, by default, a results diretory. The output directory will be created, if it doesn't exist.  

## Setting up the environment
### Environment Variables
 Either edit workspace.postman_globals.json, replacing '*****' with actual values or create a global Postman environment variables file that contains the following:
 
    {{basicUser}} -- your user name on the server being tested.
    {{basicPass}} -- your password on the server to be tested.
    {{SERVER_URL}} -- the url of the server to be tested.
### Install Newman

Install newman

[Newman](https://learning.postman.com/docs/collections/using-newman-cli/command-line-integration-with-newman/)

    npm i -g newman

Additional [third party newman reporters](https://www.npmjs.com/search?q=newman-reporter) are available to install. This list is maintained by npm. Each reporter from this list has a link to a page that contains instructions on installation and running that reporter.


## Running the collection from CLI
### Running the tests

Running these tests using either Newman or this application runs all the tests and assertions in the collection. One way to use this is to run using the CLI and then look at failing tests, maybe running each individually in Postman to get more details. Importing the collection and environment into Postman would facilitate this. 

#### To run with Newman
To run using Newman use the following command using the path to the 2 files.

    newman run cqf-measures.postman_collection.json -e workspace.postman_globals.json    

#### To run with this application
Run npm to build the packages

    npm install
Install the date-fns package
    
    npm install -g date-fns --save
You may have to run npm install after this to make sure the package is there for node to use it.

There are default parameters in the program that can be set on the commandline.
    
    Output directory: './' (current directory).
    The path and file name of the Postman collection file: '{Absolute path}collections/cqf-measures-terminology-service-tests.postman_collection.json'
    The path and file name of the environment file where the username, password, and server url are defined: '{Absolute path}/environment/workspace.postman_globals.json';

These will be used unless changed on the command line as (notice no space after the '='). Any that are missing will use the default settings. The output file name will be testResults_yyyyMMddhhmm (year + month + day of year + hour + minute of the beginning of the test run) 

    -op=<your output location>
    -cs=<the collection location and file name>
    -es=<the environment settings location and file name>

So the command line would be similar to
    node cqf-tx-testing.js -cs=/user/collections/newCollection.json -es=/user/environment/postmanEnvironment.json -op=/user/results

Output will be sent to the console and there should be a new json file /user/results/testResults_{time stamp}.json that contains the results.

### Results file format
The results file consists of summary that contains counts of:
    
    Counts of
        passing assertions
        failing assertions
        total assertions
        tests with no assertions
        total tests
    Lists of 
        passing tests
        failing tests
        tests with no assertions

This is followed by a test detail section that consists of

        name of the test
        each assertion including
            the actual assertion
            a message if the assertion failed
            a result of PASS or FAIL

