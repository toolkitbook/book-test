---
layout: default
title: C++ Toolkit test
nav: pages/ch_boost
---

<span class="label">20</span>Using the Boost Unit Test Framework
================================================================

Last Update: November 13, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter discusses the Boost Unit Test Framework and how to use it within NCBI. The NCBI C++ Toolkit has incorporated and extended the open source [Boost.Test Library](http://www.boost.org/doc/libs/1_53_0/libs/test/doc/html/index.html), and provides a simplified way for the developers to create Boost-based C++ unit tests.

The NCBI extensions add the ability to:

-   execute the code in a standard (*CNcbiApplication* -like) environment;

-   disable test cases or suites, using one of several methods;

-   establish dependencies between test cases and suites;

-   use NCBI command-line argument processing;

-   add initialization and finalization functions; and

-   use convenience macros for combining <span class="nctnt ncbi-macro">NO\_THROW</span> with other test tools.

While the framework may be of interest to outside organizations, this chapter is intended for NCBI C++ developers. See also the Doxygen documentation for [tests](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/group__Tests.html).

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Why Use the Boost Unit Test Framework?](#why-use-the-boost-unit-test-framework)

-   [How to Use the Boost Unit Test Framework](#how-to-use-the-boost-unit-test-framework)

    -   [Creating a New Unit Test](#creating-a-new-unit-test)

    -   [Customizing an Existing Unit Test](#customizing-an-existing-unit-test)

        -   [Modifying the Makefile](#modifying-the-makefile)

        -   [Modifying the Source File](#modifying-the-source-file)

            -   [Using Testing Tools](#using-testing-tools)

            -   [Adding Initialization and/or Finalization](#adding-initialization-andor-finalization)

            -   [Handling Timeouts](#handling-timeouts)

            -   [Handling Command-Line Arguments in Test Cases](#handling-command-line-arguments-in-test-cases)

            -   [Creating Test Suites](#creating-test-suites)

            -   [Managing Dependencies](#managing-dependencies)

            -   [Unit Tests with Multiple Files](#unit-tests-with-multiple-files)

        -   [Disabling Tests](#disabling-tests)

            -   [Disabling Tests with Configuration File Entries](#disabling-tests-with-configuration-file-entries)

            -   [Library-Defined Variables](#library-defined-variables)

            -   [User-Defined Variables](#user-defined-variables)

            -   [Disabling or Skipping Tests Explicitly in Code](#disabling-or-skipping-tests-explicitly-in-code)

    -   [Viewing Unit Tests Results from the Nightly Build](#viewing-unit-tests-results-from-the-nightly-build)

    -   [Running Unit Tests from a Command-Line](#running-unit-tests-from-a-command-line)

    -   [Limitations Of The Boost Unit Test Framework](#limitations-of-the-boost-unit-test-framework)

Why Use the Boost Unit Test Framework?
--------------------------------------

“...*I would like to see a practical plan for every group in Internal Services to move toward standardized testing. Then, in addition to setting an example for the other coding groups, I hope that you will have guidance for them as well about how best to move ahead in this direction. Once you have that, and are adhering to it yourselves, I will start pushing the other coding groups in that direction*.”

-   Jim Ostell, April 21, 2008

The value of unit testing is clearly recognized at the highest levels of management at NCBI. Here are some of the ways that using the Boost Unit Test Framework will directly benefit the developer:

-   The framework provides a uniform (and well-supported) testing and reporting environment.

-   Using the framework simplifies the process of creating and maintaining unit tests:

    -   The framework helps keep tests well-structured, straightforward, and easily expandable.

    -   You can concentrate on the testing of your functionality, while the framework takes care of all the testing infrastructure.

-   The framework fits into the NCBI nightly build system:

    -   All tests are run nightly on many platforms.

    -   All results are archived and available through a [web interface](http://intranet/ieb/ToolBox/STAT/test_stat/test_stat_ext.cgi).

How to Use the Boost Unit Test Framework
----------------------------------------

This chapter assumes you are starting from a working Toolkit source tree. If not, please refer to the chapters on [obtaining the source code](ch_getcode_svn.html), and [configuring and building the Toolkit](ch_config.html).

### Creating a New Unit Test

On Unix or MS Windows, use the [new\_project](ch_proj.html#ch_proj.new_project_Starting) script to create a new unit test project:

    new_project <name> app/unit_test

For example, to create a project named <span class="nctnt ncbi-monospace">foo</span>, type this in a command shell:

    new_project foo app/unit_test

This creates a directory named foo and then creates two projects within the new directory. One project will be the one named on the command-line (e.g. <span class="nctnt ncbi-monospace">foo</span>) and will contain a sample unit test using all the basic features of the Boost library. The other project will be named <span class="nctnt ncbi-monospace">unit\_test\_alt\_sample</span> and will contain samples of advanced techniques not required in most unit tests.

You can build and run these projects immediately to see how they work:

    cd foo
    make
    make check

Once your unit test is created, you must [customize](#customize) it to meet your testing requirements. This involves editing these files:

|----------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **File**                                                                   | **Purpose**                                                                                                                                                                        |
| <span class="nctnt ncbi-path">Makefile</span>                              | Main makefile for this directory - builds both the <span class="nctnt ncbi-monospace">foo</span> and <span class="nctnt ncbi-monospace">unit\_test\_alt\_sample</span> unit tests. |
| <span class="nctnt ncbi-path">Makefile.builddir</span>                     | Contains the path to a pre-built C++ Toolkit.                                                                                                                                      |
| <span class="nctnt ncbi-path">Makefile.foo\_app</span>                     | Makefile for the <span class="nctnt ncbi-monospace">foo</span> unit test.                                                                                                          |
| <span class="nctnt ncbi-path">Makefile.in</span>                           |                                                                                                                                                                |
| <span class="nctnt ncbi-path">Makefile.unit\_test\_alt\_sample\_app</span> | Makefile for the <span class="nctnt ncbi-monospace">unit\_test\_alt\_sample</span> unit test.                                                                                      |
| <span class="nctnt ncbi-path">foo.cpp</span>                               | Source code for the <span class="nctnt ncbi-monospace">foo</span> unit test.                                                                                                       |
| <span class="nctnt ncbi-path">unit\_test\_alt\_sample.cpp</span>           | Source code for the <span class="nctnt ncbi-monospace">unit\_test\_alt\_sample</span> unit test.                                                                                   |
| <span class="nctnt ncbi-path">unit\_test\_alt\_sample.ini</span>           | Configuration file for the <span class="nctnt ncbi-monospace">unit\_test\_alt\_sample</span> unit test.                                                                            |

### Customizing an Existing Unit Test

This section contains the following topics:

-   [Modifying the Makefile](#modifying-the-makefile)

-   [Modifying the Source File](#modifying-the-source-file)

    -   [Using Testing Tools](#using-testing-tools)

    -   [Adding Initialization and/or Finalization](#adding-initialization-andor-finalization)

    -   [Handling Timeouts](#handling-timeouts)

    -   [Handling Command-Line Arguments in Test Cases](#handling-command-line-arguments-in-test-cases)

    -   [Creating Test Suites](#creating-test-suites)

    -   [Managing Dependencies](#managing-dependencies)

    -   [Unit Tests with Multiple Files](#unit-tests-with-multiple-files)

-   [Disabling Tests](#disabling-tests)

    -   [Disabling Tests with Configuration File Entries](#disabling-tests-with-configuration-file-entries)

    -   [Library-Defined Variables](#library-defined-variables)

    -   [User-Defined Variables](#user-defined-variables)

    -   [Disabling or Skipping Tests Explicitly in Code](#disabling-or-skipping-tests-explicitly-in-code)

#### Modifying the Makefile

The [new\_project](ch_proj.html#ch_proj.new_project_Starting) script generates a new unit test project that includes everything needed to use the Boost Unit Test Framework, but it won’t include anything specifically needed to build the library or application you are testing.

Therefore, edit the unit test makefile (e.g. <span class="nctnt ncbi-path">Makefile.foo.app</span>) and add the appropriate paths and libraries needed by your library or application. Note that although the <span class="nctnt ncbi-cmd">new\_project</span> script creates five makefiles, you will generally need to edit only one. If you are using Windows, please see the FAQ on [adding libraries to Visual C++ projects](ch_faq.html#ch_faq.How_do_I_add_a_library_to_a_Visua).

Because the unit tests are based on the Boost Unit Test Framework, the makefiles must specify:

    REQUIRES = Boost.Test.Included

If you are using the <span class="nctnt ncbi-cmd">new\_project</span> script (recommended), this setting is included automatically. Otherwise, make sure that <span class="nctnt ncbi-monospace">Boost.Test.Included</span> is listed in <span class="nctnt ncbi-monospace">REQUIRES</span>.

<span class="nctnt highlight">Note:</span> Please also see the "[Defining and running tests](ch_proj.html#ch_proj.inside_tests)" section for unit test makefile information that isn't specific to Boost.

#### Modifying the Source File

A unit test is simply a test of a unit of code, such as a class. Because each unit has many requirements, each unit test has many test cases. Your unit test code should therefore consist of a test case for each testable requirement. Each test case should be as small and independent of other test cases as possible. For information on how to handle dependencies between test cases, see the section on [managing dependencies](#managing-dependencies).

Starting with an existing unit test source file, simply add, change, or remove test cases as appropriate for your unit test. Test cases are defined by the <span class="nctnt ncbi-macro">BOOST\_AUTO\_TEST\_CASE</span> macro, which looks similar to a function. The macro has a single argument (the test case name) and a block of code that implements the test. Test case names must be unique at each level of the test suite hierarchy (see [managing dependencies](#managing-dependencies)). Test cases should contain code that will succeed if the requirement under test is correctly implemented, and fail otherwise. Determination of success is made using Boost [testing tools](#testing-tools) such as <span class="nctnt ncbi-macro">BOOST\_REQUIRE</span> and <span class="nctnt ncbi-macro">BOOST\_CHECK</span>.

The following sections discuss modifying the source file in more detail:

-   [Using Testing Tools](#using-testing-tools)

-   [Adding Initialization and/or Finalization](#adding-initialization-andor-finalization)

-   [Handling Timeouts](#handling-timeouts)

-   [Handling Command-Line Arguments in Test Cases](#handling-command-line-arguments-in-test-cases)

-   [Creating Test Suites](#creating-test-suites)

-   [Managing Dependencies](#managing-dependencies)

-   [Unit Tests with Multiple Files](#unit-tests-with-multiple-files)

##### Using Testing Tools

Testing tools are macros that are used to detect errors and determine whether a given test case passes or fails.

While at a basic level test cases can pass or fail, it is useful to distinguish between those failures that make subsequent testing pointless or impossible and those that don’t. Therefore, there are two levels of testing: <span class="nctnt ncbi-macro">CHECK</span> (which upon failure generates an error but allows subsequent testing to continue), and <span class="nctnt ncbi-macro">REQUIRE</span> (which upon failure generates a fatal error and aborts the current test case). In addition, there is a warning level, <span class="nctnt ncbi-macro">WARN</span>, that can report something of interest without generating an error, although by default you will have to [set a command-line argument](#set-a-command-line-argument) to see warning messages.

If the failure of one test case should result in skipping another then you should [add a dependency](#add-a-dependency) between them.

Many Boost testing tools have variants for each error level. The most common Boost testing tools are:

|--------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------|
| **Testing Tool**                                                                     | **Purpose**                                                                                                 |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>(predicate)</span>                    | Fails if the Boolean predicate (any logical expression) is false.                                           |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>\_EQUAL(left, right)</span>           | Fails if the two values are not equal.                                                                      |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>\_THROW(expression, exception)</span> | Fails if execution of the expression doesn’t throw an exception of the given type (or one derived from it). |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>\_NO\_THROW(expression)</span>        | Fails if execution of the expression throws any exception.                                                  |

Note that <span class="nctnt ncbi-macro">BOOST\_\<level\>\_EQUAL(var1,var2)</span> is equivalent to <span class="nctnt ncbi-macro">BOOST\_\<level\> (var1==var2)</span>, but in the case of failure it prints the value of each variable, which can be helpful. Also, it is not a good idea to compare floating point values directly - instead, use <span class="nctnt ncbi-macro">BOOST\_\<level\>\_CLOSE(var1,var2,tolerance)</span>.

See the Boost testing tools [reference page](http://www.boost.org/doc/libs/1_53_0/libs/test/doc/html/utf/testing-tools/reference.html) for documentation on these and other testing tools.

The NCBI extensions to the Boost library add a number of convenience testing tools that enclose the similarly-named Boost testing tools in a <span class="nctnt ncbi-macro">NO\_THROW</span> test:

|----------------------------------------------------------------------------|-------------------------------------------------------------------------------|
| **Boost Testing Tool**                                                     | **NCBI "NO\_THROW " Extension**                                               |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>(predicate)</span>          | <span class="nctnt ncbi-macro">NCBITEST\_\<level\>(predicate)</span>          |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>\_EQUAL(left, right)</span> | <span class="nctnt ncbi-macro">NCBITEST\_\<level\>\_EQUAL(left, right)</span> |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>\_NE(left, right)</span>    | <span class="nctnt ncbi-macro">NCBITEST\_\<level\>\_NE(left, right)</span>    |
| <span class="nctnt ncbi-macro">BOOST\_\<level\>\_MESSAGE(pred, msg)</span> | <span class="nctnt ncbi-macro">NCBITEST\_\<level\>\_MESSAGE(pred, msg)</span> |

<span class="nctnt highlight">Note:</span> Testing tools are only supported within the context of test cases. That is, within functions defined by the <span class="nctnt ncbi-macro">BOOST\_AUTO\_TEST\_CASE</span> macro and within functions called by a test case. They are not supported in functions defined by the <span class="nctnt ncbi-macro">NCBITEST\_\*</span> macros.

##### Adding Initialization and/or Finalization

If your unit test requires initialization prior to executing test cases, or if finalization / clean-up is necessary, use these functions:

    NCBITEST_AUTO_INIT()
    {
        // Your initialization code here...
    }

    NCBITEST_AUTO_FINI()
    {
        // Your finalization code here...
    }

##### Handling Timeouts

If exceeding a maximum execution time constitutes a failure for your test case, use this:

    // change the second parameter to the duration of your timeout in seconds
    BOOST_AUTO_TEST_CASE_TIMEOUT(TestTimeout, 3);
    BOOST_AUTO_TEST_CASE(TestTimeout)
    {
        // Your test code here...
    }

##### Handling Command-Line Arguments in Test Cases

It is possible to retrieve command-line arguments from your test cases using the standard C++ Toolkit [argument handling API](ch_core.html#ch_core.cmd_line_args). The first step is to initialize the unit test to expect the arguments. Add code like the following to your source file:

    NCBITEST_INIT_CMDLINE(descrs)
    {
        // Add calls like this for each command-line argument to be used.
        descrs->AddOptionalPositional("some_arg",
                                      "Sample command-line argument.",
                                      CArgDescriptions::eString);
    }

For more examples of argument processing, see [test\_ncbiargs\_sample.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test/test_ncbiargs_sample.cpp).

Next, add code like the following to access the argument from within a test case:

    BOOST_AUTO_TEST_CASE(TestCaseName)
    {
        const CArgs& args = CNcbiApplication::Instance()->GetArgs();
        string arg_value = args["some_arg"].AsString();
        // do something with arg_value ...
    }

Adding your own command-line arguments will not affect the application’s ability to process other command-line arguments such as <span class="nctnt ncbi-cmd">-help</span> or <span class="nctnt ncbi-cmd">-dryrun</span>.

##### Creating Test Suites

Test suites are simply groups of test cases. The test cases included in a test suite are those that appear between the beginning and ending test suite declarations:

    BOOST_AUTO_TEST_SUITE(TestSuiteName)

    BOOST_AUTO_TEST_CASE(TestCase1)
    {
        //...
    }

    BOOST_AUTO_TEST_CASE(TestCase2)
    {
        //...
    }

    BOOST_AUTO_TEST_SUITE_END();

Note that the beginning test suite declaration defines the test suite name and does not include a semicolon.

##### Managing Dependencies

Test cases and suites can be dependent on other test cases or suites. This is useful when it doesn’t make sense to run a test after some other test fails:

    NCBITEST_INIT_TREE()
    {
        // define individual dependencies
        NCBITEST_DEPENDS_ON(test_case_dep, test_case_indep);
        NCBITEST_DEPENDS_ON(test_case_dep, test_suite_indep);
        NCBITEST_DEPENDS_ON(test_suite_dep, test_case_indep);
        NCBITEST_DEPENDS_ON(test_suite_dep, test_suite_indep);

        // define multiple dependencies
        NCBITEST_DEPENDS_ON_N(item_dep, 2, (item_indep1, item_indep2));
    }

When an independent test item (case or suite) fails, all of the test items that depend on it will be skipped.

##### Unit Tests with Multiple Files

The [new\_project](ch_proj.html#ch_proj.new_project_Starting) script is designed to create single-file unit tests by default, but you can add as many files as necessary to implement your unit test. Use of the <span class="nctnt ncbi-macro">BOOST\_AUTO\_TEST\_MAIN</span> macro is now deprecated.

#### Disabling Tests

The Boost Unit Test Framework was extended by NCBI to provide several ways to disable test cases and suites. Test cases and suites are disabled based on logical expressions in the application configuration file or, less commonly, by explicitly disabling or skipping them. The logical expressions are based on unit test variables which are defined either by the library or by the user. All such variables are essentially Boolean in that they are either defined (<span class="nctnt ncbi-var">true</span>) or not defined (<span class="nctnt ncbi-var">false</span>). <span class="nctnt highlight">Note:</span> these methods of disabling tests don't apply if specific tests are [run from the command-line](#run-from-the-command-line).

-   [Disabling Tests with Configuration File Entries](#disabling-tests-with-configuration-file-entries)

-   [Library-Defined Variables](#library-defined-variables)

-   [User-Defined Variables](#user-defined-variables)

-   [Disabling or Skipping Tests Explicitly in Code](#disabling-or-skipping-tests-explicitly-in-code)

##### Disabling Tests with Configuration File Entries

The <span class="nctnt ncbi-macro">[UNITTESTS\_DISABLE]</span> section of the application configuration file can be customized to disable test cases or suites. Entries in this section should specify a test case or suite name and a logical expression for disabling it (expressions that evaluate to <span class="nctnt ncbi-macro">true</span> disable the test). The logical expression can be formed from the logical constants <span class="nctnt ncbi-macro">true</span> and <span class="nctnt ncbi-macro">false</span>, numeric constants, [library-defined](#library-defined) or [user-defined](#user-defined) unit test variables, logical operators ('<span class="nctnt ncbi-monospace">!</span>', '<span class="nctnt ncbi-monospace">&&</span>', and '<span class="nctnt ncbi-monospace">\\|\\|</span>'), and parentheses.

To disable specific tests, use commands like:

    [UNITTESTS_DISABLE]
    SomeTestCaseName = OS_Windows && PLATFORM_BigEndian
    SomeTestSuiteName = (OS_Linux \|\| OS_Solaris) && COMPILER_GCC

There is a special entry <span class="nctnt ncbi-monospace">GLOBAL</span> that can be used to disable all tests. For example, to disable all tests under Cygwin, use:

    [UNITTESTS_DISABLE]
    GLOBAL = OS_Cygwin

<span class="nctnt highlight">Note</span>: If the configuration file contains either a test name or a variable name that has not been defined (e.g. due to a typo) then the test program will exit immediately with an error, without executing any tests.

##### Library-Defined Variables

When the NCBI-extended Boost Test library is built, it defines a set of unit test variables based on the build, compiler, operating system, and platform. See [Table 1](#table-1) for a list of related variables ([test\_boost.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test_boost.cpp) has the latest [list of variables](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=x_InitCommonParserVars&d=)).

Table 1. Build Generated Predefined Variables

| <span class="nctnt ncbi-code">Builds</span>         | <span class="nctnt ncbi-code">Compilers</span>           | <span class="nctnt ncbi-code">Operating Systems</span> | <span class="nctnt ncbi-code">Platforms</span>              |
|-----------------------------------------------------|----------------------------------------------------------|--------------------------------------------------------|-------------------------------------------------------------|
| <span class="nctnt ncbi-code">BUILD\_Debug</span>   | <span class="nctnt ncbi-code">COMPILER\_Compaq</span>    | <span class="nctnt ncbi-code">OS\_AIX</span>           | <span class="nctnt ncbi-code">PLATFORM\_BigEndian</span>    |
| <span class="nctnt ncbi-code">BUILD\_Dll</span>     | <span class="nctnt ncbi-code">COMPILER\_GCC</span>       | <span class="nctnt ncbi-code">OS\_BSD</span>           | <span class="nctnt ncbi-code">PLATFORM\_Bits32</span>       |
| <span class="nctnt ncbi-code">BUILD\_Release</span> | <span class="nctnt ncbi-code">COMPILER\_ICC</span>       | <span class="nctnt ncbi-code">OS\_Cygwin</span>        | <span class="nctnt ncbi-code">PLATFORM\_Bits64</span>       |
| <span class="nctnt ncbi-code">BUILD\_Static</span>  | <span class="nctnt ncbi-code">COMPILER\_KCC</span>       | <span class="nctnt ncbi-code">OS\_Irix</span>          | <span class="nctnt ncbi-code">PLATFORM\_LittleEndian</span> |
|                                 | <span class="nctnt ncbi-code">COMPILER\_MipsPro</span>   | <span class="nctnt ncbi-code">OS\_Linux</span>         |                                         |
|                                 | <span class="nctnt ncbi-code">COMPILER\_MSVC</span>      | <span class="nctnt ncbi-code">OS\_MacOS</span>         |                                         |
|                                 | <span class="nctnt ncbi-code">COMPILER\_VisualAge</span> | <span class="nctnt ncbi-code">OS\_MacOSX</span>        |                                         |
|                                 | <span class="nctnt ncbi-code">COMPILER\_WorkShop</span>  | <span class="nctnt ncbi-code">OS\_Solaris</span>       |                                         |
|                                 |                                      | <span class="nctnt ncbi-code">OS\_Tru64</span>         |                                         |
|                                 |                                      | <span class="nctnt ncbi-code">OS\_Unix</span>          |                                         |
|                                 |                                      | <span class="nctnt ncbi-code">OS\_Windows</span>       |                                         |

At run-time, the library also checks the <span class="nctnt ncbi-cmd">FEATURES</span> environment variable and creates unit test variables based on the current set of features. See [Table 2](#table-2) for a list of feature, package, and project related variables ([test\_boost.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test_boost.cpp) has the latest [list of features](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=s_NcbiFeatures&d=)).

Table 2. Check Script Generated Predefined Variables

| <span class="nctnt ncbi-code">Features</span>             | <span class="nctnt ncbi-code">Packages</span>                                                                                                 | <span class="nctnt ncbi-code">Projects</span>    |
|-----------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| <span class="nctnt ncbi-code">AIX</span>                  | <span class="nctnt ncbi-code">BerkeleyDB</span>                                                                                               | <span class="nctnt ncbi-code">algo</span>        |
| <span class="nctnt ncbi-code">BSD</span>                  | <span class="nctnt ncbi-code">BerkeleyDB\_\_</span><br/><span class="nctnt ncbi-code">(use for BerkeleyDB++)</span> | <span class="nctnt ncbi-code">app</span>         |
| <span class="nctnt ncbi-code">CompaqCompiler</span>       | <span class="nctnt ncbi-code">Boost\_Regex</span>                                                                                             | <span class="nctnt ncbi-code">bdb</span>         |
| <span class="nctnt ncbi-code">Cygwin</span>               | <span class="nctnt ncbi-code">Boost\_Spirit</span>                                                                                            | <span class="nctnt ncbi-code">cgi</span>         |
| <span class="nctnt ncbi-code">CygwinMT</span>             | <span class="nctnt ncbi-code">Boost\_Test</span>                                                                                              | <span class="nctnt ncbi-code">connext</span>     |
| <span class="nctnt ncbi-code">DLL</span>                  | <span class="nctnt ncbi-code">Boost\_Test\_Included</span>                                                                                    | <span class="nctnt ncbi-code">ctools</span>      |
| <span class="nctnt ncbi-code">DLL\_BUILD</span>           | <span class="nctnt ncbi-code">Boost\_Threads</span>                                                                                           | <span class="nctnt ncbi-code">dbapi</span>       |
| <span class="nctnt ncbi-code">Darwin</span>               | <span class="nctnt ncbi-code">BZ2</span>                                                                                                      | <span class="nctnt ncbi-code">gbench</span>      |
| <span class="nctnt ncbi-code">GCC</span>                  | <span class="nctnt ncbi-code">C\_ncbi</span>                                                                                                  | <span class="nctnt ncbi-code">gui</span>         |
| <span class="nctnt ncbi-code">ICC</span>                  | <span class="nctnt ncbi-code">C\_Toolkit</span>                                                                                               | <span class="nctnt ncbi-code">local\_bsm</span>  |
| <span class="nctnt ncbi-code">in\_house\_resources</span> | <span class="nctnt ncbi-code">CPPUNIT</span>                                                                                                  | <span class="nctnt ncbi-code">ncbi\_crypt</span> |
| <span class="nctnt ncbi-code">IRIX</span>                 | <span class="nctnt ncbi-code">EXPAT</span>                                                                                                    | <span class="nctnt ncbi-code">objects</span>     |
| <span class="nctnt ncbi-code">KCC</span>                  | <span class="nctnt ncbi-code">Fast\_CGI</span>                                                                                                | <span class="nctnt ncbi-code">serial</span>      |
| <span class="nctnt ncbi-code">Linux</span>                | <span class="nctnt ncbi-code">FLTK</span>                                                                                                     |                              |
| <span class="nctnt ncbi-code">MIPSpro</span>              | <span class="nctnt ncbi-code">FreeTDS</span>                                                                                                  |                              |
| <span class="nctnt ncbi-code">MSVC</span>                 | <span class="nctnt ncbi-code">FreeType</span>                                                                                                 |                              |
| <span class="nctnt ncbi-code">MSWin</span>                | <span class="nctnt ncbi-code">FUSE</span>                                                                                                     |                              |
| <span class="nctnt ncbi-code">MT</span>                   | <span class="nctnt ncbi-code">GIF</span>                                                                                                      |                              |
| <span class="nctnt ncbi-code">MacOS</span>                | <span class="nctnt ncbi-code">GLUT</span>                                                                                                     |                              |
| <span class="nctnt ncbi-code">Ncbi\_JNI</span>            | <span class="nctnt ncbi-code">GNUTLS</span>                                                                                                   |                              |
| <span class="nctnt ncbi-code">OSF</span>                  | <span class="nctnt ncbi-code">HDF5</span>                                                                                                     |                              |
| <span class="nctnt ncbi-code">PubSeqOS</span>             | <span class="nctnt ncbi-code">ICU</span>                                                                                                      |                              |
| <span class="nctnt ncbi-code">SRAT\_internal</span>       | <span class="nctnt ncbi-code">JPEG</span>                                                                                                     |                              |
| <span class="nctnt ncbi-code">Solaris</span>              | <span class="nctnt ncbi-code">LIBXML</span>                                                                                                   |                              |
| <span class="nctnt ncbi-code">unix</span>                 | <span class="nctnt ncbi-code">LIBXSLT</span>                                                                                                  |                              |
| <span class="nctnt ncbi-code">VisualAge</span>            | <span class="nctnt ncbi-code">LocalBZ2</span>                                                                                                 |                              |
| <span class="nctnt ncbi-code">WinMain</span>              | <span class="nctnt ncbi-code">LocalMSGMAIL2</span>                                                                                            |                              |
| <span class="nctnt ncbi-code">WorkShop</span>             | <span class="nctnt ncbi-code">LocalNCBILS</span>                                                                                              |                              |
| <span class="nctnt ncbi-code">XCODE</span>                | <span class="nctnt ncbi-code">LocalPCRE</span>                                                                                                |                              |
|                                       | <span class="nctnt ncbi-code">LocalSSS</span>                                                                                                 |                              |
|                                       | <span class="nctnt ncbi-code">LocalZ</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">LZO</span>                                                                                                      |                              |
|                                       | <span class="nctnt ncbi-code">MAGIC</span>                                                                                                    |                              |
|                                       | <span class="nctnt ncbi-code">MESA</span>                                                                                                     |                              |
|                                       | <span class="nctnt ncbi-code">MUPARSER</span>                                                                                                 |                              |
|                                       | <span class="nctnt ncbi-code">MySQL</span>                                                                                                    |                              |
|                                       | <span class="nctnt ncbi-code">NCBILS2</span>                                                                                                  |                              |
|                                       | <span class="nctnt ncbi-code">ODBC</span>                                                                                                     |                              |
|                                       | <span class="nctnt ncbi-code">OECHEM</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">OpenGL</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">OPENSSL</span>                                                                                                  |                              |
|                                       | <span class="nctnt ncbi-code">ORBacus</span>                                                                                                  |                              |
|                                       | <span class="nctnt ncbi-code">PCRE</span>                                                                                                     |                              |
|                                       | <span class="nctnt ncbi-code">PNG</span>                                                                                                      |                              |
|                                       | <span class="nctnt ncbi-code">PYTHON</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">PYTHON23</span>                                                                                                 |                              |
|                                       | <span class="nctnt ncbi-code">PYTHON24</span>                                                                                                 |                              |
|                                       | <span class="nctnt ncbi-code">PYTHON25</span>                                                                                                 |                              |
|                                       | <span class="nctnt ncbi-code">SABLOT</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">SGE</span>                                                                                                      |                              |
|                                       | <span class="nctnt ncbi-code">SP</span>                                                                                                       |                              |
|                                       | <span class="nctnt ncbi-code">SQLITE</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">SQLITE3</span>                                                                                                  |                              |
|                                       | <span class="nctnt ncbi-code">SQLITE3ASYNC</span>                                                                                             |                              |
|                                       | <span class="nctnt ncbi-code">SSSDB</span>                                                                                                    |                              |
|                                       | <span class="nctnt ncbi-code">SSSUTILS</span>                                                                                                 |                              |
|                                       | <span class="nctnt ncbi-code">Sybase</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">SybaseCTLIB</span>                                                                                              |                              |
|                                       | <span class="nctnt ncbi-code">TIFF</span>                                                                                                     |                              |
|                                       | <span class="nctnt ncbi-code">UNGIF</span>                                                                                                    |                              |
|                                       | <span class="nctnt ncbi-code">UUID</span>                                                                                                     |                              |
|                                       | <span class="nctnt ncbi-code">Xalan</span>                                                                                                    |                              |
|                                       | <span class="nctnt ncbi-code">Xerces</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">XPM</span>                                                                                                      |                              |
|                                       | <span class="nctnt ncbi-code">Z</span>                                                                                                        |                              |
|                                       | <span class="nctnt ncbi-code">wx2\_8</span>                                                                                                   |                              |
|                                       | <span class="nctnt ncbi-code">wxWidgets</span>                                                                                                |                              |
|                                       | <span class="nctnt ncbi-code">wxWindows</span>                                                                                                |                              |

The automated nightly test suite defines the <span class="nctnt ncbi-cmd">FEATURES</span> environment variable before launching the unit test applications. In this way, unit test applications can also use run-time detected features to exclude specific tests from the test suite.

<span class="nctnt highlight">Note:</span> The names of the features are modified slightly when creating unit test variables from names in the <span class="nctnt ncbi-cmd">FEATURES</span> environment variable. Specifically, each feature is prefixed by <span class="nctnt ncbi-cmd">FEATURE\_</span> and all non-alphanumeric characters are changed to underscores. For example, to require the feature <span class="nctnt ncbi-cmd">in-house-resources</span> for a test (i.e. to disable the test if the feature is not present), use:

    [UNITTESTS_DISABLE]
    SomeTestCaseName = !FEATURE_in_house_resources

##### User-Defined Variables

You can define your own variables to provide finer control on disabling tests. First, define a variable in your source file:

    NCBITEST_INIT_VARIABLES(parser)
    {
        parser->AddSymbol("my_ini_var", <some bool expression goes here>);
    }

Then add a line to the configuration file to disable a test based on the value of the new variable:

    [UNITTESTS_DISABLE]
    MyTestName = my_ini_var

User-defined variables can be used in conjunction with [command-line arguments](#command-line-arguments):

    NCBITEST_INIT_VARIABLES(parser)
    {
        const CArgs& args = CNcbiApplication::Instance()->GetArgs();
        parser->AddSymbol("my_ini_var", args["my_arg"].HasValue());
    }

Then, passing the argument on the command-line controls the disabling of the test case:

    ./foo my_arg # test is disabled 
    ./foo        # test is not disabled (at least via command-line / config file)

##### Disabling or Skipping Tests Explicitly in Code

The NCBI extensions include a macro, <span class="nctnt ncbi-macro">NCBITEST\_DISABLE</span>, to unconditionally disable a test case or suite. This macro must be placed in the <span class="nctnt ncbi-macro">NCBITEST\_INIT\_TREE</span> function:

    NCBITEST_INIT_TREE()
    {
        NCBITEST_DISABLE(test_case_name);
        NCBITEST_DISABLE(test_suite_name);
    }

The extensions also include two functions for globally disabling or skipping all tests. These functions should be called only from within the <span class="nctnt ncbi-macro">NCBITEST\_AUTO\_INIT</span> or <span class="nctnt ncbi-macro">NCBITEST\_INIT\_TREE</span> functions:

    NCBITEST_INIT_TREE()
    {
        NcbiTestSetGlobalDisabled(); // A given unit test might include one 
        NcbiTestSetGlobalSkipped();  // or the other of these, not both.
                                     // Most unit tests won’t use either.
    }

The difference between these functions is that globally disabled unit tests will report the status <span class="nctnt ncbi-macro">DIS</span> to check scripts while skipped tests will report the status <span class="nctnt ncbi-macro">SKP</span>.

### Viewing Unit Tests Results from the Nightly Build

The Boost Unit Test Framework provides more than just command-line testing. Each unit test built with the framework becomes incorporated into nightly testing and is tested on multiple platforms and under numerous configurations. All such results are archived in the database and available through a [web interface](http://intranet/ieb/ToolBox/STAT/test_stat/test_stat_ext.cgi).

The main page (see [Figure 1](#figure-1)) provides many ways to narrow down the vast quantity of statistics available. The top part of the page allows you to select test date, test result, build configuration (branch, compiler, operating system, etc), debug/release, and more. The page also has a column for selecting tests, and a column for configurations. For best results, refine the selection as much as possible, and then click on the “See test statistics” button.

[![Figure 1. Test Interface](/book-test/static/img/TestInterface.png)](/book-test/static/img/TestInterface.png "Click to see the full-resolution image")

Figure 1. Test Interface

The “See test statistics” button retrieves the desired statistics in a second page (see [Figure 2](#figure-2)). The results are presented in tables: one for each selected date, with unit tests down the left side and configurations across the top. Further refinements of the displayed results can be made by removing rows, columns, or dates; and by selecting whether all columns, all cells, or only selected cells are displayed.

[![Figure 2. Test Matrix](/book-test/static/img/TestMatrix.png)](/book-test/static/img/TestMatrix.png "Click to see the full-resolution image")

Figure 2. Test Matrix

Each cell in the results tables represents a specific unit test performed on a specific date under a specific configuration. Clicking on a cell retrieves a third page (see [Figure 3](#figure-3)) that shows information about that test and its output.

[![Figure 3. Test Result](/book-test/static/img/TestResult.png)](/book-test/static/img/TestResult.png "Click to see the full-resolution image")

Figure 3. Test Result

### Running Unit Tests from a Command-Line

To run one or more selected test cases from a command-line, use this:

    ./foo --run_test=TestCaseName1,TestCaseName2

Multiple test cases can be selected by using a comma-separated list of names.

To see all test cases in a unit test, use this:

    ./foo -dryrun

To see exactly which test cases passed and failed, use this:

    ./foo --report_level=detailed

To see warning messages, use this:

    ./foo --log_level=warning

Additional runtime parameters can be set. For a complete list, see the online [documentation](http://www.boost.org/doc/libs/1_53_0/libs/test/doc/html/utf/user-guide/runtime-config/reference.html).

### Limitations of the Boost Unit Test Framework

The currently known limitations are:

-   It is not suitable for most multi-threaded tests.

-   It is not suitable for "one-piece" applications (such as server or CGI). Such applications should be tested via their clients (which would preferably be unit test based).


