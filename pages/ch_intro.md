---
layout: default
title: C++ Toolkit test
nav: pages/ch_intro
---

<span class="label">1</span>Introduction to the C++ Toolkit
===========================================================

Last Update: February 27, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

One difficulty in understanding a major piece of software such as the C++ Toolkit is knowing where to begin in understanding the overall framework or getting the 'big picture' of how all the different components relate to each other. One approach is to dive into the details of one component and understand it in sufficient detail to get a roadmap of the rest of the components, and then repeat this process with the other components. Without a formal road map, this approach can be very time consuming and it may take a long time to locate the functionality one needs.

When trying to understand a major piece of software, it would be more effective if there is a written text that explains the overall framework without getting too lost in the details. This chapter is written with the intent to provide you with this broader picture of the C++ Toolkit.

This chapter provides an introduction to the major components that make up the Toolkit. You can use this chapter as a roadmap for the rest of the chapters that follow.

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [The CORELIB Module](#the-corelib-module)

    -   [Application Framework](#application-framework)

    -   [Argument processing](#argument-processing)

    -   [Diagnostics](#diagnostics)

    -   [Environment Interface](#environment-interface)

    -   [Files and Directories](#files-and-directories)

    -   [MT Test wrappers](#mt-test-wrappers)

    -   [Object and Ref classes](#object-and-ref-classes)

    -   [Portability definitions](#portability-definitions)

    -   [Portable Exception Handling](#portable-exception-handling)

    -   [Portable Process Pipes](#portable-process-pipes)

    -   [Registry](#registry)

    -   [STL Use Hints](#stl-use-hints)

    -   [String Manipulations](#string-manipulations)

    -   [Template Utilities](#template-utilities)

    -   [Threads](#threads)

    -   [Time](#time)

-   [The ALGORITHM Module](#the-algorithm-module)

-   [The CGI Module](#the-cgi-module)

-   [The CONNECT Module](#the-connect-module)

    -   [Socket classes](#socket-classes)

    -   [Connector and Connection Handles](#connector-and-connection-handles)

    -   [Connection Streams](#connection-streams)

    -   [Sendmail API](#sendmail-api)

    -   [Threaded Server](#threaded-server)

-   [The CTOOL Module](#the-ctool-module)

-   [The DBAPI Module](#the-dbapi-module)

    -   [Database User Classes](#database-user-classes)

    -   [Database Driver Architecture](#database-driver-architecture)

-   [The GUI Module](#the-gui-module)

-   [The HTML Module](#the-html-module)

    -   [Relationships between HTML classes](#relationships-between-html-classes)

    -   [HTML Processing](#html-processing)

-   [The OBJECT MANAGER Module](#the-object-manager-module)

-   [The SERIAL Module](#the-serial-module)

-   [The UTIL Module](#the-util-module)

    -   [Checksum](#checksum)

    -   [Console Debug Dump Viewer](#console-debug-dump-viewer)

    -   [Diff API](#diff-api)

    -   [Floating Point Comparison](#floating-point-comparison)

    -   [Lightweight Strings](#lightweight-strings)

    -   [Linked Sets](#linked-sets)

    -   [Random Number Generator](#random-number-generator)

    -   [Range Support](#range-support)

    -   [Registry based DNS](#registry-based-dns)

    -   [Regular Expressions](#regular-expressions)

    -   [Resizing Iterator](#resizing-iterator)

    -   [Rotating Log Streams](#rotating-log-streams)

    -   [Stream Support](#stream-support)

    -   [String Search](#string-search)

    -   [Synchronized and blocking queue](#synchronized-and-blocking-queue)

    -   [Thread Pools](#thread-pools)

    -   [UTF 8 Conversion](#utf-8-conversion)

The CORELIB Module
------------------

The C++ Toolkit can be seen as consisting of several major pieces of code that we will refer to as *module*. The core module is called, appropriately enough, CORELIB, and provides a portable way to write C++ code and many useful facilities such as an application framework, argument processing, template utilities, threads, etc. The CORELIB facilities are used by other major modules. The rest of the sections that follow discusses the CORELIB and the other C++ Toolkit modules in more detail.

The following is a list of the CORELIB facilities. Note that each facility may be implemented by a number of C++ classes spread across many files.

-   [Application Framework](#application-framework)

-   [Argument processing](#argument-processing)

-   [Diagnostics](#diagnostics)

-   [Environment Interface](#environment-interface)

-   [Files and Directories](#files-and-directories)

-   [MT Test wrappers](#mt-test-wrappers)

-   [Object and Ref classes](#object-and-ref-classes)

-   [Portability definitions](#portability-definitions)

-   [Portable Exception Handling](#portable-exception-handling)

-   [Portable Process Pipes](#portable-process-pipes)

-   [Registry](#registry)

-   [STL Use Hints](#stl-use-hints)

-   [String Manipulations](#string-manipulations)

-   [Template Utilities](#template-utilities)

-   [Threads](#threads)

-   [Time](#time)

A brief description of each of each of these facilities are presented in the subsections that follow:

### Application Framework

The Application framework primarily consists of an abstract class called <span class="nctnt ncbi-class">CNcbiApplication</span> which defines the high level behavior of an application. For example, every application upon loading seems to go through a cycle of doing some initialization, then some processing, and upon completion of processing, doing some clean up activity before exiting. These three phases are modeled in the <span class="nctnt ncbi-class">CNcbiApplication</span> class as interface methods <span class="nctnt ncbi-func">Init()</span>, <span class="nctnt ncbi-func">Run()</span>, and <span class="nctnt ncbi-func">Exit()</span>.

A new application is written by deriving a class from the <span class="nctnt ncbi-class">CNcbiApplication</span> base class and writing an implementation of the <span class="nctnt ncbi-func">Init()</span>, <span class="nctnt ncbi-func">Run()</span>, and <span class="nctnt ncbi-func">Exit()</span> methods. Execution control to the new application is passed by calling the application object's <span class="nctnt ncbi-func">AppMain()</span> method inherited from the <span class="nctnt ncbi-class">CNcbiApplication</span> base class (see [Figure 1](#figure-1)). The <span class="nctnt ncbi-func">AppMain()</span> method is similar to the <span class="nctnt ncbi-func">main()</span> method used in C/C++ programs and calls the <span class="nctnt ncbi-func">Init()</span>, <span class="nctnt ncbi-func">Run()</span>, and <span class="nctnt ncbi-func">Exit()</span> methods.

![Figure 1. The CNcbiApplication class](/book-test/static/img/CNcbiApplication.gif)

Figure 1. The <span class="nctnt ncbi-class">CNcbiApplication</span> class

More details on [using the CNcbiApplication class](ch_core.html#ch_core.writing_simple_app) are presented in a later chapter.

### Argument processing

In a C++ program, control is transferred from the command line to the program via the <span class="nctnt ncbi-func">main()</span> function. The <span class="nctnt ncbi-func">main()</span> function is passed a count of the number of arguments (int argc), and an array of character strings containing arguments to the program (<span class="nctnt ncbi-code">char\*\* argv</span>). As long as the argument types are simple, one can simply set up a loop to iterate through the array of argument values and process them. However, with time applications evolve and grow more complex. Often there is a need to do some more complex argument checking. For example, the application may want to enforce a check on the number and position of arguments, check the argument type (int, string, etc.), check for constraints on argument values, check for flags, check for arguments that follow a keyword (<span class="nctnt ncbi-func">-logfile mylogfile.log</span>), check for mandatory arguments, display usage help on the arguments, etc.

To make the above tasks easier, the CORELIB provides a number of portable classes that encapsulate the functionality of argument checking and processing. The main classes that provide this functionality are the <span class="nctnt ncbi-class">CArgDescriptions</span>, <span class="nctnt ncbi-class">CArgs</span>, <span class="nctnt ncbi-class">CArgValue</span> classes.

Argument descriptions such as the expected number, type, position, mandatory and optional attributes are setup during an application's initilization such as the application object's <span class="nctnt ncbi-func">Init()</span> method (see [previous section](#previous-section)) by calling the <span class="nctnt ncbi-class">CArgDescriptions</span> class methods. Then, the arguments are extracted by calling the <span class="nctnt ncbi-class">CArgs</span> class methods.

More details on [argument processing](ch_core.html#ch_core.cmd_line_args) are presented in a later chapter.

### Diagnostics

It is very useful for an application to post messages about its internal state or other diagnostic information to a file, console or for that matter any output stream. The CORELIB provides a portable diagnostics facility that enables an application to post diagnostic messages of various severity levels to an output stream. This diagnostic facility is provided by the <span class="nctnt ncbi-class">CNcbiDiag</span> class. You can set the diagnostic stream to the standard error output stream (<span class="nctnt ncbi-code">NcbiErr</span>) or to any other output stream.

You can set the severity level of the message to Information, Warning, Error, Critical, Fatal, or Trace. You can alter the severity level at any time during the use of the diagnostic stream.

More details on [diagnostic streams](ch_core.html#ch_core.CNcbiDiag) and [processing of diagnostic messages](ch_debug.html#ch_debug.std_cpp_message_post) are presented in later chapters.

### Environment Interface

An application can read the environment variable settings (such as PATH) that are in affect when the application is run. CORELIB defines a portable <span class="nctnt ncbi-class">CNcbiEnvironment</span> class that stores the environment variable settings and provides applications with methods to get the environment variable values.

More details on the [environment interface](ch_core.html#ch_core.CNcbiEnvironment) are presented in a later chapter.

### Files and Directories

An application may need access to information about a file or directory. The CORELIB provides a number of portable classes to model a system file and directory. Some of the important classes are <span class="nctnt ncbi-class">CFile</span> for modeling a file, <span class="nctnt ncbi-class">CDir</span> for modeling a directory, and <span class="nctnt ncbi-class">CMemoryFile</span> for memory mapped file.

For example, if you create a <span class="nctnt ncbi-class">CFile</span> object corresponding to a system file, you can get the file's attribute settings such as file size, permission settings, or check the existence of a file. You can get the directory where the file is located, the base name of the file, and the file's extension. There are also a number of useful functions that are made available through these classes to parse a file path or build a file path from the component parts such as a directory, base name, and extension.

More details on [file and directory classes](ch_core.html#ch_core.files_dirs) are presented in later chapters.

### MT Test wrappers

The <span class="nctnt ncbi-class">CNcbiApplication</span> class which was [discussed earlier](#discussed-earlier) provides a framework for writing portable applications. For writing portable multi-threaded applications, the CORELIB provides a <span class="nctnt ncbi-class">CThreadedApp</span> class derived from <span class="nctnt ncbi-class">CNcbiApplication</span> class which provides a framework for building multi-threaded applications.

Instead of using the <span class="nctnt ncbi-func">Init()</span>, <span class="nctnt ncbi-func">Run()</span>, <span class="nctnt ncbi-func">Exit()</span> methods for the <span class="nctnt ncbi-class">CNcbiApplication</span> class, the <span class="nctnt ncbi-class">CThreadedApp</span> class defines specialized methods such as <span class="nctnt ncbi-func">Thread\_Init()</span>, <span class="nctnt ncbi-func">Thread\_Run()</span>, <span class="nctnt ncbi-func">Thread\_Exit()</span>, <span class="nctnt ncbi-func">Thread\_Destroy()</span> for controlling thread behavior. These methods operate on a specific thread identified by a thread index parameter.

### Object and Ref classes

A major cause of errors in C/C++ programs is due to dynamic allocation of memory. Stated simply, memory for objects allocated using the new operator must be released by a corresponding delete operator. Failure to delete allocated memory results in memory leaks. There may also be programming errors caused by references to objects that have never been allocated or improperly allocated. One reason these types of problems crop up are because a programmer may dynamically allocate memory as needed, but may not deallocate it due to unanticipated execution paths.

The C++ standard provides the use of a template class, auto\_ptr , that wraps memory management inside constructors and destructors. Because a destructor is called for every constructed object, memory allocation and deallocation can be kept symmetrical with respect to each other. However, the auto\_ptr does not properly handle the issue of ownership when multiple auto pointers, point to the same object. What is needed is a reference counted smart pointer that keeps a count of the number of pointers pointing to the same object. An object can only be released when its reference count drops to zero.

The CORELIB implements a portable reference counted smart pointer through the <span class="nctnt ncbi-class">CRef</span> and <span class="nctnt ncbi-class">CObject</span> classes. The <span class="nctnt ncbi-class">CRef</span> class provides the interface methods to access the pointer and the <span class="nctnt ncbi-class">CObject</span> is used to store the object and the reference count.

More [CObject classes](ch_core.html#ch_core.smart_ptrs) are presented in a later chapter.

### Portability definitions

To help with portability, the CORELIB uses only those C/C++ standard types that have some guarantees about size and representation. In particular, use of long, long long, float is not recommended for portable code.

To help with portability, integer types such as <span class="nctnt ncbi-code">Int1</span>, <span class="nctnt ncbi-code">Uint1</span>, <span class="nctnt ncbi-code">Int2</span>, <span class="nctnt ncbi-code">Uint2</span>, <span class="nctnt ncbi-code">Int4</span>, <span class="nctnt ncbi-code">Uint4</span>, <span class="nctnt ncbi-code">Int8</span>, <span class="nctnt ncbi-code">Uint8</span> have been defined with constant limits. For example, a signed integer of two bytes size is defined as type <span class="nctnt ncbi-code">Int2</span> with a minimum size of <span class="nctnt ncbi-code">kMin\_I2</span> and a maximum size of <span class="nctnt ncbi-code">kMax\_I2</span>. There are minimum and maximum limit constants defined for each of the different integer types.

More details on [standard portable data types](ch_core.html#ch_core.std_ncbi_types) are presented in a later chapter.

### Portable Exception Handling

C++ defines a structured exception handling mechanism to catch and process errors in a block of code. When the error occurs an exception is thrown and caught by an exception handler. The exception handler can then try to recover from the error, or process the error. In the C++ standard, there is only one exception class (std::exception), that stores a text message that can be printed out. The information reported by the std::exception may not be enough for a complex system. The CORELIB defines a portable <span class="nctnt ncbi-class">CException</span> class derived from std::exception class that remedies the short comings of the standard exception class

The CORELIB defines a portable <span class="nctnt ncbi-class">CException</span> class derived from std::exception class. The <span class="nctnt ncbi-class">CException</span> class in turn serves as a base class for many other exception classes specific to an application area such as the <span class="nctnt ncbi-class">CCoreException</span>, <span class="nctnt ncbi-class">CAppException</span>, <span class="nctnt ncbi-class">CArgException</span>, <span class="nctnt ncbi-class">CFileException</span>, and so on. Each of these derived classes can add facilities specific to the application area they deal with.

These exception classes provides many useful facilities such as a unique identification for every exception that is thrown, the location (file name and line number) where the exception occurred, references to lower-level exceptions that have already been thrown so that a more complete picture of the chain of exceptions is available, ability to report the exception data into an arbitrary output channel such as a diagnostic stream, and format the message differently for each channel.

More details on [exceptions and exception handling](ch_debug.html#ch_debug.ncbi_cpp_exceptions) are presented in a later chapter.

### Portable Process Pipes

A pipe is a common mechanism used to establish communications between two separate processes. The pipe serves as a communication channel between processes.

The CORELIB defines the <span class="nctnt ncbi-class">CPipe</span> class that provides a portable inter-process communications facility between a parent process and its child process. The pipe is created by specifying the command and arguments used to start the child process and specifying the type of data channels (text or binary) that will connect the processes. Data is sent across the pipe using the <span class="nctnt ncbi-class">CPipe</span> read and write methods.

### Registry

<span class="nctnt highlight">N.B.</span> The preferred way to define configuration parameters for an application is to use the macros in the <span class="nctnt ncbi-class">CParam</span> class (e.g. <span class="nctnt ncbi-macro">NCBI\_PARAM\_DECL</span>). More [details on the CParam class and its macros](ch_core.html#ch_core.Configuration_Parame) are presented in a later chapter. If the <span class="nctnt ncbi-class">CParam</span> class cannot be used, then the registry may be used instead.

The settings for an application may be read from a configuration or initialization file (the "registry"). This configuration file may define the parameters needed by the application. For example, many Unix programs read their parameter settings from configuration files. Similarly, Windows programs may read and store information in an internal registry database, or an initialization file.

The <span class="nctnt ncbi-class">CNcbiRegistry</span> class provides a portable facility to access, modify and store runtime information read from a configuration file. The configuration file consists of sections. A section is defined by a section header of the form [***section-header-name***]. Within each section, the parameters are defined using (name, value) pairs and represented as ***name=value*** strings. The syntax closely resembles the '<span class="nctnt ncbi-path">.ini</span>' files used in Windows and also by Unix tools such as Samba.

More details on the [Registry](ch_core.html#ch_core.CNcbiRegistry) are presented in a later chapter.

### STL Use Hints

To minimize naming conflicts, all NCBI code is placed in the ncbi name space. The CORELIB defines a number of portable macros to help manage name space definitions. For example, you can use the <span class="nctnt ncbi-macro">BEGIN\_NAME\_SPACE</span> macro at the start of a section of code to place that code in the specified name space. The <span class="nctnt ncbi-macro">END\_NAME\_SPACE</span> macros is used to indicate the end the of the name space definition. To declare the use of the NCBI namespace, the macros <span class="nctnt ncbi-macro">USING\_NCBI\_SCOPE</span> is used.

A number of macros have been defined to handle non-standard behavior of C++ compilers. For example, a macro <span class="nctnt ncbi-macro">BREAK</span> is defined, that is used to break out of a loop, instead of using the break statement directly. This is done to handle a bug in the Sun WorkShop (pre 5.3 version) compiler that fails to call destructors for objects created in for-loop initializers. Another example is that some compilers (example, Sun Pro 4.2) do not understand the using namespace std; statement. Therefore, for portable code, the using namespace statement should be prohibited.

More details on the [use of portable macros](ch_style.html#ch_style.using_NCBI_namespace) are presented in a later chapter.

### String Manipulations

C++ defines the standard string class that provides operations on strings. However, compilers may exhibit non-portable string behavior especially with regards to multi-threaded programs. The CORELIB provides portable string manipulation facilities through the <span class="nctnt ncbi-class">NStr</span> class that provides a number of class-wide functions for string manipulation.

<span class="nctnt ncbi-class">NStr</span> portable functions include the string-to-X and X-to-string conversion functions where X is a data type including a pointer type, string comparisons with and without case, pattern searches within a string, string truncation, substring replacements, string splitting and join operations, string tokenization, etc.

### Template Utilities

The C++ Template classes support a number of useful template classes for data structures such as vectors, lists, sets, maps, and so on.

The CORELIB defines a number of useful utility template classes. Some examples are template classes and functions for checking for equality of objects through a pointer, checking for non-null values of pointers, getting and setting map elements, deleting all elements from a container of pointers where the container can be a list, vector, set, multiset, map or multimap.

More details on the [template utilities](ch_core.html#ch_core.template_utils) are presented in a later chapter.

### Threads

Applications can run faster, if they are structured to exploit any inherent parallelism in the application's code execution paths. Code execution paths in an application can be assigned to separate threads. When the application is run on a multiprocessor system, there can be significant improvements in performance especially when threads run in parallel on separate processors.

The CORELIB defines a portable <span class="nctnt ncbi-class">CThread</span> class that can be used to provide basic thread functionality such as thread creation, thread execution, thread termination, and thread cleanup.

To create user defined threads you need to derive your class from <span class="nctnt ncbi-class">CThread</span>, and override the thread's <span class="nctnt ncbi-func">Main()</span> method and, and if necessary the <span class="nctnt ncbi-func">OnExit()</span> method for thread-specific cleanup. Next, you create a thread object by instantiating the class you derived from <span class="nctnt ncbi-class">CThread</span>. Now you are ready to launch thread execution by calling the thread's <span class="nctnt ncbi-func">Run()</span> method. The <span class="nctnt ncbi-func">Run()</span> method starts thread execution and the thread will continue to run until it terminates. If you want the thread to run independently of the parent thread you call the thread's <span class="nctnt ncbi-func">Detach()</span> method. If you want to wait till the thread terminates, you call the thread's <span class="nctnt ncbi-func">Join()</span> method.

Synchronization between threads is provided through mutexes and read/write locks.

More details on [threads](ch_core.html#ch_core.threads) and [synchronization](ch_core.html#ch_core.mutexes) are presented in a later chapter.

### Time

The <span class="nctnt ncbi-class">CTime</span> class provides a portable interface to date and time functions. <span class="nctnt ncbi-class">CTime</span> can operate with both local and UTC time, and can be used to store data and time at a particular moment or elapsed time. The time epoch is defined as Jan 1, 1900 so you cannot use <span class="nctnt ncbi-class">CTime</span> for storing timestamps before Jan 1, 1900.

The <span class="nctnt ncbi-class">CTime</span> class can adjust for daylight savings time. For display purposes, the time format can be set to a variety of time formats specified by a format string. For example, "M/D/Y h:m:s" for a timestamp of "5/6/03 14:07:09". Additional time format specifiers are defined for full month name (B), abbreviated month name (b), nanosecond (S), timezone format (Z), full weekday name (W) and abbreviated weekday name (w).

A class <span class="nctnt ncbi-class">CStopWatch</span> is also available that acts as a stop watch and measures elapsed time via the <span class="nctnt ncbi-func">Elapsed()</span> method, after its <span class="nctnt ncbi-func">Start()</span> method is called.

More details on the [CTime class](ch_core.html#ch_core.portable_time_class) are presented in a later chapter.

The ALGORITHM Module
--------------------

The ALGORITHM module is a collection of rigorously defined, often computationally intensive algorithms performed on sequences. It is divided into three groups:

-   ALIGN. A set of global alignment algorithms, including generic Needleman-Wunsch, a linear-space Hirschberg's algorithm and a spliced (cDna/mRna-to-Genomic) alignment algorithm.

-   BLAST. Basic Local Alignment Tool code and interface.

-   SEQUENCE. Various algorithms on biological sequences, including antigenic determinant prediction, CPG-island finder, ORF finder, string matcher and others.

The CGI Module
--------------

The CGI module provides an integrated framework for writing CGI applications. It consists of classes that implement the CGI (Common Gateway Interface). These classes are used to retrieve and parse an HTTP request, and then compose and deliver an HTTP response.

The CGI module consists of a number of classes. The interaction between these classes is fairly complex, and therefore, not covered in this introductory chapter. We will attempt to only identify the major classes in this overview, and cover the details of their interaction in later chapters. Among the more important of the CGI classes are the <span class="nctnt ncbi-class">CCgiApplication</span>, <span class="nctnt ncbi-class">CCgiContext</span>, <span class="nctnt ncbi-class">CCgiRequest</span>, <span class="nctnt ncbi-class">CCgiResponse</span>, and <span class="nctnt ncbi-class">CCgiCookie</span>.

The <span class="nctnt ncbi-class">CCgiApplication</span> is used to define the CGI application and is derived from the <span class="nctnt ncbi-class">CNcbiApplication</span>[discussed eariler](#-discussed-eariler). You write a CGI application by deriving application class from <span class="nctnt ncbi-class">CCgiApplication</span> and providing an adoption of the <span class="nctnt ncbi-func">Init()</span>, <span class="nctnt ncbi-func">Run()</span>, and <span class="nctnt ncbi-func">Exit()</span> methods inherited from the <span class="nctnt ncbi-class">CNcbiApplication</span> class. Details on [how to implement the Init(), Run() and Exit() methods for a CGI application](ch_cgi.html#ch_cgi.cgi_app_class) are provided in a later chapter.

The <span class="nctnt ncbi-class">CCgiRequest</span> class is designed to receive and parse the request, and the <span class="nctnt ncbi-class">CCgiResponse</span> class outputs the response to an output stream.

The <span class="nctnt ncbi-class">CCgiCookie</span> class models a *cookie*. A cookie is a name, value string pair that can be stored on the user's web browser in an attempt to remember a session state. All incoming <span class="nctnt ncbi-class">CCgiCookies</span> are parsed and stored by the <span class="nctnt ncbi-class">CCgiRequest</span> object, and the outgoing cookies are sent along with the response by the <span class="nctnt ncbi-class">CCgiResponse</span> object.

The CGI application executes in a 'context' defined by the <span class="nctnt ncbi-class">CCgiContext</span> class. The <span class="nctnt ncbi-class">CCgiContext</span> class provides a wrapper for the <span class="nctnt ncbi-class">CCgiApplication</span>, <span class="nctnt ncbi-class">CCgiRequest</span> and <span class="nctnt ncbi-class">CCgiResponse</span> objects and drives the processing of input requests.

More details on [CGI classes and their interactions](ch_cgi.html#ch_cgi.cgi_class_overview) are presented in a later chapter.

The CONNECT Module
------------------

The CONNECT module implements a variety of interfaces and classes dealing with making connections to (mostly) network services. The core of the Connection Library is written in C which provides a low level interface to the communication protocols. The CONNECT module provides C++ interfaces so that the objects have diagnostic and error handling capabilities that are consistent with the rest of the Toolkit. The standard sockets (SOCK) API is implemented on a variety of platforms such as Unix, MS-Windows, MacOS, Darwin. The CONNECT module provides a higher level access to the SOCK API by using C++ wrapper classes.

The following is a list of topics presented in this section:

-   [Socket classes](#socket-classes)

-   [Connector and Connection Handles](#connector-and-connection-handles)

-   [Connection Streams](#connection-streams)

-   [Sendmail API](#sendmail-api)

-   [Threaded Server](#threaded-server)

### Socket classes

The C++ classes that implement the socket interface are <span class="nctnt ncbi-class">CSocket</span>, <span class="nctnt ncbi-class">CDatagramSocket</span>, <span class="nctnt ncbi-class">CListeningSocket</span>, and <span class="nctnt ncbi-class">CSocketAPI</span>. The socket defines an end point for a connection which consists of an IP address (or host name) of the end point, port number and transport protocol used (TCP, UDP).

The <span class="nctnt ncbi-class">CSocket</span> class encapsulates the descriptions of both local and remote end points. The local end point, which is the end point on the client issuing a connection request, is specified by parameters to the <span class="nctnt ncbi-class">CSocket</span> constructor. The remote end point on which the network service is running is specified by parameters to the <span class="nctnt ncbi-func">Connect()</span> method for the <span class="nctnt ncbi-class">CSocket</span> class. The <span class="nctnt ncbi-class">CSocket</span> class defines additional methods to manage the connection such as <span class="nctnt ncbi-func">Reconnect()</span> to reconnect to the same end point as the <span class="nctnt ncbi-func">Connect()</span> method; the <span class="nctnt ncbi-func">Shutdown()</span> method to terminate the connection; the <span class="nctnt ncbi-func">Wait()</span> method to wait on several sockets at once; the <span class="nctnt ncbi-func">Read()</span> and <span class="nctnt ncbi-func">Write()</span> methods to read and write data via the socket; and a number of other support methods.

<span class="nctnt ncbi-class">CSocket</span> is designed for connection-oriented services such as those running over the TCP transport protocol. For connectionless, or datagram services, such as those running over the UDP transport protocol, you must use the <span class="nctnt ncbi-class">CDatagramSocket</span> class. The local end point is specified by parameters to the <span class="nctnt ncbi-class">CDatagramSocket</span> constructor. The remote end point is specified by parameters to the <span class="nctnt ncbi-func">Connect()</span> method for the <span class="nctnt ncbi-class">CDatagramSocket</span> class. Unlike the case of the connection-oriented services, this <span class="nctnt ncbi-func">Connect()</span> method only specifies the default destination address, and does not restrict the source address of the incoming messages. The methods <span class="nctnt ncbi-func">Send()</span> and <span class="nctnt ncbi-func">Recv()</span> are used to send the datagram, and the method <span class="nctnt ncbi-func">SetBroadcast()</span> sets the socket to broadcast messages sent to the datagram socket. The <span class="nctnt ncbi-class">CDatagramSocket</span> is derived from the <span class="nctnt ncbi-class">CSocket</span> class but methods such as <span class="nctnt ncbi-func">Shutdown()</span> and <span class="nctnt ncbi-func">Reconnect()</span> that apply to connection-oriented services are not available to users of the <span class="nctnt ncbi-class">CDatagramSocket</span> class.

The <span class="nctnt ncbi-class">CListeningSocket</span> is used by server-side applications to listen for connection requests. The <span class="nctnt ncbi-class">CListeningSocket</span> constructor specifies the port to listen to and the size of the connection request queue. You can change the port that the server application listens to any time by using the <span class="nctnt ncbi-func">Listen()</span> method. The <span class="nctnt ncbi-func">Accept()</span> method accepts the connection request, and returns a <span class="nctnt ncbi-class">CSocket</span> object through which data is transferred.

The <span class="nctnt ncbi-class">CSocketAPI</span> is a C++ wrapper for class-wide common socket utility functions available for sockets such as the <span class="nctnt ncbi-func">gethostname()</span>, <span class="nctnt ncbi-func">gethostbyaddr()</span>, <span class="nctnt ncbi-func">ntoa()</span>, <span class="nctnt ncbi-func">aton()</span>, and so on.

### Connector and Connection Handles

The SOCK interface is a relatively low-level interface for connection services. The CONNECT module provides a generalization of this interface to connection services using a connection type and specialized connectors.

A connection is modeled by a connection type and a connector type. The connector type models the end point of the connection, and the connection type, the actual connection. Together, the connector and connection objects are used to define the following types of connections: file, ftp, http, memory, pipe, service, and socket.

The connector is described by a connector handle, CONNECTOR. CONNECTOR is a typedef and defined as a pointer to an internal data structure.

The connection is described by a connection handle CONN. CONN is a typedef and defined as a pointer to an internal structure. The CONN type is used as a parameter to a number of functions that handle the connection such as <span class="nctnt ncbi-func">CONN\_Create()</span>, <span class="nctnt ncbi-func">CONN\_ReInit()</span>, <span class="nctnt ncbi-func">CONN\_Read()</span>, <span class="nctnt ncbi-func">CONN\_Write()</span>, etc.

For socket connectors, for example, the CONNECTOR handle is created by a call to the <span class="nctnt ncbi-func">SOCK\_CreateConnector()</span> function and passed the host name to connect to, the port number on the host to connect to, and maximum number of retries. The CONNECTOR handle is then passed as an argument to the <span class="nctnt ncbi-func">CONN\_Create()</span> which returns a CONNECTION handle. The CONNECTION handle is then used with the connection functions (that have the prefix CONN\_) to process the connection. The connection so created is bi-directional (full duplex) and input and output data can be processed simultaneously.

The other connector types are similar to the socket connector type. In the case of a file connector, the connector handle is created by calling the <span class="nctnt ncbi-func">FILE\_CreateConnector()</span> function and passed an input file and an output file (optionally, one of these can be NULL). This connector could be used for both reading and writing files, when input comes from one file, and output goes to another file. This differs from normal file I/O when a single handle is used to access only one file, but resembles data exchange via sockets, instead. In the case of the HTTP connection, the <span class="nctnt ncbi-type">HTTP\_CreateConnector</span> type is called and passed a pointer to network information structure, a pointer to a user-header consisting of HTTP tag-values, and a bitmask representing flags that affect the HTTP response.

The general service connector is the most complex connector in the library, and can model any type of service. It can be used for data transfer between an application and a named service. The data can be sent via HTTP or directly as a byte stream (using SOCK directly). In the former case it uses the HTTP connectors and in the latter the SOCK connectors. The general service connector is used when the other connector types are not adequate for implementing the task on hand.

More details on [connector classes](ch_conn.html#ch_conn.conn_def) are presented in a later chapter.

### Connection Streams

The CONNECT module provides a higher level of abstraction to connection programming in the form of C++ connection stream classes derived from the <span class="nctnt ncbi-class">CNcbiIostream</span> class, which is in turn typedef'd as <span class="nctnt ncbi-class">std::iostream</span>. This makes the familiar stream I/O operators and manipulators available to the connection stream. In addition, <span class="nctnt ncbi-class">CConn\_IOStream</span> performs more error checking than <span class="nctnt ncbi-class">std::iostream</span> and it allows input operations to be tied to the output operations so that any input attempt first flushes the output queue from the internal buffers.

[Figure 2](#figure-2) shows the most common connection stream classes derived from <span class="nctnt ncbi-class">CConn\_IOStream</span> - <span class="nctnt ncbi-class">CConn\_HttpStream</span>, <span class="nctnt ncbi-class">CConn\_MemoryStream</span>, <span class="nctnt ncbi-class">CConn\_ServiceStream</span>, and <span class="nctnt ncbi-class">CConn\_SocketStream</span>.

![Figure 2. Connection stream classes](/book-test/static/img/CConn_IOStream.gif)

Figure 2. Connection stream classes

<span class="nctnt ncbi-class">CConn\_HttpStream</span> models a stream of data between and HTTP client and an HTTP server (such as a web server). The server end of the stream is identified by a URL of the form <span class="nctnt ncbi-monospace">http[s]://host[:port]/path[?query]</span>.

<span class="nctnt ncbi-class">CConn\_MemoryStream</span> stream models data transfer in memory similar to the C++ strstream class.

<span class="nctnt ncbi-class">CConn\_ServiceStream</span> stream models data transfer with a named service that can be found via dispatcher/load-balancing daemon and implemented as either HTTP CGI, standalone server, or NCBI service.

<span class="nctnt ncbi-class">CConn\_SocketStream</span> models a stream of bytes in a bi-directional TCP connection between two end points specified by a host/port pair. As the name suggests the socket stream uses the socket interface directly.

More details on [connection stream classes](ch_conn.html#ch_conn.cpp_connection_streams) are presented in a later chapter.

### Sendmail API

The CONNECT module provides an API that provides access to SMTP protocol. SMTP (Simple Mail Transfer Protocol) is a standard email relaying protocol used by many popular MTAs (Message Transfer Agents), such as sendmail, smail, etc, found on many systems. SMTP passes (relays) email messages between hosts in the Internet all the way from sender to recipient.

To initiate the use of the sendmail API, you must call the <span class="nctnt ncbi-func">SendMailInfo\_Int()</span> function that initializes structure <span class="nctnt ncbi-func">SSendMailInfo</span>, passed by a pointer. Your code then modifies the structure to contain proper information such as that expected in a mail header (To, From, CC, BCC fields) and other communication settings from their default values set at initialization. Then, you can send email using the <span class="nctnt ncbi-func">CORE\_SendMail()</span> or <span class="nctnt ncbi-func">CORE\_SendMailEx()</span> functions.

### Threaded Server

The CONNECT module provides support for multithreaded servers through the <span class="nctnt ncbi-class">CThreadedServer</span> class. The <span class="nctnt ncbi-class">CThreadedServer</span> class is an abstract class for network servers and uses thread pools. This class maintains a pool of threads, called worker threads, to process incoming connections. Each connection gets assigned to one of the worker threads, allowing the server to handle multiple requests in parallel while still checking for new requests.

You must derive your threaded server from the <span class="nctnt ncbi-class">CThreadedServer</span> class and define the <span class="nctnt ncbi-func">Process()</span> method to indicate what to do with each incoming connection. The <span class="nctnt ncbi-func">Process()</span> method runs asynchronously by using a separate thread for each request.

More details on threaded server classes are presented in a later chapter.

The CTOOL Module
----------------

The CTOOL module provides bridge mechanisms and conversion functions. More specifically, the CTOOL module provides a number of useful functions such as a bridge between the NCBI C++ Toolkit and the older C Toolkit for error handling, an ASN.1 connections stream that builds on top of the [connection stream](#connection-stream), and an ASN converter that provides templates for converting ASN.1-based objects between NCBI's C and C++ in-memory layouts.

The ASN.1 connections support is provides through functions <span class="nctnt ncbi-func">CreateAsnConn()</span> for creating an ASN stream connection; <span class="nctnt ncbi-func">CreateAsnConn\_ServiceEx()</span> for creating a service connection using the service name, type and connection parameters; and <span class="nctnt ncbi-func">CreateAsnConn\_Service()</span> which is a specialized case of <span class="nctnt ncbi-func">CreateAsnConn\_ServiceEx()</span> with some parameters set to zero.

The DBAPI Module
----------------

The DBAPI module supports object oriented access to databases by providing user classes that model a database as a data source to which a connection can be made, and on which ordinary SQL queries or stored procedure SQL queries can be issued. The results obtained can be navigated using a result class or using the 'cursor' mechanism that is common to many databases.

The user classes are used by a programmer to access the database. The user classes depend upon a database driver to allow low level access to the underlying relational database management system (RDBMS). Each type of RDBMS can be expected to have a different driver that provides this low level hook into the database. The database drivers are architected to provide a uniform interface to the user classes so that the database driver can be changed to connect to a different database without affecting the program code that makes use of the user classes. For a list of the database drivers for different database that are supported, see the [Supported DBAPI Drivers section](ch_dbapi.html#ch_dbapi.dbapi_drivers).

The following is a list of topics presented in this section:

-   [Database User Classes](#database-user-classes)

-   [Database Driver Architecture](#database-driver-architecture)

### Database User Classes

The interface to the database is provided by a number of C++ classes such as the <span class="nctnt ncbi-class">IDataSource, IDbConnection, IStatement, ICallableStatement, ICursor, IResultSet, IResultSetMetaData</span> . The user does not use these interfaces directly. Instead, the DBAPI module provides concrete classes that implement these interface classes. The corresponding concrete classes for the above mentioned interfaces are <span class="nctnt ncbi-class">CDataSource, CDbConnection, CStatement, CCallableStatement, CCursor, CResultSet, CResultSetMetaData</span>.

Before accessing to a specific database, the user must register the driver with the <span class="nctnt ncbi-class">CDriverManager</span> class which maintains the drivers registered for the application. The user does this by using the <span class="nctnt ncbi-class">CDriverManager</span> class' factory method <span class="nctnt ncbi-func">GetInstance()</span> to create an instance of the <span class="nctnt ncbi-class">CDriverManager</span> class and registering the driver with this driver manager object. For details on how this can be done, see the [Choosing the Driver](ch_dbapi.html#ch_dbapi.dbapi_choose_driver) section.

After the driver has been registered, the user classes can be used to access that database. There are a number of ways this can be done, but the most common method is to call the <span class="nctnt ncbi-class">IDataSource</span> factory method <span class="nctnt ncbi-func">CreateDs()</span> to create an instance of the data source. Next, the <span class="nctnt ncbi-func">CreateConnection()</span> method for the data source is called, to return a connection object that implements the <span class="nctnt ncbi-class">IConnection</span> interface. Next, the connection object's <span class="nctnt ncbi-func">Connect()</span> method is called with the user name, password, server name, database name to make the connection to the database. Next, the connection object's <span class="nctnt ncbi-func">CreateStatement()</span> method is called to create a statement object that implements the <span class="nctnt ncbi-class">IStatement</span> interface. Next, the statement object's <span class="nctnt ncbi-func">Execute()</span> method is called to execute the query. Note that additional calls to the <span class="nctnt ncbi-func">IConnection::CreateStatement()</span> results in cloning the connection for each statement which means that these connections inherit the database which was specified in the <span class="nctnt ncbi-func">Connect()</span> or <span class="nctnt ncbi-func">SetDatabase()</span> method.

Executing the statement objects' <span class="nctnt ncbi-func">Execute()</span> method returns the result set which is stored in the statement object and can be accessed using the statement object's <span class="nctnt ncbi-func">GetResultSet()</span> method. You can then call the statement object's <span class="nctnt ncbi-func">HasRows()</span> method which returns a Boolean true if there are rows to be processed. The type of the result can be obtained by calling the <span class="nctnt ncbi-func">IResultSet::GetResultType() method</span>. The <span class="nctnt ncbi-func">IStatement::ExecuteUpdate()</span> method is used for SQL statements that do not return rows (UPDATE or DELETE SQL statement), in which case the method <span class="nctnt ncbi-func">IStatement::GetRowCount()</span> returns the number of updated or deleted rows.

Calling the <span class="nctnt ncbi-func">IStatement::GetResultSet()</span> returns the rows via the result set object that implements the <span class="nctnt ncbi-class">IResultSet</span> interface. The method <span class="nctnt ncbi-func">IResultSet::Next()</span> is used to fetch each row in the result set and returns a false when no more fetch data is available; otherwise, it returns a true. All column data, except BLOB data is represented by a <span class="nctnt ncbi-class">CVariant</span> object. The method <span class="nctnt ncbi-func">IResultSet::GetVariant()</span> takes the column number as its parameter where the first column has the start value of 1.

The <span class="nctnt ncbi-class">CVariant</span> class is used to describe the fields of a record which can be of any data type. The <span class="nctnt ncbi-class">CVariant</span> has a set of accessory methods (<span class="nctnt ncbi-func">GetXXX()</span>) to extract a value of a particular type. For example, the <span class="nctnt ncbi-func">GetInt4(), GetByte(), GetString()</span>, methods will extract an Int4, Byte data value from the <span class="nctnt ncbi-class">CVariant</span> object. If data extraction is not possible because of incompatible types, the <span class="nctnt ncbi-class">CVariantException</span> is thrown. The <span class="nctnt ncbi-class">CVariant</span> has a set of factory methods for creating objects of a particular data type, such as <span class="nctnt ncbi-func">CVariant::BigInt()</span> for Int8, <span class="nctnt ncbi-func">CVariant::SmallDateTime()</span> for NCBI's <span class="nctnt ncbi-class">CTime</span>, and so on.

For sample code illustrating the above mentioned steps, see the [Data Source and Connections](ch_dbapi.html#ch_dbapi.dbapi_src_cnxns) and [Main Loop](ch_dbapi.html#ch_dbapi.dbapi_main_loop) sections.

### Database Driver Architecture

The driver can use two different methods to access the particular RDBMS. If RDBMS provides a client library (<span class="nctnt ncbi-lib">CTLib</span>) for a given computer system, then the driver utilizes this library. If there is no client library, then the driver connects to RDBMS through a special gateway server which is running on a computer system where such library does exist.

The database driver architecture has two major groups of the driver's objects: the RDBMS independent objects, and the RDBMS dependent objects specific to a RDBMS. From a user's perspective, the most important RDBMS dependent object is the driver context object. A connection to the database is made by calling the driver context's <span class="nctnt ncbi-func">Connect()</span> method. All driver contexts implement the same interface defined in the <span class="nctnt ncbi-class">I\_DriverContext</span> class.

If the application needs to connect to RDBMS libraries from different vendors, there is a problem trying to link statically with the RDBMS libraries from different vendors. The reason for this is that most of these libraries are written in C, and may use the same names which cause name collisions. Therefore, the <span class="nctnt ncbi-class">C\_DriverMgr</span> is used to overcome this problem and allow the creation of a mixture of statically linked and dynamically loaded drivers and use them together in one executable.

The low level connection to an RDBMS is specific to that RDBMS. To provide RDBMS independence, the connection information is wrapped in an RDBMS independent object <span class="nctnt ncbi-class">CDB\_Connection</span>. The commands and the results are also wrapped in an RDBMS independent object. The user is responsible for deleting these RDBMS independent objects because the life spans of the RDBMS dependent and RDBMS independent objects are not necessarily the same.

Once you have the <span class="nctnt ncbi-class">CDB\_Connection</span> object, you can use it as a factory for the different types of command objects. The command object's <span class="nctnt ncbi-func">Result()</span> method can be called to get the results. To send and to receive the data through the driver you must use the driver provided datatypes such as <span class="nctnt ncbi-class">CDB\_BigInt, CDB\_Float, CDB\_SmallDateTime</span>. These driver data types are all derived from <span class="nctnt ncbi-class">CDB\_Object</span> class.

More [details on the database driver architecture](ch_dbapi.html#ch_dbapi.dbapi_driver_ref) is presented in a later chapter.

The GUI Module
--------------

The C++ Toolkit does not include its own GUI Module. Instead, Toolkit-based GUI applications make use of third party GUI packages. Depending on the requirements, we recommend either [wxWidgets](http://www.wxwidgets.org/) or [FOX](http://www.fox-toolkit.org/).

More details on developing GUI application in conjunction with the C++ Toolkit are presented in a [later chapter](ch_gui.html).

The HTML Module
---------------

The HTML module implements a number of HTML classes that are intended for use in CGI and other programs. The HTML classes can be used to generate HTML code dynamically.

The HTML classes can be used to represent HTML page internally in memory as a graph. Each HTML element or tag is represented by a node in the graph. The attributes for an HTML element are represented as attributes in the node. A node in the graph can have other elements as children. For example, for an HTML page, the top HTML element will be described by an HTML node in the graph. The HTML node will have the HEAD and BODY nodes as its children. The BODY node will have text data and other HTML nodes as its children. The graph structure representation of an HTML page allows easy additions, deletions and modification of the page elements.

Note that while the HTML classes can be used to represent the HTML page internally in memory as a graph there is no provision for parsing of existing HTML pages to generate these classes.

The following is a list of topics presented in this section:

-   [Relationships between HTML classes](#relationships-between-html-classes)

-   [HTML Processing](#html-processing)

### Relationships between HTML classes

The base class for all nodes in the graph structure for an HTML document is the <span class="nctnt ncbi-class">CNCBINode</span>. The <span class="nctnt ncbi-class">CNCBINode</span> class is derived from <span class="nctnt ncbi-class">CObject</span> and provides the ability to add, delete, and modify the nodes in the graph. The ability to add and modify nodes is inherited by all the classes derived from <span class="nctnt ncbi-class">CNCBINode</span> (see [Figure 3](#figure-3)). The classes derived from <span class="nctnt ncbi-class">CNCBINode</span> represent the HTML elements on an HTML page. You can easily identify the HTML element that a class handles from the class names such as <span class="nctnt ncbi-class">CHTMLText</span>, <span class="nctnt ncbi-class">CHTMLButtonList</span>, etc.

![Figure 3. HTML classes derived from CNCBINode](/book-test/static/img/CNCBINode.gif)

Figure 3. HTML classes derived from <span class="nctnt ncbi-class">CNCBINode</span>

The text node classes <span class="nctnt ncbi-class">CHTMLText</span> and <span class="nctnt ncbi-class">CHTMLPlainText</span> are intended to be used directly by the user. Both <span class="nctnt ncbi-class">CHTMLText</span> and <span class="nctnt ncbi-class">CHTMLPlainText</span> are used to insert text into the generated html, with the difference that <span class="nctnt ncbi-class">CHTMLPlainText</span> class performs HTML encoding before generation. A number of other classes such as <span class="nctnt ncbi-class">CHTMLNode</span>, <span class="nctnt ncbi-class">CHTMLElement</span>, <span class="nctnt ncbi-class">CHTMLOpenElement</span>, and <span class="nctnt ncbi-class">CHTMLListElement</span> are base classes for the elements actually used to construct an HTML page, such as <span class="nctnt ncbi-class">CHTML\_head</span>, <span class="nctnt ncbi-class">CHTML\_form</span> (see [Figure 4](#figure-4)).

![Figure 4. The CHTMLNode class and its derived classes](/book-test/static/img/CHTMLNode.gif)

Figure 4. The <span class="nctnt ncbi-class">CHTMLNode</span> class and its derived classes

The <span class="nctnt ncbi-class">CHTMLNode</span> class is the base class for <span class="nctnt ncbi-class">CHTMLElement</span> and <span class="nctnt ncbi-class">CHTMLOpenElement</span> and is used for describing the HTML elements that are found in an HTML page such as HEAD, BODY, H1, BR, etc. The <span class="nctnt ncbi-class">CHTMLElement</span> tag describes those tags that have a close tag and are well formed. The <span class="nctnt ncbi-class">CHTMLOpenElement</span> class describes tags that are often found without the corresponding close tag such as the BR element that inserts a line break. The <span class="nctnt ncbi-class">CHTMLListElement</span> class is used in lists such as the OL element.

Important classes of HTML elements used in forms to input data are the input elements such as checkboxes, radio buttons, text fields, etc. The <span class="nctnt ncbi-class">CHTML\_input</span> class derived from the <span class="nctnt ncbi-class">CHTML\_OpenElement</span> class serves as the base class for a variety of input elements (see [Figure 5](#figure-5)).

![Figure 5. The CHTML\_input class and its derived classes](/book-test/static/img/CHTML_input.gif)

Figure 5. The <span class="nctnt ncbi-class">CHTML\_input</span> class and its derived classes

More [details on HTML classes and their relationships](ch_html.html#ch_html.html_classes) is presented in a later chapter.

### HTML Processing

The HTML classes can be used to dynamically generate pages. In addition to the classes described in the previous section, there are a number of page classes that are designed to help with HTML processing. The page classes serve as generalized containers for collections of other HTML components, which are mapped to the page. [Figure 6](#figure-6) describes the important classes in page class hierarchy.

![Figure 6. HTML page classes](/book-test/static/img/CHTMLPage.gif)

Figure 6. HTML page classes

The <span class="nctnt ncbi-class">CHTMLBasicPage</span> class is as a base class whose features are inherited by the <span class="nctnt ncbi-class">CHTMLPage</span> derived class - it is not intended for direct usage. Through the methods of this class, you can access or set the CgiApplication, Style, and TagMap stored in the class.

The <span class="nctnt ncbi-class">CHTMLPage</span> class when used with the appropriate HTML template file, can generate the 'bolier plate' web pages such as a standard corporate web page, with a corporate logo, a hook for an application-specific logo, a top menubar of links to several databases served by a query program, a links sidebar for application-specific links to relevant sites, a VIEW tag for an application's web interface, a bottom menubar for help links, disclaimers, and other boiler plate links. The template file is a simple HTML text file with named tags (\<@tagname@\>) which allow the insertion of new HTML blocks into a pre-formatted page.

More [details on CHTMLBasicPage, CHTMLPage and related classes](ch_html.html#ch_html.page_classes) is presented in a later chapter.

The OBJECT MANAGER Module
-------------------------

The Object Manager module is a library of C++ classes, which facilitate access to biological sequence data. It makes it possible to transparently download data from the GenBank database, investigate biological sequence data structure, retrieve sequence data, descriptions and annotations.

The Object Manager has been designed to present an interface to users and to minimize their exposure to the details of interacting with biological databases and their underlying data structures. The Object Manager, therefore, coordinates the use of biological sequence data objects, particularly the management of the details of loading data from different data sources.

The NCBI databases and software tools are designed around a particular model of biological sequence data. The data model must be very flexible because the nature of this data is not yet fully understood, and its fundamental properties and relationships are constantly being revised. NCBI uses [Abstract Syntax Notation One](http://asn1.elibel.tm.fr) (ASN.1) as a formal language to describe [biological sequence data and its associated information](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/SDKDOCS/INDEX.HTML).

The bio sequence data may be huge and downloading all of this data may not be practical or desirable. Therefore, the Object Manager transparently transmits only the data that is really needed and not all of it at once. There is a [datatool](ch_app.html#ch_app.datatool) that generates corresponding data objects (source code and header files) from the object's ASN.1 specification. The Object Manager is able to manipulate these objects.

Biological sequences are identified by a [Seq\_id](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/SDKDOCS/SEQLOC.HTML), which may have different forms.

The most general container object of bio sequence data, as defined in the NCBI data model, is [Seq\_entry](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/SDKDOCS/SEQSET.HTML). A great deal of NCBI software is designed to accept a Seq\_entry as the primary unit of data. In general, the Seq\_entry is defined recursively as a tree of Seq\_entry objects, where each node contains either [Bioseq](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/SDKDOCS/BIOSEQ.HTML) or list of other Seq\_entry objects and additional data like sequence description, sequence annotations.

Two important concepts in the Object Manager are <span class="nctnt ncbi-monospace">scope</span> and <span class="nctnt ncbi-monospace">reference resolution</span>. The client defines a scope as the sources of data where the system uses only "allowed" sources to look for data. Scopes may contain several variants of the same bio sequence (Seq\_entry). Since sequences refer to each other, the scope sets may have some data that is common to both scopes. In this case changing data in one scope should be reflected in all other scopes, which "look" at the same data.

The other concept a client uses is <span class="nctnt ncbi-monospace">reference resolution</span>. Reference resolution is used in situations where different biological sequences can refer to each other. For example, a sequence of amino acids may be the same as sequence of amino acids in another sequence. The data retrieval system should be able to resolve such references automatically answering what amino acids are actually here. Optionally, at the client's request, such automatic resolution may be turned off.

The Object Manager provides a consistent view of the data despite modifications to the data. For example, the data may change during a client's session because new biological data has been uploaded to the database while the client is still processing the old data. In this case, when the client for additional data, the system should retrieve the original bio sequence data, and not the most recent one. However, if the database changes between a client's sessions, then the next time the client session is started, the most recent data is retrieved, unless the client specifically asks for the older data.

The Object Manager is thread safe, and supports multithreading which makes it possible to work with bio sequence data from multiple threads.

The Object Manager includes numerous classes for accessing bio sequence data such as <span class="nctnt ncbi-class">CDataLoader</span> and <span class="nctnt ncbi-class">CDataSource</span> which manage global and local accesses to data, <span class="nctnt ncbi-class">CSeqVector</span> and <span class="nctnt ncbi-class">CSeqMap</span> objects to find and manipulate sequence data, a number of [specialized iterators](ch_objmgr.html#ch_objmgr.om_def.html_Iterators) to parse descriptions and annotations, among others. The <span class="nctnt ncbi-class">CObjectManager</span> and <span class="nctnt ncbi-class">CScope</span> classes provide the foundation of the library, managing data objects and coordinating their interactions.

More [details on the Object Manager and related classes](ch_objmgr.html) is presented in a later chapter.

The SERIAL Module
-----------------

Click here to see [Full Documentation on the Data Serialization Library](ch_ser.html).

Serial library provides means for loading, accessing, manipulating, and serialization of data in a formatted way. It supports serialization in [ASN.1](http://asn1.elibel.tm.fr) (text or BER encoding), [XML](http://www.w3.org/XML), and [JSON](http://json.org) formats.

The structure of data is described by some sort of formal language. In our case it can be ASN.1, DTD or XML Schema. Based on such specification, [DATATOOL](ch_app.html#ch_app.datatool) application, which is part of the NCBI C++ Toolkit, generates a collection of data storage classes that can be used to store and serialize data. The design purpose was to make these classes as lightweight as possible, moving all details of serialization into specialized classes - [“object streams”](ch_ser.html#ch_ser.objstream.html_intro). Structure of the data is described with the help of [“type information”](ch_ser.html#ch_ser.typeinfo.html). Data objects contain data and type information only. Any such data storage object can be viewed as a node tree that provides random access to its data. Serial library provides means to[traversing](ch_ser.html#ch_ser.typeinfo.html_cobjinfo) this data tree without knowing its structure in advance – using only type information; C++ code generated by DATATOOL makes it possible to access any child node directly.

[“Object streams”](ch_ser.html#ch_ser.objstream.html_intro) are intermediaries between data storage objects and input or output stream. They perform encoding or decoding of data according to format specifications. Guided by the type information embedded into data object, on reading they allocate memory when needed, fill in data, and validate that all mandatory data is present; on writing they guarantee that all relevant data is written and that the resulting document is well-formed. All it takes to read or write a top-level data object is one function call – all the details are handled by an object stream.

Closely related to serialization is the task of converting data from one format into another. One approach could be reading data object completely into memory and then writing it in another format. The only problem is that the size of data can be huge. To simplify this task and to avoid storing data in memory, serial library provides [“object stream copier”](ch_ser.html#ch_ser.objstream.html_objcopy) class. It reads data by small chunks and writes it immediately after reading. In addition to small memory footprint, it also works much faster.

Input data can be very large in size; also, reading it completely into memory could not be the goal of processing. Having a large file of data, one might want to investigate information containers only of a particular type. Serial library provides a variety of means for doing this. The list includes [read](ch_ser.html#ch_ser.objstream.html_readhooks) and [write](ch_ser.html#ch_ser.objstream.html_writehooks) hooks, several types of [stream iterators](ch_ser.html#ch_ser.stream_iterators), and [filter templates](ch_ser.html#ch_ser.serial_filter). It is worth to note that, when using read hooks to read child nodes, one might end up with an invalid top-level data object; or, when using write hooks, one might begin with an invalid object and fill in missing data on the fly – in hooks.

In essence, “hook” is a callback function that client application provides to serial library. Client application installs the hook, then reads (or writes) data object, and somewhere from the depths of serialization processing, the library calls this hook function at appropriate times, for example, when a data chunk of specified type is about to be read. It is also possible to install [context-specific hooks](ch_ser.html#ch_ser.stack_path_hooks). Such hooks are triggered when serializing a particular object type in a particular context; for example, for all objects of class A which are contained in object B.

The UTIL Module
---------------

The UTIL module is collection of some very useful utility classes that implement I/O related functions, algorithms, container classes; text related and thread related functions. Individual facilities include classes to compute checksums, implement interval search trees, lightweight strings, string search, linked sets, random number generation, UTF-8 conversions, registry based DNS, rotating log streams, thread pools, and many others.

The following sections give an overview of the utility classes:

-   [Checksum](#checksum)

-   [Console Debug Dump Viewer](#console-debug-dump-viewer)

-   [Diff API](#diff-api)

-   [Floating Point Comparison](#floating-point-comparison)

-   [Lightweight Strings](#lightweight-strings)

-   [Range Support](#range-support)

-   [Linked Sets](#linked-sets)

-   [Random Number Generator](#random-number-generator)

-   [Registry based DNS](#registry-based-dns)

-   [Regular Expressions](#regular-expressions)

-   [Resizing Iterator](#resizing-iterator)

-   [Rotating Log Streams](#rotating-log-streams)

-   [Stream Support](#stream-support)

-   [String Search](#string-search)

-   [Synchronized and blocking queue](#synchronized-and-blocking-queue)

-   [Thread Pools](#thread-pools)

-   [UTF 8 Conversion](#utf-8-conversion)

### Checksum

The Checksum class implements CRC32 (Cyclic Redundancy Checksum 32-bit) calculation. The CRC32 is a 32-bit polynomial checksum that has many applications such as verifying the integrity of a piece of data. The <span class="nctnt ncbi-class">CChecksum</span> class implements the CRC32 checksum that can be used to compute the CRC of a sequence of byte values.

The checksum calculation is set up by creating a <span class="nctnt ncbi-class">CChecksum</span> object using the <span class="nctnt ncbi-class">CChecksum</span> constructor and passing it the type of CRC to be calculated. Currently only CRC32 is defined, so you must pass it the enumeration constant eCRC32 also defined in the class.

Data on which the checksum is to be computed is passed to the <span class="nctnt ncbi-class">CChecksum's</span><span class="nctnt ncbi-func">AddLine()</span> or <span class="nctnt ncbi-func">AddChars()</span> method as a character array. As data is passed to these methods, the CRC is computed and stored in the class. You can get the value of the computed CRC using the <span class="nctnt ncbi-func">GetChecksum()</span> method. Alternatively, you can use the <span class="nctnt ncbi-func">WriteChecksum()</span> method and pass it a <span class="nctnt ncbi-class">CNcbiOstream</span> object and have the CRC written to the output stream in the following syntax:

/\* Original file checksum: lines: *nnnn*, chars: *nnnn*, CRC32: *xxxxxxxx* \*/

### Console Debug Dump Viewer

The UTIL module implements a simple Console Debug Dump Viewer that enables the printing of object information on the console, through a simple console interface. Objects that can be debugged must be inherited from <span class="nctnt ncbi-class">CDebugDumpable</span> class. The <span class="nctnt ncbi-class">CObject</span> is derived from <span class="nctnt ncbi-class">CDebugDumpable</span>, and since most other objects are derived from <span class="nctnt ncbi-class">CObject</span> this makes these objects 'debuggable'.

The Console Debug Dump Viewer is implemented by the <span class="nctnt ncbi-class">CDebugDumpViewer</span> class. This class implements a breakpoint method called <span class="nctnt ncbi-func">Bpt()</span>. This method is called with the name of the object and a pointer to the object to be debugged. This method prompts the user for commands that the user can type from the console:

    Console Debug Dump Viewer
    Stopped at  testfile.cpp(120)
    current object: myobj = xxxxxx
    Available commands:
       t[ypeid]  address
       d[ump]    address  depth
       go

The <span class="nctnt ncbi-class">CDebugDumpViewer</span> class also permits the enabling and disabling of debug dump breakpoints from the [registry](#registry).

### Diff API

The Diff API includes the <span class="nctnt ncbi-class">CDiff</span> class for character-based diffs and the <span class="nctnt ncbi-class">CDiffText</span> class for line-based diffs. The API is based on the open source [Diff, Match and Patch Library](http://code.google.com/p/google-diff-match-patch/) and the [Diff Template Library](http://code.google.com/p/dtl-cpp/).

To use the Diff API, include <span class="nctnt ncbi-lib">xdiff</span> in the <span class="nctnt ncbi-macro">LIB</span> line of your application makefile, and include <span class="nctnt ncbi-code">\<util/diff/diff.hpp\></span> in your source.

The following sample code shows how to perform both character- and line-based diffs:

    // Print difference list in human readable format
    static void s_PrintDiff(const string& msg, const string& s1, const string& s2,
        const CDiffList& diff)
    {
        NcbiCout << msg << NcbiEndl
            << "Comparing '" << s1 << "' to '" << s2 << "':" << NcbiEndl;
        ITERATE(CDiffList::TList, it, diff.GetList()) {
            string op;
            size_t n1 = 0;
            size_t n2 = 0;

            if (it->IsDelete()) {
                op = "-";
                n1 = it->GetLine().first;
            } else if (it->IsInsert()) {
                op = "+";
                n2 = it->GetLine().second;
            } else {
                op = "=";
                n1 = it->GetLine().first;
                n2 = it->GetLine().second;
            }
            NCbiCout << op << " ("
                 << n1 << "," << n2 << ")"
                 << ": " << "'" << it->GetString() << "'" << NCbiEndl;
        }
    }

    // Perform a character-based diff:
    { {
        CTempString s1("how now");
        CTempString s2("brown cow");
        CDiff d;
        CDiffList& diffs(d.Diff(s1, s2));
        s_PrintDiff("Line-based diff:", s1, s2, diffs);
    } }

    // Perform a line-based diff:
    { {
        CTempString s1("group 1\nasdf asf\ntttt\nasdf asd");
        CTempString s2("group 2\nqwerty\n\nasdf\nasf asd");
        CDiffText d;
        CDiffList& diffs(d.Diff(s1, s2));
        s_PrintDiff("Line-based diff:", s1, s2, diffs);
    } }

For more detailed usage, see the test program:

<http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c%2B%2B/src/util/diff/test/>

### Floating Point Comparison

For technical reasons, direct comparison of "close" floating point values is simply not reliable on most computers in use today. Therefore, in cases where the values being compared might be close, it is advisable to apply a tolerance when making comparisons to avoid unexpected results.

The UTIL module defines a function, <span class="nctnt ncbi-func">g\_FloatingPoint\_Compare()</span>, that implements floating point comparison using a tolerance. In practice this means that code like:

        if (a < b) {
            if (c == d ) {
                if (e > f) {

should be rewritten as:

    #include <util/floating_point.hpp>
    //...
        if (g_FloatingPoint_Compare(a, eFP_LessThan, b,
                                    eFP_WithPercent, percent) {
            if (g_FloatingPoint_Compare(c, eFP_EqualTo, d,
                                        eFP_WithFraction, fraction) {
                if (g_FloatingPoint_Compare(e, eFP_GreaterThan, f,
                                            eFP_WithPercent, percent) {

Note that compared variables must be of the same floating point type, otherwise a compile error will be generated.

For further details on this function, see its Doxygen [documentation](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/floating__point_8hpp.html#a178b404beec22ce1c48057b7a4036c23).

For technical details on the subject, including what it means to be close, see "[Comparing floating point numbers](http://www.cygnus-software.com/papers/comparingfloats/comparingfloats.htm)" by Bruce Dawson.

### Lightweight Strings

Class <span class="nctnt ncbi-class">CTempString</span> implements a light-weight string on top of a storage buffer whose lifetime management is known and controlled.

<span class="nctnt ncbi-class">CTempString</span> is designed to perform no memory allocation but provide a string interaction interface congruent with std::basic\_string\<char\>.

As such, CTempString provides a const-only access interface to its underlying storage. Care has been taken to avoid allocations and other expensive operations wherever possible.

<span class="nctnt ncbi-class">CTempString</span> has constructors from std::string and C-style string, which do not copy the string data but keep char pointer and string length.

This way the construction and destruction are very efficient.

Take into account, that the character string array kept by <span class="nctnt ncbi-class">CTempString</span> object must remain valid and unchanged during whole lifetime of the <span class="nctnt ncbi-class">CTempString</span> object.

It's convenient to use the class <span class="nctnt ncbi-class">CTempString</span> as an argument of API functions so that no allocation or deallocation will take place on of the function call.

### Linked Sets

The UTIL module defines a template container class, <span class="nctnt ncbi-class">CLinkedMultiset</span>, that can hold a linked list of multiset container types.

The <span class="nctnt ncbi-class">CLinkedMultiset</span> defines iterator methods <span class="nctnt ncbi-func">begin()</span>, <span class="nctnt ncbi-func">end()</span>, <span class="nctnt ncbi-func">find()</span>, <span class="nctnt ncbi-func">lower\_bound()</span>, <span class="nctnt ncbi-func">upper\_bound()</span>, to help traverse the container. The method <span class="nctnt ncbi-func">get()</span>, fetches the contained value, the method <span class="nctnt ncbi-func">insert()</span> inserts a new value into the container, and the method <span class="nctnt ncbi-func">erase()</span>, removes the specified value from the container.

### Random Number Generator

The UTIL module defines the <span class="nctnt ncbi-class">CRandom</span> class that can be used for generating 32-bit unsigned random numbers. The random number generator algorithm is the Lagged Fibonacci Generator (LFG) algorithm.

The random number generator is initialized with a seed value, and then the <span class="nctnt ncbi-func">GetRandom()</span> method is called to get the next random number. You can also specify that the random number value that is returned be in a specified range of values.

### Range Support

The UTIL module provides a number of container classes that support a *range* which models an interval consisting of a set of ordered values. the <span class="nctnt ncbi-class">CRange</span> class stores information about an interval, **[*from*, *to*]**, where the ***from*** and ***to*** points are inclusive. This is sometimes called a *closed interval*.

Another class, the <span class="nctnt ncbi-class">CRangeMap</span> class, is similar to the <span class="nctnt ncbi-class">CRange</span> class but allows for the storing and retrieving of data using the interval as key. The time for iterating over the interval is proportional to the amount of intervals produced by the iterator and may not be efficient in some cases.

Another class, the <span class="nctnt ncbi-class">CIntervalTree</span> class, has the same functionality as the <span class="nctnt ncbi-class">CRangeMap</span> class but uses a different algorithm; that is, one based on McCreight's algorithm. Unlike the <span class="nctnt ncbi-class">CRangeMap</span> class, the <span class="nctnt ncbi-class">CIntervalTree</span> class allows several values to have the same key interval. This class is faster and its speed is not affected by the type of data but it uses more memory (about three times as much as <span class="nctnt ncbi-class">CRangeMap</span>) and, as a result, is less efficient when the amount of interval in the set is quite big. For example, the <span class="nctnt ncbi-class">CIntervalTree</span> class becomes less efficient than <span class="nctnt ncbi-class">CRangeMap</span> when the total memory becomes greater than processor cache.

More [details on range classes](ch_core.html#ch_core.template_typename_Co) are presented in a later chapter.

### Registry based DNS

The UTIL module defines the <span class="nctnt ncbi-class">CSmallDns</span> class that implements a simple [registry](#registry) based DNS server. The <span class="nctnt ncbi-class">CSmallDns</span> class provides DNS name to IP address translations similar to a standard DNS server, except that the database used to store DNS name to IP address mappings is a non-standard local database. The database of DNS names and IP address mappings are kept in a registry-like file named by local\_hosts\_file using section [LOCAL\_DNS].

The <span class="nctnt ncbi-class">CSmallDns</span> has two methods that are responsible for providing the DNS name to IP address translations: the <span class="nctnt ncbi-func">LocalResolveDNS</span> method and the <span class="nctnt ncbi-func">LocalBackResolveDNS</span> method. The <span class="nctnt ncbi-func">LocalResolveDNS</span> method does 'forward' name resolution. That is, given a host name, it returns a string containing the IP address in the dotted decimal notation. The <span class="nctnt ncbi-func">LocalBackResolveDNS</span> method does a 'reverse lookup'. That is, given an IP address as a dotted decimal notation string, it returns the host name stored in the registry.

### Regular Expressions

The UTIL module defines the [CRegexp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCRegexp.html) class that supports defining, compiling, and searching against the "Perl compatible" flavor of regular expressions (PCRE). Note that PCRE, as implemented in <span class="nctnt ncbi-lib">$(PCRE\_LIBS)</span>, is not actually 100% compatible with the flavor of regular expressions implemented in Perl interpreters.

A simple example of using <span class="nctnt ncbi-class">CRegexp</span>:

    #include <util/xregexp/regexp.hpp>
    ...
        CRegexp regex("^(bye\|exit\|quit)$", CRegexp::fCompile_ignore_case);
        bool time2quit = regex.IsMatch(line);

To use <span class="nctnt ncbi-class">CRegexp</span>, link with the <span class="nctnt ncbi-lib">xregexp</span> library:

    LIB  = xregexp $(PCRE_LIB) xncbi
    LIBS = $(PCRE_LIBS) $(ORIG_LIBS)

<span class="nctnt highlight">Note: </span><span class="nctnt ncbi-class">CRegexp</span> has no replacement functionality - i.e. there is no API support for replacing matched text with provided text or matched groups.

### Resizing Iterator

The UTIL module defines two template classes, the <span class="nctnt ncbi-class">CResizingIterator</span> and the <span class="nctnt ncbi-class">CConstResizingIterator</span> classes that handle sequences represented as packed sequences of elements of different sizes For example, a vector \<char\> might actually hold 2-bit values, such as nucleotides, or 32-bit integer values.

The purpose of these iterator classes is to provide iterator semantics for data values that can be efficiently represented as a packed sequence of elements regardless of the size.

### Rotating Log Streams

The UTIL module defines the <span class="nctnt ncbi-class">CRotatingLogStream</span> class that can be used to implement a rotating log file. The idea being that once the log of messages gets too large, a 'rotation' operation can be performed. The default rotation is to rename the existing log file by appending it with a timestamp, and opening a new log.

The rotating log can be specified as a file, with an upper limit (in bytes) to how big the log will grow. The <span class="nctnt ncbi-class">CRotatingLogStream</span> defines a method called <span class="nctnt ncbi-func">Rotate()</span> that implements the default rotation.

### Stream Support

The UTIL module defines a number of portable classes that provide additional stream support beyond that provided by the standard C++ streams. The <span class="nctnt ncbi-class">CByteSource</span> class acts as an abstract base class (see [Figure 7](#figure-7)), for a number of stream classes derived from it. As the name of the other classes derived from <span class="nctnt ncbi-class">CByteSource</span> suggests, each of these classes provides the methods from reading from the named source. To list a few examples: <span class="nctnt ncbi-class">CFileByteSource</span> is a specialized class for reading from a named file; <span class="nctnt ncbi-class">CMemoryByteSource</span> is a specialized class for reading from a memory buffer; <span class="nctnt ncbi-class">CResultByteSource</span> is a specialized class for reading database results; <span class="nctnt ncbi-class">CStreamByteSource</span> is a specialized class from reading from the C++ input stream (istream); <span class="nctnt ncbi-class">CFStreamByteSource</span> is a specialized class from reading from the C++ input file stream (ifstream).

![Figure 7. Relationship between CByteSource and its derived classes](/book-test/static/img/CByteSource.gif)

Figure 7. Relationship between <span class="nctnt ncbi-class">CByteSource</span> and its derived classes

The classes such as <span class="nctnt ncbi-class">CSubFileByteSource</span> are used to define a slice of the source stream in terms of a start position and a length. The read operations are then confined to this slice.

Additional classes, the <span class="nctnt ncbi-class">CIStreamBuffer</span> and the <span class="nctnt ncbi-class">COStreamBuffer</span> have been defined for standard input and output buffer streams. These can be used in situations where a compiler's implementation of the standard input and output stream buffering is inefficient.

More details on the stream classes are presented in a later chapter.

### String Search

The UTIL module defines the <span class="nctnt ncbi-class">CBoyerMooreMatcher</span> class and the <span class="nctnt ncbi-class">CTextFsm</span> class which are used for searching for a single pattern over varying texts.

The <span class="nctnt ncbi-class">CBoyerMooreMatcher</span> class, as the name suggests, uses the Boyer-Moore algorithm for string searches. The <span class="nctnt ncbi-class">CTextFsm</span> is a template class that performs the search using a finite state automaton for a specified to be matched data type. Since the matched data type is often a string, the <span class="nctnt ncbi-class">CTextFsa</span> class is defined as a convenience by instantiating the <span class="nctnt ncbi-class">CTextFsm</span> with the matched type template parameter set to string.

The search can be setup as a case sensitive or case insensitive search. The default is case sensitive search. In the case of the <span class="nctnt ncbi-class">CBoyerMooreMatcher</span> class, the search can be setup for any pattern match or a whole word match. A whole word match means that a pattern was found to be between white spaces. The default is any pattern match.

### Synchronized and blocking queue

The UTIL module defines class <span class="nctnt ncbi-class">CSyncQueue</span> which implements a thread-safe queue that has “blocking” semantics: when queue is empty <span class="nctnt ncbi-func">Pop()</span> method will effectively block execution until some elements will be added to the queue; when queue have reached its maximum size <span class="nctnt ncbi-func">Push()</span> method will block execution until some elements will be extracted from queue. All these operations can be controlled by timeout. Besides that <span class="nctnt ncbi-class">CSyncQueue</span> is not bound to first-in-first-out queue paradigm. It has underlying stl container (deque by default) which will define the nature of queue. This container is set via template parameter to <span class="nctnt ncbi-class">CSyncQueue</span> and can be deque, vector, list, CSyncQueue\_set, CSyncQueue\_multiset and CSyncQueue\_priority\_queue (the latter three are small addons to STL set, multiset and priority\_queue for the sake of compatibility with <span class="nctnt ncbi-class">CSyncQueue</span>).

There is also <span class="nctnt ncbi-class">CSyncQueue::TAccessGuard</span> class which can lock the queue for some bulk operations if during them queue should not be changed by other threads.

For more details on <span class="nctnt ncbi-class">CSyncQueue</span> look here: <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/CSyncQueueDescription.html>.

### Thread Pools

The UTIL module defines a number of classes implementing pool of threads.

<span class="nctnt ncbi-class">CThreadPool</span> is the main class. It executes any tasks derived from the <span class="nctnt ncbi-class">CThreadPool\_Task</span> class. The number of threads in pool is controlled by special holder of this policy — object derived from <span class="nctnt ncbi-class">CThreadPool\_Controller</span> (default implementation is <span class="nctnt ncbi-class">CThreadPool\_Controller\_PID</span> based on Proportional-Integral-Derivative algortithm). All threads executing by <span class="nctnt ncbi-class">CThreadPool</span> are the instances of <span class="nctnt ncbi-class">CThreadPool\_Thread</span> class or its derivatives.

More [details on threaded pool classes](ch_core.html#ch_core.Thread_Pools) are presented in a later chapter.

### UTF 8 Conversion

The UTIL module provides a number of functions to convert between UTF-8 representation, ASCII 7-bit representation and Unicode representations. For example, <span class="nctnt ncbi-func">StringToCode()</span> converts the first UTF-8 character in a string to a Unicode symbol, and <span class="nctnt ncbi-func">StringToVector()</span> converts a UTF-8 string into a vector of Unicode symbols.

The result of a conversion can be success, out of range, or a two character sequence of the skip character (0xFF) followed by another character.


