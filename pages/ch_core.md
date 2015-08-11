---
layout: default
title: C++ Toolkit test
nav: pages/ch_core
---

<span class="label">8</span>Portability, Core Functionality and Application Framework
=====================================================================================

Last Update: December 29, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

-   **CORELIB library** <span class="nctnt ncbi-path">xncbi</span>:[include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib)

The CORELIB provides a portable low-level API and many useful application framework classes for argument processing, diagnostics, environment interface, object and reference classes, portability definitions, portable exceptions, stream wrappers, string manipulation, threads, etc.

This chapter provides reference material for many of CORELIB's facilities. For an overview of CORELIB, please refer to the [CORELIB section in the introductory chapter](ch_intro.html#ch_intro.intro_corelib) on the C++ Toolkit.

<span class="nctnt highlight">Note</span>: The CORELIB must be linked to every executable that uses the NCBI C++ Toolkit!

-   **UTIL library** <span class="nctnt ncbi-path">xutil</span>:[include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/util) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/util)

The UTIL module is a collection of useful classes which can be used in more then one application. This chapter provides reference material for many of UTIL's facilities. For an overview of the UTIL module please refer to the [UTIL section in the introductory chapter](ch_intro.html#ch_intro.intro_util) on the C++ Toolkit.

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Writing a Simple Application](#writing-a-simple-application)

    -   [NCBI C++ Toolkit Application Framework Classes](#ncbi-c++-toolkit-application-framework-classes)

        -   [CNcbiApplication](#cncbiapplication)

        -   [CNcbiArguments](#cncbiarguments)

        -   [CNcbiEnvironment](#cncbienvironment)

        -   [CNcbiRegistry](#cncbiregistry)

        -   [CNcbiDiag](#cncbidiag)

    -   [Creating a Simple Application](#creating-a-simple-application)

        -   [Unix-like Systems](#unix-like-systems)

        -   [MS Windows](#ms-windows)

        -   [Discussion of the Sample Application](#discussion-of-the-sample-application)

    -   [Inside the NCBI Application Class](#inside-the-ncbi-application-class)

-   [Processing Command-Line Arguments](#processing-command-line-arguments)

    -   [Capabilities of the Command-Line API](#capabilities-of-the-command-line-api)

    -   [The Relationships between the CArgDescriptions, CArgs, and CArgValue Classes](#the-relationships-between-the-cargdescriptions-cargs-and-cargvalue-classes)

    -   [Command-Line Syntax](#command-line-syntax)

    -   [The CArgDescriptions (\*) Class](#the-cargdescriptions-*-class)

        -   [The CArgDescriptions Constructor](#the-cargdescriptions-constructor)

        -   [Describing Argument Attributes](#describing-argument-attributes)

        -   [Argument Types](#argument-types)

        -   [Restricting the Input Argument Values](#restricting-the-input-argument-values)

        -   [Implementing User-defined Restrictions Using the CArgAllow Class](#implementing-user-defined-restrictions-using-the-cargallow-class)

        -   [Using CArgDescriptions in Applications](#using-cargdescriptions-in-applications)

        -   [Generating a USAGE Message](#generating-a-usage-message)

    -   [The CArgs (\*) Class: A Container Class for CArgValue (\*) Objects](#the-cargs-*-class-a-container-class-for-cargvalue-*-objects)

    -   [CArgValue (\*) Class: The Internal Representation of Argument Values](#cargvalue-*-class-the-internal-representation-of-argument-values)

    -   [Supporting Command-Based Command Lines](#supporting-command-based-command-lines)

    -   [Code Examples](#code-examples)

-   [Namespace, Name Concatenation, and Compiler-specific Macros](#namespace-name-concatenation-and-compiler-specific-macros)

    -   [NCBI Namespace](#ncbi-namespace)

    -   [Other Namespace Macros](#other-namespace-macros)

    -   [Name Concatenation](#name-concatenation)

    -   [Compiler-specific Macros](#compiler-specific-macros)

-   [Configuration Parameters](#configuration-parameters)

    -   [General Usage Information](#general-usage-information)

    -   [Macros for Creating Parameters](#macros-for-creating-parameters)

    -   [Methods for Using Parameters](#methods-for-using-parameters)

    -   [Supporting Classes](#supporting-classes)

-   [Using the CNcbiRegistry Class](#using-the-cncbiregistry-class)

    -   [Working with the Registry Class: CNcbiRegistry](#working-with-the-registry-class-cncbiregistry)

    -   [Syntax of the Registry Configuration File](#syntax-of-the-registry-configuration-file)

    -   [Search Order for Initialization (\*.ini) Files](#search-order-for-initialization-*ini-files)

    -   [Fine-Tuning Registry Parameters Using IRegistry::EFlags](#fine-tuning-registry-parameters-using-iregistryeflags)

    -   [Main Methods of CNcbiRegistry](#main-methods-of-cncbiregistry)

    -   [Additional Registry Methods](#additional-registry-methods)

-   [Portable Stream Wrappers](#portable-stream-wrappers)

-   [Working with Diagnostic Streams (\*)](#working-with-diagnostic-streams-*)

    -   [Where Diagnostic Messages Go](#where-diagnostic-messages-go)

    -   [Setting Diagnostic Severity Levels](#setting-diagnostic-severity-levels)

    -   [Diagnostic Messages Filtering](#diagnostic-messages-filtering)

    -   [Log File Format](#log-file-format)

        -   [The Old Post Format](#the-old-post-format)

        -   [The New Post Format](#the-new-post-format)

        -   [Controlling Appearance of Diagnostic Message Using Post Flags](#controlling-appearance-of-diagnostic-message-using-post-flags)

    -   [Defining the Output Stream](#defining-the-output-stream)

    -   [Tee Output to STDERR](#tee-output-to-stderr)

    -   [The Message Buffer](#the-message-buffer)

    -   [Request Exit Status Codes](#request-exit-status-codes)

        -   [Standard (HTTP-like) status codes](#standard-http-like-status-codes)

        -   [NCBI-specific status codes](#ncbi-specific-status-codes)

    -   [Error Codes and Their Descriptions](#error-codes-and-their-descriptions)

        -   [Preparing an Error Message File](#preparing-an-error-message-file)

    -   [Defining Custom Handlers Using CDiagHandler](#defining-custom-handlers-using-cdiaghandler)

    -   [The ERR\_POST and LOG\_POST Macros](#the-errpost-and-logpost-macros)

    -   [The \_TRACE macro](#the-trace-macro)

    -   [Performance Logging](#performance-logging)

    -   [Stack Traces](#stack-traces)

        -   [Printing a Stack Trace](#printing-a-stack-trace)

        -   [Obtaining a Stack Trace for Exceptions](#obtaining-a-stack-trace-for-exceptions)

-   [Debug Macros](#debug-macros)

-   [Handling Exceptions](#handling-exceptions)

-   [Defining the Standard NCBI C++ Types and Their Limits](#defining-the-standard-ncbi-c++-types-and-their-limits)

    -   [Headers Files Containing Portability Definitions](#headers-files-containing-portability-definitions)

    -   [Built-in Integral Types](#built-in-integral-types)

    -   [Auxiliary Types](#auxiliary-types)

    -   [Fixed-Size Integer Types](#fixed-size-integer-types)

    -   [The "Ncbi\_BigScalar" Type](#the-ncbibigscalar-type)

    -   [Encouraged and Discouraged Types](#encouraged-and-discouraged-types)

-   [Understanding Smart Pointers: the CObject and CRef Classes](#understanding-smart-pointers-the-cobject-and-cref-classes)

    -   [STL auto\_ptrs](#stl-autoptrs)

    -   [The CRef (\*) Class](#the-cref-*-class)

    -   [The CObject (\*) Class](#the-cobject-*-class)

    -   [The CObjectFor (\*) Class: Using Smart Pointers for Standard Types](#the-cobjectfor-*-class-using-smart-pointers-for-standard-types)

    -   [When to Use CRefs and auto\_ptrs](#when-to-use-crefs-and-autoptrs)

    -   [CRef Pitfalls](#cref-pitfalls)

        -   [Inadvertent Object Destruction](#inadvertent-object-destruction)

-   [Atomic Counters](#atomic-counters)

-   [Portable Mechanisms for Loading DLLs](#portable-mechanisms-for-loading-dlls)

    -   [CDll Constructor](#cdll-constructor)

    -   [CDll Basename](#cdll-basename)

    -   [Other CDll Methods](#other-cdll-methods)

-   [Executing Commands and Spawing Processes Using the CExec Class](#executing-commands-and-spawing-processes-using-the-cexec-class)

    -   [Executing a System Command Using the System() Method](#executing-a-system-command-using-the-system-method)

    -   [Defining Spawned Process Modes (EMode Type)](#defining-spawned-process-modes-emode-type)

    -   [Spawning a Process Using SpawnX() Methods](#spawning-a-process-using-spawnx-methods)

    -   [Waiting for a Process to Terminate Using the Wait() Method](#waiting-for-a-process-to-terminate-using-the-wait-method)

-   [Implementing Parallelism Using Threads and Synchronization Mechanisms](#implementing-parallelism-using-threads-and-synchronization-mechanisms)

    -   [Using Threads](#using-threads)

    -   [CThread (\*) Class Public Methods](#cthread-*-class-public-methods)

    -   [CThread (\*) Class Protected Methods](#cthread-*-class-protected-methods)

    -   [Thread Life Cycle](#thread-life-cycle)

    -   [Referencing Thread Objects](#referencing-thread-objects)

    -   [Thread Local Storage (CTls\<\> class [\*])](#thread-local-storage-ctls<>-class-*)

    -   [Mutexes](#mutexes)

        -   [CMutex](#cmutex)

        -   [CFastMutex](#cfastmutex)

        -   [SSystemMutex and SSystemFastMutex](#ssystemmutex-and-ssystemfastmutex)

        -   [CMutexGuard and CFastMutexGuard](#cmutexguard-and-cfastmutexguard)

        -   [Lock Classes](#lock-classes)

            -   [CRWLock](#crwlock)

            -   [CAutoRW](#cautorw)

            -   [CReadLockGuard](#creadlockguard)

            -   [CWriteLockGuard](#cwritelockguard)

            -   [CInternalRWLock](#cinternalrwlock)

            -   [CSemaphore](#csemaphore)

-   [Working with File and Directories Using CFile and CDir](#working-with-file-and-directories-using-cfile-and-cdir)

    -   [CDirEntry Class](#cdirentry-class)

    -   [CFile Class](#cfile-class)

    -   [CDir Class](#cdir-class)

    -   [CMemoryFile Class](#cmemoryfile-class)

-   [String APIs](#string-apis)

    -   [String Constants](#string-constants)

    -   [NStr Class](#nstr-class)

    -   [UTF-8 Strings](#utf-8-strings)

    -   [PCase and PNocase](#pcase-and-pnocase)

-   [Portable Time Class](#portable-time-class)

    -   [CTime Class Constructors](#ctime-class-constructors)

    -   [Other CTime Methods](#other-ctime-methods)

-   [Template Utilities](#template-utilities)

    -   [Function Objects](#function-objects)

    -   [Template Functions](#template-functions)

-   [Miscellaneous Types and Macros](#miscellaneous-types-and-macros)

    -   [Miscellaneous Enumeration Types](#miscellaneous-enumeration-types)

    -   [AutoPtr Class](#autoptr-class)

    -   [ITERATE Macros](#iterate-macros)

    -   [Sequence Position Types](#sequence-position-types)

-   [Containers](#containers)

    -   [template\<typename Coordinate\> class CRange](#template<typename-coordinate>-class-crange)

        -   [Typedefs](#typedefs)

        -   [Methods](#methods)

    -   [template\<typename Object, typename Coordinate = int\> class CRangeMap](#template<typename-object-typename-coordinate-=-int>-class-crangemap)

    -   [template\<typename Object, typename Coordinate = int\> class CRangeMultiMap](#template<typename-object-typename-coordinate-=-int>-class-crangemultimap)

    -   [class CIntervalTree](#class-cintervaltree)

-   [Thread Pools](#thread-pools)

    -   [class CThreadPool](#class-cthreadpool)

    -   [class CThreadPool\_Task](#class-cthreadpooltask)

    -   [class CThreadPool\_Thread](#class-cthreadpoolthread)

    -   [class CThreadPool\_Controller](#class-cthreadpoolcontroller)

    -   [class CThreadPool\_Controller\_PID](#class-cthreadpoolcontrollerpid)

-   [Miscellaneous Classes](#miscellaneous-classes)

    -   [class CTempString](#class-ctempstring)

    -   [class CChecksum](#class-cchecksum)

-   [Input/Output Utility Classes](#inputoutput-utility-classes)

    -   [class CIStreamBuffer](#class-cistreambuffer)

    -   [class COStreamBuffer](#class-costreambuffer)

    -   [class CByteSource](#class-cbytesource)

    -   [class CStreamByteSource](#class-cstreambytesource)

    -   [class CFStreamByteSource](#class-cfstreambytesource)

    -   [class CFileByteSource](#class-cfilebytesource)

    -   [class CMemoryByteSource](#class-cmemorybytesource)

    -   [class CByteSourceReader](#class-cbytesourcereader)

    -   [class CSubSourceCollector](#class-csubsourcecollector)

-   [Using the C++ Toolkit from a Third Party Application Framework](#using-the-c++-toolkit-from-a-third-party-application-framework)

**Demo Cases** [[src/sample/app/basic](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/basic)]

Writing a Simple Application
----------------------------

This section discusses how to write a simple application using the [CNcbiApplication](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiApplication.html) and related class. A [conceptual understanding of the uses of the CNcbiApplication and related classes](ch_intro.html#ch_intro.intro_appframe) is presented in the introductory chapter on the C++ Toolkit.

This section discusses the following topics:

-   [Basic Classes of the NCBI C++ Toolkit](#basic-classes-of-the-ncbi-c++-toolkit)

-   [Creating a Simple Application](#creating-a-simple-application)

-   [Inside the NCBI Application Class](#inside-the-ncbi-application-class)

<span class="nctnt highlight">Note:</span> The C++ Toolkit can also be [used from a third party application framework](#used-from-a-third-party-application-framework).

### NCBI C++ Toolkit Application Framework Classes

The following five fundamental classes form the foundation of the C++ Toolkit Application Framework:

-   [CNcbiApplication](#cncbiapplication)

-   [CNcbiArguments](#cncbiarguments) (see also [CArgDescriptions, CArgs, ...](#cargdescriptions-cargs-))

-   [CNcbiEnvironment](#cncbienvironment)

-   [CNcbiRegistry](#cncbiregistry)

-   [CNcbiDiag](#cncbidiag)

Each of these classes is discussed in the following sections:

#### CNcbiApplication

[CNcbiApplication](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiApplication.html) is an abstract class used to define the basic functionality and behavior of an NCBI application. Because this application class effectively supersedes the C-style <span class="nctnt ncbi-func">main()</span> function, minimally, it must provide the same functionality, i.e.:

-   a mechanism to execute the actual application

-   a data structure for holding program command-line arguments (<span class="nctnt ncbi-var">"argv"</span>)

-   a data structure for holding environment variables

In addition, the application class provides the same features previously implemented in the C Toolkit, namely:

-   mechanisms for specifying where, when, and how errors should be reported

-   methods for reading, accessing, modifying, and writing information in the application's registry (configuration) file

-   methods to describe, and then automatically parse, validate, and access program command-line arguments and to generate the <span class="nctnt ncbi-monospace">USAGE</span> message

The mechanism to execute the application is provided by <span class="nctnt ncbi-class">CNcbiApplication</span>'s member function <span class="nctnt ncbi-func">Run()</span>, for which you must write your own implementation. The <span class="nctnt ncbi-func">Run()</span> function will be automatically invoked by <span class="nctnt ncbi-func">CNcbiApplication::AppMain()</span>, after it has initialized its <span class="nctnt ncbi-class">CNcbiArguments, CNcbiEnvironment, CNcbiRegistry</span>, and <span class="nctnt ncbi-class">CNcbiDiag</span> data members.

#### CNcbiArguments

The [CNcbiArguments](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiArguments.html) class provides a data structure for holding the application's command-line arguments, along with methods for accessing and modifying these. Access to the argument values is implemented using the built-in <span class="nctnt ncbi-code">[ ]</span> operator. For example, the first argument in <span class="nctnt ncbi-var">argv</span> (following the program name) can be retrieved using the <span class="nctnt ncbi-func">CNcbiApplication::GetArguments()</span> method:

    string arg1_value = GetArguments()[1];

Here, <span class="nctnt ncbi-func">GetArguments()</span> returns the <span class="nctnt ncbi-class">CNcbiArguments</span> object, whose argument values can then be retrieved using the <span class="nctnt ncbi-code">[ ]</span> operator. Four additional <span class="nctnt ncbi-class">CNcbiArguments</span> member functions support retrieval and modification of the program name (initially <span class="nctnt ncbi-var">argv[0]</span>). A helper class, described in [Processing Command-Line Arguments](#processing-command-line-arguments), supports the generation of <span class="nctnt ncbi-monospace">USAGE</span> messages and the imposition of constraints on the values of the input arguments.

In addition to the <span class="nctnt ncbi-class">CNcbiArguments</span> class, there are other related classes used for argument processing. The <span class="nctnt ncbi-class">CArgDescriptions</span> and <span class="nctnt ncbi-class">CArgDesc</span> classes are used for describing unparsed arguments; <span class="nctnt ncbi-class">CArgs</span> and <span class="nctnt ncbi-class">CArgValue</span> for parsed argument values; <span class="nctnt ncbi-class">CArgException</span> and <span class="nctnt ncbi-class">CArgHelpException</span> for argument exceptions; and <span class="nctnt ncbi-class">CArgAllow</span>, <span class="nctnt ncbi-class">CArgAllow\_{Strings, ..., Integers, Doubles}</span> for argument constraints. These classes are discussed in the section on [Processing Command-Line Arguments](#processing-command-line-arguments).

When using the C++ Toolkit on the Mac OS, you can specify command-line arguments in a separate file with the name of your executable and ".args" extension. Each argument should be on a separate line (see [Table 1](#table-1)).

Table 1. Example of Command-line Arguments

| Command-Line Parameters                                                                                                                                                                                                      | File Content                                |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|
| -gi "Integer" (GI id of the Seq-Entry to examine) OPTIONAL ARGUMENTS: -h (Print this USAGE message; ignore other arguments) -reconstruct (Reconstruct title) -accession (Prepend accession) -organism (Append organism name) | -gi 10200 -reconstruct -accession -organism |

Please note:

File must contain Macintosh-style line breaks.

No extra spaces are allowed after argument ("-accession" and not "-accession ").

Arguments must be followed by an empty terminating line.

#### CNcbiEnvironment

The [CNcbiEnvironment](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiEnvironment.html) class provides a data structure for storing, accessing, and modifying the environment variables accessed by the C library routine <span class="nctnt ncbi-func">getenv()</span>.

The following describes the public interface to the <span class="nctnt ncbi-class">CNcbiEnvironment</span>:

    class  CNcbiEnvironment
    {
    public:
        /// Constructor.
        CNcbiEnvironment(void);
        /// Constructor with the envp parameter.
        CNcbiEnvironment(const char* const* envp);
        /// Destructor.
        virtual ~CNcbiEnvironment(void);
        /// Reset environment.
        ///
        /// Delete all cached entries, load new ones from "envp" (if not NULL).
        void Reset(const char* const* envp = 0);
        /// Get environment value by name.
        ///
        /// If environmnent value is not cached then call "Load(name)" to load
        /// the environmnent value.  The loaded name/value pair will then be
        /// cached, too, after the call to "Get()".
        const string& Get(const string& name) const;
    };

For example, to retrieve the value of environment variable <span class="nctnt ncbi-var">PATH</span>:

    string arg1_value = GetEnvironment().Get("PATH");

In this example, the <span class="nctnt ncbi-func">GetEnvironment()</span> is defined in the <span class="nctnt ncbi-class">CNcbiApplication</span> class and returns the <span class="nctnt ncbi-class">CNcbiEnvironment</span> object for which the <span class="nctnt ncbi-func">Get()</span> method is called with the environment variable <span class="nctnt ncbi-var">PATH</span>.

To delete all of the cached entries and reload new ones from the environment pointer (envp), use the <span class="nctnt ncbi-func">CNcbiEnvironment::Reset()</span> method.

#### CNcbiRegistry

Complete details for the <span class="nctnt ncbi-class">CNcbiRegistry</span> can be found in the section on [The CNcbiRegistry Class](#the-cncbiregistry-class).

#### CNcbiDiag

The [CNcbiDiag](#cncbidiag) class implements much of the functionality of the NCBI C++ Toolkit error-processing mechanisms; however, it is not intended to be used directly. Instead, use the [`{ERR\|LOG}_POST*`](#{err|log}post*) and [`_TRACE`](#trace) macros. See the sections on [Diagnostic Streams](#diagnostic-streams) and [Message Posting](ch_debug.html#ch_debug.std_cpp_message_post) for related information.

### Creating a Simple Application

This section discusses the following topics:

-   [Unix-like Systems](#unix-like-systems)

-   [MS Windows](#ms-windows)

-   [Discussion of the Sample Application](#discussion-of-the-sample-application)

#### Unix-like Systems

Using the [new\_project](ch_proj.html#ch_proj.outside_tree) shell script, create a new project <span class="nctnt ncbi-path">example</span>:

    new_project example app

This will create:

1  
the project folder -- <span class="nctnt ncbi-path">example</span>

2  
the source file -- <span class="nctnt ncbi-path">example.cpp</span>

3  
the makefiles -- <span class="nctnt ncbi-path">Makefile</span>, <span class="nctnt ncbi-path">Makefile.builddir</span>, <span class="nctnt ncbi-path">Makefile.in</span>, <span class="nctnt ncbi-path">Makefile.example.app</span>, <span class="nctnt ncbi-path">Makefile.example\_app</span>, <span class="nctnt ncbi-path">Makefile.out</span>

Then build the project and run the application:

    cd example; make; ./example

#### MS Windows

Using the [new\_project](ch_proj.html#ch_proj.outside_tree) shell script, create a new project <span class="nctnt ncbi-path">example</span>:

    new_project example app

This will create:

1  
the project folder -- <span class="nctnt ncbi-path">example</span>

2  
the source file -- <span class="nctnt ncbi-path">example\\src\\example\\basic\_sample.cpp</span> (the source file name is always <span class="nctnt ncbi-path">basic\_sample.cpp</span>, regardless of the project name)

3  
the MSVC project file -- <span class="nctnt ncbi-path">example\\compilers\\msvc1000\_prj\\static\\build\\example\\example.exe.vcproj</span>

4  
the MSVC solution file -- <span class="nctnt ncbi-path">example\\compilers\\msvc1000\_prj\\static\\build\\example.sln</span>

5  
a project makefile -- <span class="nctnt ncbi-path">example\\src\\example\\Makefile.example.app</span>

6  
other folders and files needed for building under Windows

<span class="nctnt highlight">Note:</span> If you prefer to have your source file name match your project name, you can achieve that by making the following edits before opening Visual Studio (for basic application projects, that is - other project types might require more edits):

1  
Rename the source file from <span class="nctnt ncbi-path">example\\src\\example\\basic\_sample.cpp</span> to <span class="nctnt ncbi-path">example.cpp</span>.

2  
Edit the MSVC project file <span class="nctnt ncbi-path">example\\compilers\\msvc1000\_prj\\static\\build\\example\\example.exe.vcproj</span> and replace "basic\_sample" with "example".

3  
Edit the project makefile <span class="nctnt ncbi-path">example\\src\\example\\Makefile.example.app</span> and replace "basic\_sample" with "example".

Then open the solution file <span class="nctnt ncbi-path">example\\compilers\\msvc1000\_prj\\static\\build\\example.sln</span> with MSVS and:

1  
Build the <span class="nctnt ncbi-var">-CONFIGURE-</span> project (reloading the project when prompted).

2  
Build the project and run the application.

#### Discussion of the Sample Application

In the [sample application](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/basic/basic_sample.cpp) above:

1. There is an application class derived from <span class="nctnt ncbi-class">CNcbiApplication</span>, which overrides the purely virtual function <span class="nctnt ncbi-func">Run()</span> as well as the initialization (<span class="nctnt ncbi-func">Init()</span>) and cleanup (<span class="nctnt ncbi-func">Exit()</span>) functions:

    class CSampleBasicApplication : public CNcbiApplication
    {
    private:
        virtual void Init(void);
        virtual int  Run(void);
        virtual void Exit(void);
    };

2. The program's main function creates an object of the application class and calls its <span class="nctnt ncbi-func">AppMain()</span> function:

    int main(int argc, const char* argv[])
    {
        // Execute main application function
        return CSampleBasicApplication().AppMain(argc, argv);
    }

3. The application's initialization function creates an [argument descriptions object](#argument-descriptions-object), which describes the expected command-line arguments and the usage context:

    void CSampleBasicApplication::Init(void)
    {
        // Create command-line argument descriptions
        auto_ptr<CArgDescriptions> arg_desc(new CArgDescriptions);

        // Specify USAGE context
        arg_desc->SetUsageContext(GetArguments().GetProgramBasename(),
                                  "CArgDescriptions demo program");
        ...
        // Setup arg.descriptions for this application
        SetupArgDescriptions(arg_desc.release());
    }

4. The application's <span class="nctnt ncbi-func">Run()</span> function prints those arguments into the standard output stream or in a file.

More [realistic examples of applications](ch_demo.html) that use the NCBI C++ Toolkit are available.

### Inside the NCBI Application Class

Here is a somewhat simplified view of the application's class definition:

    class CNcbiApplication
    {
    public:
        /// Main function (entry point) for the NCBI application.
        ///
        /// You can specify where to write the diagnostics
        ///  to (EAppDiagStream), and where to get
        /// the configuration file (LoadConfig()) to load
        /// to the application registry (accessible via GetConfig()).
        ///
        /// Throw exception if:
        ///  - not-only instance
        ///  - cannot load explicitly specified config.file
        ///  - SetupDiag() throws an exception
        ///
        /// If the application name is not specified, a default of "ncbi" is used.
        /// Certain flags such as -logfile, -conffile, and -version are
        /// special, so AppMain() processes them separately.
        /// @return
        ///   Exit code from Run(). Can also return a non-zero value if
        ///   the application threw an exception.
        /// @sa
        ///   Init(), Run(), Exit()
        int AppMain(int argc, const char **argv, const char **envp,
                   EAppDiagStream diag, const char* config, const string& name);

        /// Initialize the application.
        ///
        /// The default behavior of this is "do nothing". If you have
        /// special initialization logic that needs to be performed,
        ///  then you must override this method with your own logic.
        virtual void Init(void);

        /// Run the application.
        ///
        /// It is defined as a pure virtual method -- so you must(!)
        ///  supply theRun() method to implement the
        /// application-specific logic.
        /// @return
        ///   Exit code.
        virtual int  Run(void) = 0;

        /// Cleanup on application exit.
        ///
        /// Perform cleanup before exiting. The default behavior of this
        /// is "do nothing". If you have special cleanup logic that needs
        /// to be performed, then you must override this method with
        /// your own logic.
        virtual void Exit(void);

        /// Get the application's cached unprocessed command-line
        /// arguments.
        const CNcbiArguments& GetArguments(void) const;

        /// Get parsed command-line arguments.
        ///
        /// Get command-line arguments parsed according to the arg
        /// descriptions set by SetArgDescriptions(). Throw exception
        /// if no descriptions have been set.
        /// @return
        ///   The CArgs object containing parsed cmd.-line arguments.
        /// @sa
        ///   SetArgDescriptions().
        const CArgs& GetArgs(void) const;

        /// Get the application's cached environment.
        const CNcbiEnvironment& GetEnvironment(void) const;

        /// Get the application's cached configuration parameters.
        const CNcbiRegistry& GetConfig(void) const;

        /// Flush the in-memory diagnostic stream (for "eDS_ToMemory"
        /// case only).
        ///
        /// In case of "eDS_ToMemory", the diagnostics is stored in
        /// the internal application memory buffer ("m_DiagStream").
        /// Call this function to dump all the diagnostics to stream "os" and
        /// purge the buffer.
        /// @param  os
        ///   Output stream to dump diagnostics to. If it is NULL, then
        ///   nothing will be written to it (but the buffer will still be
        ///   purged).
        /// @param  close_diag
        ///   If "close_diag" is TRUE, then also destroy "m_DiagStream".
        /// @return
        ///   Total number of bytes actually written to "os".
        SIZE_TYPE FlushDiag(CNcbiOstream* os, bool close_diag = false);

        /// Get the application's "display" name.
        ///
        /// Get name of this application, suitable for displaying
        /// or for using as the base name for other files.
        /// Will be the 'name' argument of AppMain if given.
        /// Otherwise will be taken from the actual name of the
        /// application file or argv[0].
        string GetProgramDisplayName(void) const;

    protected:
        /// Setup application specific diagnostic stream.
        ///
        /// Called from SetupDiag when it is passed the eDS_AppSpecific
        /// parameter. Currently, this calls SetupDiag(eDS_ToStderr) to setup
        /// diagonistic stream to the std error channel.
        /// @return
        ///   TRUE if successful, FALSE otherwise.
       virtual bool SetupDiag_AppSpecific(void);

        /// Load configuration settings from the configuration file to
        /// the registry.
        ///
        /// Load (add) registry settings from the configuration file
        /// specified as the "conf" arg passed to AppMain(). The
        /// "conf" argument has the following special meanings:
        ///  - NULL      -- don't even try to load the registry from any
        ///                 file at all;
        ///  - non-empty -- if "conf" contains a path, then try to load
        ///                 from theconf.file of name "conf" (only!). Else -
        ///                 see NOTE.
        ///                 TIP: if the path is not fully qualified then:
        ///                      if it starts from "../" or "./" -- look
        ///                      starting from the current working dir.
        ///  - empty     -- compose conf.file name from the application
        ///                 name plus ".ini". If it does not match an existing
        ///                 file, then try to strip file extensions, e.g., for
        ///                 "my_app.cgi.exe" -- try subsequently:
        ///                 "my_app.cgi.exe.ini", "my_app.cgi.ini",
        ///                 "my_app.ini".
        ///
        /// NOTE:
        /// If "conf" arg is empty or non-empty, but without path, then
        /// config file will be sought for in the following order:
        ///  - in the current work directory;
        ///  - in the dir defined by environment variable "NCBI";
        ///  - in the user home directory;
        ///  - in the program dir.
        ///
        /// Throw an exception if "conf" is non-empty, and cannot open
        /// file.
        /// Throw an exception if file exists, but contains invalid entries.
        /// @param reg
        ///   The loaded registry is returned via the reg parameter.
        /// @param conf
        ///   The configuration file to loaded the registry entries from.
        /// @return
        ///   TRUE only if the file was non-NULL, found and successfully
        ///   read.
       virtual bool LoadConfig(CNcbiRegistry& reg, const string* conf);
       .............
    };

The <span class="nctnt ncbi-func">AppMain() </span>function is also inherited from the parent class. Although this function accepts up to six arguments, this example passes only the first two, with missing values supplied by defaults. The remaining four arguments specify:

-   (\#3) a NULL-terminated array of '\\0'-terminated character strings from which the environment variables can be read

-   (\#4) how to setup a diagnostic stream for message posting

-   (\#5) the name of a <span class="nctnt ncbi-path">.ini</span> configuration file (see [above](#above) for its default location)

-   (\#6) a program name (to be used in lieu of <span class="nctnt ncbi-var">argv[0]</span>)

<span class="nctnt ncbi-func">AppMain() </span>begins by resetting the internal data members with the actual values provided by the arguments of <span class="nctnt ncbi-func">main()</span>. Once these internal data structures have been loaded, <span class="nctnt ncbi-func">AppMain() </span>calls the virtual functions <span class="nctnt ncbi-func">Init(), Run(), </span>and <span class="nctnt ncbi-func">Exit() </span>in succession to execute the application.

The <span class="nctnt ncbi-func">Init() </span>and <span class="nctnt ncbi-func">Exit()</span> virtual functions are provided as places for developers to add their own methods for specific applications. If your application does not require additional initialization/termination, these two functions can be left empty or simply not implemented. The <span class="nctnt ncbi-func">Run()</span> method carries out the main work of the application.

The <span class="nctnt ncbi-func">FlushDiag()</span> method is useful if the diagnostic stream has been set to <span class="nctnt ncbi-code">eDS\_toMemory</span>, which means that diagnostic messages are stored in an internal application memory buffer. You can then call <span class="nctnt ncbi-func">FlushDiag()</span> to output the stored messages on the specified output stream. The method will also return the number of bytes written to the output stream. If you specify <span class="nctnt ncbi-macro">NULL</span> for the output stream, the memory buffers containing the diagnostic messages will be purged but not deallocated, and nothing will be written to the output. If the <span class="nctnt ncbi-var">close\_diag</span> parameter to <span class="nctnt ncbi-func">FlushDiag()</span> is set to true, then the memory buffers will be deallocated (and purged, of course).

The <span class="nctnt ncbi-func">GetProgramDisplayName()</span> method simply returns the name of the running application, suitable for displaying in reports or for using as the base name for building other related file names.

The protected virtual function <span class="nctnt ncbi-func">SetupDiag\_AppSpecific()</span> can be redefined to set up error posting specific for your application. <span class="nctnt ncbi-func">SetupDiag\_AppSpecific()</span> will be called inside <span class="nctnt ncbi-func">AppMain()</span> by default if the error posting has not been set up already. Also, if you pass <span class="nctnt ncbi-code">diag = eDS\_AppSpecific</span> to <span class="nctnt ncbi-func">AppMain()</span>, then <span class="nctnt ncbi-func">SetupDiag\_AppSpecific()</span> will be called for sure, regardless of the error posting setup that was active before the <span class="nctnt ncbi-func">AppMain()</span> call.

The protected virtual function <span class="nctnt ncbi-func">LoadConfig()</span> reads the program's <span class="nctnt ncbi-path">.ini </span>configuration file to load the application's parameters into the registry. The default implementation of <span class="nctnt ncbi-func">LoadConfig()</span> expects to find a configuration file named <span class="nctnt ncbi-path">\<program\_name\>.ini</span> and, if the <span class="nctnt ncbi-var">DIAG\_POST\_LEVEL</span> environment variable is set to "Info", it will generate a diagnostics message if no such file is found.

The NCBI application (built by deriving from <span class="nctnt ncbi-class">CNcbiApplication</span>) throws the exception <span class="nctnt ncbi-class">CAppException</span> when any of the following conditions are true:

-   Command-line argument description cannot be found and argument descriptions have not been disabled (via call to protected method <span class="nctnt ncbi-func">DisableArgDescription()</span>.

-   Application diagnostic stream setup has failed.

-   Registry data failed to load from a specified configuration file.

-   An attempt is made to create a second instance of the <span class="nctnt ncbi-class">CNcbiApplication</span> (at any time, only one instance can be running).

-   The specified configuration file cannot be opened.

As shown above, source files that utilize the <span class="nctnt ncbi-class">CNcbiApplication</span> class must <span class="nctnt ncbi-code">\#include</span> the header file where that class is defined, <span class="nctnt ncbi-path">corelib/ncbiapp.hpp</span>, in the <span class="nctnt ncbi-path">include/</span> directory. This header file in turn includes <span class="nctnt ncbi-path">corelib/ncbistd.hpp</span>, which should **always** be <span class="nctnt ncbi-code">\#include</span>'d.

Processing Command-Line Arguments
---------------------------------

This section discusses the classes that are used to process command-line arguments. A conceptual overview of these classes is covered in an [introductory section](ch_intro.html#ch_intro.intro_args). This section discusses these classes in detail and gives sample programs that use these classes.

This section discusses the following topics:

-   [Capabilities of the Command-Line API](#capabilities-of-the-command-line-api)

-   [The Relationships between the CArgDescriptions, CArgs, and CArgValue Classes](#the-relationships-between-the-cargdescriptions-cargs-and-cargvalue-classes)

-   [Command-Line Syntax](#command-line-syntax)

-   [The CArgDescriptions Class](#the-cargdescriptions-class)

-   [The CArgs Class: A Container Class for CArgValue Objects](#the-cargs-class-a-container-class-for-cargvalue-objects)

-   [CArgValue Class: The Internal Representation of Argument Values](#cargvalue-class-the-internal-representation-of-argument-values)

-   [Supporting Command-Based Command Lines](#supporting-command-based-command-lines)

-   [Code Examples](#code-examples)

### Capabilities of the Command-Line API

The set of classes for argument processing implement automated command line parsing. Specifically, these classes allow the developer to:

-   Specify attributes of expected arguments, such as name, synopsis, comment, data type, etc.

-   validate values of the arguments passed to the program against these specifications

-   validate the number of positional arguments in the command line

-   generate a [USAGE](#usage) message based on the argument descriptions

<span class="nctnt highlight">NOTE:</span><span class="nctnt ncbi-cmd"> -h</span> flag to print the [USAGE](#usage) is defined by default.

-   access the input argument values specifically typecast according to their descriptions

Normally, a <span class="nctnt ncbi-class">CArgDescriptions</span> object that contains the argument description is required and [should be created](#should-be-created) in the application's <span class="nctnt ncbi-func">Init()</span> function before any other initialization. Otherwise, <span class="nctnt ncbi-class">CNcbiApplication</span> creates a default one, which allows any program that uses the NCBI C++ Toolkit to provide some <span class="nctnt ncbi-monospace">standard</span> command -line options, namely:

-   to obtain a general description of the program as well as description of all available command-line parameters (<span class="nctnt ncbi-cmd">-h</span> flag)

-   to redirect the program's [diagnostic messages](#diagnostic-messages) into a specified file (<span class="nctnt ncbi-cmd">-logfile</span> key)

-   to read the program's [configuration data](#configuration-data) from a specified file (<span class="nctnt ncbi-cmd">-conffile</span> key)

See [Table 3](#table-3) for the standard command-line options for the default instance of <span class="nctnt ncbi-class">CArgDescriptions</span>.

Table 3. Standard command-line options for the default instance of CArgDescriptions

| Flag      | Description                                                     | Example                      |
|-----------|-----------------------------------------------------------------|------------------------------|
| -h        | Print description of the application's command-line parameters. | theapp -h                    |
| -logfile  | Redirect program's log into the specified file.                 | theapp -logfile theapp\_log  |
| -conffile | Read the program's configuration data from the specified file.  | theapp -conffile theapp\_cfg |

To avoid creation of a default <span class="nctnt ncbi-class">CArgDescriptions</span> object that may not be needed, for instance if the standard flags described in [Table 3](#table-3) are not used, one should call the <span class="nctnt ncbi-func">CNcbiApplication::DisableArgDescriptions()</span> function from an application object constructor.

It is also possible to use the <span class="nctnt ncbi-func">CNcbiApplication::HideStdArgs(THideStdArgs hide\_mask)</span> method to hide description of the standard arguments (<span class="nctnt ncbi-cmd">-h, -logfile, -conffile</span>) in the [USAGE](#usage) message. Please note: This only hides the description of these flags; it is still possible to use them.

### The Relationships between the <span class="nctnt ncbi-class">CArgDescriptions</span>, <span class="nctnt ncbi-class">CArgs</span>, and <span class="nctnt ncbi-class">CArgValue</span> Classes

The [CArgDescriptions](#cargdescriptions) class provides an interface to describe the data type and attributes of command-line arguments via a set of <span class="nctnt ncbi-class">AddXxx()</span> methods. Additional constraints on the argument values can be imposed using the <span class="nctnt ncbi-class">SetConstraint()</span> method. The <span class="nctnt ncbi-class">CreateArgs() </span>method is passed the values of all command-line arguments at runtime. This method verifies their overall syntactic structure and matches their values against the stored descriptions. If the arguments are parsed successfully, a new [CArgs](#cargs) object is returned by <span class="nctnt ncbi-class">CreateArgs()</span>.

The resulting [CArgs](#cargs) object will contain parsed, verified, and ready-to-use argument values, which are stored as [CArgValue](#cargvalue). The value of a particular argument can be accessed using the argument's name (as specified in the [CArgDescriptions](#cargdescriptions) object), and the returned [CArgValue](#cargvalue) object can then be safely type-cast to a correct C++ type (<span class="nctnt ncbi-type">int</span>, <span class="nctnt ncbi-type">string</span>, <span class="nctnt ncbi-type">stream</span>, etc.) because the argument types have been verified. These class relations and methods can be summarized schematically as shown in [Figure 1](#figure-1).

[![Figure 1. Argument processing class relations.](/book-test/static/img/flow.gif)](/book-test/static/img/flow.gif "Click to see the full-resolution image")

Figure 1. Argument processing class relations.

The last statement in this example implicitly references a [CArgValue](#cargvalue) object, in the value returned when the <span class="nctnt ncbi-code">[ ] </span>operator is applied to <span class="nctnt ncbi-var">myArgs</span>. The method <span class="nctnt ncbi-func">CArgValue::AsDouble() </span>is then applied to this object to retrieve a <span class="nctnt ncbi-type">double</span>.

### Command-Line Syntax

<span class="nctnt highlight">Note:</span> The C++ Toolkit supports two types of command line: "command-based" and "command-less". A "command-based" command line begins with a "command" (a case-sensitive keyword), typically followed by other arguments. A "command-less" command line doesn't contain such "commands".

This section deals primarily with command-less command lines, while the [Supporting Command-Based Command Lines](#supporting-command-based-command-lines) section covers command-based command lines.

Command-less command-line arguments fit the following profile:

    progname {arg_key, arg_key_opt, arg_key_dflt, arg_flag} [--]
             {arg_pos} {arg_pos_opt, arg_pos_dflt}
             {arg_extra} {arg_extra_opt}

where:

|-----------------|-----------------------------------------------------------|
| arg\_key        | -\<key\> \<value\> -- (mandatory)                         |
| arg\_key\_opt   | [-\<key\> \<value\>] -- (optional, without default value) |
| arg\_key\_dflt  | [-\<key\> \<value\>] -- (optional, with default value)    |
| arg\_flag       | -\<flag\> -- (always optional)                            |
| --              | optional delimiter to indicate the beginning of pos. args |
| arg\_pos        | \<value\> -- (mandatory)                                  |
| arg\_pos\_opt   | [\<value\>] -- (optional, without default value)          |
| arg\_pos\_dflt  | [\<value\>] -- (optional, with default value)             |
| arg\_extra      | \<value\> -- (dep. on the constraint policy)              |
| arg\_extra\_opt | [\<value\>] -- (dep. on the constraint policy)            |

and: <span class="nctnt ncbi-cmd">\<key\></span> must be followed by <span class="nctnt ncbi-cmd">\<value\></span>. In all cases '<span class="nctnt ncbi-cmd">-\<key\> \<value\></span>' is equivalent to '<span class="nctnt ncbi-cmd">-\<key\>=\<value\></span>'. If '=' is used as separator, the value can be empty ('<span class="nctnt ncbi-cmd">-\<key\>=</span>'). For arguments with a single-char name <span class="nctnt ncbi-monospace">fOptionalSeparator</span> flag can be set. In this case the value can be specified without any separator: <span class="nctnt ncbi-cmd">-\<k\>\<value\></span>

<span class="nctnt highlight">NOTE:</span> No other argument's name can start with the same character to avoid conflicts. <span class="nctnt ncbi-cmd">\<flag\></span> and <span class="nctnt ncbi-cmd">\<key\></span> are case-sensitive, and they can contain only alphanumeric characters and dash ('-'). Only one leading dash is allowed. The leading dash can be used to create arguments which look like <span class="nctnt ncbi-cmd">--\<key\></span> in the command line. <span class="nctnt ncbi-cmd">\<value\></span> is an arbitrary string (additional constraints can be applied in the argument description, see "EType"). {arg\_pos\*\*\*} and {arg\_extra\*\*\*} are position-dependent arguments, with no tag preceding them. {arg\_pos\*\*\*} arguments have individual names and descriptions (see methods <span class="nctnt ncbi-func">AddPositional</span>\*\*\*). {arg\_extra\*\*\*} arguments have one description for all (see method <span class="nctnt ncbi-func">AddExtra</span>). User can apply constraints on the number of mandatory and optional {arg\_extra\*\*\*} arguments.

Examples of command-less command lines:

    MyProgram1 -reverse -depth 5 -name Lisa -log foo.log 1.c 2.c 3.c
    MyProgram2 -i foo.txt -o foo.html -color red
    MyProgram3 -a -quiet -pattern 'Error:' bar.txt
    MyProgram4 -int-value=5 -str-value= -kValue

The [Supporting Command-Based Command Lines](#supporting-command-based-command-lines) section addresses how to support command-based command lines, such as:

    svn diff myapp.cpp
    svn checkin -m "message" myapp.cpp

### The <span class="nctnt ncbi-class">CArgDescriptions</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)) class

<span class="nctnt ncbi-class">CArgDescriptions</span> contains a description of unparsed arguments, that is, user-specified descriptions that are then used to parse the arguments. <span class="nctnt ncbi-class">CArgDescriptions</span> is used as a container to store the command-line argument descriptions. The argument descriptions are used for parsing and verifying actual command-line arguments.

The following is a list of topics discussed in this section:

-   [The CArgDescriptions Constructor](#the-cargdescriptions-constructor)

-   [Describing Argument Attributes](#describing-argument-attributes)

-   [Argument Types](#argument-types)

-   [Restricting the Input Argument Values](#restricting-the-input-argument-values)

-   [Implementing User-defined Restrictions Using the CArgAllow Class](#implementing-user-defined-restrictions-using-the-cargallow-class)

-   [Using CArgDescriptions in Applications](#using-cargdescriptions-in-applications)

-   [Generating a USAGE Message](#generating-a-usage-message)

#### The CArgDescriptions Constructor

The constructor for <span class="nctnt ncbi-class">CArgDescriptions</span> accepts a Boolean argument, auto\_help, set to TRUE by default.

<span class="nctnt ncbi-code">CArgDescriptions(bool auto\_help = true);</span>

If "auto\_help" is passed TRUE, then a special flag "-h" will be added to the list of accepted arguments, and passing "-h" in the command line will print out USAGE and ignore all other passed arguments.

#### Describing Argument Attributes

<span class="nctnt ncbi-class">CNcbiArguments</span> contains many methods, called <span class="nctnt ncbi-func">AddXxx()</span>. The "Xxx" refers to the types of arguments, such as mandatory key (named) arguments, optional key arguments, positional arguments, flag arguments, etc. For example, the <span class="nctnt ncbi-func">AddKey()</span> method refers to adding a description for a mandatory key argument.

The methods for <span class="nctnt ncbi-func">AddXxx()</span> are passed the following argument attributes:

-   *name*, the string that will be used to identify the variable, as in: <span class="nctnt ncbi-var">CArgs[name]</span>. For all tagged variables in a command line, *name* is also the key (or flag) to be used there, as in: "<span class="nctnt ncbi-cmd">-name value</span>" (or "<span class="nctnt ncbi-cmd">-name"</span>).

-   *synopsis*, for <span class="nctnt ncbi-type">key\_\*\*\*</span> arguments only. The automatically generated [USAGE](#usage) message includes an argument description in the format: *-name [synopsis] \<type, constraint\>* comment.

-   *comment*, to be displayed in the [USAGE](#usage) message, as described above.

-   *value type*, one of the scalar values defined in the [EType](#etype) enumeration, which defines the type of the argument.

-   *default,* for <span class="nctnt ncbi-type">key\_dflt</span> and <span class="nctnt ncbi-type">pos\_dflt</span> arguments only. A default value to be used if the argument is not included in the command line (only available for optional program arguments).

-   *flags*, the <span class="nctnt ncbi-monospace">flags</span> argument, to provide additional control of the arguments' behavior.

#### Argument Types

The [CArgDescriptions](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html) class enables registration of command-line arguments that fit one of the following pattern types:

**Mandatory named arguments:**<span class="nctnt ncbi-cmd">-\<key\> \<value\></span> (example: <span class="nctnt ncbi-cmd">-age 31</span>) Position-independent arguments that **must** be present in the command line. [AddKey (key, synopsis, comment, value\_type, flags)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)

**Optional named arguments:**<span class="nctnt ncbi-cmd">[-\<key\> \<value\>]</span> (example: <span class="nctnt ncbi-cmd">-name Lisa</span>) Position-independent arguments that are **optional**. [AddOptionalKey (key, synopsis, comment, value\_type, flags)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html) A default value can be specified in the argument's description to cover those cases where the argument does not occur in the command line[. AddDefaultKey (key, synopsis, comment, value\_type, default\_value, flags)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)

**Optional named flags:**<span class="nctnt ncbi-cmd">[-\<flag\>]</span> (example: <span class="nctnt ncbi-type">-reverse</span>) Position-independent boolean (without value) arguments. These arguments are **always** optional. [AddFlag (flag, comment, set\_value)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)

**Mandatory named positional arguments:**<span class="nctnt ncbi-cmd">\<value\></span> (example: <span class="nctnt ncbi-cmd">12 Feb</span>) These are position-<span class="nctnt ncbi-monospace">dependent</span> arguments (of any type), which are read using a <span class="nctnt ncbi-cmd">value</span> only. They do, however, have names stored with their descriptions, which they are associated with in an order-dependent fashion. Specifically, the order in which untagged argument descriptions are added to the <span class="nctnt ncbi-class">CArgDescriptions</span> object using <span class="nctnt ncbi-func">AddPositional()</span> defines the order in which these arguments should appear in the command line. [AddPositional (key, comment, value\_type, flags)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)

**Optional named positional arguments:**<span class="nctnt ncbi-cmd">[value]</span> (example: <span class="nctnt ncbi-cmd">foo.txt bar</span>) Position-<span class="nctnt ncbi-monospace">dependent</span> arguments that are <span class="nctnt ncbi-monospace">optional</span>. They always go after the <span class="nctnt ncbi-monospace">mandatory</span> positional arguments. The order in which untagged argument descriptions are added to the <span class="nctnt ncbi-class">CArgDescriptions</span> object using <span class="nctnt ncbi-func">Add[Optional\\|Default]Positional()</span> defines the order in which these arguments should appear in the command line. [AddOptionalPositional (key, comment, value\_type, flags)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)[AddDefaultPositional (key, comment, value\_type, default\_value, flags)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)

**Unnamed positional arguments** (all of the same type: <span class="nctnt ncbi-cmd">\<value1\> \\| [valueN]</span> (example: <span class="nctnt ncbi-cmd">foo.c bar.c xxx.c</span>). These are also position-<span class="nctnt ncbi-monospace">dependent</span> arguments that are read using a <span class="nctnt ncbi-cmd">value</span> only. They are expected to appear at the very end of the command line, after all named arguments. Unlike the previous argument type, however, these arguments do not have individual, named descriptions but share a single "unnamed" description. You can specify how many mandatory and how many optional arguments to expect using <span class="nctnt ncbi-var">n\_mandatory</span> and <span class="nctnt ncbi-var">n\_optional</span> parameters: [AddExtra (n\_mandatory, n\_optional, comment, type, flags)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html)

**Aliases** can be created for any arguments. They allow using an alternative argument name in the command line. However, only the original argument name can be used to access its value in the C++ code.

Any of the registered descriptions can be tested for existence and/or deleted using the following <span class="nctnt ncbi-class">CArgDescriptions</span> methods:

    bool Exist(const string& name) const;
    void Delete(const string& name);

These methods can also be applied to the unnamed positional arguments (as a <span class="nctnt ncbi-cmd">group</span>), using: <span class="nctnt ncbi-code">Exist(kEmptyStr)</span> and <span class="nctnt ncbi-code">Delete(kEmptyStr).</span>

#### Restricting the Input Argument Values

Although each argument's input value is initially loaded as a simple character string, the argument's specified type implies a restricted set of possible values. For example, if the type is <span class="nctnt ncbi-var">eInteger</span>, then any integer value is acceptable, but floating point and non-numerical values are not. The [EType](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html) enumeration quantifies the allowed types and is defined as:

    /// Available argument types.
    enum EType {
        eString = 0, ///< An arbitrary string
        eBoolean,    ///< {'true', 't', 'false', 'f'},  case-insensitive
        eInteger,    ///< Convertible into an integer number (int)
        eDouble,     ///< Convertible into a floating point number (double)
        eInputFile,  ///< Name of file (must exist and be readable)
        eOutputFile, ///< Name of file (must be writeable)
        k_EType_Size ///< For internal use only
    };

#### Implementing User-defined Restrictions Using the <span class="nctnt ncbi-class">CArgAllow</span> Class

It may be necessary to specify a restricted range for argument values. For example, an integer argument that has a range between 5 and 10. Further restrictions on the allowed values can be specified using the <span class="nctnt ncbi-func">CArgDescriptions::SetConstraint()</span> method with the [CArgAllow](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgAllow.html) class. For example:

    auto_ptr<CArgDescriptions> args(new CArgDescriptions);
    // add descriptions for "firstint" and "nextint" using AddXxx( ...)
    ...
    CArgAllow* constraint = new CArgAllow_Integers(5,10);
    args->SetConstraint("firstInt", constraint);
    args->SetConstraint("nextInt", constraint);

This specifies that the arguments named <span class="nctnt ncbi-cmd">"firstInt"</span> and <span class="nctnt ncbi-cmd">"nextInt"</span> must both be in the range [5, 10].

The [CArgAllow\_Integers](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgAllow__Integers.html) class is derived from the **abstract**<span class="nctnt ncbi-class">CArgAllow</span> class. The constructor takes the two integer arguments as lower and upper bounds for allowed values. Similarly, the [CArgAllow\_Doubles](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgAllow__Doubles.html) class can be used to specify a range of allowed floating point values. For both classes, the order of the numeric arguments does not matter, because the constructors will use min/max comparisons to generate a valid range.

A third class derived from the <span class="nctnt ncbi-class">CArgAllow</span> class is the [CArgAllow\_Strings](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgAllow__Strings.html) class. In this case, the set of allowed values cannot be specified by a <span class="nctnt ncbi-class">range</span>, but the following construct can be used to enumerate all eligible string values:

    CArgAllow* constraint = (new CArgAllow_Strings())->
                                 Allow("this)->Allow("that")->Allow("etc");
    args.SetConstraint("someString", constraint);

Here, the constructor takes no arguments, and the <span class="nctnt ncbi-func">Allow() </span>method returns <span class="nctnt ncbi-var">this</span>. Thus, a list of allowed strings can be specified by daisy-chaining a set of calls to <span class="nctnt ncbi-func">Allow()</span>. A bit unusual yet terser notation can also be used by engaging the comma operator, as in:

    args.SetConstraint("someString",
                       &(*new CArgAllow_Strings, "this", "that", "etc"));

There are two other pre-defined constraint classes: [CArgAllow\_Symbols](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgAllow__Symbols.html) and [CArgAllow\_String](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgAllow__String.html). If the value provided on the command line is not in the allowed set of values specified for that argument, then an exception will be generated. This exception can be caught and handled in the usual manner, as described in the discussion of [Generating a USAGE message.](#generating-a-usage-message)

#### Using <span class="nctnt ncbi-class">CArgDescriptions</span> in Applications

The description of program arguments should be provided in the application's <span class="nctnt ncbi-func">Init()</span> function before any other initialization. A good idea is also to specify the description of the program here:

    auto_ptr<CArgDescriptions> arg_desc(new CArgDescriptions);
    arg_desc->SetUsageContext(GetArguments().GetProgramBasename(),
                              "program's description here");
    // Define arguments, if any
    ...
    SetupArgDescriptions(arg_desc.release());

The <span class="nctnt ncbi-func">SetUsageContext()</span> method is used to define the name of the program and its description, which is to be displayed in the <span class="nctnt ncbi-monospace">USAGE</span> message. As long as the initialization of the application is completed and there is still no argument description, <span class="nctnt ncbi-class">CNcbiApplication</span> class provides a "default" one. This behavior can be overridden by calling the <span class="nctnt ncbi-func">DisableArgDescriptions()</span> method of <span class="nctnt ncbi-path">CNcbiAppliation</span>.

#### Generating a <span class="nctnt ncbi-monospace">USAGE </span>Message

One of the functions of the <span class="nctnt ncbi-class">CArgDescriptions</span> object is to generate a <span class="nctnt ncbi-monospace">USAGE</span> message automatically (this gives yet another reason to define one). Once such object is [defined](#defined), there is nothing else to worry about; <span class="nctnt ncbi-class">CNcbiApplication</span> will do the job for you. The <span class="nctnt ncbi-func">SetupArgDescriptions()</span> method includes parsing the command line and matching arguments against their descriptions. Should an error occur, e.g., a mandatory argument is missing, the program prints a message explaining what was wrong and terminates. The output in this case might look like this:

    USAGE
      myApp -h -k MandatoryKey [optarg]
    DESCRIPTION
       myApp test program
    REQUIRED ARGUMENTS
     -k <String>
       This is a mandatory alpha-num key argument
    OPTIONAL ARGUMENTS
     -h
       Print this USAGE message;  ignore other arguments
     optarg <File_Out>
       This is an optional named positional argument without default
       value

The message shows a description of the program and a summary of each argument. In this example, the description of the input file argument was defined as:

    arg_desc->AddKey( "k", "MandatoryKey",
                      "This is a mandatory alpha-num key argument",
                      CArgDescriptions::eString);

The information generated for each argument is displayed in the format:

> *me [synopsis] \<type [, constraint] \> comment [default = .....]*

The arguments in the USAGE message can be arranged into groups by using <span class="nctnt ncbi-func">SetCurrentGroup()</span> method of the <span class="nctnt ncbi-class">CArgDescriptions</span> object.

### The CArgs ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgs.html)) Class: A Container Class for CArgValue ([\*](#*)) Objects

The <span class="nctnt ncbi-class">CArgs</span> class provides a data structure where the values of the parsed arguments can be stored and includes access routines in its public interface. Argument values are obtained from the unprocessed command-line arguments via the <span class="nctnt ncbi-class">CNcbiArguments</span> class and then verified and processed according to the argument descriptions defined by the user in <span class="nctnt ncbi-class">CArgDescriptions</span>. The following describes the public interface methods in <span class="nctnt ncbi-class">CArgs</span>:

    class  CArgs
    {
    public:
        /// Constructor.
        CArgs(void);
        /// Destructor.
        ~CArgs(void);
        /// Check existence of argument description.
        ///
        /// Return TRUE if arg 'name' was described in the parent CArgDescriptions.
        bool Exist(const string& name) const;
        /// Get value of argument by name.
        ///
        /// Throw an exception if such argument does not exist.
        /// @sa
        ///   Exist() above.
        const CArgValue& operator[] (const string& name) const;
        /// Get the number of unnamed positional (a.k.a. extra) args.
        size_t GetNExtra(void) const { return m_nExtra; }
        /// Return N-th extra arg value,  N = 1 to GetNExtra().
        const CArgValue& operator[] (size_t idx) const;
        /// Print (append) all arguments to the string 'str' and return 'str'.
        string& Print(string& str) const;
        /// Add new argument name and value.
        ///
        /// Throw an exception if the 'name' is not an empty string, and if
        /// there is an argument with this name already.
        ///
        /// HINT: Use empty 'name' to add extra (unnamed) args, and they will be
        /// automatically assigned with the virtual names: '#1', '#2', '#3', etc.
        void Add(CArgValue* arg);
        /// Check if there are no arguments in this container.
        bool IsEmpty(void) const;
    };

The CArgs object is created by executing the <span class="nctnt ncbi-func">CArgDescriptions::CreateArgs()</span> method. What happens when the <span class="nctnt ncbi-func">CArgDescriptions::CreateArgs()</span> method is executed is that the arguments of the command line are validated against the registered descriptions, and a <span class="nctnt ncbi-class">CArgs</span> object is created. Each argument value is internally represented as a [CArgValue](#cargvalue) object and is added to a container managed by the <span class="nctnt ncbi-class">CArgs</span> object.

All <span class="nctnt ncbi-monospace">named</span> arguments can be accessed using the <span class="nctnt ncbi-code">[ ]</span> operator, as in: <span class="nctnt ncbi-code">myCArgs["f"]</span>, where <span class="nctnt ncbi-monospace">"f"</span> is the name registered for that argument. There are two ways to access the **N**-th <span class="nctnt ncbi-monospace">unnamed</span> positional argument: <span class="nctnt ncbi-code">myCArgs["\#N"]</span> and <span class="nctnt ncbi-code">myCArgs[N]</span>, where 1 \<= **N** \<= GetNExtra().

### <span class="nctnt ncbi-class">CArgValue</span> ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgValue.html)) Class: The Internal Representation of Argument Values

The internal representation of an argument value, as it is stored and retrieved from its [CArgs](#cargs) container, is an instance of a <span class="nctnt ncbi-class">CArgValue</span>. The primary purpose of this class is to provide type-validated loading through a set of <span class="nctnt ncbi-func">AsXxx()</span> methods where "<span class="nctnt ncbi-func">Xxx</span>" is the argument type such as "Integer", "Boolean", "Double", etc. The following describes the public interface methods in <span class="nctnt ncbi-class">CArgValue</span>:

    class  CArgValue : public CObject
    {
    public:
        /// Get argument name.
        const string& GetName(void) const { return m_Name; }
        /// Check if argument holds a value.
        ///
        /// Argument does not hold value if it was described as optional argument
        /// without default value, and if it was not passed a value in the command
        /// line.  On attempt to retrieve the value from such "no-value" argument,
        /// exception will be thrown.
        virtual bool HasValue(void) const = 0;
        operator bool (void) const { return  HasValue(); }
        bool operator!(void) const { return !HasValue(); }
        /// Get the argument's string value.
        ///
        /// If it is a value of a flag argument, then return either "true"
        /// or "false".
        /// @sa
        ///   AsInteger(), AsDouble(), AsBoolean()
        virtual const string& AsString(void) const = 0;
        /// Get the argument's integer value.
        ///
        /// If you request a wrong value type, such as a call to "AsInteger()"
        /// for a "boolean" argument, an exception is thrown.
        /// @sa
        ///   AsString(), AsDouble, AsBoolean()
        virtual int    AsInteger(void) const = 0;
        /// Get the argument's double value.
        ///
        /// If you request a wrong value type, such as a call to "AsDouble()"
        /// for a "boolean" argument, an exception is thrown.
        /// @sa
        ///   AsString(), AsInteger, AsBoolean()
        virtual double AsDouble (void) const = 0;
        /// Get the argument's boolean value.
        ///
        /// If you request a wrong value type, such as a call to "AsBoolean()"
        /// for a "integer" argument, an exception is thrown.
        /// @sa
        ///   AsString(), AsInteger, AsDouble()
        virtual bool   AsBoolean(void) const = 0;
        /// Get the argument as an input file stream.
        virtual CNcbiIstream& AsInputFile (void) const = 0;
        /// Get the argument as an output file stream.
        virtual CNcbiOstream& AsOutputFile(void) const = 0;
        /// Close the file.
        virtual void CloseFile (void) const = 0;
    };

Each of these <span class="nctnt ncbi-func">AsXxx()</span> methods will access the string storing the value of the requested argument and attempt to convert that string to the specified type, using for example, functions such as <span class="nctnt ncbi-func">atoi() </span>or <span class="nctnt ncbi-func">atof()</span>. Thus, the following construct can be used to obtain the value of a floating point argument named "f":

    float f = args["f"].AsDouble();

An exception will be generated with an appropriate error message, if:

-   the conversion fails, or

-   "f" was described as an optional key or positional argument without default value (i.e., using the <span class="nctnt ncbi-func">AddOptional\*\*\*()</span> method), and it was not defined in the command line. Note that you can check for this case using the <span class="nctnt ncbi-func">CArgValue::HasValue()</span> method.

### Supporting Command-Based Command Lines

For some applications, multiple command-based command line forms are needed, with different arguments depending on the command. For example:

    myapp list
    myapp create <queue>
    myapp post   <queue> [-imp importance] <message>
    myapp query  [queue]

Commands are case-sensitive keywords and are typically followed by other arguments. Programs that support command-based command lines can support any number of commands (each with its own set of supported arguments), and may optionally support a command-less command line in addition.

Command-based command lines have a requirement that command-less command lines don't - the ability to have optional arguments between mandatory arguments. Opening arguments address this requirement. Opening arguments are essentially identical to mandatory positional arguments except that opening arguments must precede optional arguments whereas mandatory positional arguments must follow them. Thus, opening arguments allow usage forms such as the "post" command in the above example, which has an optional argument between mandatory arguments.

At a high level, setting up a program to support a command-less command-line requires creating a <span class="nctnt ncbi-class">CArgDescriptions</span> object, adding argument descriptions to it, and passing it to <span class="nctnt ncbi-func">SetupArgDescriptions()</span>.

Setting up a program to support command-based command lines is similar, but requires a <span class="nctnt ncbi-class">CCommandArgDescriptions</span> object instead. The [CCommandArgDescriptions](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCommandArgDescriptions.html) class is derived from [CArgDescriptions](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCArgDescriptions.html), so all the same functionality is available; however, the <span class="nctnt ncbi-func">AddCommand()</span> method of <span class="nctnt ncbi-class">CCommandArgDescriptions</span> allows you to create multiple <span class="nctnt ncbi-class">CArgDescriptions</span> objects (one for each command) in addition to the overall program description. Other command-specific features are also provided, such as command grouping. <span class="nctnt highlight">Note:</span> The <span class="nctnt ncbi-type">ECommandPresence</span> parameter of the <span class="nctnt ncbi-class">CCommandArgDescriptions</span> constructor controls whether or not the user must enter a command-based command line. Use <span class="nctnt ncbi-var">eCommandOptional</span> only when you are setting up both command-less and command-based command lines.

Programs that support command-based command lines must execute these steps:

1  
Create a command descriptions object (class <span class="nctnt ncbi-class">CCommandArgDescriptions</span>) for the overall program description.

2  
Create argument descriptions objects (class <span class="nctnt ncbi-class">CArgDescriptions</span>) for each command.

3  
Add the actual argument descriptions to the argument descriptions objects using methods such as <span class="nctnt ncbi-func">AddOpening()</span>, <span class="nctnt ncbi-func">AddPositional()</span>, etc.

4  
Add each argument descriptions object to the overall command descriptions object.

5  
Determine which command was specified on the command line.

6  
Process the appropriate arguments for the given command.

For a sample program that demonstrates argument processing for command-based command lines, see [multi\_command.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/basic/multi_command.cpp).

For more information on standard command lines and general information applicable to all command line processing, see the [Command-Line Syntax](#command-line-syntax) and [CArgDescriptions](#cargdescriptions) sections.

### Code Examples

A simple application program, [test\_ncbiargs\_sample.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test/test_ncbiargs_sample.cpp) demonstrates the usage of these classes for argument processing. See also [test\_ncbiargs.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test/test_ncbiargs.cpp) (especially <span class="nctnt ncbi-func">main()</span>, <span class="nctnt ncbi-func">s\_InitTest0()</span> and <span class="nctnt ncbi-func">s\_RunTest0()</span> there), and [asn2asn.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/app/asn2asn/asn2asn.cpp) for more examples.

Namespace, Name Concatenation, and Compiler-specific Macros
-----------------------------------------------------------

The file <span class="nctnt ncbi-path">ncbistl.hpp</span> provides a number of macros on namespace usage, name concatenation, and macros for handling compiler-specific behavior.

These topics are discussed in greater detail in the following subsections:

-   [NCBI Namespace](#ncbi-namespace)

-   [Other Name Space Macros](#other-name-space-macros)

-   [Name Concatenation](#name-concatenation)

-   [Compiler Specific Macros](#compiler-specific-macros)

### NCBI Namespace

All new NCBI classes must be in the <span class="nctnt ncbi-var">ncbi::</span> namespace to avoid naming conflicts with other libraries or code. Rather than enclose all newly defined code in the following, it is, from a stylistic point of view, better to use specially defined macros such as <span class="nctnt ncbi-macro">BEGIN\_NCBI\_SCOPE</span>, <span class="nctnt ncbi-macro">END\_NCBI\_SCOPE</span>, <span class="nctnt ncbi-macro">USING\_NCBI\_SCOPE</span>:

    namespace ncbi {
        // Indented code etc.
    }

The use of <span class="nctnt ncbi-macro">BEGIN\_NCBI\_SCOPE</span>, <span class="nctnt ncbi-macro">END\_NCBI\_SCOPE</span>, and <span class="nctnt ncbi-macro">USING\_NCBI\_SCOPE</span> is discussed in [use of the NCBI name scope](ch_style.html#ch_style.using_NCBI_namespace).

### Other Namespace Macros

The <span class="nctnt ncbi-macro">BEGIN\_NCBI\_SCOPE</span>, <span class="nctnt ncbi-macro">END\_NCBI\_SCOPE</span>, and <span class="nctnt ncbi-macro">USING\_NCBI\_SCOPE</span> macros in turn use the more general purpose <span class="nctnt ncbi-macro">BEGIN\_SCOPE(ns)</span>, <span class="nctnt ncbi-macro">END\_SCOPE(ns)</span>, and <span class="nctnt ncbi-macro">USING\_SCOPE(ns)</span> macros, where the macro parameter <span class="nctnt ncbi-var">ns</span> is the namespace being defined. All NCBI-related code should be in the <span class="nctnt ncbi-var">ncbi::</span> namespace so the <span class="nctnt ncbi-macro">BEGIN\_NCBI\_SCOPE</span>, <span class="nctnt ncbi-macro">END\_NCBI\_SCOPE</span>, and <span class="nctnt ncbi-macro">USING\_NCBI\_SCOPE</span> should be adequate for new NCBI code. However, in those rare circumstances, if you need to define a new name scope, you can directly use the <span class="nctnt ncbi-macro">BEGIN\_SCOPE(ns)</span>, <span class="nctnt ncbi-macro">END\_SCOPE(ns)</span>, and <span class="nctnt ncbi-macro">USING\_SCOPE(ns)</span> macros.

### Name Concatenation

The macros <span class="nctnt ncbi-macro">NCBI\_NAME2</span> and <span class="nctnt ncbi-macro">NCBI\_NAME3</span> define concatenation of two and three names, respectively. These are used to build names for program-generated class, struct, or method names.

### Compiler-specific Macros

To cater to the idiosyncrasies of compilers that have non-standard behavior, certain macros are defined to normalize their behavior.

The <span class="nctnt ncbi-macro">BREAK(it)</span> macro advances the iterator to the end of the loop and then breaks out of the loop for the Sun WorkShop compiler with versions less than 5.3. This is done because this compiler fails to call destructors for objects created in for-loop initializers. This macro prevents trouble with iterators that contain CRefs by advancing them to the end using a while-loop, thus avoiding the "deletion of referenced CObject" errors. For other compilers, <span class="nctnt ncbi-macro">BREAK(it)</span> is defined as the keyword <span class="nctnt ncbi-code">break</span>.

The ICC compiler may fail to generate code preceded by <span class="nctnt ncbi-code">template\<\></span>. In this case, use the macro <span class="nctnt ncbi-macro">EMPTY\_TEMPLATE</span> instead, which expands to an empty string for the ICC compiler and to <span class="nctnt ncbi-code">template\<\></span> for all other compilers.

For MSVC v6.0, the <span class="nctnt ncbi-code">for</span> keyword is defined as a macro to overcome a problem with for-loops in the compiler. The local variables in a for-loop initalization are visible outside the loop:

    for (int i; i < 10; ++i) {
    // scope of i
    }
    // i should not be visible, but is visible in MSVC 6.0

Another macro called <span class="nctnt ncbi-macro">NCBI\_EAT\_SEMICOLON</span> is used in creating new names that can allow a trailing semicolon without producing a compiler warning in some compilers.

Configuration Parameters
------------------------

The <span class="nctnt ncbi-class">CParam</span> class is the preferred method for defining configuration parameters. This class enables storing parameters with per-object values, thread-wide defaults, and application-wide defaults. Global default values may be set through the application registry or the environment.

The following topics discuss using the <span class="nctnt ncbi-class">CParam</span> class.

-   [General Usage Information](#general-usage-information)

-   [Macros for Creating Parameters](#macros-for-creating-parameters)

-   [Methods for Using Parameters](#methods-for-using-parameters)

-   [Supporting Classes](#supporting-classes)

### General Usage Information

A <span class="nctnt ncbi-class">CParam</span> instance gets its initial value from one of three sources. If the application registry specifies a value, then that value will be used. Otherwise if the environment specifies a value, then that value will be used. Otherwise the default value supplied in the definition will be used. Later, the value can be changed [using various methods](#using-various-methods).

<span class="nctnt highlight">N.B.</span> statically defined instances of configuration parameters will be assigned their default values even if the environment and / or application registry specify (possibly different) values for them. This is because they are constructed (using their default value) at program startup and at that time the application framework for reading from the environment and application registry hasn't been set up yet. Therefore it is important to call the <span class="nctnt ncbi-func">Reset()</span> method for these parameters prior to reading their value. Alternatively, the <span class="nctnt ncbi-func">GetState()</span> method will indicate whether or not all possible sources were checked when a value was assigned to a configuration parameter - if they were, it will have either the value <span class="nctnt ncbi-var">eState\_Config</span> or <span class="nctnt ncbi-var">eState\_User</span>.

For more information on the application framework, the environment, and the application registry, see the sections on [CNcbiApplication](#cncbiapplication), [CNcbiEnvironment](#cncbienvironment), and [CNcbiRegistry](#cncbiregistry).

Be sure to include the header file in your source files:

    #include <corelib/ncbi_param.hpp>

and include the NCBI core library in your makefile:

    LIB = xncbi

### Macros for Creating Parameters

The <span class="nctnt ncbi-class">CParam</span> class is not designed to be used directly for creating configuration parameter variables. Instead, it supplies macros which your code should use. These macros have parameters for types, sections, names, default values, flags, and environment.

The <span class="nctnt ncbi-var">type</span> macro parameter must:

-   be a [POD](http://en.wikipedia.org/wiki/Plain_old_data_structures) type;

-   be initializable by the pre-processor from a literal;

-   be readable from and writable to streams.

Typically, the <span class="nctnt ncbi-var">type</span> is a simple type such as string, bool, int, or enum, as these are most convenient for specifying parameter values.

The <span class="nctnt ncbi-var">section</span> macro parameter indicates which section of a configuration file the parameter should be located in.

The <span class="nctnt ncbi-var">name</span> macro parameter uniquely identifies the parameter within the section.

The <span class="nctnt ncbi-var">default\_value</span> macro parameter provides the default value for the parameter - i.e. the value the parameter has from the time it is created until it is overwritten by a value from the environment, configuration file, or user code - and the value it is assigned by the <span class="nctnt ncbi-func">Reset()</span> method.

The <span class="nctnt ncbi-var">flags</span> macro parameter (a bitwise OR of enum values) can be used to control certain behavior options for the parameter. Currently, these enum values are:

| Enum Value       | Purpose                                  |
|------------------|------------------------------------------|
| eParam\_Default  | Default flags                            |
| eParam\_NoLoad   | Do not load from registry or environment |
| eParam\_NoThread | Do not use per-thread values             |

See the [enum definition](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ENcbiParamFlags&d=) for an up-to-date list.

The <span class="nctnt ncbi-var">env</span> macro parameter can be used to specify the environment variable to be searched. If the <span class="nctnt ncbi-var">env</span> macro parameter is not used, the environment will be searched for a variable having the form <span class="nctnt ncbi-macro">NCBI\_CONFIG\_\_\<section\>\_\_\<name\></span> (<span class="nctnt highlight">note:</span> the first underscore is single; the others are double).

<span class="nctnt ncbi-class">CParam</span> instances must be declared and defined before use. A typedef may also be created.

To *declare* simple parameters, use the <span class="nctnt ncbi-macro">NCBI\_PARAM\_DECL</span> macro:

    NCBI_PARAM_DECL(type, section, name);

For example, declaring a host name parameter for a server might look like:

    NCBI_PARAM_DECL(string, XyzSrv, Host);

To declare an enum:

    NCBI_PARAM_ENUM_DECL(type, section, name);

Additional macros for parameter declarations include:

-   <span class="nctnt ncbi-macro">NCBI\_PARAM\_DECL\_EXPORT</span> and <span class="nctnt ncbi-macro">NCBI\_PARAM\_ENUM\_DECL\_EXPORT</span> to include the EXPORT specifier (i.e. <span class="nctnt ncbi-macro">NCBI\_XNCBI\_EXPORT</span>). <span class="nctnt highlight">Note:</span> this form must be used if the parameter is defined in a header file and compiled into a library. Otherwise the linker may create several instances of the parameter which could contain different values.

To *define* simple parameters, use the <span class="nctnt ncbi-macro">NCBI\_PARAM\_DEF</span> or <span class="nctnt ncbi-macro">NCBI\_PARAM\_DEF\_EX</span> macro:

    NCBI_PARAM_DEF(type, section, name, default_value); // OR
    NCBI_PARAM_DEF_EX(type, section, name, default_value, flags, env);

For example, an extended definition of a host name parameter for a server could look like:

    NCBI_PARAM_DEF_EX(string, Xyz, Host, "xyz.nih.gov", eParam_NoThread, XYZ_HOST);

To define an enum:

    NCBI_PARAM_ENUM_ARRAY(type, section, name); // USE THIS AND EITHER:
    NCBI_PARAM_ENUM_DEF(type, section, name, default_value); // OR:
    NCBI_PARAM_ENUM_DEF_EX(type, section, name, default_value, flags, env);

For example, an enum definition could look like:

    NCBI_PARAM_ENUM_ARRAY(EMyEnum, MySection, MyEnumParam)
    {
        {"My_A", eMyEnum_A},
        {"My_B", eMyEnum_B},
        {"My_C", eMyEnum_C},
    };
    NCBI_PARAM_ENUM_DEF(EMyEnum, MySection, MyEnumParam, eMyEnum_B);

An additional macro for parameter definitions is:

-   <span class="nctnt ncbi-macro">NCBI\_PARAM\_DEF\_IN\_SCOPE</span> to define the parameter within a scope.

Another way to conveniently use a configuration parameter is to use the <span class="nctnt ncbi-macro">NCBI\_PARAM\_TYPE</span> macro to create an instance of a type. The following example illustrates the declaration, definition, typedef, and use of a configuration parameter:

    NCBI_PARAM_DECL(bool, NCBI, ABORT_ON_COBJECT_THROW);
    NCBI_PARAM_DEF_EX(bool, NCBI, ABORT_ON_COBJECT_THROW, false,
                      eParam_NoThread, NCBI_ABORT_ON_COBJECT_THROW);
    typedef NCBI_PARAM_TYPE(NCBI, ABORT_ON_COBJECT_THROW) TAbortOnCObectThrow;

    void CObjectException::x_InitErrCode(CException::EErrCode err_code)
    {
        CCoreException::x_InitErrCode(err_code);
        static TAbortOnCObectThrow sx_abort_on_throw;
        if ( sx_abort_on_throw.Get() ) {
            Abort();
        }
    }

### Methods for Using Parameters

Important methods of the <span class="nctnt ncbi-class">CParam</span> class are:

| Method                                                    | Static | Purpose                                                                                                                                                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-func">GetState()</span>           | Yes    | Get the current state of the parameter. The state indicates the last source checked when assigning its value. <span class="nctnt highlight">N.B.</span> it specifically does *not* indicate the origin of the current value. See the [EParamState](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCParamBase.html#0f2898884063b661395c511bcdb1c6ea) enum for specific values. |
| <span class="nctnt ncbi-func">Get()</span>                | No     | Get the current parameter value.                                                                                                                                                                                                                                                                                                                                                             |
| <span class="nctnt ncbi-func">Set()</span>                | No     | Set a new parameter value (this instance only).                                                                                                                                                                                                                                                                                                                                              |
| <span class="nctnt ncbi-func">Reset()</span>              | No     | Reset the value as if it has not been initialized yet.                                                                                                                                                                                                                                                                                                                                       |
| <span class="nctnt ncbi-func">GetDefault()</span>         | Yes    | Get the global default value.                                                                                                                                                                                                                                                                                                                                                                |
| <span class="nctnt ncbi-func">SetDefault()</span>         | Yes    | Set a new global default value.                                                                                                                                                                                                                                                                                                                                                              |
| <span class="nctnt ncbi-func">ResetDefault()</span>       | Yes    | Reload the global default value from the environment/registry or reset it to the initial value specified in NCBI\_PARAM\_DEF.                                                                                                                                                                                                                                                                |
| <span class="nctnt ncbi-func">GetThreadDefault()</span>   | Yes    | Get the thread-local default value if set, otherwise the global default value.                                                                                                                                                                                                                                                                                                               |
| <span class="nctnt ncbi-func">SetThreadDefault()</span>   | Yes    | Set a new thread-local default value.                                                                                                                                                                                                                                                                                                                                                        |
| <span class="nctnt ncbi-func">ResetThreadDefault()</span> | Yes    | Reset the thread default value as if it has not been set.                                                                                                                                                                                                                                                                                                                                    |

Typical uses involve getting the current or default values:

    // get a parameter's default value
    string bots = NCBI_PARAM_TYPE(CGI,Bots)::GetDefault();

    // get a parameter's current value
    typedef NCBI_PARAM_TYPE(READ_FASTA, USE_NEW_IMPLEMENTATION) TParam_NewImpl;
    TParam_NewImpl new_impl;
    if (new_impl.Get()) {
        // do something
    }

### Supporting Classes

The CParam class is packaged with two supporting classes: <span class="nctnt ncbi-class">CParamException</span> and <span class="nctnt ncbi-class">CParamParser</span>.

<span class="nctnt ncbi-class">CParamException</span> will be thrown by the parameter parser if invalid parameter values are specified in the environment, configuration file, or code.

<span class="nctnt ncbi-class">CParamParser</span> is a templatized helper class that parses parameter literals into parameter values, using its <span class="nctnt ncbi-func">StringToValue()</span> method. [<span class="nctnt highlight">Note:</span> the "String" in this method name refers to the string of characters in the literal being parsed (regardless of the type it represents), not to the <span class="nctnt ncbi-type">std::string</span> type.] A <span class="nctnt ncbi-func">ValueToString()</span> method is also provided for completeness.

<span class="nctnt ncbi-class">CParamParser</span> templates have been pre-defined for string, bool, int, and enum types. If you need to create a configuration parameter that is more complex than these types, then you will need to either instantiate <span class="nctnt ncbi-class">CParamParser</span> for your type or define appropriate <span class="nctnt ncbi-func">operator\<\<()</span> and <span class="nctnt ncbi-func">operator\>\>()</span> methods. This will:

-   enable parsing of the default value specified in the definition of your complex configuration parameter;

-   enable that type to be read from the application registry or environment; and

-   enable that type to be assigned values via the <span class="nctnt ncbi-func">Set\*()</span> methods.

<span class="nctnt highlight">Note:</span> Defining the appropriate <span class="nctnt ncbi-func">operator\<\<()</span> and <span class="nctnt ncbi-func">operator\>\>()</span> methods is preferrable to instantiating <span class="nctnt ncbi-class">CParamParser</span> for your type because:

-   instantiating <span class="nctnt ncbi-class">CParamParser</span> for your type would make it more difficult to change the <span class="nctnt ncbi-class">CParamParser</span> template, if that should become necessary; and

-   <span class="nctnt ncbi-func">operator\<\<()</span> and <span class="nctnt ncbi-func">operator\>\>()</span> can be useful in other contexts.

Using the CNcbiRegistry Class
-----------------------------

If for some reason the <span class="nctnt ncbi-class">CParam</span> class cannot be used to [define configuration parameters](#define-configuration-parameters), the <span class="nctnt ncbi-class">CNcbiRegistry</span> class may be used instead.

This section provides reference information on the use of the <span class="nctnt ncbi-class">CNcbiRegistry</span> class. For an overview of this class, refer to the [introductory chapter](ch_intro.html#ch_intro.intro_reg). This class is also discussed in the [library configuration chapter](ch_libconfig.html#ch_libconfig.libconfig_registry).

The following topics are discussed in this section:

-   [Working with the Registry class: CNcbiRegistry](#working-with-the-registry-class-cncbiregistry)

-   [Syntax of the Registry Configuration File](#syntax-of-the-registry-configuration-file)

-   [Search Order for Initialization (\*.ini) Files](#search-order-for-initialization-*ini-files)

-   [Fine-Tuning Registry Parameters Using IRegistry::EFlags](#fine-tuning-registry-parameters-using-iregistryeflags)

-   [Main Methods of CNcbiRegistry](#main-methods-of-cncbiregistry)

-   [Additional Registry Methods](#additional-registry-methods)

### Working with the Registry Class: CNcbiRegistry

The [CNcbiRegistry](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiRegistry.html) class is used to load, access, modify, and store runtime information read from configuration files. Previously, these files were by convention named <span class="nctnt ncbi-path">.\*rc</span> files on Unix-like systems. The convention for all platforms now is to name such files <span class="nctnt ncbi-path">\*.ini</span> (where <span class="nctnt ncbi-path">\*</span> is by default the application name). An exception to this rule is the system-wide registry, which is named <span class="nctnt ncbi-path">.ncbirc</span> on Unix-like systems and <span class="nctnt ncbi-path">ncbi.ini</span> on Windows systems. The [CNcbiRegistry](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiRegistry.html) class can read and parse configuration files, search and edit retrieved information, and write back to the file.

The following resources are checked when loading a registry:

-   the environment

-   the overrides registry

-   the application registry

-   the system registry

-   inherited registries

In addition, registries can be loaded from files programmatically.

An environment registry is created from configuration parameters specified in the environment. Often, such variables have the form <span class="nctnt ncbi-var">NCBI\_CONFIG\_\_\<section\>\_\_\<entry\></span> (note the double underscores) and can have corresponding entries in initialization files, but see the [library configuration chapter](ch_libconfig.html) for details on specific parameters. Entries in the environment registry have the highest precedence.

If the special environment variable <span class="nctnt ncbi-var">NCBI\_CONFIG\_OVERRIDES</span> is defined, the configuration file it names will be loaded as the overrides registry. This registry will have the next highest precedence after the environment.

For the application registry, the name of the configuration file can be explicitly set with the <span class="nctnt ncbi-cmd">-conffile</span> command-line argument, set (or disabled) with the <span class="nctnt ncbi-var">conf</span> argument of [CNcbiApplication::AppMain()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiApplication.html), or implicitly set (or disabled) according to [search order rules](#search-order-rules). If the <span class="nctnt ncbi-cmd">-conffile</span> command-line argument is supplied, that path will be used. If the <span class="nctnt ncbi-var">conf</span> argument to <span class="nctnt ncbi-func">AppMain()</span> is supplied, the file will be determined according to [Table 2](#table-2). Otherwise, the file will be determined according to [search order rules](#search-order-rules). The application registry follows the overrides registry in precedence.

Table 2. Location of configuration files

| conf                                           | Where to Look for the config File                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| *empty* [default]                              | Compose the config file name from the base application name plus <span class="nctnt ncbi-path">.ini</span>. Also try to strip file extensions, e.g., for the application named <span class="nctnt ncbi-app">my\_app.cgi.exe</span> try subsequently: <span class="nctnt ncbi-path">my\_app.cgi.exe.ini</span>, <span class="nctnt ncbi-path">my\_app.cgi.ini</span>, <span class="nctnt ncbi-path">my\_app.ini</span>. Using these names, search in directories as described in the "Otherwise" case for non-empty <span class="nctnt ncbi-var">conf</span> (see below).                                                                                                                                              |
| <span class="nctnt ncbi-monospace">NULL</span> | Do not even try to load the registry at all                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| *non-empty*                                    | If <span class="nctnt ncbi-var">conf</span> contains a path, then try to load from the config file named <span class="nctnt ncbi-var">conf</span> (only and exactly!). If the path is not fully qualified and it starts from <span class="nctnt ncbi-path">../</span> or <span class="nctnt ncbi-path">./</span>, then look for the config file starting from the current working dir. **Otherwise** (only a basename, without path), the config file will be searched for in the following places (in the order of preference): 1. current work directory; 2. user home directory; 3. directory defined by environment variable <span class="nctnt ncbi-var">NCBI</span>; 4. system directory; 5. program directory. |

When the application registry is successfully loaded, you can access it using the method [CNcbiApplication::GetConfig()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiApplication.html). The application will throw an exception if the config file is found, is not empty, and either cannot be opened or contains invalid entries. If the <span class="nctnt ncbi-var">conf</span> argument to [CNcbiApplication::AppMain()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiApplication.html) is not <span class="nctnt ncbi-monospace">NULL</span> and the config file cannot be found, then a warning will be posted to the application diagnostic stream.

System-wide configuration parameters can be defined in the system registry. The system registry will not be loaded if it contains the <span class="nctnt ncbi-monospace">DONT\_USE\_NCBIRC</span> entry in the <span class="nctnt ncbi-monospace">NCBI</span> section or if the environment variable <span class="nctnt ncbi-var">NCBI\_DONT\_USE\_NCBIRC</span> is defined. See the [search order](#search-order) section below for details. The system registry follows the application registry in precedence.

Configuration files may "inherit" entries from other configuration files using the <span class="nctnt ncbi-monospace">.Inherits</span> entry in the <span class="nctnt ncbi-monospace">[NCBI]</span> section. The <span class="nctnt ncbi-monospace">.Inherits</span> entry is a space- and/or comma- delimited list of file names. Files having a <span class="nctnt ncbi-path">.ini</span> extension may be listed in the <span class="nctnt ncbi-monospace">.Inherits</span> entry without the <span class="nctnt ncbi-path">.ini</span> extension. Note that extensionless file names are not supported in the <span class="nctnt ncbi-monospace">.Inherits</span> entry. Inherited registries have the same precedence as the registry that inherited them.

Registries can be programmatically loaded from files by calling <span class="nctnt ncbi-func">CNcbiRegistry::Read()</span>. <span class="nctnt ncbi-func">CNcbiApplication::LoadConfig()</span> can also be called to "manually" load the application registry - for example, if special flags are required. The precedence for programmatically loaded registries depends on the flags they are loaded with. By default (or if loaded with the <span class="nctnt ncbi-var">IRegistry::fOverride</span> flag) they will have greater precedence that previously loaded registries, but if loaded with the <span class="nctnt ncbi-var">IRegistry::fNoOverride</span> flag, they will not override existing parameters.

Although registry objects can be instantiated and manipulated independently, they are typically used by the <span class="nctnt ncbi-class">CNcbiApplication</span> class. Specifically, <span class="nctnt ncbi-func">CNcbiApplication::AppMain()</span> attempts to load a registry with entries from all of the above sources (except programmatically loaded registries). <span class="nctnt ncbi-func">AppMain()</span> will look for the system and application registries in multiple locations, and possibly with a modified name, as described in the [search order](#search-order) section below.

See the [Registry](ch_libconfig.html#ch_libconfig.libconfig_registry) and [Environment](ch_libconfig.html#ch_libconfig.Environment) sections of the library configuration chapter for more information on controlling the registry via the environment.

### Syntax of the Registry Configuration File

The configuration file is composed of <span class="nctnt ncbi-monospace">section</span> headers and "<span class="nctnt ncbi-monospace">name=value</span>" strings, which occur within the named sections. It is also possible to include comments in the file, which are indicated by a new line with a leading semicolon. An example configuration file is shown below.

    # Registry file comment (begin of file)
    # MyProgram.ini
    ; parameters for section1
    [section1]
    name1 = value1 and value1.2
    n-2.3 = "  this value has two spaces at its very beginning and at the end  "
    name3 = this is a multi\
    line value
    name4 = this is a single line ended by back slash\\
    name5 = all backslashes and \
    new lines must be \\escaped\\...
    [ section2.9-bis ]
    ; This is a comment...
    name2 = value2

All comments and empty lines are ignored by the registry file parser. Line continuations, as usual, are indicated with a backslash escape. More generally, backslashes are processed as:

-   [backslash] + [backslash] -- converted into a single [backslash]

-   [backslash] + [space(s)] + [EndOfLine] -- converted to an [EndOfLine]

-   [backslash] + ["] -- converted into a ["]

Character strings with embedded spaces do not need to be quoted, and an unescaped double quote at the very beginning or end of a value is ignored. All other combinations with [backslash] and ["] are invalid.

The following restrictions apply to the <span class="nctnt ncbi-monospace">section</span> and <span class="nctnt ncbi-monospace">name</span> identifiers occurring in a registry file:

-   the string must contain only: [a-z], [A-Z], [0-9], [\_.-/] characters

-   the interpretation of the string is **not** case sensitive, e.g., <span class="nctnt ncbi-monospace">PATH == path == PaTh</span>

-   all leading and trailing spaces will be truncated

A special syntax is provided for "including" the content of one section into another section:

    .Include = section_name

For example, this:

    [section-a]
    ;section-a specific entries...
    a1 = a one
    .Include = common

    [section-b]
    ;section-b specific entries...
    b1 = b one
    .Include = common

    [common]
    ;common entries
    c1 = c one
    c2 = c two

is equivalent to:

    [section-a]
    ;section-a specific entries...
    a1 = a one
    ;common entries
    c1 = c one
    c2 = c two

    [section-b]
    ;section-b specific entries...
    b1 = b one
    ;common entries
    c1 = c one
    c2 = c two

Another special syntax is provided for "including" other configuration files:

    [NCBI]
    .Inherits = subregistry_list

Here, <span class="nctnt ncbi-monospace">subregistry\_list</span> is a comma- or space- separated list of one or more subregistry files. Subregistry file names are not required to have a "<span class="nctnt ncbi-monospace">.ini</span>" extension. However if they do, the "<span class="nctnt ncbi-monospace">.ini</span>" can be omitted from the subregistry list. For example, the specification:

    [NCBI]
    .Inherits = a

will select "<span class="nctnt ncbi-monospace">a.ini</span>". Subregistries can also define their own subregistries, thus permitting an application to read a tree of configuration files.

Given a specification of:

    [NCBI]
    .Inherits = a b

an entry in "<span class="nctnt ncbi-monospace">a.ini</span>" or any of its subregistries will take priority over an identically named entry in "<span class="nctnt ncbi-monospace">b.ini</span>" or any of its subregistries. This could be used, for example, to retain a default configuration while working with a test configuration, such as in:

    [NCBI]
    .Inherits = mytest.ini myapp.ini

Entries in the main configuration file take priority over entries in subregistries.

Entries defined in a subregistry can be "undefined" by explicitly defining the entry as empty in a higher priority registry file.

Finally, the environment variable <span class="nctnt ncbi-cmd">NCBI\_CONFIG\_OVERRIDES</span> can be used to name a configuration file whose entries override any corresponding entries in all the processed registry files.

### Search Order for Initialization (\*.ini) Files

<span class="nctnt highlight">Note:</span> This section discusses the search order for initialization files, which is only applicable to the application and system initialization files. Please see the [Working with the Registry Class](#working-with-the-registry-class) section for a discussion about the other sources of configuration information and the relative precedence of all registry sources.

<span class="nctnt highlight">Note:</span> See [Table 2](#table-2) for rules about how the <span class="nctnt ncbi-var">conf</span> argument to <span class="nctnt ncbi-func">AppMain()</span> affects the search rules for the application initialization file. Also, if the <span class="nctnt ncbi-var">-conffile</span> command-line argument is used, then only that application initialization file is tried.

<span class="nctnt highlight">Note:</span> Several means are available to control loading of the system initialization file. It can be enabled by the <span class="nctnt ncbi-var">IRegistry::fWithNcbirc</span> flag. It can be disabled if (1) it contains the <span class="nctnt ncbi-monospace">DONT\_USE\_NCBIRC</span> entry in the <span class="nctnt ncbi-monospace">NCBI</span> section, (2) it contains syntax errors or no entries, or (3) if the environment variable <span class="nctnt ncbi-var">NCBI\_DONT\_USE\_NCBIRC</span> is defined.

With the exceptions noted above, the following rules determine the search order for application and system initialization files. Although application and system initialization files are not typically found in the same place, the same search order rules apply to both (with the above exceptions).

1  
If the environment variable <span class="nctnt ncbi-var">NCBI\_CONFIG\_PATH</span> is set, that will be the only path searched for initialization files.

2  
Otherwise, the search order includes the following directories in order:

a  
If the environment variable <span class="nctnt ncbi-var">NCBI\_DONT\_USE\_LOCAL\_CONFIG</span> is *not* defined then:

i  
The current working directory ("<span class="nctnt ncbi-path">.</span>").

ii  
The user's home directory (if it can be established).

b  
The path in the environment variable <span class="nctnt ncbi-var">NCBI</span> (if it is defined).

c  
The standard system directory ("<span class="nctnt ncbi-path">/etc</span>" on Unix-like systems, and given by the environment variable <span class="nctnt ncbi-var">SYSTEMROOT</span> on Windows).

d  
The directory containing the application, if known (this requires use of <span class="nctnt ncbi-class">CNcbiApplication</span>).

<span class="nctnt highlight">Note:</span> The search ends with the first file found.

The above rules determine the search order for directories, but there are also rules for initialization file names:

For the application registry: When the initialization file name is not explicitly specified (e.g. on the command line) then the implicit name will be formed by appending "<span class="nctnt ncbi-path">.ini</span>" to the application name. When the application name contains extensions, multiple names may be tried by sequentially stripping extensions off the application name. For example, if an application name is <span class="nctnt ncbi-path">a.b.c</span> then the sequence of initialization file names tried is: <span class="nctnt ncbi-path">a.b.c.ini</span>, <span class="nctnt ncbi-path">a.b.ini</span>, and finally <span class="nctnt ncbi-path">a.ini</span>.

On Unix-like systems, if an application <span class="nctnt ncbi-path">dir1/app1</span> is a symlink to <span class="nctnt ncbi-path">dir2/app2</span>, the directory/name search order will be:

1  
<span class="nctnt ncbi-path">./app1.ini</span>

2  
<span class="nctnt ncbi-path">$NCBI/app1.ini</span>

3  
<span class="nctnt ncbi-path">~/app1.ini</span>

4  
<span class="nctnt ncbi-path">dir1/app1.ini</span>

5  
<span class="nctnt ncbi-path">dir2/app1.ini</span>

6  
<span class="nctnt ncbi-path">./app2.ini</span>

7  
<span class="nctnt ncbi-path">$NCBI/app2.ini</span>

8  
<span class="nctnt ncbi-path">~/app2.ini</span>

9  
<span class="nctnt ncbi-path">dir1/app2.ini</span>

10  
<span class="nctnt ncbi-path">dir2/app2.ini</span>

For the system registry: The name <span class="nctnt ncbi-path">.ncbirc</span> is tried on Unix-like systems and <span class="nctnt ncbi-path">ncbi.ini</span> is tried on Windows. <span class="nctnt highlight">Note:</span> NCBI in-house Linux systems have "<span class="nctnt ncbi-path">/etc/.ncbirc</span>" symlinked to "<span class="nctnt ncbi-path">/opt/ncbi/config/.ncbirc</span>" so that applications running on production systems (or with <span class="nctnt ncbi-var">NCBI</span> unset) still pick up standard configuration settings.

### <span class="nctnt ncbi-type">Fine-Tuning Registry Parameters Using IRegistry::EFlags</span>

<span class="nctnt highlight">Note:</span> This section deals with concepts not typically needed by most C++ Toolkit users. The functionality of <span class="nctnt ncbi-class">CNcbiRegistry</span> is automatically and transparently provided when you use <span class="nctnt ncbi-class">CNcbiApplication</span>. You probably won't need to read this section unless you're working with an application that edits registry files or explicitly sets registry entry values.

Each <span class="nctnt ncbi-class">CNcbiRegistry</span> entry has a set of flags that control how it is handled, defined by this enum:

    enum EFlags {
        fTransient      = 0x1,   ///< Transient -- not saved by default
        fPersistent     = 0x100, ///< Persistent -- saved when file is written
        fOverride       = 0x2,   ///< Existing value can be overriden
        fNoOverride     = 0x200, ///< Cannot change existing value
        fTruncate       = 0x4,   ///< Leading, trailing blanks can be truncated
        fNoTruncate     = 0x400, ///< Cannot truncate parameter value
        fJustCore       = 0x8,   ///< Ignore auxiliary subregistries
        fNotJustCore    = 0x800, ///< Include auxiliary subregistries
        fIgnoreErrors   = 0x10,  ///< Continue reading after parse errors
        fInternalSpaces = 0x20,  ///< Allow internal whitespace in names
        fWithNcbirc     = 0x40,  ///< Include .ncbirc (used only by CNcbiRegistry)
        fCountCleared   = 0x80,  ///< Let explicitly cleared entries stand
        fSectionCase    = 0x1000,///< Create with case-sensitive section names
        fEntryCase      = 0x2000,///< Create with case-sensitive entry names
        fCoreLayers     = fTransient \| fPersistent \| fJustCore,
        fAllLayers      = fTransient \| fPersistent \| fNotJustCore,
        fCaseFlags      = fSectionCase \| fEntryCase
    };
    typedef int TFlags;  ///< Binary OR of "EFlags"

Some pairs of these flags are mutually exclusive and have a default if neither flag is given:

| Flag Pair                                                                                        | Default                                         |
|--------------------------------------------------------------------------------------------------|-------------------------------------------------|
| <span class="nctnt ncbi-var">fTransient</span> / <span class="nctnt ncbi-var">fPersistent</span> | <span class="nctnt ncbi-var">fPersistent</span> |
| <span class="nctnt ncbi-var">fOverride</span> / <span class="nctnt ncbi-var">fNoOverride</span>  | <span class="nctnt ncbi-var">fOverride</span>   |
| <span class="nctnt ncbi-var">fJustCore</span> / <span class="nctnt ncbi-var">fNotJustCore</span> | <span class="nctnt ncbi-var">fJustCore</span>   |

It is not necessary to use the <span class="nctnt ncbi-var">fNoTruncate</span> flag because it represents the default behavior - no values are truncated unless <span class="nctnt ncbi-var">fTruncate</span> is used.

The flag <span class="nctnt ncbi-var">fWithNcbirc</span> can be passed to the <span class="nctnt ncbi-class">CNcbiRegistry</span> constructor, the <span class="nctnt ncbi-func">CNcbiRegistry::IncludeNcbircIfAllowed()</span> method, or the <span class="nctnt ncbi-func">IRWRegistry::IncludeNcbircIfAllowed()</span> method. If it is set then the system-wide registry is used - see the [search order section](#search-order-section) for details on the system-wide registry.

For example, the following code demonstrates that the bit-wise OR of <span class="nctnt ncbi-var">fTruncate </span>and <span class="nctnt ncbi-var">fNoOverride</span> strips all leading and trailing blanks and does not override an existing value:

    CNcbiRegistry reg;
    CNcbiRegistry::TFlags flags = CNcbiRegistry::fNoOverride \|
                                  CNcbiRegistry::fTruncate;
    reg.Set("MySection", "MyName", " Not Overridden  ", flags);
    reg.Set("MySection", "MyName", " Not Saved ",       flags);
    cout << "[MySection]MyName=" << reg.Get("MySection", "MyName") << ".\n" << endl;

    // outputs "[MySection]MyName=Not Overridden."

### Main Methods of <span class="nctnt ncbi-class">CNcbiRegistry</span>

The <span class="nctnt ncbi-class">CNcbiRegistry</span> class constructor takes two arguments - an input stream to read the registry from (usually a file), and an optional <span class="nctnt ncbi-type">TFlags</span> argument, where the latter can be used to specify that all of the values should be stored as <span class="nctnt ncbi-monospace">transient</span> rather than in the default mode, which is <span class="nctnt ncbi-monospace">persistent</span>:

    CNcbiRegistry(CNcbiIstream& is, TFlags flags = 0);

Once the registry has been initialized by its constructor, it is also possible to load additional parameters from other file(s) using the <span class="nctnt ncbi-func">Read()</span> method:

    void Read(CNcbiIstream& is, TFlags flags = 0);

Valid flags for the <span class="nctnt ncbi-func">Read() </span>method include <span class="nctnt ncbi-var">eTransient </span>and <span class="nctnt ncbi-var">eNoOverride</span>. The default is for all values to be read in as <span class="nctnt ncbi-monospace">persistent</span>, with the capability of overriding any previously loaded value associated with the same name. Either or both of these defaults can be modified by specifying <span class="nctnt ncbi-var">eTransient, eNoOverride</span>, or (<span class="nctnt ncbi-var">eTransient \\| eNoOverride</span>) as the <span class="nctnt ncbi-var">flags</span> argument in the above expression.

The <span class="nctnt ncbi-func">Write() </span>method takes as its sole argument, a destination stream to which only the <span class="nctnt ncbi-monospace">persistent</span> configuration parameters will be written.

    bool Write(CNcbiOstream& os) const;

The configuration parameter values can also be set directly inside your application, using:

    bool Set(const string& section, const string& name,
             const string& value, TFlags flags = 0);

Here, valid flag values include <span class="nctnt ncbi-var">ePersistent, eNoOverride</span>, <span class="nctnt ncbi-var">eTruncate, </span>or any logical combination of these. If <span class="nctnt ncbi-var">eNoOverride</span> is set and there is a previously defined value for this parameter, then the value is not reset, and the method returns <span class="nctnt ncbi-monospace">false</span>.

The <span class="nctnt ncbi-func">Get()</span> method first searches the set of <span class="nctnt ncbi-monospace">transient</span> parameters for a parameter named <span class="nctnt ncbi-var">name</span>, in section <span class="nctnt ncbi-var">section</span>, and if this fails, continues by searching the set of <span class="nctnt ncbi-monospace">persistent</span> parameters. However, if the <span class="nctnt ncbi-var">ePersistent</span> flag is used, then only the set of <span class="nctnt ncbi-monospace">persistent</span> parameters will be searched. On success, <span class="nctnt ncbi-func">Get()</span> returns the stored value. On failure, the empty string is returned.

    const string& Get(const string& section, const string& name,
                      TFlags flags = 0) const;

### Additional Registry Methods

Four additional note-worthy methods defined in the <span class="nctnt ncbi-class">CNcbiRegistry</span> interface are:

    bool Empty(void) const;
    void Clear(void);
    void EnumerateSections(list<string>*sections) const;
    void EnumerateEntries(const string& section,  list<string>* entries) const;

<span class="nctnt ncbi-func">Empty()</span> returns <span class="nctnt ncbi-monospace">true</span> if the registry is empty. <span class="nctnt ncbi-func">Clear()</span> empties out the registry, discarding all stored parameters. <span class="nctnt ncbi-func">EnumerateSections() </span>writes all registry section names to the list of strings parameter named "sections". <span class="nctnt ncbi-func">EnumerateEntries()</span> writes the list of parameter names in section to the list of strings parameter named "entries".

Portable Stream Wrappers
------------------------

Because of differences in the C++ standard stream implementations between different compilers and platforms, the file <span class="nctnt ncbi-func">ncbistre.hpp</span> contains portable aliases for the standard classes. To provide portability between the supported platforms, it is recommended the definitions in <span class="nctnt ncbi-func">ncbistre.hpp</span> be used.

The <span class="nctnt ncbi-func">ncbistre.hpp</span> defines wrappers for many of the standard stream classes and contains conditional compilation statements triggered by macros to include portable definitions. For example, not all compilers support the newer '\#include \<iostream\>' form. In this case, the older '\#include \<iostream.h\>' is used based on whether the macro <span class="nctnt ncbi-macro">NCBI\_USE\_OLD\_IOSTREAM</span> is defined.

Instead of using the iostream, istream or ostream, you should use the portable <span class="nctnt ncbi-class">CNcbiIostream</span>, <span class="nctnt ncbi-class">CNcbiIstream</span> and <span class="nctnt ncbi-class">CNcbiOstream</span>. Similarly, instead of using the standard <span class="nctnt ncbi-var">cin</span>, <span class="nctnt ncbi-var">cout</span>, <span class="nctnt ncbi-var">cerr</span> you can use the more portable <span class="nctnt ncbi-var">NcbiCin</span>, <span class="nctnt ncbi-var">NcbiCout</span>, and <span class="nctnt ncbi-var">NcbiCerr</span>.

The <span class="nctnt ncbi-func">ncbistre.hpp</span> also defines functions that handle platform-specific end of line reads. For example, <span class="nctnt ncbi-func">Endl()</span> represents platform specific end of line, and <span class="nctnt ncbi-func">NcbiGetline()</span> reads from a specified input stream to a string, and <span class="nctnt ncbi-func">NcbiGetlineEOL()</span> reads from a specified input stream to a string taking into account platform specific end of line.

Working with Diagnostic Streams ([\*](ch_debug.html#ch_debug.std_cpp_message_post))
-----------------------------------------------------------------------------------

This section provides reference information on the use of the diagnostic stream classes. For an overview of the diagnostic stream concepts refer to the [introductory chapter](ch_intro.html#ch_intro.intro_diag).

The [CNcbiDiag](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiDiag.html) class implements the functionality of an output stream enhanced with error posting mechanisms similar to those found in the NCBI C Toolkit. A <span class="nctnt ncbi-class">CNcbiDiag </span>object has the look and feel of an output stream; its member functions and friends include output operators and format manipulators. A <span class="nctnt ncbi-class">CNcbiDiag</span> object is not itself a stream, but serves as an interface to a stream which allows multiple threads to write to the same output. Each instance of <span class="nctnt ncbi-class">CNcbiDiag</span> includes the following private data members:

-   a buffer to store (a single) message text

-   a severity level

-   a set of post flags

Limiting each instance of <span class="nctnt ncbi-class">CNcbiDiag</span> to the storage and handling of a single message ensures that multiple threads writing to the same stream will not have interleaving message texts.

The following topics are discussed in this section:

-   [Where Diagnostic Messages Go](#where-diagnostic-messages-go)

-   [Setting Diagnostic Severity Levels](#setting-diagnostic-severity-levels)

-   [Diagnostic Messages Filtering](#diagnostic-messages-filtering)

-   [Log File Format](#log-file-format)

    -   [The Old Post Format](#the-old-post-format)

    -   [The New Post Format](#the-new-post-format)

    -   [Controlling the Appearance of Diagnostic Messages using Post Flags](#controlling-the-appearance-of-diagnostic-messages-using-post-flags)

-   [Defining the Output Stream](#defining-the-output-stream)

-   [Tee Output to STDERR](#tee-output-to-stderr)

-   [The Message Buffer](#the-message-buffer)

-   [Request Exit Status Codes](#request-exit-status-codes)

    -   [Standard (HTTP-like) status codes](#standard-http-like-status-codes)

    -   [NCBI-specific status codes](#ncbi-specific-status-codes)

-   [Error codes and their Descriptions](#error-codes-and-their-descriptions)

-   [Defining Custom Handlers using CDiagHandler](#defining-custom-handlers-using-cdiaghandler)

-   [The ERR\_POST and LOG\_POST Macros](#the-errpost-and-logpost-macros)

-   [The \_TRACE macro](#the-trace-macro)

-   [Performance Logging](#performance-logging)

-   [Stack Traces](#stack-traces)

    -   [Printing a Stack Trace](#printing-a-stack-trace)

    -   [Obtaining a Stack Trace for Exceptions](#obtaining-a-stack-trace-for-exceptions)

### Where Diagnostic Messages Go

The following decision tree describes how the destination for diagnostics messages is determined.

1  
Before the application is constructed (before <span class="nctnt ncbi-func">AppMain()</span> is called), everything goes to:

1  
(Unix-like systems only) <span class="nctnt ncbi-path">/log/fallback/UNKNOWN.{log\\|err\\|trace}</span> -- if available

2  
<span class="nctnt ncbi-var">STDERR</span> -- otherwise

2  
When the application is ready, and its name is known, but before the configuration file is loaded:

1  
If <span class="nctnt ncbi-func">AppMain()</span> is passed flags <span class="nctnt ncbi-var">eDS\_Default</span> or <span class="nctnt ncbi-var">eDS\_ToStdlog</span>, then the diagnostics goes:

1  
(Unix-like systems only) if <span class="nctnt ncbi-path">/log</span> is present:

1  
if the application is described in <span class="nctnt ncbi-path">/etc/toolkitrc</span> -- to <span class="nctnt ncbi-path">/log/\<token\>/appname.{log\\|err\\|trace}</span>

2  
else if environment variable <span class="nctnt ncbi-var">$SERVER\_PORT</span> is set -- to <span class="nctnt ncbi-path">/log/$SERVER\_PORT/appname.{log\\|err\\|trace}</span>

3  
else (or if failed to switch to one of the above two locations) -- to <span class="nctnt ncbi-path">/log/srv/appname.{log\\|err\\|trace}</span>

4  
or, if failed to switch to that -- to <span class="nctnt ncbi-path">/log/fallback/appname.{ log\\|err\\|trace}</span>

2  
else (or if failed to switch to any of the /log location):

1  
<span class="nctnt ncbi-var">eDS\_ToStdlog</span> -- to <span class="nctnt ncbi-path">\<current\_working\_dir\>/appname.{ log\\|err\\|trace}</span> (and, if cannot, then continues to go to <span class="nctnt ncbi-var">STDERR</span>)

2  
<span class="nctnt ncbi-var">eDS\_Default</span> -- continues to go to <span class="nctnt ncbi-var">STDERR</span>

2  
If <span class="nctnt ncbi-func">AppMain()</span> is passed flags other than <span class="nctnt ncbi-var">eDS\_Default</span> or <span class="nctnt ncbi-var">eDS\_ToStdlog</span>, then the diagnostics goes to:

1  
<span class="nctnt ncbi-var">eDS\_ToStdout</span> -- standard output stream

2  
<span class="nctnt ncbi-var">eDS\_ToStderr</span> -- standard error stream

3  
<span class="nctnt ncbi-var">eDS\_ToMemory</span> -- the application memory

4  
<span class="nctnt ncbi-var">eDS\_Disable</span> -- nowhere

5  
<span class="nctnt ncbi-var">eDS\_User</span> -- wherever it went before the <span class="nctnt ncbi-func">AppMain()</span> call

6  
<span class="nctnt ncbi-var">eDS\_ToSyslog</span> -- system log daemon

3  
After the configuration file is loaded, and if it has an alternative location for the log files, then switch to logging to that location. See the list of logfile-related [configuration parameters](ch_libconfig.html#ch_libconfig.libconfig_logfile).

The boolean <span class="nctnt ncbi-monospace">TryRootLogFirst</span> argument in the <span class="nctnt ncbi-monospace">[LOG]</span> section of the application's config file changes the order of locations to be tested. If <span class="nctnt ncbi-monospace">TryRootLogFirst</span> is set, the application will try to open the log file under <span class="nctnt ncbi-path">/log</span> first. Only if this fails, then the location specified in the config file will be used.

<span class="nctnt highlight">Note:</span>

-   If the logging destination is switched, then a message containing both the old and new locations is logged to both locations.

-   Before the application configuration is loaded, a copy of all diagnostics is saved in memory. If the log destination is changed by the application configuration, then the saved diagnostics are dumped to the final log destination.

### Setting Diagnostic Severity Levels

Each diagnostic message has its own severity level ([EDiagSev](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EDiagSev)), which is compared to a global severity threshold to determine whether or not its message should be posted. Six levels of severity are defined by the <span class="nctnt ncbi-type">EDiagSev</span> enumeration:

    /// Severity level for the posted diagnostics.
    enum EDiagSev {
        eDiag_Info = 0, ///< Informational message
        eDiag_Warning,  ///< Warning message
        eDiag_Error,    ///< Error message
        eDiag_Critical, ///< Critical error message
        eDiag_Fatal,    ///< Fatal error -- guarantees exit(or abort)
        eDiag_Trace,    ///< Trace message
        // Limits
        eDiagSevMin = eDiag_Info,  ///< Verbosity level for min. severity
        eDiagSevMax = eDiag_Trace  ///< Verbosity level for max. severity
    };

The default is to post only those messages whose severity level exceeds the <span class="nctnt ncbi-var">eDiag\_Warning</span> level (i.e. <span class="nctnt ncbi-var">eDiag\_Error, eDiag\_Critical</span>, and <span class="nctnt ncbi-var">eDiag\_Fatal</span>). The global severity threshold for posting messages can be reset using [SetDiagPostLevel](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagPostLevel)<span class="nctnt ncbi-func"> (EDiagSev postSev)</span>. A parallel function, [SetDiagDieLevel](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagDieLevel)<span class="nctnt ncbi-func"> (EDiagSev dieSev)</span>, defines the severity level at which execution will abort.

Tracing is considered to be a special, debug-oriented feature, and therefore messages with severity level <span class="nctnt ncbi-var">eDiag\_Trace</span> are not affected by these global <span class="nctnt ncbi-monospace">post/die</span> levels. Instead, [SetDiagTrace](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagTrace)<span class="nctnt ncbi-func"> (EDiagTrace enable, EDiagTrace default)</span> is used to turn tracing on or off. By default, the tracing is off - unless you assign the environment variable <span class="nctnt ncbi-var">DIAG\_TRACE</span> to an arbitrary non-empty string or, alternatively, define a <span class="nctnt ncbi-var">DIAG\_TRACE</span> entry in the <span class="nctnt ncbi-var">[DEBUG]</span> section of your [registry](#registry) file.

The severity level can be set directly in <span class="nctnt ncbi-macro">POST</span> and <span class="nctnt ncbi-macro">TRACE</span> statements, using the severity level manipulators including <span class="nctnt ncbi-var">Info</span>, <span class="nctnt ncbi-var">Warning</span>, <span class="nctnt ncbi-var">Error</span>, <span class="nctnt ncbi-var">Critical</span>, <span class="nctnt ncbi-var">Fatal</span>, and <span class="nctnt ncbi-var">Trace</span>, for example:

    ERR_POST_X(1, Critical << "Something quite bad has happened.");

### Diagnostic Messages Filtering

Diagnostic messages from the <span class="nctnt ncbi-class">CNcbiDiag</span> and <span class="nctnt ncbi-class">CException</span> classes can be filtered by the source file path; message severity; or by the module, class, or function name. Messages from the <span class="nctnt ncbi-class">CNcbiDiag</span> class can also be filtered by error code. If a <span class="nctnt ncbi-class">CException</span> object is created by chaining to a previous exception, then all exceptions in the chain will be checked against the filter and the exception will pass if any exception in the chain passes (even if one of them is suppressed by a negative condition).

The filter can be set by the <span class="nctnt ncbi-var">TRACE\_FILTER</span> or <span class="nctnt ncbi-var">POST\_FILTER</span> entry in the <span class="nctnt ncbi-var">[DIAG]</span> section of the registry file or during runtime through [SetDiagFilter()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagFilter). Messages with a severity level of <span class="nctnt ncbi-var">Fatal</span> are not filtered; messages with a severity level of <span class="nctnt ncbi-var">Trace</span> are filtered by <span class="nctnt ncbi-var">TRACE\_FILTER</span>; and all other messages are filtered by <span class="nctnt ncbi-var">POST\_FILTER</span>.

Filter strings contain filtering conditions separated by a space. An empty filter string means that all messages will appear in the log unfiltered. Filtering conditions are processed from left to right until a condition that matches the message is found. If the message does not match any of the conditions, then the message will be filtered out. Filtering conditions in the string may be preceded by an exclamation mark, which reverses the behavior (so if a message matches the condition it will be suppressed). See [Table 4](#table-4) for filtering condition samples and syntax.

Table 4. Filter String Samples

<table>
<colgroup>
<col width="25%" />
<col width="25%" />
<col width="25%" />
<col width="25%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Filter</th>
<th align="left">Description</th>
<th align="left">Matches</th>
<th align="left">Non Matches</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><span class="nctnt ncbi-path">/corelib</span></td>
<td align="left">Log message from source file located in <span class="nctnt ncbi-path">src/corelib</span> or <span class="nctnt ncbi-path">include/corelib</span> or subdirectories.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-path">src/corelib/ncbidiag.cpp</span></p></li>
<li><p><span class="nctnt ncbi-path">src/corelib/test/test_ncbiexec.cpp</span></p></li>
<li><p><span class="nctnt ncbi-path">include/corelib/ncbidiag.hpp</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-path">src/cgi/cgiapp.cpp</span></p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><span class="nctnt ncbi-path">/corelib/test</span></td>
<td align="left">Log message from source file located in <span class="nctnt ncbi-path">src/corelib/test</span> or <span class="nctnt ncbi-path">include/corelib/test</span> or subdirectories.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-path">src/corelib/test/test_ncbiexec.cpp</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-path">src/corelib/ncbidiag.cpp</span></p></li>
<li><p><span class="nctnt ncbi-path">include/corelib/ncbidiag.hpp</span></p></li>
<li><p><span class="nctnt ncbi-path">src/cgi/cgiapp.cpp</span></p></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><span class="nctnt ncbi-path">/corelib/</span></td>
<td align="left">Log message from source file located in <span class="nctnt ncbi-path">src/corelib</span> or <span class="nctnt ncbi-path">include/corelib</span>, but not subdirectories.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-path">src/corelib/ncbidiag.cpp</span></p></li>
<li><p><span class="nctnt ncbi-path">include/corelib/ncbidiag.hpp</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-path">src/corelib/test/test_ncbiexec.cpp</span></p></li>
<li><p><span class="nctnt ncbi-path">src/cgi/cgiapp.cpp</span></p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><span class="nctnt ncbi-lib">corelib</span></td>
<td align="left">Log message with module name set to &quot;corelib&quot; and any class or function name.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span></td>
<td align="left">Log message with module name set to &quot;corelib&quot;, class name set to &quot;<span class="nctnt ncbi-class">CNcbiDiag</span>&quot; and any function name.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">::<span class="nctnt ncbi-class">CNcbiDiag</span></td>
<td align="left">Log message with class name set to &quot;<span class="nctnt ncbi-class">CNcbiDiag</span>&quot; and any module or function name.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span></p></li>
<li><p><span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">?</td>
<td align="left">Log message with module name not set and any class or function name.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left"><span class="nctnt ncbi-lib">corelib</span>::?</td>
<td align="left">Log message with module name set to &quot;corelib&quot;, class name not set and any function name.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left"><span class="nctnt ncbi-func">GetModule()</span></td>
<td align="left">Log message with function name set to &quot;<span class="nctnt ncbi-func">GetModule</span>&quot; and any class or module name.</td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span>::<span class="nctnt ncbi-func">GetModule()</span></p></li>
<li><p><span class="nctnt ncbi-func">GetModule()</span></p></li>
</ul></td>
<td align="left"><ul>
<li><p><span class="nctnt ncbi-lib">Corelib</span></p></li>
<li><p><span class="nctnt ncbi-lib">corelib</span>::<span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
<li><p><span class="nctnt ncbi-class">CNcbiDiag</span></p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">(20.11)</td>
<td align="left">Log messages with error code 20 and subcode 11.</td>
<td align="left"><ul>
<li><p>ErrCode(20,11)</p></li>
</ul></td>
<td align="left"><ul>
<li><p>ErrCode(20,10)</p></li>
<li><p>ErrCode(123,11)</p></li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">(20-80.)</td>
<td align="left">Log messages with error code from 20 to 80 and any subcode.</td>
<td align="left"><ul>
<li><p>ErrCode(20,11)</p></li>
<li><p>ErrCode(20,10)</p></li>
<li><p>ErrCode(51,1)</p></li>
</ul></td>
<td align="left"><ul>
<li><p>ErrCode(123,11)</p></li>
</ul></td>
</tr>
<tr class="even">
<td align="left">(20-80,120,311-400.1-50,60)</td>
<td align="left">Log messages with error code from 20 to 80, 120, from 311 to 400 and subcode from 1 to 50 and 60.</td>
<td align="left"><ul>
<li><p>ErrCode(20,11)</p></li>
<li><p>ErrCode(321,60)</p></li>
</ul></td>
<td align="left"><ul>
<li><p>ErrCode(20,51)</p></li>
<li><p>ErrCode(321,61)</p></li>
</ul></td>
</tr>
</tbody>
</table>

For example:

-   To log diagnostic messages from source files located in <span class="nctnt ncbi-path">src/corelib</span> with error codes from 101 to 106 and any subcode, use the following filter: “<span class="nctnt ncbi-monospace">/corelib (101-106.)</span>”.

-   To exclude log messages from sources in <span class="nctnt ncbi-path">src/serial</span> and <span class="nctnt ncbi-path">src/dbapi</span>, use this filter: “<span class="nctnt ncbi-monospace">!/serial !/dbapi</span>”.

-   To log messages from sources in <span class="nctnt ncbi-path">src/serial</span> excluding those with error code 802 and subcodes 4 and 10 through 12, and to exclude messages from sources in <span class="nctnt ncbi-path">src/dbapi/driver</span>, use the following filter: “<span class="nctnt ncbi-monospace">/serial !(802.4,10-12) !/dbapi/driver</span>”.

### Log File Format

The format of the log file can be customized. One of the most basic choices is between the "[old post format](#old-post-format)" and the "[new post format](#new-post-format)". The old format essentially posts arbitrary strings whereas the new format adds many standard fields, and structures the messages so they can be automatically indexed for rapid searching and/or error statistics.

The old format is used by default. To use the new format:

    int main(int argc, const char* argv[])
    {
        GetDiagContext().SetOldPostFormat(false); // use the new format

        return CMyApp().AppMain(argc, argv);
    }

This function should be called before the application's constructor for the setting to be used from the very beginning.

See also:

-   the [Diagnostic Trace](ch_libconfig.html#ch_libconfig.libconfig_diag) section in the library configuration chapter for details on selecting the format using the environment or registry; and

-   the [ERR\_POST and LOG\_POST Macros](#errpost-and-logpost-macros) section for more details on creating the log messages.

<span class="nctnt highlight">Note:</span> The old and new post formats described below apply to log messages generated by programs using the C++ Toolkit diagnostics API. Log messages generated in other ways may have different formats.

#### The Old Post Format

The old format for log messages is simply a message - prefixed with the severity level if it is an error message:

    [<severity>: ]<Message>

#### The New Post Format

The new format for the application log and error postings is:

    <pid>/<tid>/<rid>/<state> <guid> <psn>/<tsn> <time> <host> <client> <session> <application> <event> <message>

Fields in the new post format:

| Field       | Description                                                                                           | Width                                         | Type or format                                                                                                                                                                                                   |
|-------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| pid         | Process ID                                                                                            | ≥ 5                                           | Uint8 (decimal)                                                                                                                                                                                                  |
| tid         | Thread ID                                                                                             | ≥ 3                                           | Uint8 (decimal)                                                                                                                                                                                                  |
| rid         | Request ID (e.g. iteration number for a CGI)                                                          | ≥ 4                                           | int (decimal)                                                                                                                                                                                                    |
| state       | Application state code                                                                                | 2                                             | string                                                                                                                                                                                                           |
| guid        | [Globally unique process ID](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=x_CreateUID) | 16                                            | Int8 (hexadecimal)                                                                                                                                                                                               |
| psn         | Serial number of the posting within the process                                                       | ≥ 4                                           | int (decimal)                                                                                                                                                                                                    |
| tsn         | Serial number of the posting within the thread                                                        | ≥ 4                                           | int (decimal)                                                                                                                                                                                                    |
| time        | Astronomical date and time at which the message was posted                                            | ≥ 23<br/>(often 26) | <span class="nctnt ncbi-monospace">YYYY-MM-DD**T**hh:mm:ss.sss[sss[sss]]</span><br/>While seconds typically have six digits after the decimal, there could be more or as few as three. |
| host        | Name of the host where the process runs                                                               | 15                                            | string (UNK\_HOST if unknown)                                                                                                                                                                                    |
| client      | Client IP address                                                                                     | 15                                            | valid IP address string (UNK\_CLIENT if unknown)                                                                                                                                                                 |
| session     | Session ID                                                                                            | ≥ 24                                          | string (UNK\_SESSION if unknown)                                                                                                                                                                                 |
| application | Name of the application (see note below)                                                              | varies                                        | string (UNK\_APP if unknown)                                                                                                                                                                                     |
| event       | What was happening to cause the post (e.g. app start)                                                 | 13                                            | string (see the [Events and Messages](#events-and-messages) section)                                                                                                                                             |
| message     | The logged message                                                                                    | varies                                        | string (see the [Events and Messages](#events-and-messages) section)                                                                                                                                             |

<span class="nctnt highlight">Note:</span> Regarding the width and padding of standard fields:

-   Minimum-width numeric fields are right-justified and zero-padded - for example, a pid of 123 will get printed as "00123" while a pid of 1234567 will get printed as "1234567".

-   Minimum-width text fields and fixed-width fields are left-justified and space-padded.

-   Most fields have a fixed or minimum width to improve readability by generally aligning fields in adjacent rows.

The application name is set to the executable name (without path and extension) by default. Sometimes however the executable's name can be too generic (like "summary" or "fetch"). To change it use <span class="nctnt ncbi-func">CNcbiApplication::SetProgramDisplayName()</span> function. Better yet, just rename the executable itself. It's a good practice to prefix the application names with something project-specific (like "pc\_summary" for PubChem or "efetch" for E-Utils).

For more details, see:

-   [Application States](#application-states)

-   [Events and Messages](#events-and-messages)

-   [Examples](#examples)

##### Application States

Application state codes:

| Application State Code                                                                         | Meaning                                     |
|------------------------------------------------------------------------------------------------|---------------------------------------------|
| <span class="nctnt ncbi-monospace">PB</span> (or <span class="nctnt ncbi-monospace">AB</span>) | program is starting                         |
| <span class="nctnt ncbi-monospace">P</span> (or <span class="nctnt ncbi-monospace">A</span>)   | program is running (outside of any request) |
| <span class="nctnt ncbi-monospace">PE</span> (or <span class="nctnt ncbi-monospace">AE</span>) | program is exiting                          |
| <span class="nctnt ncbi-monospace">RB</span>                                                   | request is starting                         |
| <span class="nctnt ncbi-monospace">R</span>                                                    | request is being processed                  |
| <span class="nctnt ncbi-monospace">RE</span>                                                   | request is exiting                          |

<span class="nctnt highlight">Note:</span> The "A" and "P" codes are essentially synonymous. The "P" codes are generated by newer programs, but the "A" codes may still be present in some data.

The normal state transitions are:

<span>[![Image ch\_core\_log\_fmt\_app\_states.png](/book-test/static/img/ch_core_log_fmt_app_states.png)](/book-test/static/img/ch_core_log_fmt_app_states.png "Click to see the full-resolution image")</span>

##### Events and Messages

The following sections describe the events and messages seen in the log files:

-   [The application starts](#the-application-starts)

-   [The application stops](#the-application-stops)

-   [A request starts](#a-request-starts)

-   [The application posts extra information (within the context of a request)](#the-application-posts-extra-information-within-the-context-of-a-request)

-   [A request stops](#a-request-stops)

-   [The application posts a diagnostic message](#the-application-posts-a-diagnostic-message)

-   [The application posts performance logging information](#the-application-posts-performance-logging-information)

##### Event: The application starts

The <span class="nctnt ncbi-monospace">\<event\> \<message\></span> portion of the log output will contain:

    start

(The message field is empty for the <span class="nctnt ncbi-monospace">start</span> event.)

##### Event: The application stops

The <span class="nctnt ncbi-monospace">\<event\> \<message\></span> portion of the log output will contain:

    stop <exit_code> <timespan> [SIG=<exit_signal>]

Application stop event - message sub-fields:

| Sub-field                                              | Description                                          |
|--------------------------------------------------------|------------------------------------------------------|
| <span class="nctnt ncbi-monospace">exit\_code</span>   | Application exit code (zero if not set)              |
| <span class="nctnt ncbi-monospace">timespan</span>     | Application execution time                           |
| <span class="nctnt ncbi-monospace">exit\_signal</span> | Signal number, if application exited due to a signal |

For example:

    stop            0 0.149036509

##### Event: A request starts

The <span class="nctnt ncbi-monospace">\<event\> \<message\></span> portion of the log output will contain:

    request-start [application_defined_data]

The message field for the <span class="nctnt ncbi-monospace">request-start</span> event optionally contains application-specific arbitrary data, for example:

    request-start _type=conn

<span class="nctnt highlight">Note:</span> Make your log data more parsable!

Although the <span class="nctnt ncbi-monospace">request-start</span> data may be arbitrary, it should be URL-encoded. In many cases the logs are collected and stored in the database for analysis. The NCBI log system now parses and indexes the application-supplied data in the <span class="nctnt ncbi-monospace">request-start</span> and <span class="nctnt ncbi-monospace">extra</span> log lines, provided that the data is URL-encoded.

##### Event: The application posts extra information (within the context of a request)

The <span class="nctnt ncbi-monospace">\<event\> \<message\></span> portion of the log output will contain:

    extra <application_defined_data>

The message field for the <span class="nctnt ncbi-monospace">extra</span> event has the same format as the message field for the <span class="nctnt ncbi-monospace">request-start</span> event.

##### Event: A request stops

The <span class="nctnt ncbi-monospace">\<event\> \<message\></span> portion of the log output will contain:

    request-stop <status> <request_timespan> [bytes_read] [bytes_written]

The message sub-fields for <span class="nctnt ncbi-monospace">request-stop</span> events are:

Request stop event - message sub-fields:

| Sub-field                                                | Description                                                                  |
|----------------------------------------------------------|------------------------------------------------------------------------------|
| <span class="nctnt ncbi-monospace">status</span>         | [Exit status of the request](#exit-status-of-the-request) (zero if not set)  |
| <span class="nctnt ncbi-monospace">timespan</span>       | Request execution time (zero if not set)                                     |
| <span class="nctnt ncbi-monospace">bytes\_read</span>    | Input data read during the request execution, in bytes (zero if not set)     |
| <span class="nctnt ncbi-monospace">bytes\_written</span> | Output data written during the request execution, in bytes (zero if not set) |

For example:

    request-stop  200 0.105005566

##### Event: The application posts a diagnostic message

The <span class="nctnt ncbi-monospace">\<event\> \<message\></span> portion of the log output will contain:

    <severity>: <module>(<err_code>.<err_subcode> \| <err_text>) "<file>", line <line>: <class>::<func> --- <prefixes> <user_message> <err_code_message> <err_code_explanation>

Thus, the <span class="nctnt ncbi-monospace">\<event\></span> field is really just the diagnostic message severity, and the <span class="nctnt ncbi-monospace">\<message\></span> field is composed of a number of sub-fields.

Diagnostic message event / severity field - message sub-fields:

| Field or sub-field                                                                                          | Description                                                                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-monospace">event</span> / <span class="nctnt ncbi-monospace">severity</span>        | Diagnostic message severity = { Trace \\| Info \\| Warning \\| Error \\| Critical \\| Fatal \\| Message[T\\|I\\|W\\|E\\|C\\|F] } - left-justified and space-padded to 10 characters |
| <span class="nctnt ncbi-monospace">module</span>                                                            | Module where the post originates from (in most cases the module corresponds to a single library)                                                                                    |
| <span class="nctnt ncbi-monospace">err\_code</span>, <span class="nctnt ncbi-monospace">err\_subcode</span> | Numeric error code and subcode                                                                                                                                                      |
| <span class="nctnt ncbi-monospace">err\_text</span>                                                         | If the error has no numeric code, sometimes it can be represented as text                                                                                                           |
| <span class="nctnt ncbi-monospace">file</span>, <span class="nctnt ncbi-monospace">line</span>              | File name and line number where the posting occured                                                                                                                                 |
| <span class="nctnt ncbi-monospace">class</span>, <span class="nctnt ncbi-monospace">func</span>             | Class and/or function name where the posting occured: {Class:: \\| Class::Function() \\| ::Function()}                                                                              |
| <span class="nctnt ncbi-monospace">prefixes</span>                                                          | User-defined prefixes for the message                                                                                                                                               |
| <span class="nctnt ncbi-monospace">user\_message</span>                                                     | The message itself                                                                                                                                                                  |
| <span class="nctnt ncbi-monospace">err\_code\_message</span>                                                | Short error code description                                                                                                                                                        |
| <span class="nctnt ncbi-monospace">err\_code\_explanation</span>                                            | Detailed explanation of the error code                                                                                                                                              |

##### Event: The application posts performance logging information

The <span class="nctnt ncbi-monospace">\<event\> \<message\></span> portion of the log output will contain:

    perf <exit_code> <timespan> <performance_parameters>

The message sub-fields for <span class="nctnt ncbi-monospace">perf</span> events are:

Performance logging event - message sub-fields:

| Sub-field                                                         | Description                                                                                                                                                                   |
|-------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-monospace">exit\_code</span>              | Application exit code (zero if not set)                                                                                                                                       |
| <span class="nctnt ncbi-monospace">timespan</span>                | Application execution time                                                                                                                                                    |
| <span class="nctnt ncbi-monospace">performance\_parameters</span> | URL-encoded name=value pairs -- the resource name given to the logger, the status message (if given), and any others from <span class="nctnt ncbi-func">AddParameter()</span> |

For example:

    perf            200 0.000246 resource=task+one&status_msg=task+one+finished

##### Examples

An example of application events:

<span>[![Image ch\_core\_log\_fmt\_event.png](/book-test/static/img/ch_core_log_fmt_event.png)](/book-test/static/img/ch_core_log_fmt_event.png "Click to see the full-resolution image")</span>

(Click to see the full-resolution image.)

An example of diagnostic messages:

<span>[![Image ch\_core\_log\_fmt\_diagnostic.png](/book-test/static/img/ch_core_log_fmt_diagnostic.png)](/book-test/static/img/ch_core_log_fmt_diagnostic.png "Click to see the full-resolution image")</span>

(Click to see the full-resolution image.)

#### Controlling the Appearance of Diagnostic Messages using Post Flags

The post flags define additional information that will be inserted into the output messages and appear along with the message body. The standard format of a message is:

    "<file>", line <line>: <severity>: (<err_code>.<err_subcode>) [<prefix1>::<prefix2>::<prefixN>] <message>\n
    <err_code_message>\n
    <err_code_explanation>

where the presence of each field in the output is controlled by the post flags [EDiagPostFlag](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EDiagPostFlag) associated with the particular diagnostic message. The post flags are:

    enum EDiagPostFlag {
        eDPF_File               = 0x1, ///< Set by default #if _DEBUG; else not set
        eDPF_LongFilename       = 0x2, ///< Set by default #if _DEBUG; else not set
        eDPF_Line               = 0x4, ///< Set by default #if _DEBUG; else not set
        eDPF_Prefix             = 0x8, ///< Set by default (always)
        eDPF_Severity           = 0x10,  ///< Set by default (always)
        eDPF_ErrorID            = 0x20,  ///< Module, error code and subcode
        eDPF_DateTime           = 0x80,  ///< Include date and time
        eDPF_ErrCodeMessage     = 0x100, ///< Set by default (always)
        eDPF_ErrCodeExplanation = 0x200, ///< Set by default (always)
        eDPF_ErrCodeUseSeverity = 0x400, ///< Set by default (always)
        eDPF_Location           = 0x800, ///< Include class and function
                                         ///< if any, not set by default
        eDPF_PID                = 0x1000,  ///< Process ID
        eDPF_TID                = 0x2000,  ///< Thread ID
        eDPF_SerialNo           = 0x4000,  ///< Serial # of the post, process-wide
        eDPF_SerialNo_Thread    = 0x8000,  ///< Serial # of the post, in the thread
        eDPF_RequestId          = 0x10000, ///< fcgi iteration number or request ID
        eDPF_Iteration          = 0x10000, ///< @deprecated
        eDPF_UID                = 0x20000, ///< UID of the log

        eDPF_ErrCode            = eDPF_ErrorID,  ///< @deprecated
        eDPF_ErrSubCode         = eDPF_ErrorID,  ///< @deprecated
        /// All flags (except for the "unusual" ones!)
        eDPF_All                = 0xFFFFF,

        /// Default flags to use when tracing.
    #if defined(NCBI_THREADS)
        eDPF_Trace              = 0xF81F,
    #else
        eDPF_Trace              = 0x581F,
    #endif

        /// Print the posted message only; without severity, location, prefix, etc.
        eDPF_Log                = 0x0,

        // "Unusual" flags -- not included in "eDPF_All"
        eDPF_PreMergeLines      = 0x100000, ///< Remove EOLs before calling handler
        eDPF_MergeLines         = 0x200000, ///< Ask diag.handlers to remove EOLs
        eDPF_OmitInfoSev        = 0x400000, ///< No sev. indication if eDiag_Info
        eDPF_OmitSeparator      = 0x800000, ///< No '---' separator before message

        eDPF_AppLog             = 0x1000000, ///< Post message to application log
        eDPF_IsMessage          = 0x2000000, ///< Print "Message" severity name.

        /// Hint for the current handler to make message output as atomic as
        /// possible (e.g. for stream and file handlers).
        eDPF_AtomicWrite        = 0x4000000,

        /// Use global default flags (merge with).
        /// @sa SetDiagPostFlag(), UnsetDiagPostFlag(), IsSetDiagPostFlag()
        eDPF_Default            = 0x10000000,

        /// Important bits which should be taken from the globally set flags
        /// even if a user attempts to override (or forgets to set) them
        /// when calling CNcbiDiag().
        eDPF_ImportantFlagsMask = eDPF_PreMergeLines \|
                                  eDPF_MergeLines \|
                                  eDPF_OmitInfoSev \|
                                  eDPF_OmitSeparator \|
                                  eDPF_AtomicWrite,

        /// Use flags provided by user as-is, do not allow CNcbiDiag to replace
        /// "important" flags by the globally set ones.
        eDPF_UseExactUserFlags  = 0x20000000
    };

The default message format displays only the severity level and the message body. This can be overridden inside the constructor for a specific message, or globally, using [SetDiagPostFlag()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagPostFlag) on a selected flag. For example:

        SetDiagPostFlag(eDPF_DateTime); // set flag globally

### Defining the Output Stream

The logging framework uses a global output stream. The default is to post messages to <span class="nctnt ncbi-var">CERR</span> ouput stream, but the stream destination can be reset at any time using:

    SetDiagStream(CNcbiOstream* os, bool quick_flush,
                  FDiagCleanup cleanup, void* cleanup_data)

This function can be called numerous times, thus allowing different sections of the executable to write to different files. At any given time however, all messages will be associated with the same global output stream. Because the messages are completely buffered, each message will appear on whatever stream is active at the time the message actually completes.

And, of course, you can [provide](#provide) (using [SetDiagHandler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagHandler)) your own message posting handler [CDiagHandler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDiagHandler.html), which does not necessarily write the messages to a standard C++ output stream. To preserve compatibility with old code, SetDiagHandler also continues to accept raw callback functions of type [FDiagHandler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=FDiagHandler).

If the output is sent to a file, you can split it into separate files:

-   Application log - standard events (<span class="nctnt ncbi-monospace">start</span>, <span class="nctnt ncbi-monospace">stop</span>, <span class="nctnt ncbi-monospace">request-start</span>, <span class="nctnt ncbi-monospace">request-stop</span> and user defined <span class="nctnt ncbi-monospace">extra</span> events).

-   Error log - all messages with severity <span class="nctnt ncbi-var">Warning</span> and above.

-   Trace log - messages having severity <span class="nctnt ncbi-var">Info</span> and <span class="nctnt ncbi-var">Trace</span> messages.

-   Performance log - messages from [performance logging](#performance-logging).

All log files have the same name but different extensions: <span class="nctnt ncbi-path">.log</span>, <span class="nctnt ncbi-path">.err</span>, <span class="nctnt ncbi-path">.trace</span>, and <span class="nctnt ncbi-path">.perf</span>.

To turn on the log file splitting, call (before the log file initialization):

    int main(int argc, const char* argv[])
    {
        SetSplitLogFile(true);

        return CMyApp().AppMain(argc, argv);
    }

This function should be called before the application's constructor for the setting to be used from the very beginning.

### Tee Output to STDERR

Sometimes it is helpful to generate human-readable diagnostics on the console in addition to storing detailed diagnostics in the machine-parsable log files. In these cases, it is likely that both the message severity required to trigger output and the output format should be different for the log file and the console. For example:

| Destination | Severity | Format                         |
|-------------|----------|--------------------------------|
| Log File    | Error    | [new](#new) (machine-parsable) |
| Console     | Warning  | [old](#old) (human-readable)   |

To set up this sort of tee, set these configuration parameters (see the [library configuration chapter](ch_libconfig.html#ch_libconfig.libconfig_diag) for details):

| Configuration Parameter                                      | Example Value | Notes                                                            |
|--------------------------------------------------------------|---------------|------------------------------------------------------------------|
| <span class="nctnt ncbi-var">DIAG\_TEE\_TO\_STDERR</span>    | True          | This turns on the tee.                                           |
| <span class="nctnt ncbi-var">DIAG\_OLD\_POST\_FORMAT</span>  | False         | This makes the log file use the new format.                      |
| <span class="nctnt ncbi-var">DIAG\_POST\_LEVEL</span>        | Error         | This sets the minimum severity required to post to the log file. |
| <span class="nctnt ncbi-var">DIAG\_TEE\_MIN\_SEVERITY</span> | Warning       | This sets the minimum severity required to post to the console.  |

Alternatively, you can use the <span class="nctnt ncbi-func">Console</span> manipulator to indicate that output should go to the console (in human-readable format):

    ERR_POST_X(1, Console << "My ERR_POST message.");

<span class="nctnt highlight">Note:</span> Output sent to the console using this manipulator will also go to the log file if the message severity at least meets the severity threshold for the log file. The effect of the manipulator lasts until the next flush, which typically occurs after each post.

### The Message Buffer

Diagnostic messages (i.e. instances of the <span class="nctnt ncbi-class">CNcbiDiag</span> class) have a buffer that is initialized when the message is first instantiated. Additional information can then be appended to the message using the overloaded stream operator <span class="nctnt ncbi-code">\<\<</span>. Messages can then be terminated explicitly using <span class="nctnt ncbi-class">CNcbiDiag</span>'s stream manipulator <span class="nctnt ncbi-class">Endm</span>, or implicitly, when the <span class="nctnt ncbi-class">CNcbiDiag</span> object exits scope.

Implicit message termination also occurs as a side effect of applying one of the [severity level manipulators](#severity-level-manipulators). Whenever the severity level is changed, <span class="nctnt ncbi-class">CNcbiDiag</span> also automatically executes the following two <span class="nctnt ncbi-monospace">manipulators</span>:

-   <span class="nctnt ncbi-class">Endm</span> -- the message is complete and the message buffer will be flushed

-   <span class="nctnt ncbi-class">Reset</span> -- empty the contents of the current message buffer

When the message controlled by an instance of <span class="nctnt ncbi-class">CNcbiDiag</span> is complete, <span class="nctnt ncbi-class">CNcbiDiag</span> calls a global callback function (of type [FDiagHandler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=FDiagHandler)) and passes the message (along with its severity level) as the function arguments. The default callback function posts errors to the currently designated output stream, with the action (continue or abort) determined by the severity level of the message.

### Request Exit Status Codes

This section describes the possible values of the request exit codes used in NCBI. They appear in the application access log as:

    request-stop <status> .....

Request exit status codes are either [standard](#standard) or [NCBI-specific](#ncbi-specific).

#### Standard (HTTP-like) status codes

The NCBI request exit codes must conform to the HTTP status codes:

<http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html>

#### NCBI-specific status codes

If the situation cannot be described using one of the [standard (HTTP) status codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html), then an NCBI specific code should be used.

The NCBI-specific status codes must be different from the [standard (HTTP) status codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html). At the same time these codes better follow at least the range requirements of the [standard (HTTP) status codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html), that is they better belong to one of the following ranges:

| Range     | Description                        |
|-----------|------------------------------------|
| 120 – 199 | Informational/provisional response |
| 220 – 299 | Success                            |
| 320 – 399 | Redirection                        |
| 420 – 499 | Bad request (client error)         |
| 520 – 599 | Server Error                       |

So far we have the following NCBI specific status codes:

| Value        | Description                                                                                                              |
|--------------|--------------------------------------------------------------------------------------------------------------------------|
| 0            | Unknown error                                                                                                            |
| 555          | NCBI Network Dispatcher refused a request from and outside user which is in its "abusers list"                           |
| 1000 + errno | Unclassifiable server error when only errno is known (NOTE: the value of errno can be different on different platforms!) |

### Error codes and their Descriptions

Error codes and subcodes are posted to an output stream only if applicable [post flags](#post-flags) were set. In addition to error codes, the logging framework can also post text explanations. The [CDiagErrCodeInfo](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDiagErrCodeInfo.html) class is used to find the error message that corresponds to a given error code/subcode. Such descriptions could be specified directly in the program code or placed in a separate message file. It is even possible to use several such files simultaneously. <span class="nctnt ncbi-class">CDiagErrCodeInfo</span> can also read error descriptions from any input stream(s), not necessarily files.

#### Preparing an Error Message File

The error message file contains plain ASCII text data. We would suggest using the <span class="nctnt ncbi-path">.msg</span> extension, but this is not mandatory. For example, the message file for an application named <span class="nctnt ncbi-app">SomeApp</span> might be called <span class="nctnt ncbi-path">SomeApp.msg</span>.

The message file must contain a line with the keyword <span class="nctnt ncbi-monospace">MODULE</span> in it, followed by the name of the module (in our example <span class="nctnt ncbi-monospace">SomeApp</span>). This line must be placed in the beginning of the file, before any other declarations. Lines with symbol <span class="nctnt ncbi-code">\#</span> in the first position are treated as comments and ignored.

Here is an example of the message file:

    # This is a message file for application "SomeApp"
    MODULE SomeApp
    # ------ Code 1 ------
    $$ NoMemory, 1, Fatal : Memory allocation error
    # ------ Code 2 ------
    $$ File, 2, Critical : File error
    $^ Open, 1 : Error open a specified file
    This often indicates that the file simply does not exist.
    Or, it may exist but you do not have permission to access
    the file in the requested mode.
    $^ Read, 2, Error : Error read file
    Not sure what would cause this...
    $^ Write, 3, Critical
    This may indicate that the filesystem is full.
    # ------ Code 3 ------
    $$ Math, 3
    $^ Param, 20
    $^ Range, 3

Lines beginning with <span class="nctnt ncbi-code">$$</span> define a top-level error code. Similarly, lines beginning with <span class="nctnt ncbi-code">$^</span> define subcodes of the top-level error code. In the above example <span class="nctnt ncbi-monospace">Open</span> is a subcode of <span class="nctnt ncbi-monospace">File</span> top-level error, which means the error with code 2 and subcode 1.

Both types of lines have similar structure:

    $$/$^ <mnemonic_name>, <code> [, <severity> ] [: <message> ] \n
    [ <explanation> ]

where

-   <span class="nctnt ncbi-var">mnemonic\_name</span> (*required*) Internal name of the error code/subcode. This is used as a part of an error name in a program code - so, it should also be a correct C/C++ identifier.

-   <span class="nctnt ncbi-var">code</span> (*required*) Integer identifier of the error.

-   <span class="nctnt ncbi-var">severity</span> (*optional*) This may be supplied to specify the severity level of the error. It may be specified as a severity level string (valid values are <span class="nctnt ncbi-monospace">Info, Warning, Error, Critical, Fatal, Trace</span>) or as an integer in the range from 0 (<span class="nctnt ncbi-var">Info</span>) to 5 (<span class="nctnt ncbi-var">Trace</span>). While integer values are acceptable, string values are more readable. If the severity level was not specified or could not be recognized, it is ignored, or inherited from a higher level (the severity of a subcode becomes the same as the severity of a top-level error code, which contains this subcode). As long as diagnostic <span class="nctnt ncbi-var">eDPF\_ErrCodeUseSeverity</span> flag is set, the severity level specified in the message file overrides the one specified in a program, which allows for runtime customization. In the above example, <span class="nctnt ncbi-monospace">Critical</span> severity level will be used for all <span class="nctnt ncbi-monospace">File</span> errors, except <span class="nctnt ncbi-monospace">Read</span> subcode, which would have <span class="nctnt ncbi-monospace">Error</span> severity level.

-   <span class="nctnt ncbi-var">message</span> (*optional*) Short description of the error. It must be a single-line message. As long as diagnostic <span class="nctnt ncbi-var">eDPF\_ErrCodeMessage</span> flag is set, this message is posted as a part of the diagnostic output.

-   <span class="nctnt ncbi-var">explanation</span> (*optional*) Following a top-level error code or a subcode definition string, it may be one or several lines of an explanation text. Its purpose is to provide additional information, which could be more detailed description of the error, or possible reasons of the problem. This text is posted in a diagnostic channel only if <span class="nctnt ncbi-var">eDPF\_ErrCodeExplanaton</span> flag was set.

Error message files can be automatically read by setting a configuration parameter. You can either define the <span class="nctnt ncbi-monospace">MessageFile</span> entry in the <span class="nctnt ncbi-monospace">DEBUG</span> section of the application registry, or set the environment variable <span class="nctnt ncbi-var">NCBI\_CONFIG\_\_DEBUG\_\_MessageFile</span> (note the double-underscores and character case).

### Defining Custom Handlers using CDiagHandler

The user can install his own handler (of type [CDiagHandler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDiagHandler.html),) using [SetDiagHandler()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagHandler). CDiagHandler is a simple abstract class:

    class  CDiagHandler
    {
    public:
        /// Destructor.
        virtual ~CDiagHandler(void) {}
        /// Post message to handler.
        virtual void Post(const SDiagMessage& mess) = 0;
    };

where [SDiagMessage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SDiagMessage) is a simple struct defined in <span class="nctnt ncbi-path">ncbidiag.hpp</span> whose data members' values are obtained from the <span class="nctnt ncbi-class">CNcbiDiag</span> object. The transfer of data values occurs at the time that <span class="nctnt ncbi-func">Post</span> is invoked. See also the section on [Message posting](ch_debug.html#ch_debug.std_cpp_message_post) for a more technical discussion.

### The ERR\_POST and LOG\_POST Macros

A family of <span class="nctnt ncbi-macro">ERR\_POST\*</span> macros and a corresponding family of <span class="nctnt ncbi-macro">LOG\_POST\*</span> macros are available for routine message posting.

The log entries produced by the two families are almost identical for the [new post format](#new-post-format) - the <span class="nctnt ncbi-macro">ERR\_POST\*</span> entries contain a full word for the severity (e.g. "<span class="nctnt ncbi-monospace">Error</span>") while the <span class="nctnt ncbi-macro">LOG\_POST\*</span> entries contain the word "<span class="nctnt ncbi-monospace">Message</span>" and a one-character severity code (e.g. "<span class="nctnt ncbi-monospace">Message[E]</span>"). For the [old post format](#old-post-format), <span class="nctnt ncbi-macro">LOG\_POST\*</span> macros simply contain the message, while <span class="nctnt ncbi-macro">ERR\_POST\*</span> entries contain the severity, error code, and message. [Message filtering](#message-filtering) works exactly the same way for the two families of macros.

The macros are:

-   <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST(msg)</span> – for posting a simple message. <span class="nctnt highlight">Note:</span> these macros are deprecated. Use <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST\_X</span> instead (except for tests) for more flexible error statistics and logging.

-   <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST\_X(subcode, msg)</span> – for posting a default error code, a given subcode, and a message. Each call to <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST\_X</span> must use a different subcode for proper error statistics and logging. The default error code is selected by <span class="nctnt ncbi-macro">NCBI\_USE\_ERRCODE\_X</span>. The error code is selected from those defined by <span class="nctnt ncbi-macro">NCBI\_DEFINE\_ERRCODE\_X</span> in the appropriate header file, e.g. <span class="nctnt ncbi-path">include/corelib/error\_codes.h</span>.

-   <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST\_EX(code, subcode, msg)</span> – for posting a given error code, a given error subcode, and a message. This macro should only be used if you have to use a variable for the subcode, or to specify an error code other than the current default. In all other cases (except for tests), use <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST\_X</span> for more flexible error statistics and logging.

-   <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST\_XX(code, subcode, msg)</span> – these macros must be used in place of <span class="nctnt ncbi-macro">{ERR\\|LOG}\_POST\_X</span> within header files so that the same error code will be used for header-defined code, regardless of the error codes that including files may use.

The <span class="nctnt ncbi-macro">LOG\_POST\_\*</span> macros just write a string to the log file, and are useful if a human-readable log file is desired. The output from the <span class="nctnt ncbi-macro">ERR\_POST\_\*</span> macros is not easily read by humans, but facilitates automatic indexing for searching and/or error statistics. There are multiple flags to [control the appearance of the message](#control-the-appearance-of-the-message) generated by the <span class="nctnt ncbi-macro">ERR\_POST\_\*</span> macros.

The <span class="nctnt ncbi-macro">LOG\_POST\_\*</span> and <span class="nctnt ncbi-macro">ERR\_POST\_\*</span> macros implicitly create a temporary <span class="nctnt ncbi-class">CNcbiDiag</span> object and put the passed "message" into it with a default severity of <span class="nctnt ncbi-var">Error</span>. A [severity level manipulator](#severity-level-manipulator) can be applied if desired, to modify the message's severity level. For example:

    long lll = 345;
    ERR_POST_X(1, "My ERR_POST message, print long: " << lll);

would write to the diagnostic stream something like:

    Error: (1501.1) My ERR_POST message, print long: 345

while:

    double ddd = 123.345;
    ERR_POST_X(1, Warning << "My ERR_POST message, print double: " << ddd);

would write to the diagnostic stream something like:

    Warning: (1501.1) My ERR_POST message, print double: 123.345

See the [Log File Format](#log-file-format) section for more information on controlling the format of diagnostics messages.

<span class="nctnt highlight">Note:</span> Most of the above macros make use of the macro definition <span class="nctnt ncbi-macro">NCBI\_USE\_ERRCODE\_X</span>. This definition must be present in your source code, and must be defined in terms of an existing error code name. By convention, error code names are defined in header file named <span class="nctnt ncbi-path">error\_codes.hpp</span> in the relevant directory, for example <span class="nctnt ncbi-path">include/corelib/error\_codes.hpp</span>.

To set up new error codes, pick appropriate names and error code numbers that don't match existing values, and decide how many subcodes you'll need for each error code. For example, the following sets up three error codes to deal with different categories of errors within a library, and specifies the number of subcodes for each category:

    // Note: The following should be in src/app/my_prog/error_codes.hpp.
    ...
    BEGIN_NCBI_SCOPE
    ...
    NCBI_DEFINE_ERRCODE_X(MyLib_Cat1, 1501, 5);
    NCBI_DEFINE_ERRCODE_X(MyLib_Cat2, 1502, 6);
    NCBI_DEFINE_ERRCODE_X(MyLib_Cat3, 1503, 1);
    // where:
    //      MyLib_*   -- the error code names
    //      1501, etc -- the error code numbers, typically starting at N*100+1
    //      5, etc    -- how many subcodes you need for the given error code
    ...
    END_NCBI_SCOPE

Now you can use the error code in your library's implementation:

    // The following should be in your source files.
    ...
    // include the relevant error_codes header, for example:
    #include <include/corelib/error_codes.hpp>
    ...
    #define NCBI_USE_ERRCODE_X   MyLib_Cat1 // sets the default error code for this file
    ...
        ERR_POST_X(5, Critical << "Your message here."); // uses the default error code

Generally, the default error code and the <span class="nctnt ncbi-macro">ERR\_POST\_X</span> macro should be used. If it is necessary to use a non-default error code, that error code and the appropriate subcode may be used with the [ErrCode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ErrCode) manipulator in the <span class="nctnt ncbi-macro">ERR\_POST</span> macro. For example:

    // use a non-default error code (1501 in this example) and subcode 3
    ERR_POST(ErrCode(1501, 3) << "My error message.");

### The \_TRACE macro

The <span class="nctnt ncbi-macro">\_TRACE(message)</span> macro is a debugging tool that allows the user to insert trace statements that will only be posted if the code was [compiled in debug mode](ch_debug.html#ch_debug.debug_mode_internal), and provided that the tracing has been turned on. If <span class="nctnt ncbi-var">DIAG\_TRACE</span> is defined as an environment variable, or as an entry in the [DEBUG] section of your configuration file (<span class="nctnt ncbi-path">\*.ini</span>), the initial state of tracing is <span class="nctnt ncbi-monospace">on</span>. By default, if no such variable or registry entry is defined, tracing is <span class="nctnt ncbi-monospace">off</span>. [SetDiagTrace](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagTrace)<span class="nctnt ncbi-func"> (EDiagTrace enable, EDiagTrace default)</span> is used to turn tracing on/off.

Just like <span class="nctnt ncbi-macro">ERR\_POST</span>, the <span class="nctnt ncbi-macro">\_TRACE</span> macro takes a message, and the message will be posted only if tracing has been enabled. For example:

    SetDiagTrace(eDT_Disable);
    _TRACE("Testing the _TRACE macro");
    SetDiagTrace(eDT_Enable);
    _TRACE("Testing the _TRACE macro AGAIN");

Here, only the second trace message will be posted, as tracing is disabled when the first <span class="nctnt ncbi-macro">\_TRACE()</span> macro call is executed.

### Performance Logging

The C++ Toolkit includes a [performance logging API](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/perf_log.hpp) that is independent of the general diagnostics API. This allows independent control, analysis, and management of the performance data. Performance log files are created just like [other log files](#other-log-files), except that the extension is <span class="nctnt ncbi-path">.perf</span> instead of <span class="nctnt ncbi-path">.log</span>, for example. Performance data can be found in AppLog by searching for the "perf" event (see the [events and messages](#events-and-messages) section for more details about events).

The performance logging classes and macros are:

-   <span class="nctnt ncbi-class">CPerfLogGuard</span>

    -   The [CPerfLogGuard](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCPerfLogGuard.html) class will generally be the first choice for performance logging. If you want to use a <span class="nctnt ncbi-macro">PERF\_POST\*</span> macro, then use [CPerfLogger](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCPerfLogger.html) to create the logger object.

    -   <span class="nctnt ncbi-class">CPerfLogGuard</span> measures elapsed time and posts a one-line entry in the performance log.

    -   <span class="nctnt ncbi-class">CPerfLogGuard</span> should be used for measuring just one operation.

    -   Extra parameters can be added using <span class="nctnt ncbi-func">AddParameter()</span>.

    -   You can call <span class="nctnt ncbi-func">Start()</span> and <span class="nctnt ncbi-func">Suspend()</span> as many times as you want after creating the logger and before posting or discarding.

    -   End measurement with <span class="nctnt ncbi-func">Post()</span> or <span class="nctnt ncbi-func">Discard()</span>. If one of these isn't called before the logger is destroyed, the destructor will post a log entry with a status code of 500.

    -   <span class="nctnt ncbi-class">CPerfLogGuard</span> has built-in integrity checks to ensure that only one <span class="nctnt ncbi-func">Post()</span> or <span class="nctnt ncbi-func">Discard()</span> call is made, <span class="nctnt ncbi-func">Suspend()</span> isn't called when the time isn't running, etc.

-   <span class="nctnt ncbi-class">CPerfLogger</span>

    -   The [CPerfLogger](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCPerfLogger.html) class can be used on its own, but it's best to only use it if you need to create a logger for use in a <span class="nctnt ncbi-macro">PERF\_POST\*</span> macro. <span class="nctnt ncbi-class">CPerfLogger</span> is slightly lower-level than <span class="nctnt ncbi-class">CPerfLogGuard</span> but is otherwise very similar, except that <span class="nctnt ncbi-class">CPerfLogGuard</span> offers generally desirable guard features.

    -   <span class="nctnt highlight">Note:</span> If you use <span class="nctnt ncbi-class">CPerfLogger</span> on its own, and logging is off, then neither logging nor timing will be done. However, the extra record will be put into the log if the following construct is used:<br/><br/><span class="nctnt ncbi-code">perf\_logger.Post(...).Print(...)</span><br/><br/>Therefore, it's best to avoid that construct and use the <span class="nctnt ncbi-class">CPerfLogGuard</span> class or a <span class="nctnt ncbi-macro">PERF\_POST</span> macro instead.

-   <span class="nctnt ncbi-macro">PERF\_POST</span>

    -   Use the [PERF\_POST](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/group__Diagnostics.html#ga8da8da548df436e673c0274f9bcb6770) macro if you find it more convenient than <span class="nctnt ncbi-class">CPerfLogGuard</span>, or if you'd like to possibly save a few CPU cycles when performance logging is globally turned off.

-   <span class="nctnt ncbi-macro">PERF\_POST\_DB</span>

    -   Use the [PERF\_POST\_DB](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/group__Diagnostics.html#ga89e52c8d2496233dd47fdbb91980d9f8) macro for the same reasons as the <span class="nctnt ncbi-macro">PERF\_POST</span> macro, but specifically when working with a database.

Performance logging is turned off by default, but can be globally turned on using the environment variable <span class="nctnt ncbi-var">LOG\_PerfLogging</span> or the registry:

    [Log]
    PerfLogging = true

It can also be turned on or off at runtime by calling <span class="nctnt ncbi-func">CPerfLogger::SetON()</span>.

Here is a typical usage example:

    CPerfLogGuard perf("unique task description");
    // do something to be timed
    perf.Post(200, "finished");

This example shows nested logging:

    void some_func(void)
    {
        CPerfLogGuard perf_overall("instrument an entire function");

        CPerfLogGuard perf_init("init");
        // initialization code to be timed ...
        perf_init.Post(
            200,                // status code
            "init finished");   // status description string

        CPerfLogGuard perf_loop("loop");
        for (int i=0; i<10; ++i) {
            CPerfLogGuard perf_task1("task1");
            // sub-task 1 ...
            perf_task1.Post(200); // the status description string is optional

            if (true) {
                CPerfLogGuard perf_cond("conditional");
                perf_cond.AddParameter("iter", NStr::NumericToString(i));
                // conditional task ...
                perf_cond.Post(200, "conditional finished");
            }
        }
        perf_loop.Post(200, "loop finished");

        perf_overall.Post(200, "function finished"); 
    }

### Stack Traces

<span class="nctnt ncbi-class">CStackTrace</span> objects have special formatting: a "<span class="nctnt ncbi-monospace">Stack trace:</span>" line is added before the stack trace and standard indentation is used. This formatting is also used when printing the stack trace for exceptions.

Using stack traces with diagnostics is discussed in the following topics:

-   [Printing a Stack Trace](#printing-a-stack-trace)

-   [Obtaining a Stack Trace for Exceptions](#obtaining-a-stack-trace-for-exceptions)

#### Printing a Stack Trace

A stack trace can be saved simply by creating a CStackTrace object. Then the object can be posted in an error message, for example:

    ERR_POST_X(1, Error << "Your message here." << CStackTrace());

An example of a stack trace output on Linux:

    Error: (1501.1) Your message here.
         Stack trace:
          ./my_prog ???:0 ncbi::CStackTraceImpl::CStackTraceImpl() offset=0x5D
          ./my_prog ???:0 ncbi::CStackTrace::CStackTrace(std::string const&) offset=0x28
          ./my_prog ???:0 CMyProg::Run() offset=0xAF3
          ./my_prog ???:0 ncbi::CNcbiApplication::x_TryMain(ncbi::EAppMyProgStream, char const*, int*, bool*) offset=0x6C8
          ./my_prog ???:0 ncbi::CNcbiApplication::AppMain(int, char const* const*, char const* const*, ncbi::EAppMyProgStream, char const*, std::string const&) offset=0x11BA
          ./my_prog ???:0 main offset=0x60
          /lib64/tls/libc.so.6 ???:0 __libc_start_main offset=0xEA
          ./my_prog ???:0 std::__throw_logic_error(char const*) offset=0x62

#### Obtaining a Stack Trace for Exceptions

The stack trace can be saved by <span class="nctnt ncbi-class">CException</span> and derived classes automatically if the exception's severity is equal to or above the level set in the <span class="nctnt ncbi-var">EXCEPTION\_STACK\_TRACE\_LEVEL</span> environment variable or [configuration parameter](ch_libconfig.html#ch_libconfig.NCBI). The default level is <span class="nctnt ncbi-var">Critical</span>, so that most exceptions do not save the stack trace (the default exception's severity is <span class="nctnt ncbi-var">Error</span>).

When printing an exception, the diagnostics code checks if a stack trace is available and if so, automatically prints the stack trace along with the exception.

An example of an exception with a stack trace on Linux:

    Error: (106.16) Application's execution failed
    NCBI C++ Exception:
        Error: (CMyException::eMyErrorXyz) Your message here.
         Stack trace:
          ./my_prog ???:0 ncbi::CStackTraceImpl::CStackTraceImpl() offset=0x5D
          ./my_prog ???:0 ncbi::CStackTrace::CStackTrace(std::string const&) offset=0x28
          ./my_prog ???:0 ncbi::CException::x_GetStackTrace() offset=0x86
          ./my_prog ???:0 ncbi::CException::x_Init(ncbi::CTestCompileInfo const&, std::string const&, ncbi::CException const*, ncbi::ETestSev) offset=0xE9
          ./my_prog ???:0 ncbi::CException::CException(ncbi::CTestCompileInfo const&, ncbi::CException const*, ncbi::CException::EErrCode, std::string const&, ncbi::ETestSev) offset=0x119
          ./my_prog ???:0 CMyException::CMyException(ncbi::CTestCompileInfo const&, ncbi::CException const*, CMyException::EErrCode, std::string const&, ncbi::ETestSev) offset=0x43
          ./my_prog ???:0 CMyTestTest::Run() offset=0xD3A
          ./my_prog ???:0 ncbi::CNcbiApplication::x_TryMain(ncbi::EAppTestStream, char const*, int*, bool*) offset=0x6C8
          ./my_prog ???:0 ncbi::CNcbiApplication::AppMain(int, char const* const*, char const* const*, ncbi::EAppTestStream, char const*, std::string const&) offset=0x11BA
          ./my_prog ???:0 main offset=0x60
          /lib64/tls/libc.so.6 ???:0 __libc_start_main offset=0xEA
          ./my_prog ???:0 std::__throw_logic_error(char const*) offset=0x62

Debug Macros
------------

A number of debug macros such as <span class="nctnt ncbi-macro">\_TRACE</span>, <span class="nctnt ncbi-macro">\_TROUBLE</span>, <span class="nctnt ncbi-macro">\_ASSERT</span>, <span class="nctnt ncbi-macro">\_VERIFY</span>, <span class="nctnt ncbi-macro">\_DEBUG\_ARG</span> can be used when the <span class="nctnt ncbi-macro">\_DEBUG</span> macro is defined.

These macros are part of CORELIB. However, they are discussed in a separate chapter on [Debugging, Exceptions, and Error Handling](ch_debug.html).

Handling Exceptions
-------------------

The CORELIB defines an extended exception handling mechanism based on the C++ std::exception, but which considerably extends this mechanism to provide a backlog, history of unfinished tasks, and more meaningful reporting on the exception itself.

While the extended exception handling mechanism is part of CORELIB, it is discussed in a separate chapter on [Debugging, Exceptions, and Error Handling](ch_debug.html).

Defining the Standard NCBI C++ types and their Limits
-----------------------------------------------------

The following section provides a reference to the files and limit values used to in the C++ Toolkit to write portable code. An [introduction to the scope of some of these portability definitions](ch_intro.html#ch_intro.intro_pdef) is presented the introduction chapter.

The following topics are discussed in this section:

-   [Headers Files containing Portability Definitions](#headers-files-containing-portability-definitions)

-   [Built-in Integral Types](#built-in-integral-types)

-   [Auxiliary Types](#auxiliary-types)

-   [Fixed-size Integer Types](#fixed-size-integer-types)

-   [The "Ncbi\_BigScalar" Type](#the-ncbibigscalar-type)

-   [Encouraged and Discouraged Types](#encouraged-and-discouraged-types)

### Headers Files containing Portability Definitions

-   [corelib/ncbitype.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbitype.h) -- definitions of NCBI fixed-size integer types

-   [corelib/ncbi\_limits.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbi_limits.h) -- numeric limits for:

    -   NCBI fixed-size integer types

    -   built-in integer types

    -   built-in floating-point types

-   [corelib/ncbi\_limits.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbi_limits.hpp) -- temporary (and incomplete) replacement for the Standard C++ Template Library's API

### Built-in Integral Types

We [encourage](#encourage) the use of standard C/C++ types shown in [Table 5](#table-5), and we state that the following assumptions (no less, no more) on their sizes and limits will be valid for all supported platforms:

Table 5. Standard C/C++ Types

| Name                                                                                    | Size(bytes) | Min                                                                 | Max                                                                    | Note                                                                                                    |
|-----------------------------------------------------------------------------------------|-------------|---------------------------------------------------------------------|------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-type">char</span>                                               | 1           | <span class="nctnt ncbi-var">kMin\_Char</span> (0 or -128)          | <span class="nctnt ncbi-var">kMax\_Char</span> (256 or 127)            | It can be either signed or unsigned! Use it wherever you don't care of +/- (e.g. in character strings). |
| <span class="nctnt ncbi-type">signed char</span>                                        | 1           | <span class="nctnt ncbi-var">kMin\_SChar</span> (-128)              | <span class="nctnt ncbi-var">kMax\_SChar</span> (127)                  |                                                                                     |
| <span class="nctnt ncbi-type">unsigned char</span>                                      | 1           | <span class="nctnt ncbi-var">kMin\_UChar</span> (0)                 | <span class="nctnt ncbi-var">kMax\_UChar</span> (255)                  |                                                                                     |
| <span class="nctnt ncbi-type">short, signed short</span>                                | 2 or more   | <span class="nctnt ncbi-var">kMin\_Short</span> (-32768 or less)    | <span class="nctnt ncbi-var">kMax\_Short</span> (32767 or greater)     | Use <span class="nctnt ncbi-type">"int"</span> if size isn't critical                                   |
| <span class="nctnt ncbi-type">usigned short</span>                                      | 2 or more   | <span class="nctnt ncbi-var">kMin\_UShort</span> (0)                | <span class="nctnt ncbi-var">kMax\_UShort</span> (65535 or greater)    | Use <span class="nctnt ncbi-type">"unsigned int"</span> if size isn't critical                          |
| <span class="nctnt ncbi-var">int</span>, <span class="nctnt ncbi-var">signed int</span> | 4 or more   | <span class="nctnt ncbi-var">kMin\_Int</span> (-2147483648 or less) | <span class="nctnt ncbi-var">kMax\_Int</span> (2147483647 or greater)  |                                                                                     |
| <span class="nctnt ncbi-type">unsigned int</span>                                       | 4 or more   | <span class="nctnt ncbi-var">kMin\_UInt</span> (0)                  | <span class="nctnt ncbi-var">kMax\_UInt</span> (4294967295 or greater) |                                                                                     |
| <span class="nctnt ncbi-type">double</span>                                             | 4 or more   | <span class="nctnt ncbi-var">kMin\_Double</span>                    | <span class="nctnt ncbi-var">kMax\_Double</span>                       |                                                                                     |

Types <span class="nctnt ncbi-type">"long"</span> and <span class="nctnt ncbi-type">"float"</span> are [**discouraged**](#discouraged) to use in the portable code.

Type <span class="nctnt ncbi-type">"long long"</span> is **prohibited** to use in the portable code.

### Auxiliary Types

Use type <span class="nctnt ncbi-type">"bool"</span> to represent boolean values. It accepts one of { <span class="nctnt ncbi-monospace">false</span>, <span class="nctnt ncbi-monospace">true</span> }.

Use type <span class="nctnt ncbi-type">"size\_t"</span> to represent a size of memory structure, e.g. obtained as a result of <span class="nctnt ncbi-code">sizeof</span> operation.

Use type <span class="nctnt ncbi-type">"SIZE\_TYPE"</span> to represent a size of standard C++ <span class="nctnt ncbi-type">"string"</span> - this is a portable substitution for <span class="nctnt ncbi-type">"std::string::size\_type"</span>.

### Fixed-size Integer Types

Sometimes it is necessary to use an integer type which:

-   has a well-known fixed size(and lower/upper limits)

-   be just the same on all platforms(but maybe a byte/bit order, depending on the processor architecture)

NCBI C++ standard headers provide the fixed-size integer types shown in [Table 6](#table-6):

Table 6. Fixed-integer Types

| Name                                              | Size(bytes) | Min                                          | Max                                           |
|---------------------------------------------------|-------------|----------------------------------------------|-----------------------------------------------|
| <span class="nctnt ncbi-type">Char, Int1</span>   | 1           | <span class="nctnt ncbi-var">kMin\_I1</span> | <span class="nctnt ncbi-var">kMax\_I1</span>  |
| <span class="nctnt ncbi-type">Uchar, Uint1</span> | 1           | 0                                            | <span class="nctnt ncbi-var">kMax\_UI1</span> |
| <span class="nctnt ncbi-type">Int2</span>         | 2           | <span class="nctnt ncbi-var">kMin\_I2</span> | <span class="nctnt ncbi-var">kMax\_I2</span>  |
| <span class="nctnt ncbi-type">Uint2</span>        | 2           | 0                                            | <span class="nctnt ncbi-var">kMax\_UI2</span> |
| <span class="nctnt ncbi-type">Int4</span>         | 4           | <span class="nctnt ncbi-var">kMin\_I4</span> | <span class="nctnt ncbi-var">kMax\_I4</span>  |
| <span class="nctnt ncbi-type">Uint4</span>        | 4           | 0                                            | <span class="nctnt ncbi-var">kMax\_UI4</span> |
| <span class="nctnt ncbi-type">Int8</span>         | 8           | <span class="nctnt ncbi-var">kMin\_I8</span> | <span class="nctnt ncbi-var">kMax\_I8</span>  |
| <span class="nctnt ncbi-type">Uint8</span>        | 8           | 0                                            | <span class="nctnt ncbi-var">kMax\_UI8</span> |

In [Table 7](#table-7), the <span class="nctnt ncbi-var">"kM\*\_\*"</span> are constants of relevant fixed-size integer type. They are guaranteed to be equal to the appropriate <span class="nctnt ncbi-monospace">preprocessor constants</span> from the old NCBI C headers(<span class="nctnt ncbi-var">"INT\*\_M\*"</span>). Please also note that the mentioned <span class="nctnt ncbi-var">"INT\*\_M\*"</span> are not defined in the C++ headers - in order to discourage their use in the C++ code.

Table 7. Correspondence between the kM\*\_\* constants and the old style INT\*\_M\* constants

| Constant(NCBI C++)                            | Value                 | Define(NCBI C)                                 |
|-----------------------------------------------|-----------------------|------------------------------------------------|
| <span class="nctnt ncbi-var">kMin\_I1</span>  | -128                  | <span class="nctnt ncbi-var">INT1\_MIN</span>  |
| <span class="nctnt ncbi-var">kMax\_I1</span>  | +127                  | <span class="nctnt ncbi-var">INT1\_MAX</span>  |
| <span class="nctnt ncbi-var">kMax\_UI1</span> | +255                  | <span class="nctnt ncbi-var">UINT1\_MAX</span> |
| <span class="nctnt ncbi-var">kMin\_I2</span>  | -32768                | <span class="nctnt ncbi-var">INT2\_MIN</span>  |
| <span class="nctnt ncbi-var">kMax\_I2</span>  | +32767                | <span class="nctnt ncbi-var">INT2\_MAX</span>  |
| <span class="nctnt ncbi-var">kMax\_UI2</span> | +65535                | <span class="nctnt ncbi-var">UINT2\_MAX</span> |
| <span class="nctnt ncbi-var">kMin\_I4</span>  | -2147483648           | <span class="nctnt ncbi-var">INT4\_MIN</span>  |
| <span class="nctnt ncbi-var">kMax\_I4</span>  | +2147483647           | <span class="nctnt ncbi-var">INT4\_MAX</span>  |
| <span class="nctnt ncbi-var">kMax\_UI4</span> | +4294967295           | <span class="nctnt ncbi-var">UINT4\_MAX</span> |
| <span class="nctnt ncbi-var">kMin\_I8</span>  | -9223372036854775808  | <span class="nctnt ncbi-var">INT8\_MIN</span>  |
| <span class="nctnt ncbi-var">kMax\_I8</span>  | +9223372036854775807  | <span class="nctnt ncbi-var">INT8\_MAX</span>  |
| <span class="nctnt ncbi-var">kMax\_UI8</span> | +18446744073709551615 | <span class="nctnt ncbi-var">UINT8\_MAX</span> |

### The <span class="nctnt ncbi-type">"Ncbi\_BigScalar"</span> Type

NCBI C++ standard headers also define a special type <span class="nctnt ncbi-type">"Ncbi\_BigScalar"</span>. The only assumption that can be made(and used in your code) is that <span class="nctnt ncbi-type">"Ncbi\_BigScalar"</span> variable has a size which is enough to hold any integral, floating-point or pointer variable like <span class="nctnt ncbi-type">"Int8"</span>, or <span class="nctnt ncbi-type">"double"("long double")</span>, or <span class="nctnt ncbi-type">"void\*"</span>. This type can be useful e.g. to hold a callback data of arbitrary fundamental type; however, in general, the use of this type is discouraged.

### Encouraged and Discouraged Types

For the sake of code portability and for better compatibility with the third-party and system libraries, one should follow the following set of rules:

-   Use standard C/C++ integer types <span class="nctnt ncbi-type">"char", "signed char", "unsigned char", "short", "unsigned short", "int", "unsigned int"</span> in **any** case where the assumptions made for them in [Table 5](#table-5) are enough.

-   It is not recommended to use <span class="nctnt ncbi-type">"long"</span> type unless it is absolutely necessary (e.g. in the lower-level or third-party code), and even if you have to, then try to localize the use of <span class="nctnt ncbi-type">"long"</span> as much as possible.

-   The same(as for <span class="nctnt ncbi-type">"long"</span>) is for the fixed-size types enlisted in [Table 6](#table-6). If you have to use these in your code, try to keep them inside your modules and avoid mixing them with standard C/C++ types (as in assignments, function arg-by-value passing and in arithmetic expressions) as much as possible.

-   For the policy on other types see in sections ["Auxiliary types"](#auxiliary-types) and "Floating point types".

Understanding Smart Pointers: the <span class="nctnt ncbi-class">CObject</span> and <span class="nctnt ncbi-class">CRef</span> Classes
--------------------------------------------------------------------------------------------------------------------------------------

This section provides reference information on the use of <span class="nctnt ncbi-class">CRef</span> and <span class="nctnt ncbi-class">CObject</span> classes. For an overview of these classes refer to the [introductory chapter](ch_intro.html#ch_intro.intro_cref).

The following is a list of topics discussed in this section:

-   [STL auto\_ptrs](#stl-autoptrs)

-   [The CRef Class](#the-cref-class)

-   [The CObject Class](#the-cobject-class)

-   [The CObjectFor class: using smart pointers for standard types](#the-cobjectfor-class-using-smart-pointers-for-standard-types)

-   [When to use CRefs and auto\_ptrs](#when-to-use-crefs-and-autoptrs)

-   [CRef Pitfalls](#cref-pitfalls)

### STL auto\_ptrs

C programmers are well-acquainted with the advantages and pitfalls of using pointers. As is often the case, the good news is also the bad news:

-   memory can be dynamically allocated as needed, but may not be deallocated as needed, due to unanticipated execution paths;

-   void pointers allow heterogeneous function arguments of different types, but type information may not be there when you need it.

C++ adds some additional considerations to pointer management: STL containers cannot hold <span class="nctnt ncbi-monospace">reference</span> objects, so you are left with the choice of using either pointers or <span class="nctnt ncbi-monospace">copies</span> of objects. Neither choice is attractive, as pointers can cause memory leaks and the copy constructor may be expensive.

The idea behind a C++ <span class="nctnt ncbi-monospace">smart pointer</span> is to create a wrapper class capable of holding a pointer. The wrapper class's constructors and destructors can then handle memory management as the object goes in and out of scope. The problem with this solution is that it does not handle multiple pointers to the same resource properly, and it raises the issue of ownership. This is essentially what the [auto\_ptr](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=auto_ptr) offers, but this strategy is only safe to use when the resource maps to a single pointer variable.

For example, the following code has two very serious problems:

    int* ip = new int(5);
    {
        auto_ptr<int> a1(ip);
        auto_ptr<int> a2(ip);
    }
    *ip = 10/(*ip);

The first problem occurs inside the block where the two <span class="nctnt ncbi-class">auto\_ptr</span>s are defined. Both are referencing the same variable pointed to by yet another C pointer, and each considers itself to be the owner of that reference. Thus, when the block is exited, the <span class="nctnt ncbi-code">delete[]</span> operation is executed twice for the same pointer.

Even if this first problem did not occur - for example if only one <span class="nctnt ncbi-class">auto\_ptr</span> had been defined - the second problem occurs when we try to dereference <span class="nctnt ncbi-var">ip</span>. The <span class="nctnt ncbi-code">delete</span> operation occurring as the block exits has now freed the dynamic memory to which ip points, so \*ip now references unallocated memory.

The problem with using <span class="nctnt ncbi-class">auto\_ptr</span> is that it provides semantics of strict ownership. When an <span class="nctnt ncbi-class">auto\_ptr</span> is destructed, it deletes the object it points to, and therefore the object should not be pointed to simultaneously by others. Two or more <span class="nctnt ncbi-class">auto\_ptr</span>s should not own the same object; that is, point to the same object. This can occur if two <span class="nctnt ncbi-class">auto\_ptr</span>s are initialized to the same object, as seen in the above example where auto pointers <span class="nctnt ncbi-var">a1</span> and <span class="nctnt ncbi-var">a2</span> are both initialized with <span class="nctnt ncbi-var">ip</span>. In using <span class="nctnt ncbi-class">auto\_ptr</span>, the programmer must ensure that situations similar to the above do not occur.

### The CRef ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCRef.html)) Class

These issues are addressed in the NCBI C++ Toolkit by using <span class="nctnt ncbi-monospace">reference-counted</span> smart pointers: a resource cannot be deallocated until **all** references to it have ceased to exist. The implementation of a smart pointer in the NCBI C++ Toolkit is actually divided between two classes: [CRef](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCRef.html) and [CObject](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCObject.html).

The <span class="nctnt ncbi-class">CRef</span> class essentially provides a pointer interface to a <span class="nctnt ncbi-class">CObject</span>, while the <span class="nctnt ncbi-class">CObject</span> actually stores the data and maintains the reference count to it. The constructor used to create a new <span class="nctnt ncbi-class">CRef</span> pointing to a particular <span class="nctnt ncbi-class">CObject</span> automatically increments the object's reference count. Similarly, the <span class="nctnt ncbi-class">CRef</span> destructor automatically decrements the reference count. In both cases however, the modification of the reference count is implemented by a member function of the <span class="nctnt ncbi-class">CObject</span>. The <span class="nctnt ncbi-class">CRef</span> class itself does not have direct access to the reference count and contains only a single data member - its pointer to a <span class="nctnt ncbi-class">CObject</span>. In addition to the <span class="nctnt ncbi-class">CRef</span> class's constructors and destructors, its interface to the <span class="nctnt ncbi-class">CObject</span> pointer includes access/mutate functions such as:

    bool Empty()
    bool NotEmpty()
    CObject* GetPointer()
    CObject& GetObject()
    CObject* Release()
    void Reset(CObject* newPtr)
    void Reset(void)
    operator bool()
    bool operator!()
    CRefBase& operator=(const CRefBase& ref)

Both the <span class="nctnt ncbi-func">Release()</span> and <span class="nctnt ncbi-func">Reset()</span> functions set the <span class="nctnt ncbi-class">CRef</span> object's <span class="nctnt ncbi-var">m\_ptr</span> to 0, thus effectively removing the reference to its <span class="nctnt ncbi-class">CObject</span>. There are important distinctions between these two functions however. The <span class="nctnt ncbi-func">Release()</span> method removes the reference without destroying the object, while the <span class="nctnt ncbi-func">Reset()</span> method may lead to the destruction of the object if there are no other references to it.

If the <span class="nctnt ncbi-class">CObject</span>'s internal reference count is 1 at the time <span class="nctnt ncbi-func">Release()</span> is invoked, that reference count will be decremented to 0, and a pointer to the <span class="nctnt ncbi-class">CObject</span> is returned. The <span class="nctnt ncbi-func">Release()</span> method can throw two types of exceptions: (1) a <span class="nctnt ncbi-monospace">null pointer</span> exception if <span class="nctnt ncbi-var">m\_ptr</span> is already <span class="nctnt ncbi-monospace">0</span>, and (2) an <span class="nctnt ncbi-monospace">Illegal CObject::ReleaseReference()</span> exception if there are currently other references to that object. An object must be free of all references (but this one) before it can be "released". In contrast, the <span class="nctnt ncbi-func">Reset(void)</span> function simply resets the <span class="nctnt ncbi-class">CRef</span>'s <span class="nctnt ncbi-var">m\_ptr</span> to 0, decrements the <span class="nctnt ncbi-class">CObject</span>'s reference count, and, if the <span class="nctnt ncbi-class">CObject</span> has no other references and was dynamically allocated, deletes the <span class="nctnt ncbi-class">CObject</span>.

Each member function of the <span class="nctnt ncbi-class">CRef</span> class also has a <span class="nctnt ncbi-code">const</span> implementation that is invoked when the pointer is to a <span class="nctnt ncbi-code">const</span> object. In addition, there is also a [CConstRef](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCConstRef.html) class that parallels the <span class="nctnt ncbi-class">CRef</span> class. Both <span class="nctnt ncbi-class">CRef</span> and <span class="nctnt ncbi-class">CConstRef</span> are implemented as template classes, where the template argument specifies the type of object which will be pointed to. For example, in the section on [Traversing an ASN.1 Data Structure](ch_ser.html#ch_ser.traverse.html_accessMember) we examined the structure of the <span class="nctnt ncbi-class">CBiostruc</span> class and found the following type definition

    typedef list< CRef< ::CBiostruc_id > > TId;

As described there, this <span class="nctnt ncbi-code">typedef</span> defines <span class="nctnt ncbi-type">TId</span> to be a list of pointers to [CBiostruc\_id](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCBiostruc__id.html) objects. And as you might expect, <span class="nctnt ncbi-class">CBiostruc\_id</span> is a specialized subclass of <span class="nctnt ncbi-class">CObject</span>.

### The CObject ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCObject.html)) Class

The <span class="nctnt ncbi-class">CObject</span> class serves as a base class for all objects requiring a reference count. There is little overhead entailed by deriving a new class from this base class, and most objects in the NCBI C++ Toolkit are derived from the <span class="nctnt ncbi-class">CObject</span> class. For example, [CNCBINode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNCBINode.html) is a direct descendant of <span class="nctnt ncbi-class">CObject</span>, and all of the other <span class="nctnt ncbi-type">HTML</span> classes descend either directly or indirectly from <span class="nctnt ncbi-class">CNCBINode</span>. Similarly, all of the ASN.1 classes defined in the [include/objects](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objects) directory, as well as many of the classes defined in the [include/serial](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/serial) directory are derived either directly or indirectly from the <span class="nctnt ncbi-class">CObject</span> class.

The <span class="nctnt ncbi-class">CObject</span> class contains a single private data member, the reference counter, and a set of member functions which provide an interface to the reference counter. As such, it is truly a base class which has no stand-alone utility, as it does not even provide allocation for data values. It is the <span class="nctnt ncbi-monospace">descendant</span> classes, which inherit all the functionality of the <span class="nctnt ncbi-class">CObject</span> class, that provide the necessary richness in representation and allocation required for the widely diverse set of objects implemented in the NCBI C++ Toolkit. Nevertheless, it is often necessary to use smart pointers on simple data types, such as <span class="nctnt ncbi-type">int</span>, <span class="nctnt ncbi-type">string</span> etc. The <span class="nctnt ncbi-class">CObjectFor</span> class, described below, was designed for this purpose.

### The CObjectFor ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCObjectFor.html)) class: using smart pointers for standard types

The <span class="nctnt ncbi-class">CObjectFor</span> class is derived directly from <span class="nctnt ncbi-class">CObject</span>, and is implemented as a template class whose argument specifies the standard type that will be pointed to. In addition to the reference counter inherited from its parent class, <span class="nctnt ncbi-class">CObjectFor</span> has a private data member of the parameterized type, and a member function <span class="nctnt ncbi-func">GetData()</span> to access it.

An example program, [smart.cpp](ch_demo.html#ch_demo.examples_1_4), uses the [CRef](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCRef.html) and [CObjectFor](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCObjectFor.html) classes, and demonstrates the differences in memory management that arise using <span class="nctnt ncbi-class">auto\_ptr</span> and <span class="nctnt ncbi-class">CRef</span>. Using an <span class="nctnt ncbi-class">auto\_ptr</span> to reference an <span class="nctnt ncbi-type">int</span>, the program tests whether or not the reference is still accessible after an auxilliary <span class="nctnt ncbi-class">auto\_ptr</span> which goes out of scope has also been used to reference it. The same sequence is then tested using <span class="nctnt ncbi-class">CRef</span> objects instead.

In the first case, the original auto\_ptr, <span class="nctnt ncbi-var">orig\_ap</span>, becomes <span class="nctnt ncbi-monospace">NULL</span> at the moment when ownership is transferred to <span class="nctnt ncbi-var">copy\_ap</span> by the copy constructor. Using <span class="nctnt ncbi-class">CRef</span> objects however, the reference contained in the original <span class="nctnt ncbi-class">CRef</span> remains accessible (via <span class="nctnt ncbi-var">orig</span>) in all blocks where <span class="nctnt ncbi-var">orig</span> is defined. Moreover, the reference itself, i.e. the object pointed to, continues to exist until **all** references to it have been removed.

### When to use CRefs and auto\_ptrs

There is some overhead in using <span class="nctnt ncbi-class">CRef</span> and <span class="nctnt ncbi-class">auto\_ptr</span>, and these objects should only be used where needed. Memory leaks are generally caused as a result of unexpected execution paths. For example:

    {
        int *num = new int(5);
        ComplexFunction (num);
        delete num;
        ...
    }

If <span class="nctnt ncbi-func">ComplexFunction()</span> executes normally, control returns to the block where it was invoked, and memory is freed by the <span class="nctnt ncbi-code">delete</span> statement. Unforeseen events however, may trigger exceptions, causing control to pass elsewhere. In these cases, the <span class="nctnt ncbi-code">delete</span> statement may never be reached. The use of a <span class="nctnt ncbi-class">CRef</span> or an <span class="nctnt ncbi-class">auto\_ptr</span> is appropriate for these situations, as they both guarantee that the object will be destroyed when the reference goes out of scope.

One situation where they may not be required is when a pointer is embedded inside another object. If that object's destructor also handles the deallocation of its embedded objects, then it is sufficient to use a <span class="nctnt ncbi-class">CRef</span> on the containing object only.

### CRef Pitfalls

#### Inadvertent Object Destruction

When the last reference to a <span class="nctnt ncbi-class">CRef</span> object goes out of scope or the <span class="nctnt ncbi-class">CRef</span> is otherwise marked for garbage collection, the object to which the <span class="nctnt ncbi-class">CRef</span> points is also destroyed. This feature helps to prevent memory leaks, but it also requires care in the use of <span class="nctnt ncbi-class">CRef</span>s within methods and functions.

    class CMy : public CObject
    {
    .....
    };
    void f(CMy* a)
    {
        CRef b = a;
        return;
    }
    .....
        CMy* a = new CMy();
        f(a);
        // the object "a" is now destroyed!

In this example the function <span class="nctnt ncbi-func">f()</span> establishes a local <span class="nctnt ncbi-class">CRef</span> to the <span class="nctnt ncbi-class">CMy</span> object <span class="nctnt ncbi-var">a</span>. On exiting <span class="nctnt ncbi-func">f()</span> the <span class="nctnt ncbi-class">CRef</span><span class="nctnt ncbi-var">b</span> is destroyed, including the implied destruction of the <span class="nctnt ncbi-class">CMy</span> objects <span class="nctnt ncbi-var">a</span>. To avoid this behavior, pass a <span class="nctnt ncbi-class">CRef</span> to the function <span class="nctnt ncbi-func">f()</span> instead of a normal pointer variable:

    CRef a = new CMy();
    f(a);
    // the CMy object pointed to by "a" is not destroyed!

Atomic Counters
---------------

The CORELIB implements efficient atomic counters that are used for <span class="nctnt ncbi-class">CObject</span> reference counts. The classes <span class="nctnt ncbi-class">CAtomicCounter</span> and <span class="nctnt ncbi-class">CMutableAtomicCounter</span> provide respectively a base atomic counter class, and a mutable atomic counter for multithreaded applications. These classes are used to in reference counted [smart pointers](#smart-pointers).

The <span class="nctnt ncbi-class">CAtomicCounter</span> base class provides the base methods <span class="nctnt ncbi-func">Get()</span>, <span class="nctnt ncbi-func">Set()</span>, <span class="nctnt ncbi-func">Add()</span> for atomic counters:

    class CAtomicCounter
    {
    public:
        ///< Alias TValue for TNCBIAtomicValue
        typedef TNCBIAtomicValue TValue;
        /// Get atomic counter value.
        TValue Get(void) const THROWS_NONE;
        /// Set atomic counter value.
        void   Set(TValue new_value) THROWS_NONE;
        /// Atomically add value (=delta), and return new counter value.
        TValue Add(int delta) THROWS_NONE;
        .......
    };

<span class="nctnt ncbi-macro">TNCBIAtomicValue</span> is defined as a macro and its definition is platform dependent. If threads are not used (Macro <span class="nctnt ncbi-macro">NCBI\_NO\_THREADS</span> defined), <span class="nctnt ncbi-macro">TNCBIAtomicValue</span> is an <span class="nctnt ncbi-type">unsigned int</span> value. If threads are used, then a number of defines in file "<span class="nctnt ncbi-path">ncbictr.hpp</span>" ensure that a platform specific definition is selected for <span class="nctnt ncbi-macro">TNCBIAtomicValue</span>.

The <span class="nctnt ncbi-class">CMutableAtomicCounter</span> uses the <span class="nctnt ncbi-class">CAtomicCounter</span> as its internal structure of the atomic counter but declares this counter value as mutable. The <span class="nctnt ncbi-func">Get()</span>, <span class="nctnt ncbi-func">Set()</span>, <span class="nctnt ncbi-func">Add()</span> methods for <span class="nctnt ncbi-class">CMutableAtomicCounter</span> are implemented by calls to the corresponding <span class="nctnt ncbi-func">Get()</span>, <span class="nctnt ncbi-func">Set()</span>, <span class="nctnt ncbi-func">Add()</span> methods for the <span class="nctnt ncbi-class">CAtomicCounter</span>:

    class  CMutableAtomicCounter
    {
    public:
        typedef CAtomicCounter::TValue TValue; ///< Alias TValue simplifies syntax
        /// Get atomic counter value.
        TValue Get(void) const THROWS_NONE
            { return m_Counter.Get(); }
        /// Set atomic counter value.
        void   Set(TValue new_value) const THROWS_NONE
            { m_Counter.Set(new_value); }
        /// Atomically add value (=delta), and return new counter value.
        TValue Add(int delta) const THROWS_NONE
            { return m_Counter.Add(delta); }
    private:
        ...
    };

Portable mechanisms for loading DLLs
------------------------------------

The <span class="nctnt ncbi-class">CDll</span> class defines a portable way of dynamically loading shared libraries and finding entry points for functions in the library. Currently this portable behavior is defined for Unix-like systems and Windows only. On Unix-like systems, loading of the shared library is implemented using the Unix system call <span class="nctnt ncbi-func">dlopen()</span> and the entry point address obtained using the Unix system call <span class="nctnt ncbi-func">dlsym()</span>. On Windows systems the system call <span class="nctnt ncbi-func">LoadLibraray()</span> is used to load the library, and the system call <span class="nctnt ncbi-func">GetProcAddress()</span> is used to get a function's entry point address.

All methods of <span class="nctnt ncbi-class">CDll</span> class, except the destructor, throw the exception <span class="nctnt ncbi-monospace">CCoreException::eDll</span> on error.

You can specify when to load the DLL - when the <span class="nctnt ncbi-class">CDll</span> object is created (loading in the constructor), or by an explicit call to <span class="nctnt ncbi-func">CDll::Load()</span>. You can also specify whether the DLL is unloaded automatically when <span class="nctnt ncbi-class">CDll's</span> destructor is called or if the DLL should remain loaded in memory. This behavior is controlled by arguments to <span class="nctnt ncbi-class">CDll's</span> constructor.

The following additional topics are described in this section:

-   [CDll Constructor](#cdll-constructor)

-   [CDll Basename](#cdll-basename)

-   [Other CDll Methods](#other-cdll-methods)

### CDll Constructor

The CDll class has four constructors:

    CDll(const string& name, TFlags flags);

    CDll(const string& path, const string& name, TFlags flags);

    CDll(const string& name,
         ELoad         when_to_load = eLoadNow,
         EAutoUnload   auto_unload  = eNoAutoUnload,
         EBasename     treate_as    = eBasename);

    CDll(const string& path, const string& name,
         ELoad         when_to_load = eLoadNow,
         EAutoUnload   auto_unload  = eNoAutoUnload,
         EBasename     treate_as    = eBasename);

The first and second constructor forms are the same with the exception that the second constructor uses two parameters - the <span class="nctnt ncbi-var">path</span> and <span class="nctnt ncbi-var">name</span> parameters - to build a path to the DLL, whereas in the first constructor, the <span class="nctnt ncbi-var">name</span> parameter contains the full path to the DLL. The third and fourth forms are likewise similar.

The first pair of constructors differ from the second pair in that the first two take a single parameter that is a set of flags, whereas the second pair take three separate parameters for flags. The first two are newer, and the last two are provided for backward compatibility.

The parameter <span class="nctnt ncbi-var">when\_to\_load</span> is defined as an enum type of <span class="nctnt ncbi-type">ELoad</span> and has the values <span class="nctnt ncbi-monospace">eLoadNow</span> or <span class="nctnt ncbi-monospace">eLoadLater</span>. When <span class="nctnt ncbi-monospace">eLoadNow</span> is passed to the constructor (default value), the DLL is loaded in the constructor; otherwise it has to be loaded via an explicit call to the <span class="nctnt ncbi-func">Load()</span> method.

The parameter <span class="nctnt ncbi-var">auto\_load</span> is defined as an enum type of <span class="nctnt ncbi-type">EAutoLoad</span> and has the values <span class="nctnt ncbi-monospace">eAutoUnload</span> or <span class="nctnt ncbi-monospace">eNoAutoUnload</span>. When <span class="nctnt ncbi-monospace">eAutoUnload</span> is passed to the constructor (default value), the DLL is unloaded in the destructor; otherwise it will remain loaded in memory.

The parameter <span class="nctnt ncbi-var">treat\_as</span> is defined as an enum type of <span class="nctnt ncbi-type">EBasename</span> and has the values <span class="nctnt ncbi-monospace">eBasename</span> or <span class="nctnt ncbi-monospace">eExactName</span>. When <span class="nctnt ncbi-monospace">eBasename</span> is passed to the constructor (default value), the name parameter is treated as a basename if it looks like one; otherwise the exact name or "as is" value is used with no addition of prefix or suffix.

The parameter <span class="nctnt ncbi-var">flags</span> is defined as an enum type of <span class="nctnt ncbi-type">EFlags</span> and has the values <span class="nctnt ncbi-monospace">fLoadNow</span>, <span class="nctnt ncbi-monospace">fLoadLater</span>, <span class="nctnt ncbi-monospace">fAutoUnload</span>, <span class="nctnt ncbi-monospace">fNoAutoUnload</span>, <span class="nctnt ncbi-monospace">fBaseName</span>, <span class="nctnt ncbi-monospace">fExactName</span>, <span class="nctnt ncbi-monospace">fGlobal</span>, <span class="nctnt ncbi-monospace">fLocal</span>, and <span class="nctnt ncbi-monospace">fDefault</span>. The flags <span class="nctnt ncbi-monospace">fLoadNow</span>, <span class="nctnt ncbi-monospace">fLoadLater</span>, <span class="nctnt ncbi-monospace">fAutoUnload</span>, <span class="nctnt ncbi-monospace">fNoAutoUnload</span>, <span class="nctnt ncbi-monospace">fBaseName</span>, and <span class="nctnt ncbi-monospace">fExactName</span> correspond to the similarly named enum values as described in the above paragraphs. The flag <span class="nctnt ncbi-monospace">fGlobal</span> indicates that the DLL should be loaded as <span class="nctnt ncbi-monospace">RTLD\_GLOBAL</span>, while the flag <span class="nctnt ncbi-monospace">fLocal</span> indicates that the DLL should be loaded as <span class="nctnt ncbi-monospace">RTLD\_LOCAL</span>. The flag <span class="nctnt ncbi-monospace">fDefault</span> is defined as:

    fDefault = fLoadNow \| fNoAutoUnload \| fBaseName \| fGlobal

### CDll Basename

The DLL name is considered the basename if it does not contain embedded '/', '\\', or ':' symbols. Also, in this case, if the DLL name does not match the pattern "lib\*.so", "lib\*.so.\*", or "\*.dll" and if <span class="nctnt ncbi-monospace">eExactName</span> flag is not passed to the constructor, then it will be automatically transformed according to the following rules:

| OS        | Rule                                                                  |
|-----------|-----------------------------------------------------------------------|
| Unix-like | <span class="nctnt ncbi-monospace">\<name\> -\> lib\<name\>.so</span> |
| Windows   | <span class="nctnt ncbi-monospace">\<name\> -\> \<name\>.dll</span>   |

If the DLL is specified by its basename, then it will be searched after the transformation described above in the following locations:

-   Unix:

    -   The directories that are listed in the <span class="nctnt ncbi-var">LD\_LIBRARY\_PATH</span> environment variable which are analyzed once at the process startup.

    -   The directory from which the application loaded

    -   Hard-coded (e.g. with \`ldconfig' on Linux) paths

-   Windows:

    -   The directory from which the application is loaded

    -   The current directory

    -   The Windows system directory

    -   The Windows directory

    -   The directories that are listed in the <span class="nctnt ncbi-var">PATH</span> environment variable

### Other CDll Methods

Two methods mentioned earlier for the <span class="nctnt ncbi-class">CDll</span> class are the <span class="nctnt ncbi-func">Load()</span> and <span class="nctnt ncbi-func">Unload()</span> methods. The <span class="nctnt ncbi-func">Load()</span> method loads the DLL using the name specified in the constructor's DLL <span class="nctnt ncbi-var">name</span> parameter. The <span class="nctnt ncbi-func">Load()</span> method is expected to be used when the DLL is not explictly loaded in the constructor. That is, when the <span class="nctnt ncbi-class">CDll</span> constructor is passed the <span class="nctnt ncbi-monospace">eLoadLater</span> parameter. If the <span class="nctnt ncbi-func">Load()</span> is called more than once without calling <span class="nctnt ncbi-func">Unload()</span> in between, then it will do nothing. The syntax of the <span class="nctnt ncbi-func">Load()</span> methods is

    void Load(void);

The <span class="nctnt ncbi-func">Unload()</span> method unloads that DLL whose name was specified in the constructor's DLL <span class="nctnt ncbi-var">name</span> parameter. The <span class="nctnt ncbi-func">Unload()</span> method is expected to be used when the DLL is not explicitly unloaded in the destructor. This occurs, when the <span class="nctnt ncbi-class">CDll</span> constructor is passed the <span class="nctnt ncbi-monospace">eNoAutoUnload</span> parameter. If the <span class="nctnt ncbi-func">Unload()</span> is called when the DLL is not loaded, then it will do nothing. The syntax of the <span class="nctnt ncbi-func">Unload()</span> methods is

    void Unload(void);

Once the DLL is loaded, you can call the DLL's functions by first getting the function's entry point (address), and using this to call the function. The function template <span class="nctnt ncbi-func">GetEntryPoint()</span> method is used to get the entry point address and is defined as:

    template <class TPointer>
    TPointer GetEntryPoint(const string& name, TPointer* entry_ptr);

This method returns the entry point's address on success, or NULL on error. If the DLL is not loaded when this method is called, then this method will call <span class="nctnt ncbi-func">Load()</span> to load the DLL which can result in throwing an exception if <span class="nctnt ncbi-func">Load()</span> fails.

Some sample code illustrating the use of these methods is shown in [src/corelib/test/test\_ncbidll.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test/test_ncbidll.cpp)

Executing Commands and Spawning Processes using the CExec class
---------------------------------------------------------------

The <span class="nctnt ncbi-class">CExec</span> defines a portable execute class that can be used to execute system commands and spawn new processes.

The following topics relating to the <span class="nctnt ncbi-class">CExec</span> class are discussed, next:

-   [Executing a System Command using the System() Method](#executing-a-system-command-using-the-system-method)

-   [Defining Spawned Process Modes (EMode type)](#defining-spawned-process-modes-emode-type)

-   [Spawning a Process using SpawnX() Methods](#spawning-a-process-using-spawnx-methods)

-   [Waiting for a Process to Terminate using the Wait() method](#waiting-for-a-process-to-terminate-using-the-wait-method)

### Executing a System Command using the System() Method

You can use the class-wide <span class="nctnt ncbi-func">CExec::System()</span> method to execute a system command:

    static int System(const char* cmdline);

<span class="nctnt ncbi-func">CExec::System()</span> returns the executed command's exit code and throws an exception if the command failed to execute. If cmdline is a null pointer, <span class="nctnt ncbi-func">CExec::System()</span> checks if the shell (command interpreter) exists and is executable. If the shell is available, <span class="nctnt ncbi-func">System()</span> returns a non-zero value; otherwise, it returns 0.

### Defining Spawned Process Modes (EMode type)

The spawned process can be created in several modes defined by the enum type <span class="nctnt ncbi-func">EMode</span>. The meanings of the enum values for <span class="nctnt ncbi-func">EMode</span> type are:

-   <span class="nctnt ncbi-monospace">eOverlay</span>: This mode overlays the calling process with new process, destroying the calling process.

-   <span class="nctnt ncbi-monospace">eWait</span>: This mode suspends the calling thread until execution of a new process is complete. That is, the called process is called synchronously.

-   <span class="nctnt ncbi-monospace">eNoWait</span>: This is the opposite of <span class="nctnt ncbi-monospace">eWait</span>. This mode continues to execute the calling process concurrently with the new called process in an asynchronous fashion.

-   <span class="nctnt ncbi-monospace">eDetach</span>: This mode continues to execute the calling process and new process is "detached" and run in background with no access to console or keyboard. Calls to <span class="nctnt ncbi-func">Wait()</span> against new process will fail. This is an asynchronous spawn.

### Spawning a Process using SpawnX() Methods

A new process can be spawned by calling any of the class-wide methods named <span class="nctnt ncbi-func">SpawnX()</span> which have the form:

    static int SpawnX(const EMode mode,
                      const char *cmdname,
                      const char *argv,
                      ...
                      );

The parameter <span class="nctnt ncbi-var">mode</span> has the meanings discussed in the section [Defining Spawned Process Modes (EMode type)](#defining-spawned-process-modes-emode-type). The parameter <span class="nctnt ncbi-var">cmdname</span> is the command-line string to start the process, and parameter <span class="nctnt ncbi-var">argv</span> is the argument vector containing arguments to the process.

The <span class="nctnt ncbi-func">X</span> in the function name is a one to three letter suffix indicating the type of the spawn function. Each of the letters in the suffix <span class="nctnt ncbi-func">X</span>, for <span class="nctnt ncbi-func">SpawnX()</span> have the following meanings:

-   <span class="nctnt ncbi-monospace">L</span>: The letter "L" as suffix refers to the fact that command-line arguments are passed separately as arguments.

-   <span class="nctnt ncbi-monospace">E</span>: The letter "E" as suffix refers to the fact that environment pointer, <span class="nctnt ncbi-var">envp</span>, is passed as an array of pointers to environment settings to the new process. The <span class="nctnt ncbi-monospace">NULL</span> environment pointer indicates that the new process will inherit the parents' process's environment.

-   <span class="nctnt ncbi-monospace">P</span>: The letter "P" as suffix refers to the fact that the <span class="nctnt ncbi-var">PATH</span> environment variable is used to find file to execute. Note that on a Unix-like system this feature works in functions without letter "P" in the function name.

-   <span class="nctnt ncbi-monospace">V</span>: The letter "V" as suffix refers to the fact that the number of command-line arguments is variable.

Using the above letter combinations as suffixes, the following spawn functions are defined:

-   <span class="nctnt ncbi-func">SpawnL()</span>: In the <span class="nctnt ncbi-func">SpawnL()</span> version, the command-line arguments are passed individually. <span class="nctnt ncbi-func">SpawnL()</span> is typically used when number of parameters to the new process is known in advance.

-   <span class="nctnt ncbi-func">SpawnLE()</span>: In the <span class="nctnt ncbi-func">SpawnLE()</span> version, the command-line arguments and environment pointer are passed individually. <span class="nctnt ncbi-func">SpawnLE()</span> is typically used when number of parameters to the new process and individual environment parameter settings are known in advance.

-   <span class="nctnt ncbi-func">SpawnLP()</span>: In the <span class="nctnt ncbi-func">SpawnLP()</span> version, the command-line arguments are passed individually and the <span class="nctnt ncbi-var">PATH</span> environment variable is used to find the file to execute. <span class="nctnt ncbi-func">SpawnLP()</span> is typically used when number of parameters to the new process is known in advance but the exact path to the executable is not known.

-   <span class="nctnt ncbi-func">SpawnLPE()</span>: In the <span class="nctnt ncbi-func">SpawnLPE()</span> the command-line arguments and environment pointer are passed individually, and the <span class="nctnt ncbi-var">PATH</span> environment variable is used to find the file to execute. <span class="nctnt ncbi-func">SpawnLPE()</span> is typically used when the number of parameters to the new process and individual environment parameter settings are known in advance, but the exact path to the executable is not known.

-   <span class="nctnt ncbi-func">SpawnV()</span>: In the <span class="nctnt ncbi-func">SpawnV()</span> version, the command-line arguments are a variable number. The array of pointers to arguments must have a length of 1 or more and you must assign parameters for the new process beginning from 1.

-   <span class="nctnt ncbi-func">SpawnVE()</span>: In the <span class="nctnt ncbi-func">SpawnVE()</span> version, the command-line arguments are a variable number. The array of pointers to arguments must have a length of 1 or more and you must assign parameters for the new process beginning from 1. The individual environment parameter settings are known in advance and passed explicitly.

-   <span class="nctnt ncbi-func">SpawnVP()</span>: In the <span class="nctnt ncbi-func">SpawnVP()</span> version, the command-line arguments are a variable number. The array of pointers to arguments must have a length of 1 or more and you must assign parameters for the new process beginning from 1. The <span class="nctnt ncbi-var">PATH</span> environment variable is used to find the file to execute.

-   <span class="nctnt ncbi-func">SpawnVPE()</span>: In the <span class="nctnt ncbi-func">SpawnVPE()</span> version, the command-line arguments are a variable number. The array of pointers to arguments must have a length of 1 or more and you must assign parameters for the new process beginning from 1. The <span class="nctnt ncbi-var">PATH</span> environment variable is used to find the file to execute, and the environment is passed via an environment vector pointer.

Refer to the [include/corelib/ncbiexec.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbiexec.hpp) file to view the exact form of the <span class="nctnt ncbi-func">SpawnX()</span> function calls.

Some sample code illustrating the use of these methods is shown in [src/corelib/test/test\_ncbiexec.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test/test_ncbiexec.cpp)

### Waiting for a Process to Terminate using the Wait() method

The <span class="nctnt ncbi-class">CExec</span> class defines a <span class="nctnt ncbi-func">Wait()</span> method that causes a process to wait until the child process terminates:

    static int Wait(const int pid);

The argument to the <span class="nctnt ncbi-func">Wait()</span> method is the pid (process ID) of the child process on which the caller is waiting to terminate. <span class="nctnt ncbi-func">Wait()</span> returns immediately if the specified child process has already terminated and returns an exit code of the child process, if there are no errors; or a -1, if an error has occurred.

Implementing Parallelism using Threads and Synchronization Mechanisms
---------------------------------------------------------------------

This section provides reference information on how to add multithreading to your application and how to use basic synchronization objects. For an overview of these concepts refer to the [introductory topic on this subject](ch_intro.html#ch_intro.intro_threads).

Note that all classes are defined in [include/corelib/ncbithr.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbithr.hpp) and [include/corelib/ncbimtx.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbimtx.hpp).

The following topics are discussed in this section:

-   [Using Threads](#using-threads)

-   [CThread class public methods](#cthread-class-public-methods)

-   [CThread class protected methods](#cthread-class-protected-methods)

-   [Thread Life Cycle](#thread-life-cycle)

-   [Referencing thread objects](#referencing-thread-objects)

-   [Synchronization](#synchronization)

-   [Thread local storage (CTls\<\> class [\*])](#thread-local-storage-ctls<>-class-*)

### Using Threads

<span class="nctnt ncbi-class">CThread</span> class is defined in <span class="nctnt ncbi-path">include/corelib/ncbithr.hpp</span>. The <span class="nctnt ncbi-class">CThread</span> class provides all basic thread functionality: thread creation, launching, termination, and cleanup. To create user-defined thread one needs only to provide the thread's <span class="nctnt ncbi-func">Main()</span> function and, in some cases, create a new constructor to transfer data to the thread object, and override <span class="nctnt ncbi-func">OnExit()</span> method for thread-specific data cleanup. To create a custom thread:

1  
Derive your class from <span class="nctnt ncbi-class">CThread</span>, override <span class="nctnt ncbi-func">Main()</span> and, if necessary, <span class="nctnt ncbi-func">OnExit()</span> methods.

2  
Create thread object in your application. You can do this only with <span class="nctnt ncbi-code">new</span> operator, since static or in-stack thread objects are prohibited (see below). The best way to reference thread objects is to use <span class="nctnt ncbi-type">CRef\<CThread\></span> class.

3  
Call <span class="nctnt ncbi-func">Run()</span> to start the thread execution.

4  
Call <span class="nctnt ncbi-func">Detach()</span> to let the thread run independently (it will destroy itself on termination then), or use <span class="nctnt ncbi-func">Join()</span> to wait for the thread termination.

The code should look like:

    #include <corelib/ncbistd.hpp>
    #include <corelib/ncbithr.hpp>
    USING_NCBI_SCOPE;
    class CMyThread : public CThread
    {
    public:
        CMyThread(int index) : m_Index(index) {}
        virtual void* Main(void);
        virtual void OnExit(void);
    private:
        int m_Index;
        int* heap_var;
    };
    void* CMyThread::Main(void)
    {
        cout << "Thread " << m_Index << endl;
        heap_var = new int; // to be destroyed by OnExit()
        *heap_var = 12345;
        int* return_value = new int; // return to the main thread
        *return_value = m_Index;
        return return_value;
    }
    void CMyThread::OnExit(void)
    {
        delete heap_var;
    }
    int main(void)
    {
        CMyThread* thread = new CMyThread(33);
        thread->Run();
        int* result;
        thread->Join(reinterpret_cast<void**>(&result));
        cout << "Returned value: " << *result << endl;
        delete result;
        return 0;
    }

The above simple application will start one child thread, passing <span class="nctnt ncbi-monospace">33</span> as the <span class="nctnt ncbi-var">index</span> value. The thread prints <span class="nctnt ncbi-monospace">"Thread 33"</span> message, allocates and initializes two integer variables, and terminates. The thread's <span class="nctnt ncbi-func">Main()</span> function returns a pointer to one of the allocated values. This pointer is then passed to <span class="nctnt ncbi-func">Join()</span> method and can be used by another thread. The other integer allocated by <span class="nctnt ncbi-func">Main()</span> is destroyed by <span class="nctnt ncbi-func">OnExit()</span> method.

It is important not to terminate the program until there are running threads. Program termination will cause all the running threads to terminate also. In the above example <span class="nctnt ncbi-func">Join()</span> function is used to wait for the child thread termination.

The following subsections discuss the individual classes in more detail.

### CThread ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CThread)) class public methods

<span class="nctnt ncbi-func">CThread(void)</span> Create the thread object (without running it). <span class="nctnt ncbi-func">bool Run(void)</span> Spawn the new thread, initialize internal <span class="nctnt ncbi-class">CThread</span> data and launch user-provided <span class="nctnt ncbi-func">Main()</span>. The method guarantees that the new thread will start before it returns to the calling function. <span class="nctnt ncbi-func">void Detach(void)</span> Inform the thread that user does not need to wait for its termination. Detached thread will destroy itself after termination. If <span class="nctnt ncbi-func">Detach()</span> is called for a thread, which has already terminated, it will be scheduled for destruction immediately. Only one call to <span class="nctnt ncbi-func">Detach()</span> is allowed for each thread object. <span class="nctnt ncbi-func">void Join(void\*\* exit\_data)</span> Wait for the thread termination. <span class="nctnt ncbi-func">Join()</span> will store the <span class="nctnt ncbi-type">void</span> pointer as returned by the user's <span class="nctnt ncbi-func">Main()</span> method, or passed to the <span class="nctnt ncbi-func">Exit()</span> function to the <span class="nctnt ncbi-var">exit\_data</span>. Then the thread will be scheduled for destruction. Only one call to <span class="nctnt ncbi-func">Join()</span> is allowed for each thread object. If called more than once, <span class="nctnt ncbi-func">Join()</span> will cause a runtime error. <span class="nctnt ncbi-func">static void Exit(void\* exit\_data)</span> This function may be called by a thread object itself to terminate the thread. The thread will be terminated and, if already detached, scheduled for destruction. <span class="nctnt ncbi-var">exit\_data</span> value is transferred to the <span class="nctnt ncbi-func">Join()</span> function as if it was returned by the <span class="nctnt ncbi-func">Main()</span>. <span class="nctnt ncbi-func">Exit()</span> will also call virtual method <span class="nctnt ncbi-func">OnExit()</span> to execute user-provided cleanup code (if any). <span class="nctnt ncbi-func">bool Discard(void)</span> Schedules the thread object for destruction if it has not been run yet. This function is provided since there is no other way to delete a thread object without running it. On success, return <span class="nctnt ncbi-monospace">true</span>. If the thread has already been run, <span class="nctnt ncbi-func">Discard()</span> do nothing and return <span class="nctnt ncbi-monospace">false</span>. <span class="nctnt ncbi-func">static CThread::TID GetSelf(void)</span> This method returns a unique thread ID. This ID may be then used to identify threads, for example, to track the owner of a shared resource. Since the main thread has no associated <span class="nctnt ncbi-class">CThread</span> object, a special value of 0 (zero) is reserved for the main thread ID.

### CThread ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CThread)) class protected methods

<span class="nctnt ncbi-func">virtual void\* Main(void)Main()</span> is the thread's main function (just like an application <span class="nctnt ncbi-func">main()</span> function). This method is not defined in the <span class="nctnt ncbi-class">CThread</span> class. It must be provided by derived user-defined class. The return value is passed to the <span class="nctnt ncbi-func">Join()</span> function (and thus may be used by another thread for some sort of inter-thread communication). <span class="nctnt ncbi-func">virtual void OnExit(void)</span> This method is called (in the context of the thread) just before the thread termination to cleanup thread-specific resources. <span class="nctnt ncbi-func">OnExit()</span> is NOT called by <span class="nctnt ncbi-func">Discard()</span>, since the thread has not been run in this case and there are no thread-specific data to destroy. <span class="nctnt ncbi-func">virtual ~CThread(void)</span> The destructor is protected to avoid thread object premature destruction. For this reason, no thread object can be static or stack-allocated. It is important to declare any <span class="nctnt ncbi-func">CThread</span> derived class destructor as <span class="nctnt ncbi-code">protected</span>.

### Thread Life Cycle

[Figure 2](#figure-2) shows a typical thread life cycle. The figure demonstrates that thread constructors are called from the parent thread. The child thread is spawned by the <span class="nctnt ncbi-func">Run()</span> function only. Then, the user-provided <span class="nctnt ncbi-func">Main()</span> method (containing code created by user) gets executed. The thread's destructor may be called in the context of either parent or child thread depending on the state of the thread at the moment when <span class="nctnt ncbi-func">Join()</span> or <span class="nctnt ncbi-func">Detach()</span> is called.

[![Figure 2. Thread Life Cycle](/book-test/static/img/thread_life_cycle.gif)](/book-test/static/img/thread_life_cycle.gif "Click to see the full-resolution image")

Figure 2. Thread Life Cycle

There are two possible ways to terminate a thread. By default, after user-provided <span class="nctnt ncbi-func">Main()</span> function return, the <span class="nctnt ncbi-func">Exit()</span> is called implicitly to terminate the thread. User functions can call <span class="nctnt ncbi-func">CThread::Exit()</span> directly. Since <span class="nctnt ncbi-func">Exit()</span> is a static method, the calling function does not need to be a thread class member or have a reference to the thread object. <span class="nctnt ncbi-func">Exit()</span> will terminate the thread in which context it is called.

The <span class="nctnt ncbi-class">CThread</span> destructor is <span class="nctnt ncbi-code">protected</span>. The same must be true for any user-defined thread class in order to prohibit creation of static or automatic thread objects. For the same reason, a thread object can not be destroyed by explicit <span class="nctnt ncbi-code">delete</span>. All threads destroy themselves on termination, detaching, or joining.

On thread termination, <span class="nctnt ncbi-func">Exit()</span> checks if the thread has been detached and, if this is true, destroys the thread object. If the thread has not been detached, the thread object will remain "zombie" unless detached or joined. Either <span class="nctnt ncbi-func">Detach()</span> or <span class="nctnt ncbi-func">Join()</span> will destroy the object if the thread has been terminated. One should keep in mind, that it is not safe to use the thread object after a call to <span class="nctnt ncbi-func">Join()</span> or <span class="nctnt ncbi-func">Detach()</span> since the object may happen to be destroyed. To avoid this situation, the <span class="nctnt ncbi-type">CRef\<CThread\></span> can be used. The thread object will not be destroyed until there is at least one <span class="nctnt ncbi-class">CRef</span> to the object (although it may be terminated and scheduled for destruction).

In other words, a thread object will be destroyed when all of the following conditions are satisfied:

-   the thread has been run and terminated by an implicit or explicit call to <span class="nctnt ncbi-func">Exit()</span>

-   the thread has been detached or joined

-   no <span class="nctnt ncbi-class">CRef</span> references the thread object

Which thread will actually destroy a thread object depends on several conditions. If the thread has been detached before termination, the <span class="nctnt ncbi-func">Exit()</span> method will destroy it, provided there are no <span class="nctnt ncbi-class">CRef</span> references to the object. When joined, the thread will be destroyed in the context of a joining thread. If <span class="nctnt ncbi-func">Detach()</span> is called after thread termination, it will destroy the thread in the context of detaching thread. And, finally, if there are several <span class="nctnt ncbi-class">CRef</span> objects referencing the same thread, it will be destroyed after the last <span class="nctnt ncbi-class">CRef</span> release.

This means that cleaning up thread-specific data can not be done from the thread destructor. One should override <span class="nctnt ncbi-func">OnExit()</span> method instead. <span class="nctnt ncbi-func">OnExit()</span> is guaranteed to be called in the context of the thread before the thread termination. The destructor can be used to cleanup non-thread-local data only.

There is one more possibility to destroy a thread. If a thread has been created, but does not need to be run, one can use <span class="nctnt ncbi-func">Discard()</span> method to destroy the thread object without running it. Again, the object will not be destroyed until there are <span class="nctnt ncbi-class">CRef</span>s referencing it.

### Referencing Thread Objects

It should be emphasized that regular (C) pointer to a thread object is not reliable. The thread may terminate at unpredictable moment, destroying itself. There is no possibility to safely access thread object after <span class="nctnt ncbi-func">Join()</span> using C pointers. The only solution to this problem is to use <span class="nctnt ncbi-class">CRef</span> class. <span class="nctnt ncbi-class">CThread</span> class provides a mechanism to prevent premature destruction if there are <span class="nctnt ncbi-class">CRef</span> references to the thread object.

### Thread local storage (CTls\<\> class [[\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CTls)])

The library provides a template class to store thread specific data: <span class="nctnt ncbi-class">CTls\<\></span>. This means that each thread can keep its own data in the same TLS object. To perform any kind of cleanup one can provide cleanup function and additional cleanup data when storing a value in the TLS object. The following example demonstrates the usage of TLS:

    CRef< CTls<int> > tls(new CTls<int>);
    void TlsCleanup(int* p_value, void* /* data */ )
    {
        delete p_value;
    }
    ...
    void* CMyThread::Main()
    {
        int* p_value = new int;
        *p_value = 1;
        tls->SetValue(p_value, TlsCleanup);
        ...
        p_value = new int;
        *p_value = 2;
        tls->SetValue(p_value, TlsCleanup);
        ...
        if (*tls->GetValue() == 2) {
            ...
        }
        ...
    }

In the above example the second call to <span class="nctnt ncbi-func">SetValue()</span> will cause the <span class="nctnt ncbi-func">TlsCleanup()</span> to deallocate the first integer variable. To cleanup the last value stored in each TLS, the <span class="nctnt ncbi-func">CThread::Exit()</span> function will automatically call <span class="nctnt ncbi-func">CTls\<\>::Reset()</span> for each TLS used by the thread.

By default, all TLS objects are destroyed on program termination, since in most cases it is not guaranteed that a TLS object is not (or will not be) used by a thread. For the same reason the <span class="nctnt ncbi-class">CTls\<\></span> destructor is protected, so that no TLS can be created in the stack memory. The best way of keeping TLS objects is to use <span class="nctnt ncbi-class">CRef</span>.

Calling <span class="nctnt ncbi-func">Discard()</span> will schedule the TLS to be destroyed as soon as there are no <span class="nctnt ncbi-class">CRef</span> references to the object left. The method should be used with care.

### Mutexes

The <span class="nctnt ncbi-path">ncbimtx.hpp</span> defines platform-independent mutex classes, <span class="nctnt ncbi-class">CMutex</span>, <span class="nctnt ncbi-class">CFastMutex</span>, <span class="nctnt ncbi-class">CMutexGuard</span>, and <span class="nctnt ncbi-class">CFastMutexGuard</span>. These mutex classes are in turn built on the platform-dependent mutex classes <span class="nctnt ncbi-class">SSystemMutex</span> and <span class="nctnt ncbi-class">SSystemFastMutex</span>.

In addition to the mutex classes, there are a number of classes that can be used for explicit locks such as the <span class="nctnt ncbi-class">CRWLock</span>, <span class="nctnt ncbi-class">CAutoRW</span>, <span class="nctnt ncbi-class">CReadLockGuard</span>, <span class="nctnt ncbi-class">CWriteLockGuard</span> and the platform-dependent read/write lock, <span class="nctnt ncbi-class">CInternalRWLock</span>.

Finally, there is the <span class="nctnt ncbi-class">CSemaphore</span> class which is an application-wide semaphore.

These classes are discussed in the subsections that follow:

-   [CMutex](#cmutex)

-   [CFastMutex](#cfastmutex)

-   [SSystemMutex and SSystemFastMutex](#ssystemmutex-and-ssystemfastmutex)

-   [CMutexGuard and CFastMutexGuard](#cmutexguard-and-cfastmutexguard)

-   [Lock Classes](#lock-classes)

#### CMutex

The <span class="nctnt ncbi-class">CMutex</span> class provides the API for acquiring a mutex. This mutex allows nesting with runtime checks so recursive locks by the same thread is possible. This mutex checks the mutex owner before unlocking. <span class="nctnt ncbi-class">CMutex</span> is slower than <span class="nctnt ncbi-class">CFastMutex</span> and should be used when performance is less important than data protection. If performance is more important than data protection, use <span class="nctnt ncbi-class">CFastMutex</span>, instead.

The main methods for <span class="nctnt ncbi-class">CMutex</span> operation are <span class="nctnt ncbi-func">Lock()</span>, <span class="nctnt ncbi-func">TryLock()</span> and <span class="nctnt ncbi-func">Unlock()</span>:

    void Lock(void);
    bool TryLock(void);
    void Unlock(void);

The <span class="nctnt ncbi-func">Lock()</span> mutex method is used by a thread to acquire a lock. The lock can be acquired only if the mutex is unlocked; that is, not in use. If a thread has acquired a lock before, the lock counter is incremented. This is called nesting. The lock counter is only decremented when the same thread issues an <span class="nctnt ncbi-func">Unlock()</span>. In other words, each call to <span class="nctnt ncbi-func">Lock()</span> must have a corresponding <span class="nctnt ncbi-func">Unlock()</span> by the same thread. If the mutex has been locked by another thread, then the thread must wait until it is unlocked. When the mutex is unlocked, the waiting thread can acquire the lock. This, then, is like a lock on an unlocked mutex.

The <span class="nctnt ncbi-func">TryLock()</span> mutex can be used to probe the mutex to see if a lock is possible, and if it is, acquire a lock on the mutex. If the mutex has already been locked, <span class="nctnt ncbi-func">TryLock()</span> returns <span class="nctnt ncbi-monospace">FALSE</span>. If the mutex is unlocked, than <span class="nctnt ncbi-func">TryLock()</span> acquires a lock on the mutex just as <span class="nctnt ncbi-func">Lock()</span> does, and returns <span class="nctnt ncbi-monospace">TRUE</span>.

The <span class="nctnt ncbi-func">Unlock()</span> method is used to decrease the lock counter if the mutex has been acquired by this thread. When the lock counter becomes zero, then the mutex is completely released (unlocked). If the mutex is not locked or locked by another thread, then the exception CMutexException (eOwner) is thrown.

The <span class="nctnt ncbi-class">CMutex</span> uses the functionality of <span class="nctnt ncbi-class">CFastMutex</span>. Because <span class="nctnt ncbi-class">CMutex</span> allows nested locks and performs checks of mutex owner it is somewhat slower than <span class="nctnt ncbi-class">CFastMutex</span>, but capable of protecting complicated code, and safer to use. To guarantee for a mutex release, <span class="nctnt ncbi-class">CMutexGuard</span> can be used. The mutex is locked by the <span class="nctnt ncbi-class">CMutexGuard</span> constructor and unlocked by its destructor. Macro <span class="nctnt ncbi-macro">DEFINE\_STATIC\_MUTEX(id)</span> will define static mutex variable with name <span class="nctnt ncbi-var">id</span>. Macro <span class="nctnt ncbi-macro">DECLARE\_CLASS\_STATIC\_MUTEX(id)</span> will declare static class member of mutex type name <span class="nctnt ncbi-var">id</span>. Macro <span class="nctnt ncbi-macro">DEFINE\_CLASS\_STATIC\_MUTEX(class, id)</span> will define class static mutex variable <span class="nctnt ncbi-var">class::id</span>. The following example demonstrates usage of <span class="nctnt ncbi-var">CMutex</span>, including lock nesting:

    static int Count = 0;
    DEFINE_STATIC_MUTEX(CountMutex);

    void Add2(void)
    {
        CMutexGuard guard(CountMutex);
        Count += 2;
        if (Count < 20) {
            Add3();
        }
    }

    void Add3(void)
    {
        CMutexGuard guard(CountMutex);
        Count += 3;
        if (Count < 20) {
            Add2();
        }
    }

This example will result in several nested locks of the same mutex with the guaranteed release of each lock.

It is important not to unlock the mutex protected by a mutex guard. <span class="nctnt ncbi-class">CFastMutexGuard</span> and <span class="nctnt ncbi-class">CMutexGuard</span> both unlock the associated mutex on destruction. It the mutex is already unlocked this will cause a runtime error. Instead of unlocking the mutex directly one can use <span class="nctnt ncbi-func">CFastMutexGuard::Release()</span> or <span class="nctnt ncbi-func">CMutexGuard::Release()</span> method. These methods unlock the mutex and unlink it from the guard.

In addition to usual <span class="nctnt ncbi-func">Lock()</span> and <span class="nctnt ncbi-func">Unlock()</span> methods, the <span class="nctnt ncbi-class">CMutex</span> class implements a method to test the mutex state before locking it. <span class="nctnt ncbi-func">TryLock()</span> method attempts to acquire the mutex for the calling thread and returns <span class="nctnt ncbi-monospace">true</span> on success (this includes nested locks by the same thread) or <span class="nctnt ncbi-monospace">false</span> if the mutex has been acquired by another thread. After a successful <span class="nctnt ncbi-func">TryLock()</span> the mutex should be unlocked like after regular <span class="nctnt ncbi-func">Lock()</span>.

#### CFastMutex

The <span class="nctnt ncbi-class">CFastMutex</span> class provides the API for acquiring a mutex. Unlike <span class="nctnt ncbi-class">CMutex</span>, this mutex does not permit nesting and does not check the mutex owner before unlocking. <span class="nctnt ncbi-class">CFastMutex</span> is, however, faster than <span class="nctnt ncbi-class">CMutex</span> and should be used when performance is more important than data protection. If performance is less important than data protection, use <span class="nctnt ncbi-class">CMutex</span>, instead.

The main methods for <span class="nctnt ncbi-class">CFastMutex</span> operation are <span class="nctnt ncbi-func">Lock()</span>, <span class="nctnt ncbi-func">TryLock()</span> and <span class="nctnt ncbi-func">Unlock()</span>:

    void Lock(void);
    bool TryLock(void);
    void Unlock(void);

The <span class="nctnt ncbi-func">Lock()</span> mutex method is used by a thread to acquire a lock without any nesting or ownership checks.

The <span class="nctnt ncbi-func">TryLock()</span> mutex can be used to probe the mutex to see if a lock is possible, and if it is, acquire a lock on the mutex. If the mutex has already been locked, <span class="nctnt ncbi-func">TryLock()</span> returns <span class="nctnt ncbi-monospace">FALSE</span>. If the mutex is unlocked, than <span class="nctnt ncbi-func">TryLock()</span> acquires a lock on the mutex just as <span class="nctnt ncbi-func">Lock()</span> does, and returns <span class="nctnt ncbi-monospace">TRUE</span>. The locking is done without any nesting or ownership checks.

The <span class="nctnt ncbi-func">Unlock()</span> method is used to unlock the mutex without any nesting or ownership checks.

The <span class="nctnt ncbi-class">CFastMutex</span> should be used only to protect small and simple parts of code. To guarantee for the mutex release the <span class="nctnt ncbi-class">CFastMutexGuard</span> class may be used. The mutex is locked by the <span class="nctnt ncbi-class">CFastMutexGuard</span> constructor and unlocked by its destructor. To avoid problems with initialization of static objects on different platforms, special macro definitions are intended to be used to declare static mutexes. Macro <span class="nctnt ncbi-macro">DEFINE\_STATIC\_FAST\_MUTEX(id)</span> will define static mutex variable with name <span class="nctnt ncbi-var">id</span>. Macro <span class="nctnt ncbi-macro">DECLARE\_CLASS\_STATIC\_FAST\_MUTEX(id)</span> will declare static class member of mutex type with name <span class="nctnt ncbi-var">id</span>. Macro <span class="nctnt ncbi-macro">DEFINE\_CLASS\_STATIC\_FAST\_MUTEX(class, id)</span> will define static class mutex variable <span class="nctnt ncbi-var">class::id</span>. The example below demonstrates how to protect an integer variable with the fast mutex:

    void ThreadSafe(void)
    {
        static int Count = 0;
        DEFINE_STATIC_FAST_MUTEX(CountMutex);
        ...
        { {
            CFastMutexGuard guard(CountMutex);
            Count++;
        } }
        ...
    }

#### SSystemMutex and SSystemFastMutex

The <span class="nctnt ncbi-class">CMutex</span> class is built on the platform-dependent mutex class, <span class="nctnt ncbi-class">SSystemMutex</span>. The <span class="nctnt ncbi-class">SSystemMutex</span> is in turn built using the <span class="nctnt ncbi-class">SSystemFastMutex</span> class with additional provisions for keeping track of the thread ownership using the <span class="nctnt ncbi-class">CThreadSystemID</span>, and a counter for the number of in the same thread locks (nested or recursive locks).

Each of the <span class="nctnt ncbi-class">SSystemMutex</span> and <span class="nctnt ncbi-class">SSystemFastMutex</span> classes have the <span class="nctnt ncbi-func">Lock()</span>, <span class="nctnt ncbi-func">TryLock()</span> and <span class="nctnt ncbi-func">Unlock()</span> methods that are platform specific. These methods are used by the platform independent classes, <span class="nctnt ncbi-class">CMutex</span> and <span class="nctnt ncbi-class">CFastMutex</span> to provide locking and unlocking services.

#### CMutexGuard and CFastMutexGuard

The <span class="nctnt ncbi-class">CMutexGuard</span> and the <span class="nctnt ncbi-class">CFastMutexGuard</span> classes provide platform independent read and write lock guards to the mutexes. These classes are aliased as typedefs TReadLockGuard and TWriteLockGuard in the <span class="nctnt ncbi-class">CMutexGuard</span> and the <span class="nctnt ncbi-class">CFastMutexGuard</span> classes.

#### Lock Classes

This class implements sharing a resource between multiple reading and writing threads. The following rules are used for locking:

-   if unlocked, the RWLock can be acquired for either R-lock or W-lock

-   if R-locked, the RWLock can be R-locked by the same thread or other threads

-   if W-locked, the RWLock can not be acquired by other threads (a call to <span class="nctnt ncbi-func">ReadLock()</span> or <span class="nctnt ncbi-func">WriteLock()</span> by another thread will suspend that thread until the RW-lock release).

-   R-lock after W-lock by the same thread is allowed but treated as a nested W-lock

-   W-lock after R-lock by the same thread results in a runtime error

Like <span class="nctnt ncbi-class">CMutex</span>, CRWLock also provides methods for checking its current state: <span class="nctnt ncbi-func">TryReadLock()</span> and <span class="nctnt ncbi-func">TryWriteLock()</span>. Both methods try to acquire the RW-lock, returning <span class="nctnt ncbi-monospace">true</span> on success (the RW-lock becomes R-locked or W-locked) or <span class="nctnt ncbi-monospace">false</span> if the RW-lock can not be acquired for the calling thread.

The following subsections describe these locks in more detail:

-   [CRWLock](#crwlock)

-   [CAutoRW](#cautorw)

-   [CReadLockGuard](#creadlockguard)

-   [CWriteLockGuard](#cwritelockguard)

-   [CInternalRWLock](#cinternalrwlock)

-   [CSemaphore](#csemaphore)

##### CRWLock

The <span class="nctnt ncbi-class">CRWLock</span> class allows read-after-write (R-after-W) locks for multiple readers or a single writer with recursive locks. The R-after-W lock is considered to be a recursive Write-lock. The write-after-read (W-after-R) is not permitted and can be caught when <span class="nctnt ncbi-macro">\_DEBUG</span> is defined. When <span class="nctnt ncbi-macro">\_DEBUG</span> is not defined, it does not always detect the W-after-R correctly, so a deadlock can occur in these circumstances. Therefore, it is important to test your application in the <span class="nctnt ncbi-macro">\_DEBUG</span> mode first.

The main methods in the class API are <span class="nctnt ncbi-func">ReadLock()</span>, <span class="nctnt ncbi-func">WriteLock()</span>, <span class="nctnt ncbi-func">TryReadLock()</span>, <span class="nctnt ncbi-func">TryWriteLock()</span> and <span class="nctnt ncbi-func">Unlock()</span>.

    void ReadLock(void);
    void WriteLock(void);
    bool TryReadLock(void);
    bool TryWriteLock(void);
    void Unlock(void);

The <span class="nctnt ncbi-func">ReadLock()</span> is used to acquire a read lock. If a write lock has already been acquired by another thread, then this thread waits until it is released.

The <span class="nctnt ncbi-func">WriteLock()</span> is used to acquire a write lock. If a read or write lock has already been acquired by another thread, then this thread waits until it is released.

The <span class="nctnt ncbi-func">TryReadLock()</span> and <span class="nctnt ncbi-func">TryWriteLock()</span> methods are used to try and acquire a read or write lock, respectively, if at all possible. If a lock cannot be acquired, they immediately return with a <span class="nctnt ncbi-monospace">FALSE</span> value and do not wait to acquire a lock like the <span class="nctnt ncbi-func">ReadLock()</span> and <span class="nctnt ncbi-func">WriteLock()</span> methods. If a lock is successfully acquired, a <span class="nctnt ncbi-monospace">TRUE</span> value is returned.

As expected from the name, the <span class="nctnt ncbi-func">Unlock()</span> method releases the RW-lock.

##### CAutoRW

The <span class="nctnt ncbi-class">CAutoRW</span> class is used to provide a Read Write lock that is automatically released by the <span class="nctnt ncbi-class">CAutoRW</span> class' destructor. The locking mechanism is provided by a <span class="nctnt ncbi-class">CRWLock</span> object that is initialized when the <span class="nctnt ncbi-class">CAutoRW</span> class constructor is called.

An acquired lock can be released by an explicit call to the class <span class="nctnt ncbi-func">Release()</span> method. The lock can also be released by the class destructor. When the destructor is called the lock if successfully acquired and not already released by <span class="nctnt ncbi-func">Release()</span> is released.

##### CReadLockGuard

The <span class="nctnt ncbi-class">CReadLockGuard</span> class is used to provide a basic read lock guard that can be used by other classes. This class is derived from the <span class="nctnt ncbi-class">CAutoRW</span> class.

The class constructor can be passed a <span class="nctnt ncbi-class">CRWLock</span> object on which a read lock is acquired, and which is registered to be released by the class destructor. The class's <span class="nctnt ncbi-func">Guard()</span> method can also be called with a <span class="nctnt ncbi-class">CRWLock</span> object and if this is not the same as the already registered <span class="nctnt ncbi-class">CRWLock</span> object, the old registered object is released, and the new <span class="nctnt ncbi-class">CRWLock</span> object is registered and a read lock acquired on it.

##### CWriteLockGuard

The <span class="nctnt ncbi-class">CWriteLockGuard</span> class is used to provide a basic write lock guard that can be used by other classes. The <span class="nctnt ncbi-class">CWriteLockGuard</span> class is similar to the <span class="nctnt ncbi-class">CReadLockGuard</span> class except that it provides a write lock instead of a read lock. This class is derived from the <span class="nctnt ncbi-class">CAutoRW</span> class.

The class constructor can be passed a <span class="nctnt ncbi-class">CRWLock</span> object on which a write lock is acquired, and which is registered to be released by the class destructor. The class's <span class="nctnt ncbi-func">Guard()</span> method can also be called with a <span class="nctnt ncbi-class">CRWLock</span> object and if this is not the same as the already registered <span class="nctnt ncbi-class">CRWLock</span> object, the old registered object is released, and the new <span class="nctnt ncbi-class">CRWLock</span> object is registered and a write lock acquired on it.

##### CInternalRWLock

The <span class="nctnt ncbi-class">CInternalRWLock</span> class holds platform dependent RW-lock data such as data on semaphores and mutexes. This class is not meant to be used directly by user applications. This class is used by other classes such as the <span class="nctnt ncbi-class">CRWLock</span> class.

##### CSemaphore

The <span class="nctnt ncbi-class">CSemaphore</span> class implements a general purpose counting semaphore. The constructor is passed an initial count for the semaphore and a maximum semaphore count.

When the <span class="nctnt ncbi-func">Wait()</span> method is executed for the semaphore, the counter is decremented by one. If the semaphore's count is zero then the thread waits until it is not zero. A variation on the <span class="nctnt ncbi-func">Wait()</span> method is the <span class="nctnt ncbi-func">TryWait()</span> method which is used to prevent long waits. The <span class="nctnt ncbi-func">TryWait()</span> can be passed a timeout value in seconds and nanoseconds:

    bool TryWait(unsigned int timeout_sec = 0, unsigned int timeout_nsec = 0);

The <span class="nctnt ncbi-func">TryWait()</span> method can wait for the specified time for the semaphore's count to exceed zero. If that happens, the counter is decremented by one and <span class="nctnt ncbi-func">TryWait()</span> returns <span class="nctnt ncbi-monospace">TRUE</span>; otherwise, it returns <span class="nctnt ncbi-monospace">FALSE</span>.

The semaphore count is incremented by the <span class="nctnt ncbi-func">Post()</span> method and an exception is thrown if the maximum count is exceeded.

Working with File and Directories Using CFile and CDir
------------------------------------------------------

An application may need to work with files and directories. The CORELIB provides a number of portable classes to model a system file and directory. The base class for the files and directories is <span class="nctnt ncbi-class">CDirEntry</span>. Other classes such as <span class="nctnt ncbi-class">CDir</span> and <span class="nctnt ncbi-class">CFile</span> that deal with directories and files are derived form this base class.

The following sections discuss the file and directory classes in more detail:

-   [Executing a System Command using the System() Method](#executing-a-system-command-using-the-system-method)

-   [Defining Spawned Process Modes (EMode type)](#defining-spawned-process-modes-emode-type)

-   [Spawning a Process using SpawnX() Methods](#spawning-a-process-using-spawnx-methods)

-   [Waiting for a Process to Terminate using the Wait() method](#waiting-for-a-process-to-terminate-using-the-wait-method)

### CDirEntry class

This class models the directory entry structure for the file system and assumes that the path argument has the following form, where any or all components may be missing:

    <dir><title><ext>

where:

-   <span class="nctnt ncbi-path">\<dir\></span> -- is the file path ("<span class="nctnt ncbi-path">/usr/local/bin/</span>" or "<span class="nctnt ncbi-path">c:\\windows\\</span>")

-   <span class="nctnt ncbi-path">\<title\></span> -- is the file name without ext ("<span class="nctnt ncbi-path">autoexec</span>")

-   <span class="nctnt ncbi-path">\<ext\></span> -- is the file extension ("<span class="nctnt ncbi-path">.bat</span>" - whatever goes after the last dot)

The supported filename formats are for the Windows, Unix, and Mac file systems.

The [CDirEntry](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDirEntry.html) class provides the base methods such as the following for dealing with the components of a path name :

-   <span class="nctnt ncbi-func">GetPath()</span>: Get pathname.

-   <span class="nctnt ncbi-func">GetDir()</span>: Get the Directory component for this directory entry.

-   <span class="nctnt ncbi-func">GetBase()</span>: Get the base entry name without extension.

-   <span class="nctnt ncbi-func">GetName()</span>: Get the base entry name with extension.

-   <span class="nctnt ncbi-func">GetExt()</span>: Get the extension name.

-   <span class="nctnt ncbi-func">MakePath()</span>: Given the components of a path, combine them to create a path string.

-   <span class="nctnt ncbi-func">SplitPath()</span>: Given a path string, split them into its constituent components.

-   <span class="nctnt ncbi-func">GetPathSeparator()</span>: Get path separator symbol specific for the platform such as a '\\' or '/'.

-   <span class="nctnt ncbi-func">IsPathSeparator()</span>: Check character "c" as path separator symbol specific for the platform.

-   <span class="nctnt ncbi-func">AddTrailingPathSeparator()</span>: Add a trailing path separator, if needed.

-   <span class="nctnt ncbi-func">ConvertToOSPath()</span>: Convert relative "path" on any OS to current OS dependent relative path.

-   <span class="nctnt ncbi-func">IsAbsolutePath()</span>: Note that the "path" must be for current OS.

-   <span class="nctnt ncbi-func">ConcatPath()</span>: Concatenate the two parts of the path for the current OS.

-   <span class="nctnt ncbi-func">ConcatPathEx()</span>: Concatenate the two parts of the path for any OS.

-   <span class="nctnt ncbi-func">MatchesMask()</span>: Match "name" against the filename "mask".

-   <span class="nctnt ncbi-func">Rename()</span>: Rename entry to specified "new\_path".

-   <span class="nctnt ncbi-func">Remove()</span>: Remove the directory entry.

The last method on the list, the <span class="nctnt ncbi-func">Remove()</span> method accepts an enumeration type parameter, <span class="nctnt ncbi-type">EDirRemoveMode</span>, which specifies the extent of the directory removal operation - you can delete only an empty directory, only files in a directory but not any subdirectories, or remove the entire directory tree:

    /// Directory remove mode.
    enum EDirRemoveMode {
        eOnlyEmpty,     ///< Remove only empty directory
        eNonRecursive,  ///< Remove all files in directory, but not remove
                        ///< subdirectories and files in it
        eRecursive      ///< Remove all files and subdirectories
    };

<span class="nctnt ncbi-class">CDirEntry</span> knows about different types of files or directory entries. Most of these file types are modeled after the Unix file system but can also handle the file system types for the Windows platform. The different file system types are represented by the enumeration type <span class="nctnt ncbi-type">EType</span> which is defined as follows :

    /// Which directory entry type.
    enum EType {
        eFile = 0,     ///< Regular file
        eDir,          ///< Directory
        ePipe,         ///< Pipe
        eLink,         ///< Symbolic link     (Unix only)
        eSocket,       ///< Socket            (Unix only)
        eDoor,         ///< Door              (Unix only)
        eBlockSpecial, ///< Block special     (Unix only)
        eCharSpecial,  ///< Character special
        //
        eUnknown       ///< Unknown type
    };

<span class="nctnt ncbi-class">CDirEntry</span> knows about permission settings for a directory entry. Again, these are modeled after the Unix file system. The different permissions are represented by the enumeration type <span class="nctnt ncbi-type">EMode</span> which is defined as follows :

    /// Directory entry's access permissions.
    enum EMode {
        fExecute = 1,       ///< Execute permission
        fWrite   = 2,       ///< Write permission
        fRead    = 4,       ///< Read permission
        // initial defaults for dirs
        fDefaultDirUser  = fRead \| fExecute \| fWrite,
                            ///< Default user permission for dir.
        fDefaultDirGroup = fRead \| fExecute,
                            ///< Default group permission for dir.
        fDefaultDirOther = fRead \| fExecute,
                            ///< Default other permission for dir.
        // initial defaults for non-dir entries (files, etc.)
        fDefaultUser     = fRead \| fWrite,
                            ///< Default user permission for file
        fDefaultGroup    = fRead,
                            ///< Default group permission for file
        fDefaultOther    = fRead,
                            ///< Default other permission for file
        fDefault = 8        ///< Special flag:  ignore all other flags,
                            ///< use current default mode
    };
    typedef unsigned int TMode;  ///< Binary OR of "EMode"

The directory entry permissions of read(r), write(w), execute(x), are defined for the "user", "group" and "others" The initial default permission for directories is "rwx" for "user", "rx" for "group" and "rx" for "others". These defaults allow a user to create directory entries while the "group" and "others" can only change to the directory and read a listing of the directory contents. The initial default permission for files is "rw" for "user", "r" for "group" and "r" for "others". These defaults allow a user to read and write to a file while the "group" and "others" can only read the file.

These directory permissions handle most situations but don't handle all permission types. For example, there is currently no provision for handling the Unix "sticky bit" or the "suid" or "sgid" bits. Moreover, operating systems such as Windows NT/2000/2003 and Solaris use Access Control Lists (ACL) settings for files. There is no provision in <span class="nctnt ncbi-class">CDirEntry</span> to handle ACLs

Other methods in <span class="nctnt ncbi-class">CDirEntry</span> deal specifically with checking the attributes of a directory entry such as the following methods:

-   <span class="nctnt ncbi-func">IsFile()</span>: Check if directory entry is a file.

-   <span class="nctnt ncbi-func">IsDir()</span>: Check if directory entry is a directory.

-   <span class="nctnt ncbi-func">GetType()</span>: Get type of directory entry. This returns an <span class="nctnt ncbi-type">EType</span> value.

-   <span class="nctnt ncbi-func">GetTime()</span>: Get time stamp of directory entry.

-   <span class="nctnt ncbi-func">GetMode()</span>: Get permission mode for the directory entry.

-   <span class="nctnt ncbi-func">SetMode()</span>: Set permission mode for the directory entry.

-   <span class="nctnt ncbi-func">static void SetDefaultModeGlobal()</span>: Set default mode globally for all <span class="nctnt ncbi-class">CDirEntry</span> objects. This is a class-wide static method and applies to all objects of this class.

-   <span class="nctnt ncbi-func">SetDefaultMode()</span>: Set mode for this one object only.

These methods are inherited by the derived classes <span class="nctnt ncbi-class">CDir</span> and <span class="nctnt ncbi-class">CFile</span> that are used to access directories and files, respectively.

### CFile class

The [CFile](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCFile.html) is derived from the base class, <span class="nctnt ncbi-class">CDirEntry</span>. Besides inheriting the methods discussed in the [previous section](#previous-section), the following new methods specific to files are defined in the <span class="nctnt ncbi-class">CFile</span> class:

-   <span class="nctnt ncbi-func">Exists()</span>: Check existence for a file.

-   <span class="nctnt ncbi-func">GetLength()</span>: Get size of file.

-   <span class="nctnt ncbi-func">GetTmpName()</span>: Get temporary file name.

-   <span class="nctnt ncbi-func">GetTmpNameEx()</span>: Get temporary file name in a specific directory and having a specified prefix value.

-   <span class="nctnt ncbi-func">CreateTmpFile()</span>: Create temporary file and return pointer to corresponding stream.

-   <span class="nctnt ncbi-func">CreateTmpFileEx()</span>: Create temporary file and return pointer to corresponding stream. You can additionally specify the directory in which to create the temporary file and the prefix to use for the temporary file name.

The methods <span class="nctnt ncbi-func">CreateTmpFile()</span> and <span class="nctnt ncbi-func">CreateTmpFileEx()</span> allow the creation of either a text or binary file. These two types of files are defined by the enumeration type, <span class="nctnt ncbi-type">ETextBinary</span>, and the methods accept a parameter of this type to indicate the type of file to be created:

    /// What type of temporary file to create.
    enum ETextBinary {
        eText,          ///<Create text file
        eBinary         ///< Create binary file
    };

Additionally, you can specify the type of operations (read, write) that should be permitted on the temporary files. These are defined by the enumeration type, <span class="nctnt ncbi-type">EAllowRead</span>, and the <span class="nctnt ncbi-func">CreateTmpFile()</span> and <span class="nctnt ncbi-func">CreateTmpFileEx()</span> methods accept a parameter of this type to indicate the type operations that are permitted:

    /// Which operations to allow on temporary file.
    enum EAllowRead {
        eAllowRead,     ///< Allow read and write
        eWriteOnly      ///< Allow write only
    };

### CDir class

The [CDir](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDir.html) is derived from the base class, <span class="nctnt ncbi-class">CDirEntry</span>. Besides inheriting the methods discussed in the [CDirEntry section](#cdirentry-section), the following new methods specific to directories are defined in the <span class="nctnt ncbi-class">CDir</span> class:

-   <span class="nctnt ncbi-func">Exists()</span>: Check existence for a directory.

-   <span class="nctnt ncbi-func">GetHome()</span>: Get the user's home directory.

-   <span class="nctnt ncbi-func">GetCwd()</span>: Get the current working directory.

-   <span class="nctnt ncbi-func">GetEntries()</span>: Get directory entries based on a specified mask parameter. Retuns a vector of pointers to <span class="nctnt ncbi-class">CDirEntry</span> objects defined by <span class="nctnt ncbi-type">TEntries</span>

-   <span class="nctnt ncbi-func">Create()</span>: Create the directory using the directory name passed in the constructor.

-   <span class="nctnt ncbi-func">CreatePath()</span>: Create the directory path recursively possibly more than one at a time.

-   <span class="nctnt ncbi-func">Remove()</span>: Delete existing directory.

The last method on the list, the <span class="nctnt ncbi-func">Remove()</span> method accepts an enumeration type parameter, <span class="nctnt ncbi-type">EDirRemoveMode</span>, defined in the [CDirEntry](#cdirentry) class which specifies the extent of the directory removal operation - you can delete only an empty directory, only files in a directory but not any subdirectories, or remove the entire directory tree.

### CMemoryFile class

The [CMemoryFile](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCMemoryFile.html) is derived from the base class, <span class="nctnt ncbi-class">CDirEntry</span>. This class creates a virtual image of a disk file in memory that allow normal file operations to be permitted, but the file operations are actually performed on the image of the file in memory. This can result in considerable improvements in speed when there are many "disk intensive" file operations being performed on a file which is mapped to memory.

Besides inheriting the methods discussed in the [CDirEntry section](#cdirentry-section), the following new methods specific to memory mapped are defined in the <span class="nctnt ncbi-class">CMemoryFile </span>class:

-   <span class="nctnt ncbi-func">IsSupported()</span>: Check if memory-mapping is supported by the C++ Toolkit on this platform.

-   <span class="nctnt ncbi-func">GetPtr()</span>: Get pointer to beginning of data in the memory mapped file.

-   <span class="nctnt ncbi-func">GetSize()</span>: Get size of the mapped area.

-   <span class="nctnt ncbi-func">Flush()</span>: Flush by writing all modified copies of memory pages to the underlying file.

-   <span class="nctnt ncbi-func">Unmap()</span>: Unmap file if it has already been mapped.

-   <span class="nctnt ncbi-func">MemMapAdvise()</span>: Advise on memory map usage.

-   <span class="nctnt ncbi-func">MemMapAdviseAddr()</span>: Advise on memory map usage for specified region.

The methods <span class="nctnt ncbi-func">MemMapAdvise()</span> and <span class="nctnt ncbi-func">MemMapAdviseAddr()</span> allow one to advise on the expected usage pattern for the memory mapped file. The expected usage pattern is defined by the enumeration type, <span class="nctnt ncbi-type">EMemMapAdvise</span>, and these methods accept a parameter of this type to indicate the usage pattern:

    /// What type of data access pattern will be used for mapped region.
    ///
    /// Advises the VM system that the a certain region of user mapped memory
    /// will be accessed following a type of pattern. The VM system uses this
    /// information to optimize work with mapped memory.
    ///
    /// NOTE: Now works on Unix platform only.
    typedef enum {
        eMMA_Normal,  ///< No further special treatment
        eMMA_Random,  ///< Expect random page references
        eMMA_Sequential,  ///< Expect sequential page references
        eMMA_WillNeed,  ///< Will need these pages
        eMMA_DontNeed  ///< Don't need these pages
    } EMemMapAdvise;

The memory usage advice is implemented on Unix platforms only, and is not supported on Windows platforms.

String APIs
-----------

The <span class="nctnt ncbi-path">ncbistr.hpp</span> file defines a number of useful constants, types and functions for handling string types. Most of the string functions are defined as class-wides static members of the class <span class="nctnt ncbi-class">NStr</span>.

The following sections provide additional details on string APIs

-   [String Constants](#string-constants)

-   [NStr Class](#nstr-class)

-   [UTF-8 Strings](#utf-8-strings)

-   [PCase and PNocase](#pcase-and-pnocase)

### String Constants

For convenience, two types of empty strings are provided. A C-language style string that terminates with the null character ('\\0') and a C++ style empty string.

The C-language style empty string constants are <span class="nctnt ncbi-macro">NcbiEmptyCStr</span> which is a macro definition for the <span class="nctnt ncbi-var">NCBI\_NS\_NCBI::kEmptyCStr</span>. So the <span class="nctnt ncbi-var">NcbiEmptyStr</span> and <span class="nctnt ncbi-var">kEmptyCStr</span> are, for all practical purposes, equivalent.

The C++-language style empty string constants are <span class="nctnt ncbi-macro">NcbiEmptyString</span> and the <span class="nctnt ncbi-macro">kEmptyStr</span> which are macro definitions for the <span class="nctnt ncbi-func">NCBI\_NS\_NCBI::CNcbiEmptyString::Get()</span> method that returns an empty string. So the <span class="nctnt ncbi-macro">NcbiEmptyString</span> and <span class="nctnt ncbi-macro">kEmptyStr</span> are, for all practical purposes, equivalent.

The <span class="nctnt ncbi-var">SIZE\_TYPE</span> is an alias for the string::size\_type, and the <span class="nctnt ncbi-var">NPOS</span> defines a constant that is returned when a substring search fails, or to indicate an unspecified string position.

### NStr Class

The <span class="nctnt ncbi-class">NStr</span> class encapsulates a number of class-wide static methods. These include string concatenation, string conversion, string comparison, string search functions. Most of these string operations should be familiar to developers by name. For details, see the <span class="nctnt ncbi-class">NStr</span> [static methods documentation](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classNStr.html#pub-static-methods).

### UTF-8 Strings

The <span class="nctnt ncbi-class">CStringUTF8</span> class extends the C++ string class and provides support for Unicode Transformation Format-8 (UTF-8) strings.

This class supports constructors where the input argument is a string reference, char\* pointer, and wide string, and wide character pointers. Wide string support exists if the macro <span class="nctnt ncbi-macro">HAVE\_WSTRING</span> is defined:

    CStringUTF8(const string& src);
    CStringUTF8(const char* src);
    CStringUTF8(const wstring& src);
    CStringUTF8(const wchar_t* src);

The <span class="nctnt ncbi-class">CStringUTF8</span> class defines assignment(=) and append-to string (+=) operators where the string assigned or appended can be a <span class="nctnt ncbi-path">CStringUTF8</span> reference, string reference, char\* pointer, wstring reference, wchar\_t\* pointer.

Conversion to ASCII from <span class="nctnt ncbi-class">CStringUTF8</span> is defined by the <span class="nctnt ncbi-func">AsAscii()</span> method. This method can throw a StringException with error codes 'eFormat' or 'eConvert' if the string has a wrong UTF-8 format or cannot be converted to ASCII.

    string AsAscii(void) const;
    wstring AsUnicode(void) const

### PCase and PNocase

The <span class="nctnt ncbi-class">PCase</span> and <span class="nctnt ncbi-class">PNocase</span> structures define case-sensitive and case-insensitive comparison functions, respectively. These comparison functions are the <span class="nctnt ncbi-func">Compare()</span>, <span class="nctnt ncbi-func">Less()</span>, <span class="nctnt ncbi-func">Equals()</span>, <span class="nctnt ncbi-func">operator()</span>. The <span class="nctnt ncbi-func">Compare()</span> returns an integer (-1 for less than, 0 for equal to, 1 for greater than). The <span class="nctnt ncbi-func">Less()</span> and <span class="nctnt ncbi-func">Equals()</span> return a TRUE if the first string is less than or equal to the second string. The <span class="nctnt ncbi-func">operator()</span> returns TRUE if the first string is less than the second.

A convenience template function <span class="nctnt ncbi-func">AStrEquiv</span> is defined that accepts the two classes to be compared as template parameters and a third template parameter that can be the comparison class such as the <span class="nctnt ncbi-class">PCase</span> and <span class="nctnt ncbi-class">PNocase</span> defined above.

Portable Time Class
-------------------

The <span class="nctnt ncbi-path">ncbitime.hpp</span> defines <span class="nctnt ncbi-class">CTime</span>, the standard Date/Time class that also can be used to represent elapsed time. Please note that the <span class="nctnt ncbi-class">CTime</span> class works for dates after 1/1/1900 and should not be used for elapsed time prior to this date. Also, since <span class="nctnt ncbi-monospace">Mac OS 9</span> does not support the daylight savings flag, <span class="nctnt ncbi-class">CTime</span> does not support daylight savings on this platform.

The subsections that follow discuss the following topics:

-   [CTime Class Constructors](#ctime-class-constructors)

-   [Other CTime Methods](#other-ctime-methods)

### <span class="nctnt ncbi-class">CTime</span> Class Constructors

The <span class="nctnt ncbi-class">CTime</span> class defines three basic constructors that accept commonly used time description arguments and some explicit conversion and copy constructors. The basic constructors are the following:

-   Constructor 1:<br/><span class="nctnt ncbi-code"> CTime(EInitMode mode = eEmpty,</span><br/><span class="nctnt ncbi-code"> ETimeZone tz = eLocal,</span><br/><span class="nctnt ncbi-code"> ETimeZonePrecision tzp = eTZPrecisionDefault);</span>

-   Constructor 2:<br/><span class="nctnt ncbi-code"> CTime(int year,</span><br/><span class="nctnt ncbi-code"> int month,</span><br/><span class="nctnt ncbi-code"> int day,</span><br/><span class="nctnt ncbi-code"> int hour = 0,</span><br/><span class="nctnt ncbi-code"> int minute = 0,</span><br/><span class="nctnt ncbi-code"> int second = 0,</span><br/><span class="nctnt ncbi-code"> long nanosecond = 0,</span><br/><span class="nctnt ncbi-code"> ETimeZone tz = Local,</span><br/><span class="nctnt ncbi-code"> ETimeZonePrecision tzp = eTZPrecisionDefault);</span>

-   Constructor 3:<br/><span class="nctnt ncbi-code"> CTime(int year,</span><br/><span class="nctnt ncbi-code"> int yearDayNumber,</span><br/><span class="nctnt ncbi-code"> ETimeZone tz = eLocal,</span><br/><span class="nctnt ncbi-code"> ETimeZonePrecision tzp = eTZPrecisionDefault);</span>

In Constructor 1, the <span class="nctnt ncbi-type">EInitMode</span> is an enumeration type defined in the <span class="nctnt ncbi-class">CTime</span> class that can be used to specify whether to build the time object with empty time value (<span class="nctnt ncbi-monospace">eEmpty</span>) or current time (<span class="nctnt ncbi-monospace">eCurrent</span>). The <span class="nctnt ncbi-type">ETimeZone</span> is an enumeration type also defined in the <span class="nctnt ncbi-class">CTime</span> class that is used to specify the local time zone (<span class="nctnt ncbi-monospace">eLocal</span>) or GMT (<span class="nctnt ncbi-monospace">eGmt</span>. The <span class="nctnt ncbi-type">ETimeZonePrecision</span> is an enumeration type also defined in the <span class="nctnt ncbi-class">CTime</span> class that can be used to specify the time zone precision to be used for adjusting the daylight savings time. The default value is <span class="nctnt ncbi-monospace">eNone</span>, which means that daylight savings do not affect time calculations.

Constructor 2 differs from Constructor 1 with respect to how the timestamp is specified. Here the time stamp is explictly specified as the year, month, day, hour, minute, second, and nanosecond values. The other parameters of type <span class="nctnt ncbi-type">ETimeZone</span> and <span class="nctnt ncbi-type">ETimeZonePrecision</span> have the meanings discussed in the previous paragraph.

Constructor 3 allows the timestamp to be constructed as the Nth day (<span class="nctnt ncbi-var">yearDayNumber</span>) of a year(<span class="nctnt ncbi-var">year</span>). The other parameters of type <span class="nctnt ncbi-type">EtimeZone</span> and <span class="nctnt ncbi-type">ETimeZonePrecision</span> have the meanings discussed in the previous paragraph.

The explicit conversion constructor allows the conversion to be made from a string representation of time. The default value of the format string is <span class="nctnt ncbi-var">kEmptyStr</span>, which implies that the format string has the format "M/D/Y h:m:s". As one would expect, the format specifiers M, D, Y, h, m, and s have the meanings month, day, year, hour, minute, and second, respectively:

    explicit CTime(const string& str,
                   const string&        fmt = kEmptyStr,
                   ETimeZone            tz  = eLocal,
                   ETimeZonePrecision   tzp = eTZPrecisionDefault);

There is also a copy constructor defined that permits copy operations for <span class="nctnt ncbi-class">CTime</span> objects.

### Other <span class="nctnt ncbi-class">CTime</span> Methods

Once the <span class="nctnt ncbi-class">CTime</span> object is constructed, it can be accessed using the <span class="nctnt ncbi-func">SetTimeT()</span> and <span class="nctnt ncbi-func">GetTimeT()</span> methods. The <span class="nctnt ncbi-func">SetTimeT()</span> method is used to set the <span class="nctnt ncbi-class">CTime</span> with the timestamp passed by the <span class="nctnt ncbi-type">time\_t</span> parameter. The <span class="nctnt ncbi-func">GetTimeT()</span> method returns the time stored in the <span class="nctnt ncbi-class">CTime</span> object as a <span class="nctnt ncbi-type">time\_t</span> value. The <span class="nctnt ncbi-type">time\_t</span> value measures seconds since January 1, 1900; therefore, do not use these methods if the timestamp is less than 1900. Also, time formats are in GMT time format.

A series of methods that set the time using the database formats <span class="nctnt ncbi-type">TDBTimeI</span> and <span class="nctnt ncbi-type">TDBTimeU</span> are also defined. These database time formats contain local time only and are defined as typedefs in <span class="nctnt ncbi-path">ncbitime.hpp</span>. The mutator methods are <span class="nctnt ncbi-func">SetTimeDBI()</span> and <span class="nctnt ncbi-func">SetTimeDBU()</span>, and the accessor methods are <span class="nctnt ncbi-func">GetTimeDBI()</span> and <span class="nctnt ncbi-func">GetTimeDBU()</span>.

You can set the time to the current time using the <span class="nctnt ncbi-func">SetCurrent()</span> method, or set it to "empty" using the <span class="nctnt ncbi-func">Clear()</span> method. If you want to measure time as days only and strip the hour, minute, and second information, you can use <span class="nctnt ncbi-func">Truncate()</span> method.

You can get or set the current time format using the <span class="nctnt ncbi-func">GetFormat()</span> and <span class="nctnt ncbi-func">SetFormat()</span> methods.

You can get and set the individual components of time, such as year, day, month, hour, minute, second, and nanosecond. The accessor methods for these components are named after the component itself, and their meanings are obvious, e.g., <span class="nctnt ncbi-func">Year()</span> for getting the year component, <span class="nctnt ncbi-func">Month()</span> for getting the month component, <span class="nctnt ncbi-func">Day()</span> for getting the day component, <span class="nctnt ncbi-func">Hour()</span> for getting the hour component, <span class="nctnt ncbi-func">Minute()</span> for getting the minute component, <span class="nctnt ncbi-func">Second()</span> for getting the second component, and <span class="nctnt ncbi-func">NanoSecond()</span> for getting the nanosecond component. The corresponding mutator methods for setting the individual components are the same as the accessor, except that they have the prefix "Set" before them. For example, the mutator method for setting the day is <span class="nctnt ncbi-func">SetDay()</span>. A word of caution on setting the individual components: You can easily set the timestamp to invalid values, such as changing the number of days in the month of February to 29 when it is not a leap year, or 30 or 31.

A number of methods are available to get useful information from a <span class="nctnt ncbi-class">CTime</span> object. To get a day's year number (1 to 366) use <span class="nctnt ncbi-func">YearDayNumber()</span>. To get the week number in a year, use <span class="nctnt ncbi-func">YearWeekNumber()</span>. To get the week number in a month, use <span class="nctnt ncbi-func">MonthWeekNumber()</span>. You can get the day of week (Sunday=0) by using <span class="nctnt ncbi-func">DayOfWeek()</span>, or the number of days in the current month by using <span class="nctnt ncbi-func">DaysInMonth()</span>.

There are times when you need to add months, days, hours, minutes, or seconds to an existing <span class="nctnt ncbi-class">CTime</span> object. You can do this by using the <span class="nctnt ncbi-func">AddXXX()</span> methods, where the "XXX" is the time component such as "Year", "Month", "Day", "Hour", "Minute", "Second", "NanoSecond" that is to be added to. Be aware that because the number of days in a month can vary, adding months may change the day number in the timestamp. Operator methods for adding to (+=), subtracting from (-=), incrementing (++), and decrementing (--) days are also available.

If you need to compare two timestamps, you can use the operator methods for equality (==), in-equality (!=), earlier than (\<), later than (\>), or a combination test, such as earlier than or equal to (\<=) or later than or equal to (\>=).

You can measure the difference between two timestamps in days, hours, minutes, seconds, or nanoseconds. The timestamp difference methods have the form <span class="nctnt ncbi-func">DiffXXX()</span>, where "XXX" is the time unit in which you want the difference calculated such as "Day", "Hour", "Minute", "Second", or "NanoSecond". Thus, <span class="nctnt ncbi-func">DiffHour()</span> can be used to calculate the difference in hours.

There are times when you may need to do a check on the timestamp. You can use <span class="nctnt ncbi-func">IsLeap()</span> to check if the time is in a leap year, or if it is empty by using <span class="nctnt ncbi-func">IsEmpty()</span>, or if it is valid by using <span class="nctnt ncbi-func">IsValid()</span>, or if it is local time by using <span class="nctnt ncbi-func">IsLocalTime()</span>, or if it is GMT time by using <span class="nctnt ncbi-func">IsGmtTime()</span>.

If you need to work with time zones explicitly, you can use <span class="nctnt ncbi-func">GetTimeZoneFormat()</span> to get the current time zone format, and <span class="nctnt ncbi-func">SetTimeZoneFormat()</span> to change it. You can use <span class="nctnt ncbi-func">GetTimeZonePrecision()</span> to get the current time zone precision and <span class="nctnt ncbi-func">SetTimeZonePrecision()</span> to change it. To get the time zone difference between local time and GMT, use <span class="nctnt ncbi-func">TimeZoneOffset()</span>. To get current time as local time use <span class="nctnt ncbi-func">GetLocalTime()</span>, and as GMT time use <span class="nctnt ncbi-func">GetGmtTime()</span>. To convert current time to a specified time zone, use <span class="nctnt ncbi-func">ToTime()</span>, or to convert to local time use <span class="nctnt ncbi-func">ToLocalTime()</span>.

Also defined for <span class="nctnt ncbi-class">CTime</span> are assignment operators to assign a <span class="nctnt ncbi-class">CTime</span> object to another <span class="nctnt ncbi-class">CTime</span> and an assignment operator where the right hand side is a time value string.

Template Utilities
------------------

The <span class="nctnt ncbi-path">ncbiutil.hpp</span> file defines a number of useful template functions, classes, and struct definitions that are used in other parts of the library.

The following topics are discussed in this section:

-   [Function Objects](#function-objects)

-   [Template Functions](#template-functions)

### Function Objects

The <span class="nctnt ncbi-class">p\_equal\_to</span> and <span class="nctnt ncbi-class">pair\_equal\_to</span> are template function classes that are derived from the standard <span class="nctnt ncbi-class">binary\_function</span> base class. The <span class="nctnt ncbi-class">p\_equal\_to</span> checks for equality of objects pointed to by a pointer and <span class="nctnt ncbi-class">pair\_equal\_to</span> checks whether a pair's second element matches a given value. Another <span class="nctnt ncbi-class">PPtrLess</span> function class allows comparison of objects pointed to by a smart pointer.

The <span class="nctnt ncbi-class">CNameGetter</span> template defines the function <span class="nctnt ncbi-func">GetKey()</span>, which returns the name attribute for the template parameter.

### Template Functions

Defined here are a number of inline template functions that make it easier to perform common operations on map objects.

<span class="nctnt ncbi-func">NotNull()</span> checks for a null pointer value and throws a <span class="nctnt ncbi-class">CCoreException</span>, if a null value is detected. If the pointer value is not null, it is simply returned.

<span class="nctnt ncbi-func">GetMapElement()</span> searches a map object for an element and returns the element, if found. If the element is not found, it returns a default value, which is usually set to 0 (null).

<span class="nctnt ncbi-func">SetMapElement()</span> sets the map element. If the element to be set is null, its existing key is erased.

<span class="nctnt ncbi-func">InsertMapElement()</span> inserts a new map element.

<span class="nctnt ncbi-func">GetMapString()</span> and <span class="nctnt ncbi-func">SetMapString()</span> are similar to the more general <span class="nctnt ncbi-func">GetMapElement()</span> and <span class="nctnt ncbi-func">SetMapElement()</span>, except that they search a map object for a string. In the case of <span class="nctnt ncbi-func">GetMapString()</span>, it returns a string, if found, and an empty string ("") if not found.

There are three overloads for the <span class="nctnt ncbi-func">DeleteElements()</span> template function. One overload accepts a container (list, vector, set, multiset) of pointers and deletes all elements in the container and clears the container afterwards. The other overloads work with map and multimap objects. In each case, they delete the pointers in the map object and clear the map container afterwards.

The <span class="nctnt ncbi-func">AutoMap()</span> template function works with a cache pointed to <span class="nctnt ncbi-type">auto\_ptr</span>. It retrieves the result from the cache, and if the cache is empty, it inserts a value into the cache from a specified source.

A <span class="nctnt ncbi-func">FindBestChoice()</span> template function is defined that returns the best choice (lowest score) value in the container. The container and scoring functions are specified as template parameters. The <span class="nctnt ncbi-func">FindBestChoice()</span> in turn uses the <span class="nctnt ncbi-class">CBestChoiceTracker</span> template class, which uses the standard unary\_function as its base class. The <span class="nctnt ncbi-class">CBestChoiceTracker</span> contains the logic to record the scoring function and keep track of the current value and the best score.

Miscellaneous Types and Macros
------------------------------

The <span class="nctnt ncbi-path">ncbimisc.hpp</span> file defines a number of useful enumeration types and macros that are used in other parts of the library.

The following topics are discussed in this section:

-   [Miscellaneous Enumeration Types](#miscellaneous-enumeration-types)

-   [AutoPtr Class](#autoptr-class)

-   [ITERATE Macros](#iterate-macros)

-   [Sequence Position Types](#sequence-position-types)

### Miscellaneous Enumeration Types

The enum type <span class="nctnt ncbi-type">EOwnership</span> defines the constants <span class="nctnt ncbi-monospace">eNoOwnership</span> and <span class="nctnt ncbi-monospace">eTakeOwnership</span>. These are used to specify relationships between objects.

The enum type <span class="nctnt ncbi-type">ENullable</span> defines the constants <span class="nctnt ncbi-monospace">eNullable</span> and <span class="nctnt ncbi-monospace">eNotNullable</span>. These are used to specify if a data element can hold a null or not-null value.

### AutoPtr Class

The <span class="nctnt ncbi-path">ncbimisc.hpp</span> file defines an <span class="nctnt ncbi-class">auto\_ptr</span> class if the <span class="nctnt ncbi-macro">HAVE\_NO\_AUTO\_PTR</span> macro is undefined. This is useful in replacing the <span class="nctnt ncbi-class">std::auto\_ptr</span> of STL for compilers with poor "auto\_ptr" implementation. Section [STL auto\_ptrs](#stl-autoptrs) discusses details on the use of <span class="nctnt ncbi-class">auto\_ptr</span>.

Another class related to the <span class="nctnt ncbi-class">auto\_ptr</span> class is the <span class="nctnt ncbi-class">AutoPtr</span> class. The Standard <span class="nctnt ncbi-class">auto\_ptr</span> class from STL does not allow the auto\_ptr to be put in STL containers such as list, vector, map etc. Because of the nature of how ownership works in an auto\_ptr class, the copy constructor and assignment operator of AutoPtr modify the state of the source <span class="nctnt ncbi-class">AutoPtr</span> object as it transfers ownership to the target <span class="nctnt ncbi-class">AutoPtr</span> object.

A certain amount of flexibility has been provided in terms of how the pointer is to be deleted. This is done by passing a second argument to the <span class="nctnt ncbi-class">AutoPtr</span> template. This second argument allows the passing of a functor object that defines the deletion of the object. You can define "malloc" pointers in <span class="nctnt ncbi-class">AutoPtr</span>, or you can use an <span class="nctnt ncbi-class">ArrayDeleter</span> template class to properly delete an array of objects using "delete[]". By default, the internal pointer will be deleted using the "delete" operator.

### ITERATE macros

When working with STL (or STL-like) container classes, it is common to use a for-statement to iterate through the elements in a container, for example:

    for (Type::const_iterator it = cont.begin();  it != cont.end();  ++it)

However, there are a number of ways that iterating in this way can fail. For example, suppose the function <span class="nctnt ncbi-func">GetNames()</span> returns a vector of strings by value and is used like this:

    for (vector<string>::iterator it = GetNames().begin();  it != GetNames().end();  ++it)

This code has the serious problem that the termination condition will never be met because every time <span class="nctnt ncbi-func">GetNames()</span> is called a new object is created, and therefore neither the initial iterator returned by <span class="nctnt ncbi-func">begin()</span> nor the iterator returned by <span class="nctnt ncbi-func">operator++()</span> will ever match the iterator returned by <span class="nctnt ncbi-func">end()</span>. Code like this is not common but does occasionally get written, resulting in a bug and wasted time.

A simpler criticism of the for-statement approach is that the call to <span class="nctnt ncbi-func">end()</span> is repeated unnecessarily.

Therefore, to make it easier to write code that will correctly and efficiently iterate through the elements in STL and STL-like containers, the <span class="nctnt ncbi-macro">ITERATE</span> and <span class="nctnt ncbi-macro">NON\_CONST\_ITERATE</span> macros were defined. Using <span class="nctnt ncbi-macro">ITERATE</span> , the for-statement at the start of this section becomes simply:

    ITERATE(Type, it, cont)

<span class="nctnt highlight">Note:</span> The container argument must be an lvalue and may be evaluated more than once, so it must always evaluate to the same container instance.

<span class="nctnt ncbi-macro">ITERATE</span> uses a constant iterator; <span class="nctnt ncbi-macro">NON\_CONST\_ITERATE</span> uses a non-constant iterator.

The <span class="nctnt ncbi-macro">ITERATE</span> and <span class="nctnt ncbi-macro">NON\_CONST\_ITERATE</span> macros are defined in [include/corelib/ncbimisc.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbimisc.hpp), along with related macros including <span class="nctnt ncbi-macro">NON\_CONST\_SET\_ITERATE</span>, <span class="nctnt ncbi-macro">ERASE\_ITERATE</span>, <span class="nctnt ncbi-macro">VECTOR\_ERASE</span>, <span class="nctnt ncbi-macro">REVERSE\_ITERATE</span>, <span class="nctnt ncbi-macro">ITERATE\_SIMPLE</span>, and more.

### Sequence Position Types

The <span class="nctnt ncbi-type">TSeqPos</span> and and <span class="nctnt ncbi-type">TSignedSeqPos</span> are defined to specify sequence locations and length. <span class="nctnt ncbi-type">TSeqPos</span> is defined as an unsigned int, and <span class="nctnt ncbi-type">TSignedSqPos</span> is a signed int that should be used only when negative values are a possibility for reporting differences between positions, or for error reporting, although exceptions are generally better for error reporting.

Containers
----------

The Container classes are template classes that provide many useful container types. The template parameter refers to the types of objects whose collection is being described. An overview of some of the [container classes is presented in the introductory chapter](#container-classes-is-presented-in-the-introductory-chapter) on the C++ Toolkit.

The following classes are described in this section:

-   [template\<typename Coordinate\> class CRange](#template<typename-coordinate>-class-crange)

-   [template\<typename Object, typename Coordinate = int\> class CRangeMap](#template<typename-object-typename-coordinate-=-int>-class-crangemap)

-   [template\<typename Object, typename Coordinate = int\> class CRangeMultiMap](#template<typename-object-typename-coordinate-=-int>-class-crangemultimap)

-   [class CIntervalTree](#class-cintervaltree)

### template\<typename Coordinate\> class CRange

Class for storing information about some interval (from:to). From and to points are inclusive.

#### Typedefs

    position_type

synonym of Coordinate.

#### Methods

    CRange();
    CRange(position_type from, position_type to);

constructors

    static position_type GetEmptyFrom();
    static position_type GetEmptyTo();
    static position_type GetWholeFrom();
    static position_type GetWholeTo();

get special coordinate values

    static CRange<position_type> GetEmpty();
    static CRange<position_type> GetWhole();

get special interval objects

    bool HaveEmptyBound() const;

check if any bound have special 'empty' value

    bool HaveInfiniteBound() const;

check if any bound have special 'whole' value

    bool Empty() const;

check if interval is empty (any bound have special 'empty' value or left bound greater then right bound)

    bool Regular() const;

check if interval's bounds are not special and length is positive

    position_type GetFrom() const;
    position_type GetTo() const;
    position_type GetLength() const;

get parameters of interval

    CRange<position_type>& SetFrom();
    CRange<position_type>& SetTo();

set bounds of interval

    CRange<position_type>& SetLength();

set length of interval leaving left bound (from) unchanged

    CRange<position_type>& SetLengthDown();

set length of interval leaving right bound (to) unchanged

    bool IntersectingWith(CRange<position_type> range) const;

check if non empty intervals intersect

    bool IntersectingWithPossiblyEmpty(CRange<position_type> range) const;

check if intervals intersect

### template\<typename Object, typename Coordinate = int\> class CRangeMap

Class for storing and retrieving data using interval as key. Also allows efficient iteration over intervals intersecting with specified interval. Time of iteration is proportional to amount of intervals produced by iterator. In some cases, algorithm is not so efficient and may slowdown.

### template\<typename Object, typename Coordinate = int\> class CRangeMultiMap

Almost the same as [CRangeMap](#crangemap) but allows several values have the same key interval.

### class CIntervalTree

Class with the same functionality as [CRangeMap](#crangemap) although with different algorithm. It is faster and its speed is not affected by type of data but it uses more memory (triple as [CRangeMap](#crangemap)) and, as a result, less efficient when amount of interval in set is quite big. It uses about 140 bytes per interval for 64 bit program so you can calculate if <span class="nctnt ncbi-class">CIntervalTree</span> is acceptable. For example, it becomes less efficient than [CRangeMap](#crangemap) when total memory becomes greater than processor cache.

Thread Pools
------------

<span class="nctnt ncbi-class">CThreadPool</span> is the main class that implements a pool of threads. It executes any tasks derived from the <span class="nctnt ncbi-class">CThreadPool\_Task</span> class. The number of threads in pool is controlled by special holder of this policy: object derived from <span class="nctnt ncbi-class">CThreadPool\_Controller</span> (default implementation is <span class="nctnt ncbi-class">CThreadPool\_Controller\_PID</span> based on Proportional-Integral-Derivative algorithm). All threads executing by <span class="nctnt ncbi-class">CThreadPool</span> are the instances of <span class="nctnt ncbi-class">CThreadPool\_Thread</span> class or its derivatives.

The following classes are discussed in this section:

-   [CThreadPool](#cthreadpool)

-   [CThreadPool\_Task](#cthreadpooltask)

-   [CThreadPool\_Thread](#cthreadpoolthread)

-   [CThreadPool\_Controller](#cthreadpoolcontroller)

-   [CThreadPool\_Controller\_PID](#cthreadpoolcontrollerpid)

### Class CThreadPool

Main class implementing functionality of pool of threads. <span class="nctnt ncbi-class">CThreadPool</span> can be created in 2 ways:

-   with minimum and maximum limits on count of simultaneously working threads and default object controlling the number of threads in pool during <span class="nctnt ncbi-class">CThreadPool</span> lifecycle (instance of <span class="nctnt ncbi-class">CThreadPool\_Controller\_PID</span>);

-   with custom object controlling the number of threads (instance of class derived from <span class="nctnt ncbi-class">CThreadPool\_Controller</span>). This object will control among all other the minimum and maximum limits on count of simultaneously working threads.

Both constructors take additional parameter - maximum number of tasks waiting in the inner <span class="nctnt ncbi-class">CThreadPool</span>’s queue for their execution. When this limit will be reached next call to <span class="nctnt ncbi-func">AddTask()</span> will block until some task from queue is executed and there is free room for new task.

<span class="nctnt ncbi-class">CThreadPool</span> has the ability to execute among ordinary tasks some exclusive ones. After call to <span class="nctnt ncbi-func">RequestExclusiveExecution()</span> all threads in pool will suspend their work (finishing currently executing tasks) and exclusive task will be executed in the special exclusive thread.

If there’s necessity to implement some special per-thread logic in <span class="nctnt ncbi-class">CThreadPool</span> then class can be derived to override virtual method <span class="nctnt ncbi-func">CreateThread()</span> in which some custom object derived from <span class="nctnt ncbi-class">CThreadPool\_Thread</span> can be created.

### Class CThreadPool\_Task

Abstract class derived from [CObject](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CObject&d=C), encapsulating task for execution in a <span class="nctnt ncbi-class">CThreadPool</span>. The pure virtual method <span class="nctnt ncbi-func">EStatus Execute(void)</span> is called when some thread in pool becomes free and ready to execute this task. The lifetime of the task is controlled inside pool by <span class="nctnt ncbi-class">CRef</span>\<\> classes.

### Class CThreadPool\_Thread

Base class for a thread running inside <span class="nctnt ncbi-class">CThreadPool</span> and executing its tasks. Class can be derived to implement some per-thread functionality in <span class="nctnt ncbi-class">CThreadPool</span>. For this purpose there are protected virtual methods <span class="nctnt ncbi-func">Initialize()</span> and <span class="nctnt ncbi-func">Finalize()</span> which are called at the start and finish of the thread correspondingly. And there are methods <span class="nctnt ncbi-func">GetPool()</span> and <span class="nctnt ncbi-func">GetCurrentTask()</span> for application needs.

### Class CThreadPool\_Controller

Abstract base class for implementations of policies of threads creation and deletion inside pool.

### Class CThreadPool\_Controller\_PID

Default object controlling number of threads working in the pool. Implementation is based on Proportional-Integral-Derivative algorithm for keeping in memory just threads that are necessary for efficient work.

Miscellaneous Classes
---------------------

The following classes are discussed in this section. For an overview of these classes see the [Lightweight Strings](ch_intro.html#ch_intro.intro_lightstring) and the [Checksum](ch_intro.html#ch_intro.intro_checksum) sections in the introductory chapter on the C++ Toolkit.

-   [class CTempString](#class-ctempstring)

-   [class CChecksum](#class-cchecksum)

### class CTempString

Class <span class="nctnt ncbi-class">CTempString</span> implements a light-weight string on top of a storage buffer whose lifetime management is known and controlled.

<span class="nctnt ncbi-class">CTempString</span> is designed to avoid memory allocation but provide a string interaction interface congruent with std::basic\_string\<char\>.

As such, <span class="nctnt ncbi-class">CTempString</span> provides a const-only access interface to its underlying storage. Care has been taken to avoid allocations and other expensive operations wherever possible.

<span class="nctnt ncbi-class">CTempString</span> has constructors from std::string and C-style string, which do not copy the string data but keep char pointer and string length.This way the construction and destruction are very efficient.

Take into account, that the character string array kept by <span class="nctnt ncbi-class">CTempString</span> object must remain valid and unchanged during whole lifetime of the <span class="nctnt ncbi-class">CTempString</span> object.

It's convenient to use the class <span class="nctnt ncbi-class">CTempString</span> as an argument of API functions so that no allocation or deallocation will take place on of the function call.

### class CChecksum

Class for CRC32 checksum calculation. It also has methods for adding and checking checkum line in text files.

Input/Output Utility Classes
----------------------------

This section provides reference information on a number of Input/Output Utility classes. For an overview of these classes see the [Stream Support section in the introductory chapter](ch_intro.html#ch_intro.intro_streamsupport) on the C++ Toolkit.

-   [class CIStreamBuffer](#class-cistreambuffer)

-   [class COStreamBuffer](#class-costreambuffer)

-   [class CByteSource](#class-cbytesource)

-   [class CStreamByteSource](#class-cstreambytesource)

-   [class CFStreamByteSource](#class-cfstreambytesource)

-   [class CFileByteSource](#class-cfilebytesource)

-   [class CMemoryByteSource](#class-cmemorybytesource)

-   [class CByteSourceReader](#class-cbytesourcereader)

-   [class CSubSourceCollector](#class-csubsourcecollector)

### class CIStreamBuffer

Class for additional buffering of standard C++ input streams (sometimes standard C++ iostreams performance quite bad). Uses [CByteSource](#cbytesource) as a data source.

### class COStreamBuffer

Class for additional buffering of standard C++ output streams (sometimes standard C++ iostreams performance quite bad).

### class CByteSource

Abstract class for abstract source of byte data (file, stream, memory etc).

### class CStreamByteSource

[CByteSource](#cbytesource) subclass for reading from C++ istream.

### class CFStreamByteSource

[CByteSource](#cbytesource) subclass for reading from C++ ifstream.

### class CFileByteSource

[CByteSource](#cbytesource) subclass for reading from named file.

### class CMemoryByteSource

[CByteSource](#cbytesource) subclass for reading from memory buffer.

### class CByteSourceReader

Abstract class for reading data from [CByteSource](#cbytesource).

### class CSubSourceCollector

Abstract class for obtaining piece of [CByteSource](#cbytesource) as separate source.

Using the C++ Toolkit from a Third Party Application Framework
--------------------------------------------------------------

The NCBI C++ Toolkit includes an API, via [corelib/ncbi\_toolkit.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbi_toolkit.hpp), that provides an easy way to initialize the NCBI C++ Toolkit internals to use the Toolkit from other application frameworks. This is particularly helpful when those frameworks provide their own logging.

To initialize the NCBI C++ Toolkit internal infrastructure use the function:

    void  NcbiToolkit_Init
       (int                                argc,
        const TNcbiToolkit_XChar* const*   argv,
        const TNcbiToolkit_XChar* const*   envp        = NULL,
        INcbiToolkit_LogHandler*           log_handler = NULL);

where the parameter meanings are:

| Parameter                                        | Meaning                                                                                                                                                                 |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-var">argc</span>         | Argument count [argc in a regular main(argc, argv)].                                                                                                                    |
| <span class="nctnt ncbi-var">argv</span>         | Argument vector [argv in a regular main(argc, argv)].                                                                                                                   |
| <span class="nctnt ncbi-var">envp</span>         | Environment pointer [envp in a regular main(argc, argv, envp)]; a null pointer (the default) corresponds to the standard system array (environ on most Unix platforms). |
| <span class="nctnt ncbi-var">log\_handler</span> | Handler for diagnostic messages that are emitted by the C++ Toolkit code.                                                                                               |

<span class="nctnt highlight">Note:</span> The <span class="nctnt ncbi-type">TNcbiToolkit\_XChar</span> parameter type is used for compatibility with applications that use Unicode under Windows.

When your application is finished using the NCBI C++ Toolkit, be sure to release the Toolkit resources by calling:

    void  NcbiToolkit_Fini(void);

The following program illustrates how to forward the NCBI C++ Toolkit logging to another application framework:

    #include <ncbi_pch.hpp>
    #include <iostream>
    #include <corelib/ncbi_toolkit.hpp>
    #include <corelib/ncbifile.hpp>

    using namespace std;
    using namespace ncbi;

    class MyLogHandler : public INcbiToolkit_LogHandler
    {
    public:
        void Post(const CNcbiToolkit_LogMessage& msg)
        {
            // This is where you could pass log messages generated by the
            // NCBI C++ Toolkit to another application framework, e.g.:
            //     some_framework::ERR_POST(msg.Message());
            // In this demo, I'll just print out the message.
            cout << "Log message from C++ Toolkit:\n" << msg.Message() << endl;
        }
    };

    int main(int argc,
             const TNcbiToolkit_XChar* const* argv,
             const TNcbiToolkit_XChar* const* envp)
    {
        // Initialize the NCBI C++ Toolkit application framework.
        MyLogHandler    log_handler;
        NcbiToolkit_Init(argc,argv,envp,&log_handler);

        // Use a part of the NCBI C++ Toolkit that will cause a log message.
        // This will cause MyLogHandler::Post() to get called, where the log
        // message can get passed to the third party application framework.
        CFileAPI::SetLogging(eOn);
        CDirEntry baddir(CDirEntry("<bad>"));
        baddir.Stat(0);

        // Release resources used by the NCBI C++ Toolkit application framework.
        NcbiToolkit_Fini();

        return 0;
    }

<span class="nctnt highlight">Note:</span> This API is in the <span class="nctnt ncbi-monospace">ncbi</span> namespace.


