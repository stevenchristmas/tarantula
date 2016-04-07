# Tarantula
Tarantula is an automated technique for localizing faults in software.  The creators of Tarantula have a website where you can read all about their research with Tarantula and software testing in general:

    http://spideruci.org/research/

This is a very basic Python implementation of Tarantula that uses the pass/fail results of random tests in combination with the statement coverage reports produced by gcov to identify the most "suspicious" lines of code in a faulty program.  After running many tests and producing at least one failing test, Tarantula prints a ranked list of the most suspicious lines.  Suspiciousness ranges from 0 to 1, where 1 is the most suspicious and 0 is not at all suspicious.  Tarantula does not print unsuspicious lines.

Using this version of Tarantula requires you to have a random tester which takes a random seed as its sole command line argument, which is used to generate the random test.  The random tester performs a single test on the faulty program and returns zero for passing tests or one for failing tests.

Tarantula calls the random tester with a random seed, collects the return code, and parses the gcov coverage report to determine which lines were executed in passing and failing tests. After all tests are complete, Tarantula analyzes the coverage results of all tests to find the most suspicious lines of code.

Since Tarantula relies on the gcov report, the faulty code must be compiled with the "--coverage" flag to enable gcov.  I have only tested with code written in C and compiled with GCC, but it should also work with G++.


## Usage
Modify the variables inside the Setup section of tarantula.py.

    <> numTests is the number of times tarantula should call the random
        tester
    <> randTester should be the path to a random tester that takes as
        the its only command line argument a number which it uses as a
        random seed for test generation.  The randTester should return
        or exit with 1 in failing test cases or 0 in passing cases.
    <> progFile is the path to the source file where tarantula is
        searching for suspicious code.  progFile should be compiled
        with gcov enabled (use the "-coverage" flag when compiling with
        gcc/g++), and the randTester should run tests on the compiled
        code.
    <> gcdaFile is the path to the .gcda that gvoc produces when running
        "gcov progFile".

The random testers I have written and used with this implementation of tarantula have a structure like this:

    int main(int argc, char *argv[]){

        // randTest performs some random test on the code under test,
        // returning a value less than 0 if the test fails
        if(randTest(atoi(argv[1])) < 0)
            return 1;

        return 0;
    }


## Credits
http://spideruci.org/research/
