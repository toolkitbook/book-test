---
layout: default
title: C++ Toolkit test
nav: pages/ch_style
---

<span class="label">7</span>Programming Policies and Guidelines
===============================================================

Last Update: September 24, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter discusses policies and guidelines for the development of NCBI software.

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Choice of Language](#choice-of-language)

-   [Source Code Conventions](#source-code-conventions)

    -   [Public Domain Notice](#public-domain-notice)

    -   [Naming Conventions](#naming-conventions)

    -   [Name Prefixing and/or the Use of Namespaces](#name-prefixing-andor-the-use-of-namespaces)

    -   [Use of the NCBI Name Scope](#use-of-the-ncbi-name-scope)

    -   [Use of Include Directives](#use-of-include-directives)

    -   [Code Indentation and Bracing](#code-indentation-and-bracing)

    -   [Class Declaration](#class-declaration)

    -   [Function Declaration](#function-declaration)

    -   [Function Definition](#function-definition)

    -   [Use of Whitespace](#use-of-whitespace)

    -   [Alternative Tokens](#alternative-tokens)

    -   [Standard Header Template](#standard-header-template)

-   [Doxygen Comments](#doxygen-comments)

-   [C++ Guidelines](#c++-guidelines)

    -   [Introduction to Some C++ and STL Features and Techniques](#introduction-to-some-c++-and-stl-features-and-techniques)

        -   [C++ Implementation Guide](#c++-implementation-guide)

            -   [Limitations on Using C++11 Features](#limitations-on-using-c++11-features)

            -   [Use of STL (Standard Template Library)](#use-of-stl-standard-template-library)

            -   [Use of C++ Exceptions](#use-of-c++-exceptions)

            -   [Design](#design)

            -   [Make Your Code Readable](#make-your-code-readable)

        -   [C++ Tips and Tricks](#c++-tips-and-tricks)

        -   [Standard Template Library (STL)](#standard-template-library-stl-)

            -   [STL Tips and Tricks](#stl-tips-and-tricks)

    -   [C++/STL Pitfalls and Discouraged/Prohibited Features](#c++stl-pitfalls-and-discouragedprohibited-features)

        -   [STL and Standard C++ Library's Bad Guys](#stl-and-standard-c++-library's-bad-guys)

            -   [Non-Standard STL Classes](#non-standard-stl-classes)

        -   [C++ Bad Guys](#c++-bad-guys)

            -   [Operator Overload](#operator-overload)

            -   [Assignment and Copy Constructor Overload](#assignment-and-copy-constructor-overload)

            -   [Omitting "void" in a No-Argument Function Prototype](#omitting-void-in-a-no-argument-function-prototype)

            -   [Do Not Mix malloc and new](#do-not-mix-malloc-and-new)

        -   [Miscellaneous Gotchas](#miscellaneous-gotchas)

-   [Source Code Repositories](#source-code-repositories)

-   [Testing](#testing)

Choice of Language
------------------

**C++** is typically the language of choice for C++ Toolkit libraries and applications. The policy for language choice in other areas within NCBI is:

-   **C/C++** -- for high-performance standalone backend servers and CGIs, computationally intensive algorithms and large data flow processing tools used in production.

-   **sh** or **bash** -- for primitive scripting.

-   **Python** -- for advanced scripting. See its usage policy [here](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Recommended_Python_Database_Interface_modules#Recommended_Python_Database_Interface_modules1).

-   **Perl** -- for advanced scripting. The [Python usage policy](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Recommended_Python_Database_Interface_modules#Recommended_Python_Database_Interface_modules1) can be applied to Perl as well.

-   **Java** -- for Eclipse programming and in-house QA and testing tools.

See the "[Recommended programming and scripting languages](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Recommended_programming_and_scripting_languages)" Wiki page for more information and updates to this policy. Send proposals for corrections, additions and extensions of the policy on language choice to the languages mailing list, <span class="oem_mark">languages@ncbi.nlm.nih.gov</span>.

Source Code Conventions
-----------------------

This section contains C++ style guidelines, although many of these guidelines could also apply, at least in principle, to other languages. Adherence to these guidelines will promote uniform coding, better documentation, easy to read code, and therefore more maintainable code.

The following topics are discussed in this section:

-   [Public Domain Notice](#public-domain-notice)

-   [Naming Conventions](#naming-conventions)

    -   [Type Names](#type-names)

    -   [Preprocessor Define/Macro](#preprocessor-definemacro)

    -   [Function Arguments and Local Variables](#function-arguments-and-local-variables)

    -   [Constants](#constants)

    -   [Class and Structure Data Members (Fields)](#class-and-structure-data-members-fields)

    -   [Class Member Functions (Methods)](#class-member-functions-methods)

    -   [Module Static Functions and Data](#module-static-functions-and-data)

    -   [Global ("extern") Functions and Data](#global-extern-functions-and-data)

-   [Name Prefixing and/or the Use of Namespaces](#name-prefixing-andor-the-use-of-namespaces)

-   [Use of the NCBI Name Scope](#use-of-the-ncbi-name-scope)

-   [Use of Include Directives](#use-of-include-directives)

-   [Code Indentation and Bracing](#code-indentation-and-bracing)

-   [Class Declaration](#class-declaration)

-   [Function Declaration](#function-declaration)

-   [Function Definition](#function-definition)

-   [Use of Whitespace](#use-of-whitespace)

-   [Alternative Tokens](#alternative-tokens)

-   [Standard Header Template](#standard-header-template)

### Public Domain Notice

All NCBI-authored C/C++ source files **must** begin with a comment containing NCBI's public domain notice, shown below. Ideally (subject to the developer’s discretion), so should any other publicly released source code and data (including scripting languages and data specifications).

    /*  $Id$
     * ===========================================================================
     *
     *                            PUBLIC DOMAIN NOTICE
     *               National Center for Biotechnology Information
     *
     *  This software/database is a "United States Government Work" under the
     *  terms of the United States Copyright Act.  It was written as part of
     *  the author's official duties as a United States Government employee and
     *  thus cannot be copyrighted.  This software/database is freely available
     *  to the public for use. The National Library of Medicine and the U.S.
     *  Government have not placed any restriction on its use or reproduction.
     *
     *  Although all reasonable efforts have been taken to ensure the accuracy
     *  and reliability of the software and data, the NLM and the U.S.
     *  Government do not and cannot warrant the performance or results that
     *  may be obtained by using this software or data. The NLM and the U.S.
     *  Government disclaim all warranties, express or implied, including
     *  warranties of performance, merchantability or fitness for any particular
     *  purpose.
     *
     *  Please cite the author in any work or product based on this material.
     *
     * ===========================================================================
     */

If you have questions, please email to <span class="oem_mark">cpp-core@ncbi.nlm.nih.gov</span>.

### Naming Conventions

Table 1. Naming Conventions

SYNOPSIS
EXAMPLE
**Type Names**
***C****ClassTypeName*
<span class="nctnt ncbi-code">class CMyClass { ..... };</span>
***I****InterfaceName*
<span class="nctnt ncbi-code">class IMyInterface { ..... };</span>
***S****StructTypeName*
<span class="nctnt ncbi-code">struct SMyStruct { ..... };</span>
***U****UnionTypeName*
<span class="nctnt ncbi-code">union UMyUnion { ..... };</span>
***E****EnumTypeName*
<span class="nctnt ncbi-code">enum EMyEnum { ..... };</span>
***F****FunctionTypeName*
<span class="nctnt ncbi-code">typedef int (\*FMyFunc)(void);</span>
***P****PredicateName*
<span class="nctnt ncbi-code">struct PMyPred { bool operator() (.... , ....); };</span>
***T****AuxiliaryTypedef* [(\*)](#*)
<span class="nctnt ncbi-code">typedef map\<int,string\> TMyMapIntStr;</span>
***T****Iterator****\_I***
<span class="nctnt ncbi-code">typedef list\<int\>::iterator TMyList\_I;</span>
***T****ConstIterator****\_CI***
<span class="nctnt ncbi-code">typedef set\<string\>::const\_iterator TMySet\_CI;</span>
***N****Namespace* [(see also)](#see-also)
<span class="nctnt ncbi-code">namespace NMyNamespace { ..... }</span>
**Preprocessor Define/Macro**
*MACRO\_NAME*
<span class="nctnt ncbi-code">\#define MY\_DEFINE 12345</span>
*macro\_arg\_name*
<span class="nctnt ncbi-code">\#define MY\_MACRO(x, y) (((x) + 1) \< (y))</span>
**Function Arguments and Local Variables**
*func\_local\_var\_name*
<span class="nctnt ncbi-code">void MyFunc(int foo, const CMyClass& a\_class)</span><br/><span class="nctnt ncbi-code">{ </span><br/><span class="nctnt ncbi-code">    size\_t foo\_size;</span><br/><span class="nctnt ncbi-code">    int bar;</span>
**Constants**
***k****ConstantName*
<span class="nctnt ncbi-code">const int kMyConst = 123;</span>
***e****EnumValueName*
<span class="nctnt ncbi-code">enum EMyEnum { </span><br/><span class="nctnt ncbi-code">    eMyEnum\_1 = 11, </span><br/><span class="nctnt ncbi-code">    eMyEnum\_2 = 22, </span><br/><span class="nctnt ncbi-code">    eMyEnum\_3 = 33 </span><br/><span class="nctnt ncbi-code">};</span>
***f****FlagValueName*
<span class="nctnt ncbi-code">enum EMyFlags {</span><br/><span class="nctnt ncbi-code">    fMyFlag\_1 = (1\<\<0), ///\< = 0x1 (describe)</span><br/><span class="nctnt ncbi-code">    fMyFlag\_2 = (1\<\<1), ///\< = 0x2 (describe)</span><br/><span class="nctnt ncbi-code">    fMyFlag\_3 = (1\<\<2) ///\< = 0x4 (describe)</span><br/><span class="nctnt ncbi-code">};</span> <br/><span class="nctnt ncbi-code">typedef int TMyFlags; ///\< holds bitwise OR of "EMyFlags"</span>
**Class and Structure Data Members (Fields)**
***m\_****ClassMemberName*
<span class="nctnt ncbi-code">class C { short int m\_MyClassData; };</span>
*struct\_field\_name*
<span class="nctnt ncbi-code">struct S { int my\_struct\_field; };</span>
***sm\_****ClassStaticMemberName*
<span class="nctnt ncbi-code">class C { static double sm\_MyClassStaticData; };</span>
**Class Member Functions (Methods)**
*ClassMethod*
<span class="nctnt ncbi-code">bool MyClassMethod(void);</span>
***x\_****ClassPrivateMethod*
<span class="nctnt ncbi-code">int x\_MyClassPrivateMethod(char c);</span>
**Module Static Functions and Data**
***s\_****StaticFunc*
<span class="nctnt ncbi-code">static char s\_MyStaticFunc(void);</span>
***s\_****StaticVar*
<span class="nctnt ncbi-code">static int s\_MyStaticVar;</span>
**Global (*"extern"*) Functions and Data**
***g\_****GlobalFunc*
<span class="nctnt ncbi-code">double g\_MyGlobalFunc(void);</span>
***g\_****GlobalVar*
<span class="nctnt ncbi-code">short g\_MyGlobalVar;</span>

(\*) The auxiliary typedefs (like ***T****AuxiliaryTypedef*) are usually used for an ad-hoc type mappings (especially when using templates) and not when a real type definition takes place.

### Name Prefixing and/or the Use of Namespaces

In addition to the above naming conventions that highlight the nature and/or the scope of things, one should also use prefixes to:

-   avoid name conflicts

-   indicate the package that the entity belongs to

For example, if you are creating a new class called "<span class="nctnt ncbi-path">Bar</span>" in package "<span class="nctnt ncbi-path">Foo</span>" then it is good practice to name it "<span class="nctnt ncbi-class">CFooBar</span>" rather than just "<span class="nctnt ncbi-class">CBar</span>". Similarly, you should name new constants like "<span class="nctnt ncbi-var">kFooSomeconst</span>", new types like "<span class="nctnt ncbi-type">TFooSometype</span>", etc.

### Use of the NCBI Name Scope

<span class="nctnt ncbi-code">\<ncbistl.hpp\></span>

All NCBI-made “core” API code must be put into the <span class="nctnt ncbi-code">"ncbi::"</span> namespace. For this purpose, there are two preprocessor macros, <span class="nctnt ncbi-macro">BEGIN\_NCBI\_SCOPE</span> and <span class="nctnt ncbi-macro">END\_NCBI\_SCOPE</span>, that must enclose **all** NCBI C++ API code -- both declarations and definitions (see [examples](ch_proj.html#ch_proj.new_modules)). Inside these "brackets", all <span class="nctnt ncbi-code">"std::"</span> and <span class="nctnt ncbi-code">"ncbi::"</span> scope prefixes can (and must!) be omitted.

For code that does not define a new API but merely **uses** the NCBI C++ API, there is a macro <span class="nctnt ncbi-macro">USING\_NCBI\_SCOPE;</span> (semicolon-terminated) that brings all types and prototypes from the <span class="nctnt ncbi-code">"std::"</span> and <span class="nctnt ncbi-code">"ncbi::"</span> namespaces into the current scope, eliminating the need for the <span class="nctnt ncbi-code">"std::"</span> and <span class="nctnt ncbi-code">"ncbi::"</span> prefixes.

Use macro <span class="nctnt ncbi-macro">NCBI\_USING\_NAMESPACE\_STD;</span> (semicolon-terminated) if you want to bring all types and prototypes from the <span class="nctnt ncbi-code">"std::"</span> namespace into the current scope, without bringing in anything from the <span class="nctnt ncbi-code">"ncbi::"</span> namespace.

### Use of Include Directives

If a header file is in the local directory or not on the INCLUDE path, use quotes in the include directive (e.g. <span class="nctnt ncbi-code">\#include "foo.hpp"</span>). In all other cases use angle brackets (e.g. <span class="nctnt ncbi-code">\#include \<bar/foo.hpp\></span>).

In general, if a header file is commonly used, it must be on the INCLUDE path and therefore requires the bracketed form.

### Code Indentation and Bracing

**4-space indentation only**! Tabulation symbol **must not** be used for indentation.

Try not to cross the "standard page boundary" of **80** symbols.

In <span class="nctnt ncbi-code">if, for, while, do, switch, case</span>, etc. and type definition statements:

    if (...) {
        .....;
    } else if (...) {
        .....;
    } else {
        .....;
    }

 

    if (...) {
        .....;
    }
    else if (...) {
        .....;
    }
    else {
        .....;
    }

 

    for (...;  ...;  ...) {
        .....;
    }

 

    while (...) {
        .....;
    }

 

    do {
        .....;
    }
    while (...);

 

    switch (...) {
    case ...: {
        .....;
        break;
    }
    } // switch

 

    struct\|union\|enum <[S\|U\|E]TypeName> {
        .....;
    };

 

    class \| struct \| union <[C\|I\|P\|S\|U]TypeName>
    {
        .....;
    };

 

    try {
        .....;
    }
    catch (exception& e) {
        .....;
    }

### Class Declaration

Class declarations should be rich in [Doxygen-style comments](#doxygen-style-comments). This will increase the value of the Doxygen-based API documentation.

    /// @file FileName
    /// Description of file -- note that this is _required_ if you want
    /// to document global objects such as typedefs, enums, etc.

    ///////////////////////////////////////////////////////////////////////
    ///
    /// CFooClass
    ///
    /// Brief description of class (or class template, struct, union) --
    /// it must be followed by an empty comment line.
    ///
    /// A detailed description of the class -- it follows after an empty
    /// line from the above brief description. Note that comments can
    /// span several lines and that the three /// are required.

    class CFooClass
    {
    public:
        // Constructors and Destructor:

        /// A brief description of the constructor.
        ///
        /// A detailed description of the constructor.
        CFooClass(const char* init_str = NULL); ///< describe parameter here

        /// A brief description for another constructor.
        CFooClass(int init_int); ///< describe parameter here

        ~CFooClass(void); // Usually needs no Doxygen-style comment.

        // Members and Methods:

        /// A brief description of TestMe.
        ///
        /// A detailed description of TestMe. Use the following when 
        /// parameter descriptions are going to be long, and you are 
        /// describing a complex method:
        /// @param foo
        ///   An int value meaning something.
        /// @param bar
        ///   A constant character pointer meaning something.
        /// @return
        ///   The TestMe() results.
        /// @sa CFooClass(), ~CFooClass() and TestMeToo() - see also.
        float TestMe(int foo, const char* bar);

        /// A brief description of TestMeToo.
        ///
        /// Details for TestMeToo. Use this style if the parameter 
        /// descriptions are going to be on one line each:
        /// @sa TestMe()
        virtual void TestMeToo
        (char         par1,  ///< short description for par1
         unsigned int par2   ///< short description for par2
         ) = 0;

        /// Brief description of a function pointer type
        /// (note that global objects like this will not be documented
        /// unless the file itself is documented with the @file command).
        ///
        /// Detailed description of the function pointer type.
        typedef char* (*FHandler)
            (int start,  ///< argument description 1 -- what start means
             int stop    ///< argument description 2 -- what stop  means
             );

        // (NOTE:  The use of public data members is
        //         strictly discouraged!
        //         If used they should be well documented!)
        /// Describe public member here, explain why it’s public.
        int   m_PublicData;

    protected:
        /// Brief description of a data member -- notice no details are
        /// given here since a brief description is adequate.
        double m_FooBar;

        /// Brief function description here.
        /// Detailed description here. More description.
        /// @return Return value description here.
        static int ProtectedFunc(char ch); ///< describe parameter here

    private:
        /// Brief member description here.
        /// Detailed description here. More description.
        int    m_PrivateData;

        /// Brief static member description here.
        static int    sm_PrivateStaticData;

        /// Brief function description here.
        /// Detailed description here. More description.
        /// @return Return value description here.
        double x_PrivateFunc(int some_int = 1); ///< describe parameter here

        // Friends
        friend bool  SomeFriendFunc(void);
        friend class CSomeFriendClass;

        // Prohibit default initialization and assignment
        // -- e.g. when the member-by-member copying is dangerous.

        /// This method is declared as private but is not
        /// implemented to prevent member-wise copying.
        CFooClass(const CFooClass&);

        /// This method is declared as private but is not
        /// implemented to prevent member-wise copying.
        CFooClass& operator= (const CFooClass&);
    };

### Function Declaration

[Doxygen-style comments](#doxygen-style-comments) for functions should describe what the function does, its parameters, and what it returns.

For global function declarations, put all Doxygen-style comments in the header file. Prefix global functions with <span class="nctnt ncbi-var">g\_</span>.

    /// A brief description of MyFunc2.
    ///
    /// Explain here what MyFunc2() does.
    /// @return explain here what MyFunc2() returns.
    bool g_MyFunc2
    (double  arg1,      ///< short description of "arg1"
     string* arg2,      ///< short description of "arg2"
     long    arg3 = 12  ///< short description of "arg3"
     );

### Function Definition

[Doxygen-style comments](#doxygen-style-comments) are not needed for member function definitions or global function definitions because their comments are put with their declarations in the header file.

For static functions, put all Doxygen-style comments immediately before the function definition. Prefix static functions with <span class="nctnt ncbi-var">s\_</span>.

    bool g_MyFunc2
    (double  arg1,
     string* arg2,
     long    arg3
     )
    {
        .......
        .......
        return true;
    }

    /// A brief description of s_MyFunc3.
    ///
    /// Explain here what s_MyFunc3() does.
    /// @return explain here what s_MyFunc3() returns.
    static long s_MyFunc3(void)
    {
        .......
        .......
    }

### Use of Whitespace

As the above examples do not make all of our policies on whitespace clear, here are some explicit guidelines:

-   When reasonably possible, use spaces to align corresponding elements vertically. (This overrides most of the rules below.)

-   Leave one space on either side of most binary operators, and two spaces on either side of boolean <span class="nctnt ncbi-code">&&</span> and <span class="nctnt ncbi-code">\\|\\|</span>.

-   Put one space between the names of flow-control keywords and macros and their arguments, but no space after the names of functions except when necessary for alignment.

-   Leave two spaces after the semicolons in <span class="nctnt ncbi-code">for (...; ...; ...)</span>.

-   Leave whitespace around negated conditions so that the <span class="nctnt ncbi-code">!</span> stands out better.

-   Leave two blank lines between function definitions.

### Alternative Tokens

The alternative tokens "<span class="nctnt ncbi-code">and</span>", "<span class="nctnt ncbi-code">not\_eq</span>", "<span class="nctnt ncbi-code">:\></span>", etc. should not be used in place of the primary tokens "<span class="nctnt ncbi-code">&&</span>", "<span class="nctnt ncbi-code">!=</span>", "<span class="nctnt ncbi-code">]</span>", etc. Although alternative tokens are valid in C++, the primary tokens are more familiar and conventional. Using alternative tokens would therefore make code less readable without much benefit to most C++ developers.

### Standard Header Template

A standard header template file, [header\_template.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/common/header_template.hpp), has been provided in the <span class="nctnt ncbi-path">include/common</span> directory that can be used as a template for creating header files. This header file adheres to the standards outlined in the previous sections and uses a documentation style for files, classes, methods, macros etc. that allows for automatic generation of documentation from the source code. It is strongly suggested that you obtain a copy of this file and model your documentation using the examples in that file.

Doxygen Comments
----------------

[Doxygen](http://www.stack.nl/~dimitri/doxygen/) is an automated API documentation tool. It relies on special comments placed at appropriate places in the source code. Because the comments are in the source code near what they document, the documentation is more likely to be kept up-to-date when the code changes. A configuration and parsing system scans the code and creates the desired output (e.g. HTML).

Doxygen documentation is a valuable tool for software developers, as it automatically creates comprehensive cross-referencing of modules, namespaces, classes, and files. It creates inheritance diagrams, collaboration diagrams, header dependency graphs, and documents each class, struct, union, interface, define, typedef, enum, function, and variable (see the NCBI C++ Toolkit [Doxygen browser](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/)). However, developers must write meaningful comments to get the most out of it.

Doxygen-style comments are essentially extensions of C/C++ comments, e.g. the use of a triple-slash instead of a double-slash. Doxygen-style comments refer to the entity following them by default, but can be made to refer to the entity preceding them by appending the ‘<span class="nctnt ncbi-code">\<</span>’ symbol to the comment token (e.g. ‘<span class="nctnt ncbi-code">///\<</span>’).

Doxygen commands are keywords within Doxygen comments that are used during the document generation process. Common commands are <span class="nctnt ncbi-code">@param</span>, <span class="nctnt ncbi-code">@return</span>, and <span class="nctnt ncbi-code">@sa</span> (i.e. ‘see also’).

Please do not use superfluous comments, such as ‘<span class="nctnt ncbi-code">/// Destructor</span>’. Especially do not use the same superfluous comment multiple times, such as using the same ‘<span class="nctnt ncbi-code">/// Constructor</span>’ comment for different constructors!

Please see the [Doxygen manual](http://www.stack.nl/~dimitri/doxygen/manual.html) for complete usage information. More information can also be found in the chapter on [Toolkit browsers](ch_browse.html).

C++ Guidelines
--------------

This section discusses the following topics:

-   [Introduction to Some C++ and STL Features and Techniques](#introduction-to-some-c++-and-stl-features-and-techniques)

    -   [C++ Implementation Guide](#c++-implementation-guide)

        -   [Limitations on Using C++11 Features](#limitations-on-using-c++11-features)

        -   [Use of STL (Standard Template Library)](#use-of-stl-standard-template-library)

        -   [Use of C++ Exceptions](#use-of-c++-exceptions)

        -   [Design](#design)

        -   [Make Your Code Readable](#make-your-code-readable)

    -   [C++ Tips and Tricks](#c++-tips-and-tricks)

    -   [Standard Template Library (STL)](#standard-template-library-stl-)

        -   [STL Tips and Tricks](#stl-tips-and-tricks)

-   [C++/STL Pitfalls and Discouraged/Prohibited Features](#c++stl-pitfalls-and-discouragedprohibited-features)

    -   [STL and Standard C++ Library's Bad Guys](#stl-and-standard-c++-library's-bad-guys)

        -   [Non-Standard STL Classes](#non-standard-stl-classes)

    -   [C++ Bad Guys](#c++-bad-guys)

        -   [Operator Overload](#operator-overload)

        -   [Assignment and Copy Constructor Overload](#assignment-and-copy-constructor-overload)

        -   [Omitting "void" in a No-Argument Function Prototype](#omitting-void-in-a-no-argument-function-prototype)

        -   [Do Not Mix malloc and new](#do-not-mix-malloc-and-new)

    -   [Miscellaneous Gotchas](#miscellaneous-gotchas)

### Introduction to Some C++ and STL Features and Techniques

#### C++ Implementation Guide

##### Limitations on Using C++11 Features

Many new features of the C++11 Standard are not yet implemented (or implemented poorly) by at least some actual compilers, so - please be very careful about using these new C++11 features.

Do not use C++11 features in code that is:

-   Platform-portable, such as the ["core" (public, non-GUI, non-INTERNAL) part of the C++ Toolkit](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/);

-   Part of regular [coremake builds](https://svn.ncbi.nlm.nih.gov/viewvc/toolkit/trunk/c++/scripts/internal/projects/netopt.lst?view=markup);

-   Supposed to be compiled with older compilers (like GCC-4.4.2, MSVC10, etc) that don't support C++11; or

-   Known to be a dependency for the projects that still need to be compiled with older compilers.

##### Use of STL (Standard Template Library)

Use the [Standard Template Library (STL)](#standard-template-library-stl), which is part of ANSI/ISO C++. It'll make programming easier, as well as make it easier for others to understand and maintain your code.

##### Use of C++ Exceptions

-   Exceptions are useful. However, since exceptions unwind the stack, you must be careful to destroy all resources (such as memory on the heap and file handles) in every intermediate step in the stack unwinding. That means you must always catch exceptions, even those you don't handle, and delete everything you are using locally. In most cases it's very convenient and safe to use the [auto\_ptr](http://www.parashift.com/c++-faq-lite/exceptions.html#faq-17.4) template to ensure the freeing of temporary allocated dynamic memory for the case of exception.

-   Avoid using exception specifications in function declarations, such as:

<!-- -->

    void foo(void) throw ();
    void bar(void) throw (std::exception);

##### Design

-   Use abstract base classes. This increases the reusability of code. Whether a base class should be abstract or not depends on the potential for reuse.

-   Don't expose class member variables, rather expose member functions that manipulate the member variables. This increases reusability and flexibility. For example, this frees you from having the string in-process -- it could be in another process or even on another machine.

-   Don't use multiple inheritance (i.e. <span class="nctnt ncbi-code">class A: public B, public C {}</span>) unless creating interface instead of implementation. Otherwise, you'll run into all sorts of problems with conflicting members, especially if someone else owns a base class. The best time to use multiple inheritance is when a subclass multiply inherits from abstract base classes with only pure virtual functions.

<span class="nctnt highlight">NOTE:</span> Some people prefer the [Unified Modelling Language](http://www.rational.com/uml/index.jtmpl) to describe the relationships between objects.

##### Make Your Code Readable

Use <span class="nctnt ncbi-var">NULL</span> instead of <span class="nctnt ncbi-var">0</span> when passing a null pointer. For example:

    MyFunc(0,0);    // Just looking at this call, you can’t tell which
                    // parameter might be an int and which might be
                    // a pointer.

    MyFunc(0,NULL); // When looking at this call, it’s pretty clear
                    // that the first parameter is an int and
                    // the second is a pointer.

Avoid using <span class="nctnt ncbi-var">bool</span> as a type for function arguments. For example, this might be hard to understand:

    // Just looking at this call, you can’t tell what
    // the third parameter means:
    CompareStrings(s1, s2, true);

Instead, create a meaningful enumerated type that captures the meaning of the parameter. For example, try something like this:

    /////////////////////////////////////////////////////////////////////
    ///
    ///  ECaseSensitivity --
    ///
    ///  Control case-sensitivity of string comparisons.
    ///
    enum ECaseSensitivity {
        eCaseSensitive,   ///< Consider case when comparing.
        eIgnoreCase       ///< Don’t consider case when comparing.
    };

    .....

    ///  Brief description of function here.
    ///  @return
    ///      describe return value here.
    int CompareStrings
    (const string& s1,            ///< First string.
     const string& s2,            ///< Second string.
     ECaseSensitivity comp_case); ///< Controls case-sensitivity
                                  ///< of comparisons.

    .....

    // This call is more understandable because the third parameter
    // is an enum constant rather than a bool constant.
    CompareStrings(s1, s2, eIgnoreCase);

As an added benefit, using an enumerated type for parameters instead of <span class="nctnt ncbi-var">bool</span> gives you the ability to expand the enumerated type to include more variants in the future if necessary - without changing the parameter type.

#### C++ Tips and Tricks

-   Writing something like <span class="nctnt ncbi-code">map\<int, int, less\<int\>\></span> will give you weird errors; instead write <span class="nctnt ncbi-code">map\<int, int, less\<int\> \></span>. This is because <span class="nctnt ncbi-code">\>\></span> is reserved word.

-   Do use pass-by-reference. It'll cut down on the number of pointer related errors.

-   Use <span class="nctnt ncbi-code">const</span> (or <span class="nctnt ncbi-code">enum</span>) instead of <span class="nctnt ncbi-code">\#define</span> when you can. This is much easier to debug.

-   Header files should contain what they contain in C along with classes, const's, and in-line functions.

See the [C++ FAQ](http://www.parashift.com/c++-faq-lite)

#### Standard Template Library (STL)

The STL is a library included in ANSI/ISO C++ for stream, string, and container (linked lists, etc.) manipulation.

##### STL Tips and Tricks

<span class="nctnt ncbi-func">end()</span> does not return an iterator to the last element of a container, rather it returns a iterator just beyond the last element of the container. This is so you can do constructs like

    for (iter = container.begin();  iter != container.end();  iter++)

If you want to access the last element, use "<span class="nctnt ncbi-code">--container.end()</span>". <span class="nctnt highlight">Note:</span> If you use this construct to find the last element, you must first ensure that the container is not empty, otherwise you could get corrupt data or a crash.

The C++ Toolkit includes macros that simplify iterating. For example, the above code simplifies to:

    ITERATE(Type, iter, container)

For more info on <span class="nctnt ncbi-macro">ITERATE</span> (and related macros), see the [ITERATE macros](ch_core.html#ch_core.ITERATE_macros) section.

Iterator misuse causes the same problems as pointer misuse. There are versions of the STL that flag incorrect use of iterators.

Iterators are guaranteed to remain valid after insertion and deletion from <span class="nctnt ncbi-class">list</span> containers, but not <span class="nctnt ncbi-class">vector</span> containers. Check to see if the container you are using preserves iterators.

If you create a container of pointers to objects, the objects are not destroyed when the container is destroyed, only the pointers are. Other than maintaining the objects yourself, there are several strategies for handling this situation detailed in the [literature](app1.appendix1.html#app1.books.html).

If you pass a container to a function, don't add a local object to the container. The local variable will be destroyed when you leave the function.

### C++/STL Pitfalls and Discouraged/Prohibited Features

-   [STL and Standard C++ Library's Bad Guys](#stl-and-standard-c++-library's-bad-guys)

    -   [Non-Standard Classes](#non-standard-classes)

-   [C++ Bad Guys](#c++-bad-guys)

    -   [Operator Overload](#operator-overload)

    -   [Assignment and Copy Constructor Overload](#assignment-and-copy-constructor-overload)

    -   [Omitting "void" in a No-Argument Function Prototype](#omitting-void-in-a-no-argument-function-prototype)

    -   [Do Not Mix malloc and new](#do-not-mix-malloc-and-new)

-   [Miscellaneous Gotchas](#miscellaneous-gotchas)

#### STL and Standard C++ Library's Bad Guys

##### Non-Standard STL Classes

-   Don't use the <span class="nctnt ncbi-class">rope</span> class from some versions of the STL. This is a non-standard addition. If you have questions about what is/isn't in the standard library, consult the [C++ standards](http://www.parashift.com/c++-faq-lite/big-picture.html#faq-6.12).

-   The NCBI C++ Toolkit includes <span class="nctnt ncbi-class">hash\_map</span>, <span class="nctnt ncbi-class">hash\_multimap</span>, <span class="nctnt ncbi-class">hash\_set</span>, and <span class="nctnt ncbi-class">hash\_multiset</span> classes (from headers [\<corelib/hash\_map.hpp\>](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/hash__map_8hpp.html) and [\<corelib/hash\_set.hpp\>](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/hash__set_8hpp.html)). These classes are more portable than, and should be used instead of, the STL's respective <span class="nctnt ncbi-class">hash\_\*</span> classes.

#### C++ Bad Guys

##### Operator Overload

Do not use operator overloading for the objects where they have unnatural or ambiguous meaning. For example, the defining of <span class="nctnt ncbi-code">operator==()</span> for your class <span class="nctnt ncbi-class">"CFoo"</span> so that there exist { CFoo a,b,c; } such that <span class="nctnt ncbi-code">(a == b)</span> and <span class="nctnt ncbi-code">(b == c)</span> are <span class="nctnt ncbi-monospace">true</span> while <span class="nctnt ncbi-code">(a == c)</span> is <span class="nctnt ncbi-monospace">false</span> would be a very bad idea. It turns out that otherwise, especially in large projects, people have different ideas of what an overloaded operator means, leading to all sorts of bugs.

##### Assignment and Copy Constructor Overload

Be advised that the default initialization <span class="nctnt ncbi-code">{CFoo foo = bar;}</span> and assignment <span class="nctnt ncbi-code">{CFoo foo; ...; foo = bar;}</span> do a member-by-member copying. This is not suitable and can be dangerous sometimes. And if you decide to overwrite this default behavior by your own code like:

    class CFoo {
        // a copy constructor for initialization
        CFoo(const CFoo& bar) { ... }
        // an overloaded assignment(=) operator
        CFoo& operator=(const CFoo& bar) { if (&bar != this) ... }
    };

it is **extremely important** that:

-   **both** copy constructor and overloaded assignment be defined

-   they have **just the same** meaning; that is <span class="nctnt ncbi-code">{CFoo foo = bar;}</span> is equivalent to <span class="nctnt ncbi-code">{CFoo foo; foo = bar;}</span>

-   there is a check to prevent self-assignment in your overloaded assignment operator

In many cases when you don't want to have the assignment and copy constructor at all, just add to your class something like:

    class CFoo {
        .............................
    private:
        // Prohibit default initialization and assignment
        CFooClass(const CFooClass&);
        CFooClass& operator=(const CFooClass&);
    };

##### Omitting <span class="nctnt ncbi-code">"void"</span> in a No-Argument Function Prototype

Do not omit <span class="nctnt ncbi-code">"void"</span> in the prototype of a function without arguments (e.g. always write <span class="nctnt ncbi-func">"int f(void)"</span> rather than just <span class="nctnt ncbi-func">"int f()"</span>).

##### Do Not Mix malloc and new

On some platforms, malloc and new may use completely different memory managers, so never "free()" what you created using "new" and never "delete" what you created using "malloc()". Also, when calling C code from C++ **always** allocate any structs or other items using "malloc()". The C routine may use "realloc()" or "free()" on the items, which can cause memory corruption if you allocated using "new."

#### Miscellaneous Gotchas

It is beyond the scope of this document to discuss all C++ gotchas, but this section mentions some important ones:

-   **Avoid** <span class="nctnt ncbi-var">std::endl</span> **when possible**.<br/><br/>Indiscriminate use of <span class="nctnt ncbi-var">ncbi::NcbiEndl</span> or <span class="nctnt ncbi-var">std::endl</span> may lead to very serious problems.<br/><br/>One problem caused by <span class="nctnt ncbi-var">std::endl</span> is a potentially huge performance hit when writing to disk or socket. Another problem is that flushing a compressed stream can short-circuit the compression algorithm, thereby resulting in decreased compression.<br/><br/>Therefore, unless there's a need to flush, just use <span class="nctnt ncbi-monospace">\\n</span> instead of <span class="nctnt ncbi-var">ncbi::NcbiEndl</span> or <span class="nctnt ncbi-var">std::endl</span>.<br/>

-   **Other topics to be aware of:**

    -   line separator characters on Unix, Windows, and Mac

    -   <span class="nctnt ncbi-monospace">\\r</span> and <span class="nctnt ncbi-monospace">\\n</span> on binary vs. text streams (especially when dealing with HTTP and other protocols)

    -   character sets (e.g. Unicode) vs. encodings (e.g. UTF-8)

    -   <span class="nctnt ncbi-type">char</span> vs. <span class="nctnt ncbi-type">w\_char</span> and <span class="nctnt ncbi-monospace">'\\n'</span> vs. <span class="nctnt ncbi-monospace">L'\\n'</span>

    -   flushing vs. physical writing - <span class="nctnt ncbi-var">flush</span> (manipulator), <span class="nctnt ncbi-func">fflush()</span>, <span class="nctnt ncbi-func">sync()</span>, disk write-cache, I/O performance, etc.

    -   <span class="nctnt ncbi-class">iostream</span> buffering (<span class="nctnt ncbi-var">cin</span>/<span class="nctnt ncbi-var">cout</span>/<span class="nctnt ncbi-var">cerr</span>/<span class="nctnt ncbi-var">clog</span> including <span class="nctnt ncbi-func">tie()</span> and <span class="nctnt ncbi-var">unitbuf</span>)<br/>

For more information, see the [C++ Draft Standard](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2011/n3242.pdf), or search related topics on the internet.

Source Code Repositories
------------------------

The following Subversion repositories have been set up for general use within NCBI:

|----------------------------------------------------------------------|------------------------------------------------------------|
| **Repository**                                                       | **Purpose**                                                |
| [toolkit](https://svn.ncbi.nlm.nih.gov/viewvc/toolkit/)              | C++ Toolkit (core and internal) development                |
| [gbench](https://svn.ncbi.nlm.nih.gov/viewvc/gbench/)                | GUI / GBENCH                                               |
| [staff](https://svn.ncbi.nlm.nih.gov/viewvc/staff/)                  | individuals' projects (not parts of any official projects) |
| [misc\_projects](https://svn.ncbi.nlm.nih.gov/viewvc/misc_projects/) | projects not falling into any of the other categories      |

Note for NCBI developers: Using these repositories has the additional advantages that they are:

-   backed up;

-   partially included in automated builds and tests (along with reporting via [email](ch_proj.html#ch_proj.inside_tests) and on the [intranet](http://intranet/ieb/ToolBox/STAT/test_stat/test_stat_ext.cgi)) on multiple platforms and compiler configurations; and

-   integrated with [JIRA](https://jira.ncbi.nlm.nih.gov/secure/Dashboard.jspa) and [FishEye](http://fisheye:8008/).

Testing
-------

Unit testing using the [Boost Unit Test Framework](ch_boost.html) is strongly encouraged for libraries. Within NCBI, unit tests can be incorporated into the nightly automated testsuite by using the <span class="nctnt ncbi-macro">CHECK\_CMD</span> macro in the makefile. All testsuite results are available on the [testsuite web page](http://intranet/ieb/ToolBox/STAT/test_stat/test_stat_ext.cgi). Users can also be automatically emailed with build and/or test results by using the <span class="nctnt ncbi-macro">WATCHERS</span> macro. Please see the chapter on [Using the Boost Unit Test Framework](ch_boost.html) for more information.

Applications should also be tested, and shell scripts are often convenient for this purpose.

Data files used for testing purposes should be checked into SVN with the source code unless they are very large.


