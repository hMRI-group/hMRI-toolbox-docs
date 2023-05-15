# Unit testing
## minimal unit test for functions
Unit test functions (and with them their filenames) should end with `Test`. So assuming you have a function `example` in a file `example.m` you should create a test file `exampleTest.m` with the following function in it:
```matlab
%% Main function to generate tests
function tests = exampleTest
    tests = functiontests(localfunctions);
end
```
This enables the unit test environment.

In this file you can now add test functions. Each of those test functions must do a qualification at its end. Available qualifications are: https://de.mathworks.com/help/matlab/matlab_prog/types-of-qualifications.html. Assuming our function `example` computes the square root of the input the first test case could be this:
```matlab
function testFunctionA(testData)
    actRoot = example(3*3);
    expRoot = 3;
    verifyEqual(testData,actRoot,expRoot)
end
```
`testData` is the internal data structure to keep track of the testing (and thus is given to the qualification `verifyEqual` as first parameter). If you have multiple test cases you want to check, just add another test function. All of them will be tested.

The tests can be run via `runtests` with the name of the testfile as parameter. In our case `runtests('exampleTest.m')`

## setting up (and clearing) global and local environment for tests
Sometimes it is necessary to prepare data or other things for the test. Those can be done globally (wont be changed for the whole test run) and locally (will be reset for each test case).

The global setup is done by adding a function `setupOnce(testData)` to the test file. Notice that this function gets `testData` as a parameter. This gives you the option to add your global data into the data structure for the testing (in contrast to cluttering the global matlab data). This function is called once at the start when calling `runtests`. It's counterpart `teardownOnce(testCase)` is called right before `runtests` end. This can be used for any cleanup if neccessary. Also note that `testData` will be deleted anyway. So if you had no variables outside `testData` you won't have to clear those.

The local setup functions `setup(testData)` and `teardown(testCase)` do pretty much the same. But for each test case individually. That means `setup(testData)` is run right before each of the test functions and `teardown(testCase)` right after it. This can be useful if you need reset variables for each test.

## processing the results

The function `runtests` returns a [TestsResults](https://www.mathworks.com/help/matlab/ref/matlab.unittest.testresult-class.html) object.
Assuming you put the results of `runtests` into the variable `a` you can write the results as a json file like this:

```matlab
a=runtests(myFunctionTests);
file=fopen('test_results.json','w');
fwrite(file,jsonencode(a));
fclose(file);
```

(see also [mathworks' tutorial for unit tests](https://de.mathworks.com/help/matlab/matlab_prog/write-simple-test-case-with-functions.html))