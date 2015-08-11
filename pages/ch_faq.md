---
layout: default
title: C++ Toolkit test
nav: pages/ch_faq
---

<span class="label">29</span>FAQs, Useful Documentation Links, and Mailing Lists
================================================================================

Last Update: August 8, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter contains frequently asked questions and useful links.

### Chapter Outline

-   [FAQs](#faqs)

    -   [Security](#security)

        -   [How do I prevent SQL Injection attacks?](#how-do-i-prevent-sql-injection-attacks)

        -   [How do I prevent Cross-Site Scripting (XSS) attacks?](#how-do-i-prevent-cross-site-scripting-xss-attacks)

    -   [General](#general)

        -   [How do I prepare my development environment for using the C++ Toolkit?](#how-do-i-prepare-my-development-environment-for-using-the-c++-toolkit)

        -   [The GetTypeInfo() method is not declared or defined in any of the objects for which it is part of the interface?](#the-gettypeinfo-method-is-not-declared-or-defined-in-any-of-the-objects-for-which-it-is-part-of-the-interface)

        -   [Which include file should be used in \*.cpp files, class.hpp or class\_.hpp?](#which-include-file-should-be-used-in-*cpp-files-classhpp-or-classhpp)

        -   [How can I disable the XML declaration or DOCTYPE with the serial library?](#how-can-i-disable-the-xml-declaration-or-doctype-with-the-serial-library)

    -   [Compiling](#compiling)

        -   [How do I compile for 32-bit on a 64-bit machine in a typical C++ Toolkit app?](#how-do-i-compile-for-32-bit-on-a-64-bit-machine-in-a-typical-c++-toolkit-app)

        -   [Which Visual C++ project should I build?](#which-visual-c++-project-should-i-build)

        -   [What compiler options are required to be compatible with C++ Toolkit libraries?](#what-compiler-options-are-required-to-be-compatible-with-c++-toolkit-libraries)

            -   [Checked iterators](#checked-iterators)

            -   [C++ exceptions](#c++-exceptions)

            -   [Runtime library](#runtime-library)

    -   [Linking](#linking)

        -   [How does one find the libraries to link when the linker complains of undefined symbols?](#how-does-one-find-the-libraries-to-link-when-the-linker-complains-of-undefined-symbols)

        -   [How do I add a library to a Visual C++ project?](#how-do-i-add-a-library-to-a-visual-c++-project)

        -   [Linker complains it "cannot find symbol" in something like: "SunWS\_cache/CC\_obj\_b/bXmZkg3zX5VBJvYgjABX.o"](#linker-complains-it-cannot-find-symbol-in-something-like-sunwscacheccobjbbxmzkg3zx5vbjvygjabxo)

        -   [MAKE complains it does not know "how to make target: /home/qqq/c++/WorkShop6-Debug/lib/.seqset.dep"](#make-complains-it-does-not-know-how-to-make-target-homeqqqc++workshop6-debuglibseqsetdep)

        -   [Still getting bizarre errors with unresolved symbols, unfound libraries, etc., and nothing seems to help out much](#still-getting-bizarre-errors-with-unresolved-symbols-unfound-libraries-etc-and-nothing-seems-to-help-out-much)

    -   [Debugging](#debugging)

        -   [Debugger (DBX) warns it "cannot find file /home/coremake/c++/foobar.cpp", then it does not show source code](#debugger-dbx-warns-it-cannot-find-file-homecoremakec++foobarcpp-then-it-does-not-show-source-code)

    -   [ASN](#asn)

        -   [Creating an out-of-tree application that uses your own local ASN.1 spec and a pre-built C++ Toolkit](#creating-an-out-of-tree-application-that-uses-your-own-local-asn1-spec-and-a-pre-built-c++-toolkit)

        -   [How to add new ASN.1 module from the C Toolkit to the C++ Toolkit?](#how-to-add-new-asn1-module-from-the-c-toolkit-to-the-c++-toolkit)

        -   [Converting ASN.1 object in memory from C to C++ representation (or vice versa)](#converting-asn1-object-in-memory-from-c-to-c++-representation-or-vice-versa-)

-   [Useful Documentation Links](#useful-documentation-links)

-   [Mailing Lists](#mailing-lists)

FAQs
----

### Security

Following are some of the common questions regarding security. If you have a different question, or if these questions don't fully address your concern, please email your question to <span class="oem_mark">cpp-core@ncbi.nlm.nih.gov</span>.

-   [How do I prevent SQL Injection attacks?](#how-do-i-prevent-sql-injection-attacks)

-   [How do I prevent Cross-Site Scripting (XSS) attacks?](#how-do-i-prevent-cross-site-scripting-xss-attacks)

#### How do I prevent SQL Injection attacks?

Summary:

1  
No SQL EVER passed in by a user is allowed.

2  
Use stored procedures whenever possible.

3  
If stored procedures are not possible, and if the SQL statement needs to be constructed from user input, use parameterized SQL whenever possible.

4  
If constructing dynamic SQL from user input is unavoidable, you MUST sanitize the user input.

Please see the NCBI document "[SQL Security and Hygiene](https://sp.ncbi.nlm.nih.gov/IEB/ISS/dbas/Shared%20Documents/SQL%20Security%20and%20Hygiene.doc)" for more details.

Sample code is also available for [SDBAPI](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/sdbapi/) and [DBAPI](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/).

For more information on using a database from your application, see the "[Database Access](ch_dbapi.html)" chapter of the C++ Toolkit book.

#### How do I prevent Cross-Site Scripting (XSS) attacks?

NEVER trust user-supplied strings - always sanitize them before using them.

| Before including a user-supplied string in: | Sanitize the string with:                                                                                                                     |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| a URL                                       | <span class="nctnt ncbi-func">[NStr::URLEncode()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=URLEncode)</span>               |
| JavaScript                                  | <span class="nctnt ncbi-func">[NStr::JavaScriptEncode()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=JavaScriptEncode)</span> |
| XML                                         | <span class="nctnt ncbi-func">[NStr::XmlEncode()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=XmlEncode)</span>               |
| HTML                                        | <span class="nctnt ncbi-func">[NStr::HtmlEncode()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=HtmlEncode)</span>             |
| JSON                                        | <span class="nctnt ncbi-func">[NStr::JsonEncode()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=JsonEncode)</span>             |
| SQL                                         | <span class="nctnt ncbi-func">[NStr::SQLEncode()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SQLEncode)</span>               |

<span class="nctnt highlight">Note:</span> In addition to sanitizing URLs with [NStr::URLEncode()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=URLEncode), the [CUrl](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCUrl.html) class can be used to take apart and reassemble a URL. If the original URL was malformed an error would be produced. At the very least, improper URL segments would be mangled.

### General

#### How do I prepare my development environment for using the C++ Toolkit?

That depends on your development environment and whether you are inside or outside of NCBI:

-   [Unix or Mac OS X inside NCBI](#unix-or-mac-os-x-inside-ncbi)

-   [Unix or Mac OX X outside NCBI](#unix-or-mac-ox-x-outside-ncbi)

-   [Windows inside NCBI](#windows-inside-ncbi)

-   [Windows outside NCBI](#windows-outside-ncbi)

##### Unix or Mac OS X inside NCBI

All developer Unix accounts should be automatically prepared for using the C++ Toolkit. You should have a <span class="nctnt ncbi-path">~/.ncbi\_hints</span> file with a non-trivial <span class="nctnt ncbi-monospace">facilities</span> line that will be sourced when logging in. If everything is set up properly, the following commands should provide meaningful output:

    svn --version
    new_project
    echo $NCBI

##### Unix or Mac OX X outside NCBI

After [downloading the Toolkit source](ch_getcode_svn.html#ch_getcode_svn.ftp_download), set environment variable <span class="nctnt ncbi-var">NCBI</span> to <span class="nctnt ncbi-path">\<toolkit\_root\></span> (where <span class="nctnt ncbi-path">\<toolkit\_root\></span> is the top-level directory containing <span class="nctnt ncbi-path">configure</span>) and add <span class="nctnt ncbi-path">$NCBI/scripts/common</span> to your <span class="nctnt ncbi-var">PATH</span>.

Once the Toolkit is [configured and built](ch_config.html#ch_config.UNIX), then you can use it.

##### Windows inside NCBI

A [supported version of MSVC](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/public_releases/release_notes.html#release_notes.Platforms_OSs__compi) must be installed.

A Subversion client must be installed. Normally, the systems team installs Subversion clients on new PCs and updates the client for everyone at the same time. Therefore, your PC should have a suitable Subversion client installed. To make sure that your client is working properly, enter <span class="nctnt ncbi-cmd">svn --version</span> in your <span class="nctnt ncbi-cmd">cmd.exe</span> shell. If this doesn't work, email <span class="oem_mark">pc.systems@ncbi.nlm.nih.gov</span>.

Your <span class="nctnt ncbi-monospace">PATH</span> should include <span class="nctnt ncbi-path">\\\\snowman\\win-coremake\\Scripts\\bin</span>.

If you want to step into the source for the C++ Toolkit libraries while debugging, then drive <span class="nctnt ncbi-path">S:</span> must be mapped to <span class="nctnt ncbi-path">\\\\snowman\\win-coremake\\Lib</span>. You can map it or let the <span class="nctnt ncbi-cmd">new\_project</span> script map it for you.

##### Windows outside NCBI

A [supported version of MSVC](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/public_releases/release_notes.html#release_notes.Platforms_OSs__compi) must be installed.

[Download the Toolkit source](ch_getcode_svn.html#ch_getcode_svn.ftp_download).

Once the Toolkit is [configured and built](ch_config.html#ch_config.MS_Windows), then you can use it.

#### The <span class="nctnt ncbi-func">GetTypeInfo()</span> method is not declared or defined in any of the objects for which it is part of the interface

The macro DECLARE\_INTERNAL\_TYPE\_INFO() is used in the <span class="nctnt ncbi-path">\*.hpp</span> files to declare the <span class="nctnt ncbi-func">GetTypeInfo()</span>. There are several macros that are used to implement <span class="nctnt ncbi-func">GetTypeInfo()</span> methods in <span class="nctnt ncbi-path">\*.cpp</span> files. These macros are generally named and used as follows:

    BEGIN_*_INFO(...)
    {
        ADD_*(...)
        ...
    }

See [User-defined Type Information in the Programming Manual](ch_ser.html#ch_ser.usrtypeinfo.html) for more information.

#### Which include file should be used in \*.cpp files, class.hpp or class\_.hpp?

Include the **class**.hpp (file without underscore). Never instantiate or use a class of the form C\*\_Base directly. Instead use the C\* form which inherits from the C\*\_Base class (e.g., don't use CSeq\_id\_Base directly -- use CSeq\_id instead).

#### How can I disable the XML declaration or DOCTYPE with the serial library?

Here's a code snippet that shows all combinations:

    // serialize XML with both an XML declaration and with a DOCTYPE (default)
    ostr << MSerial_Xml << obj;

    // serialize XML without an XML declaration
    ostr << MSerial_Xml(fSerial_Xml_NoXmlDecl) << obj;

    // serialize XML without a DOCTYPE
    ostr << MSerial_Xml(fSerial_Xml_NoRefDTD) << obj;

    // serialize XML without either an XML declaration or a DOCTYPE
    ostr << MSerial_Xml(fSerial_Xml_NoXmlDecl \| fSerial_Xml_NoRefDTD) << obj;

<span class="nctnt highlight">Note:</span> The serial library can read XML whether or not it contains the XML declaration or DOCTYPE without using special flags. For example:

    istr >> MSerial_Xml >> obj;

### Compiling

#### How do I compile for 32-bit on a 64-bit machine in a typical C++ Toolkit app?

Our 64-bit Linux systems only support building 64-bit code; to produce 32-bit binaries, you'll need a 32-bit system.

#### Which Visual C++ project should I build?

After creating a new project, you may notice quite a few projects appear in the solution, besides your program, and that the **-HIERARCHICAL-VIEW-** project is bold (indicating that it's the startup project). Do not build any of these projects or the solution as a whole. Instead, set your program as the default startup project and build it.

You can build **-CONFIGURE-DIALOG-** if you need to reconfigure your project (see the section on [using the configuration GUI](ch_config.html#ch_config.Configuring_with_the_Java_GUI)), and you will need to build **-CONFIGURE-** if you add libraries (see the question below on [adding a library to a Visual C++ project](#adding-a-library-to-a-visual-c++-project)).

#### What compiler options are required to be compatible with C++ Toolkit libraries?

These compiler options must be properly set under Microsoft Visual C++:

-   [C++ exceptions](#c++-exceptions)

-   [Runtime library](#runtime-library)

-   [Checked iterators](#checked-iterators)

##### C++ exceptions

NCBI C++ Toolkit libraries use the <span class="nctnt ncbi-monospace">/EHsc</span> compiler option with Visual C++ to:

-   ensure that C++ objects that will go out of scope as a result of the exception are destroyed;

-   ensure that only C++ exceptions should be caught; and

-   assume that extern C functions never throw a C++ exception.

For more information, see the MSDN [page](http://msdn.microsoft.com/en-us/library/1deeycx5.aspx) on <span class="nctnt ncbi-monospace">/EH</span>.

##### Runtime library

You must specify the appropriate Visual C++ runtime library to link with:

| Configuration | Compiler Option                                |
|---------------|------------------------------------------------|
| DebugDLL      | <span class="nctnt ncbi-monospace">/MDd</span> |
| DebugMT       | <span class="nctnt ncbi-monospace">/MTd</span> |
| ReleaseDLL    | <span class="nctnt ncbi-monospace">/MD</span>  |
| ReleaseMT     | <span class="nctnt ncbi-monospace">/MT</span>  |

For more information, see the MSDN [page](http://msdn.microsoft.com/en-us/library/2kzt1wy3.aspx) on runtime library options.

##### Checked iterators

<span class="nctnt highlight">Note:</span> Parts of this section refer to Visual C++ 2008, which is no longer supported. This content is currently retained for historical reference only, and may be removed in the future.

Microsoft Visual C++ provides the option of using "Checked Iterators" to ensure that you do not overwrite the bounds of your STL containers. Checked iterators have a different internal structure than, and are therefore incompatible with, non-checked iterators. If both are used in the same program, it will probably crash. Checked iterators also have somewhat lower performance than non-checked iterators.

Therefore, when building with Visual C++, you must ensure that the same checked iterators setting is used for all compilation units that use STL iterators. This includes the Visual C++ standard libraries, the NCBI C++ Toolkit libraries, your code, and any other libraries you link with.

To disable checked iterators, set <span class="nctnt ncbi-monospace">\_SECURE\_SCL=0</span>; to enable them, set <span class="nctnt ncbi-monospace">\_SECURE\_SCL=1</span>.

The Visual C++ defaults for <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> are:

| Visual C++ Version | Debug | Release |
|--------------------|-------|---------|
| 2010               | 1     | 0       |
| 2008               | 1     | 1       |

By default, the compiler options for NCBI C++ Toolkit libraries do not specify the <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> option for debug configurations, and specify <span class="nctnt ncbi-monospace">\_SECURE\_SCL=0</span> for release configurations. Therefore they use checked iterators for debug configurations, but not for release configurations.

<span class="nctnt highlight">Note:</span> Your code may crash if any two libraries you link with don't use the same settings. For example:

-   You're building a release configuration using Visual C++ 2008. You build the C++ Toolkit separately and use it as a third party package (in which case it will use <span class="nctnt ncbi-monospace">\_SECURE\_SCL=0</span>). Your other code and/or other libraries are compiled with default settings (which for release in VS2008 sets <span class="nctnt ncbi-monospace">\_SECURE\_SCL=1</span>).

-   You're using a third party library that uses different settings than the C++ Toolkit.

If you need to use a different setting for <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> than the Toolkit uses, you will have to recompile all Toolkit libraries that you want to link with. To change this setting and rebuild the Toolkit:

1  
Open [src\\build-system\\Makefile.mk.in.msvc](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/Makefile.mk.in.msvc).

2  
Edit the <span class="nctnt ncbi-monospace">PreprocessorDefinitions</span> entry in the <span class="nctnt ncbi-monospace">[Compiler.\*release]</span> section for the desired configuration(s), using <span class="nctnt ncbi-monospace">\_SECURE\_SCL=0;</span> or <span class="nctnt ncbi-monospace">\_SECURE\_SCL=1;</span>.

3  
Build the <span class="nctnt ncbi-monospace">-CONFIGURE-</span> project in the solution that contains all the Toolkit libraries you want to use. See the section on [choosing a build scope](ch_config.html#ch_config.Choosing_a_Build_Scope) for tips on picking the solution. Ignore the reload solution prompts - when the build completes, then close and reopen the solution.

4  
Build the <span class="nctnt ncbi-monospace">-BUILD-ALL-</span> project to rebuild the libraries.

A similar situation exists for the <span class="nctnt ncbi-monospace">\_HAS\_ITERATOR\_DEBUGGING</span> macro, however the C++ Toolkit does not set this macro for either 2008 or 2010, so you are unlikely to encounter any problems due to this setting. It's possible (however unlikely) that other third party libraries could turn this macro off in debug, in which case you'd have to rebuild so the settings for all libraries match.

By default, <span class="nctnt ncbi-monospace">\_HAS\_ITERATOR\_DEBUGGING</span> is turned on in debug but can be turned off. However, it cannot be turned on in release.

Finally, the macro <span class="nctnt ncbi-monospace">\_ITERATOR\_DEBUG\_LEVEL</span> was introduced with Visual C++ 2010 to simplify the use of <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> and <span class="nctnt ncbi-monospace">\_HAS\_ITERATOR\_DEBUGGING</span>.

If you set <span class="nctnt ncbi-monospace">\_ITERATOR\_DEBUG\_LEVEL</span>, then <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> and <span class="nctnt ncbi-monospace">\_HAS\_ITERATOR\_DEBUGGING</span> will be set according to this table:

| <span class="nctnt ncbi-monospace">\_ITERATOR\_DEBUG\_LEVEL</span> | <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> | <span class="nctnt ncbi-monospace">\_HAS\_ITERATOR\_DEBUGGING</span> |
|--------------------------------------------------------------------|---------------------------------------------------------|----------------------------------------------------------------------|
| 0                                                                  | 0                                                       | 0                                                                    |
| 1                                                                  | 1                                                       | 0                                                                    |
| 2                                                                  | 1                                                       | 1                                                                    |

If you don't set <span class="nctnt ncbi-monospace">\_ITERATOR\_DEBUG\_LEVEL</span>, it will be set automatically according to the values of <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> and <span class="nctnt ncbi-monospace">\_HAS\_ITERATOR\_DEBUGGING</span> per the above table. Therefore, you can use either <span class="nctnt ncbi-monospace">\_ITERATOR\_DEBUG\_LEVEL</span> or <span class="nctnt ncbi-monospace">\_SECURE\_SCL</span> and <span class="nctnt ncbi-monospace">\_HAS\_ITERATOR\_DEBUGGING</span> as you see fit. In most cases, you won't need to set any of them. You just need to know about them in case you link with libraries that use different settings.

For more information, see:

-   [Checked Iterators](http://msdn.microsoft.com/en-us/library/aa985965.aspx)

-   [What's New in Visual C++](http://msdn.microsoft.com/en-us/library/dd465215.aspx)

-   [Breaking Changes in Visual C++](http://msdn.microsoft.com/en-us/library/bb531344.aspx)

### Linking

#### How does one find the libraries to link when the linker complains of undefined symbols?

Two tools are available to resolve the common linking questions:

| Question                                              | Tool                                                                                                  |
|-------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Which libraries contain a given symbol?               | [Library search](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lib_search/libsearch.cgi?public=yes) |
| Which other libraries does a given library depend on? | [Library dependencies](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/dglistdeps.cgi)      |

For example, suppose the linker complains about the symbol <span class="nctnt ncbi-func">ncbi::CIStreamBuffer::FindChar(char)</span> being undefined. Here is how to use these tools to resolve this problem:

1  
To find the library(s) where the unresolved symbol is defined, use the [Library search](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lib_search/libsearch.cgi?public=yes) tool:<br/><br/>Using the example above, enter <span class="nctnt ncbi-monospace">FindChar</span> as a search term. The library where this symbol is defined is [libxutil.a (ncbi\_core)](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/depgraphs/xutil.html).<br/><br/>Now that you have the library that defines the symbol, you can proceed to find the library dependencies it introduces. <span class="nctnt highlight">Note:</span> The simplest way to do this is by just clicking on the library in the search results to show its dependencies. Alternatively, you can proceed to step 2.<br/>

2  
The [Library dependencies](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/dglistdeps.cgi) tool finds all the other libraries that a given library depends on. This tool can also help you create the <span class="nctnt ncbi-monospace">LIB</span> and <span class="nctnt ncbi-monospace">LIBS</span> lines in your makefile. For example, enter your current <span class="nctnt ncbi-monospace">LIB</span> and <span class="nctnt ncbi-monospace">LIBS</span> lines plus the new library from step 1, and it will generate optimized <span class="nctnt ncbi-monospace">LIB</span> and <span class="nctnt ncbi-monospace">LIBS</span> lines containing the library needed for your symbol and any other needed libraries.<br/><br/>Continuing with the example above, entering <span class="nctnt ncbi-monospace">libxutil.a</span> (or just <span class="nctnt ncbi-monospace">xutil</span>) will create this result:<br/><br/><span class="nctnt ncbi-code">LIB  = </span>[xutil](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/xutil.html) [xncbi](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/xncbi.html)<br/><span class="nctnt ncbi-code">LIBS = $(ORIG\_LIBS)</span><br/><br/>Clicking on any of the links will show the required items for that link plus a dependency graph for the clicked item. The nodes in the diagram are also navigable.

<span class="nctnt highlight">Note:</span> If you are using Visual C++, please also see the question about [adding libraries to Visual C++ projects](#adding-libraries-to-visual-c++-projects).

To make it easier to work with the NCBI C++ Toolkit's many libraries, we have generated illustrations of their dependency relationships, available for various scopes and in various formats:

NCBI C++ Library Dependency Graphs (including internal libraries)

|                                       | GIF                                                                                | PNG                                                                                     | PDF                                                                                     | PostScript                                                                       | Text                                                                                    |
|-----------------------------------------------------------|------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------|
| All libraries                                             |                                                                |                                                                     | [PDF](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/depgraphs/depgraph-full.pdf) | [PS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-full.ps) | [TXT](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/depgraphs/depgraph-full.txt) |
| Just C++ Toolkit libraries                                |                                                                |                                                                     | [PDF](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/depgraphs/depgraph-c++.pdf)  | [PS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-c++.ps)  |                                                                     |
| Highly connected or otherwise noteworthy public libraries | [GIF](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-core.gif) | [PNG](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/depgraphs/depgraph-core.png) | [PDF](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/depgraphs/depgraph-core.pdf) | [PS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-core.ps) |                                                                     |

NCBI C++ Library Dependency Graphs (public libraries only)

|                                       | GIF                                                                                | PNG                                                                                | PDF                                                                                        | PostScript                                                                               | Text                                                                                 |
|-----------------------------------------------------------|------------------------------------------------------------------------------------|------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------|
| All libraries                                             |                                                                |                                                                | [PDF](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-public.pdf)       | [PS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-public.ps)       | [TXT](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-public.txt) |
| Non-GUI libraries                                         |                                                                |                                                                | [PDF](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-public-nogui.pdf) | [PS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-public-nogui.ps) |                                                                  |
| GUI libraries                                             | [GIF](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-gui.gif)  | [PNG](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-gui.png)  | [PDF](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-gui.pdf)          | [PS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-gui.ps)          |                                                                  |
| Highly connected or otherwise noteworthy public libraries | [GIF](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-core.gif) | [PNG](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-core.png) | [PDF](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-core.pdf)         | [PS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/depgraph-core.ps)         |                                                                  |

In cases where the above methods do not work, you can also search manually using the following steps:

1  
Look for the source file that defines the symbol. This can be done by going to the [LXR source browser](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDate) and doing an identifier search on the symbol (e.g., [CDate](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDate&d=) or [XmlEncode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=XmlEncode&d=)). Look for a source file where the identifier is defined (e.g. in the "Defined as a class in" section for <span class="nctnt ncbi-class">CDate</span>, or in the "Defined as a function in" section for <span class="nctnt ncbi-func">XmlEncode()</span>). For serializable object classes (such as <span class="nctnt ncbi-class">CDate</span>) look for the base class definition. Follow a link to this source file.

2  
Near the top of the LXR page for the source file is a path, and each component of the path links to another LXR page. Click the link to the last directory.

3  
The resulting LXR page for the directory should list the makefile for the library of interest (e.g. <span class="nctnt ncbi-path">Makefile.general.lib</span> for <span class="nctnt ncbi-func">CDate</span> or <span class="nctnt ncbi-path">Makefile.corelib.lib</span> for <span class="nctnt ncbi-func">XmlEncode()</span>). Click on the link to the makefile. You should see the LIB line with the name of the library that contains your symbol.

4  
Add the library name to the list of libraries you already have and enter them into the [library dependencies tool](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/depgraphs/dglistdeps.cgi) to create your final LIB and LIBS lines.

In some cases, the library name is a variant on the subdirectory name. These variants are summarized in [Table 1](#table-1).

Table 1. Examples where the library name is a variant on the sub directory name

| Directory                              | Library                            |
|----------------------------------------|------------------------------------|
| corelib/test                           | test\_mt                           |
| corelib                                | xncbi                              |
| ctools/asn                             | xasn                               |
| cgi                                    | xcgi or xfcgi                      |
| connect                                | xconnect                           |
| connect/test                           | xconntest                          |
| ctools                                 | xctools                            |
| html                                   | xhtml                              |
| objects/mmdb{1,2,3}                    | mmdb (consolidated)                |
| objects/seq{,align,block,feat,loc,res} | seq (consolidated) or $(SEQ\_LIBS) |
| objmgr                                 | xobjmgr                            |
| objmgr/util                            | xobjutil                           |
| objtools/alnmgr                        | xalnmgr                            |
| serial                                 | xser                               |
| util                                   | xutil                              |

Most often, difficulties arise when one is linking an application using the numerous <span class="nctnt ncbi-path">"objects/"</span> libraries. To give you some relief, here are some examples involving such libraries. They show the right order of libraries, as well as which libraries you may actually need. Using this as a starting point, it's **much** easier to find the right combination of libraries:

-   first, to find and add missing libraries using the generic technique described above

-   then, try to throw out libraries which you believe are not actually needed

<!-- -->

    LIB = id1 seqset $(SEQ_LIBS) pub medline biblio general \ 
          xser xconnect xutil xncbi 
    LIB = ncbimime cdd cn3d mmdb scoremat seqset $(SEQ_LIBS) \ 
          pub medline biblio general xser xutil xncbi

#### How do I add a library to a Visual C++ project?

If you are using Visual C++, you should [add the appropriate LIB and LIBS lines](#add-the-appropriate-lib-and-libs-lines) to the <span class="nctnt ncbi-path">Makefile.\<your\_project\>.app</span> file located in the source directory, then build the **-CONFIGURE-** target, then close and reopen the solution. This process will update the project properties with the proper search directories and required libraries.

#### Linker complains it "cannot find symbol" in something like: "SunWS\_cache/CC\_obj\_b/bXmZkg3zX5VBJvYgjABX.o"

Go to the relevant build dir, clean and rebuild everything using:

    cd /home/qqq/c++/WorkShop6-Debug/build/FooBar 
    make purge_r all_r

#### MAKE complains it does not know "how to make target: /home/qqq/c++/WorkShop6-Debug/lib/.seqset.dep"

This means that the "<span class="nctnt ncbi-lib">libseqset.a</span>" library is not built. To build it:

    cd /home/qqq/c++/WorkShop6-Debug/build/objects/seqset 
    make

#### Still getting bizarre errors with unresolved symbols, unfound libraries, etc., and nothing seems to help out much

As the last resort, try to CVS update, reconfigure, clean and rebuild everything:

    cd /home/qqq/c++/ 
    cvs -q upd -d 
    compilers/WorkShop6.sh 32 ........ 
    make purge_r 
    make all_r

### Debugging

#### Debugger (DBX) warns it "cannot find file /home/coremake/c++/foobar.cpp", then it does not show source code

This happens when you link to the public C++ Toolkit libraries (from <span class="nctnt ncbi-path">"$NCBI/c++/\*/lib/"</span>), which are built on other hosts and thus hard-coded with the source paths on these other hosts. All you have to do is to point DBX to the public sources (at <span class="nctnt ncbi-path">"$NCBI/c++"</span>) by just adding to your DBX resource file (<span class="nctnt ncbi-path">~/.dbxrc</span>) the following lines:

    pathmap /home/coremake/c++ /netopt/ncbi_tools/c++ 
    pathmap /home/coremake/c++2 /netopt/ncbi_tools/c++ 
    pathmap /home/coremake/c++3 /netopt/ncbi_tools/c++ 
    pathmap /j/coremake/c++ /netopt/ncbi_tools/c++ 
    pathmap /j/coremake/c++2 /netopt/ncbi_tools/c++ 
    pathmap /j/coremake/c++3 /netopt/ncbi_tools/c++

### ASN

#### Creating an out-of-tree application that uses your own local ASN.1 spec and a pre-built C++ Toolkit

Lets say you have your ASN.1 specification (call it <span class="nctnt ncbi-path">foo.asn</span>) and now you want to build an application (call its source code <span class="nctnt ncbi-path">foo\_main.cpp</span>) which performs serialization of objects described in <span class="nctnt ncbi-path">foo.asn</span>. To complicate things, lets also assume that your ASN.1 spec depends on (imports) one of the ASN.1 specs already in the C++ Toolkit, like <span class="nctnt ncbi-monospace">Date</span> described in the <span class="nctnt ncbi-monospace">NCBI-General</span> module of <span class="nctnt ncbi-path">general.asn</span>. For example, your <span class="nctnt ncbi-path">foo.asn</span> could look like:

    NCBI-Foo DEFINITIONS ::= 
    BEGIN 
    EXPORTS Foo; 
    IMPORTS Date FROM NCBI-General; 
    Foo ::= SEQUENCE { 
        str  VisibleString, 
        date Date 
    } 
    END

Now, lets assume that the pre-built version of the NCBI C++ Toolkit is available at <span class="nctnt ncbi-path">$NCBI/c++</span>, and that you want to use the Toolkit's pre-built sources and libraries in your application. First, generate (using [datatool](ch_app.html#ch_app.datatool)) the serialization sources, and create the serialization library:

    ## Create new project directory, with a model makefile for your
    ## local ASN.1 serialization library, and copy "foo.asn"
    cd ~/tmp
    new_project foo lib/asn
    cd foo 
    cp /bar/bar/bar/foo.asn .

    ## Using DATATOOL, generate data serialization sources for your
    ## ASN.1 specs described in "foo.asn":
    datatool -oR $NCBI/c++ -m foo.asn \
     -M "objects/general/general.asn" -oA -oc foo -opc . -oph .

    ## Adjust in the library makefile "Makefile.foo.lib"
    SRC = foo__ foo___

    ## Build the library
    make -f Makefile.foo_lib

Then, create and build the application:

    ## Create new application project, and copy your app sources.
    new_project foo_main app
    cd foo_main
    cp /bar/bar/bar/foo_main.cpp .

    ## Adjust the application makefile "Makefile.foo_main.app"
    PRE_LIBS = -L.. -lfoo
    CPPFLAGS = -I.. $(ORIG_CPPFLAGS)
    LIB = general xser xutil xncbi

    ## Build the application
    make -f Makefile.foo_main_app

#### How to add new ASN.1 specification to the C++ Toolkit?

<span class="nctnt highlight">Caution!</span> If you are not in the C++ core developers group, please do not do it yourself! -- instead, just send your request to <span class="nctnt ncbi-monospace">cpp-core@ncbi.nlm.nih.gov</span>.

#### Converting ASN.1 object in memory from C to C++ representation (or vice versa)

The C++ Toolkit header [ctools/asn\_converter.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/ctools/asn_converter.hpp) now provides a template class ([CAsnConverter\<\>](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CAsnConverter&d=C)) for this exact purpose.

Useful Documentation Links
--------------------------

-   [[Doc] ISO/ANSI C++ Draft Standard Working Papers (Intranet only)](http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2005/n1905.pdf)

-   [[Doc] MSDN Online Search](http://search.microsoft.com/?mkt=en-US)

-   [[Literature] Books and links to C++ and STL manuals](app1.appendix1.html#app1.books.html)

-   [[Example] NCBI C++ makefile hierarchy for project "corelib/"](ch_proj.html#ch_proj.makefile_hierarchy)

-   [[Chart] NCBI C++ source tree hierarchy](ch_start.html#ch_start.F1)

-   [[Chart] NCBI C++ build tree hierarchy](ch_start.html#ch_start.F2)

-   [[Chart] NCBI C++ Library Dependency graph](#chart-ncbi-c++-library-dependency-graph)

-   [[Doc] NCBI IDX Database Documentation (Intranet only)](https://sp.ncbi.nlm.nih.gov/IEB/pss/idxapi/Shared%20Documents/Forms/AllItems.aspx)

-   [[Doc] Documentation styles](ch_style.html)

Mailing Lists
-------------

-   Announcements: <http://www.ncbi.nlm.nih.gov/mailman/listinfo/cpp-announce> (read-only)

-   Everybody: <http://www.ncbi.nlm.nih.gov/mailman/listinfo/cpp>

-   Core developers: <http://www.ncbi.nlm.nih.gov/mailman/listinfo/cpp-core>

-   Object Manager: <http://www.ncbi.nlm.nih.gov/mailman/listinfo/cpp-objmgr>

-   GUI: <http://www.ncbi.nlm.nih.gov/mailman/listinfo/cpp-gui>

-   SVN and CVS logs: <http://www.ncbi.nlm.nih.gov/mailman/listinfo/cpp-cvs> (read-only)

[Internal mailing lists](http://intranet.ncbi.nlm.nih.gov/wiki-private/CxxToolkit/index.cgi/Internal_Mailing_Lists) are also available to those inside NCBI.


