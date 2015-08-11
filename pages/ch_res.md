---
layout: default
title: C++ Toolkit test
nav: pages/ch_res
---

<span class="label">26</span>C Toolkit Resources for C++ Toolkit Users
======================================================================

Last Update: February 3, 2014.

Overview
--------

For certain tasks in the C++ Toolkit environment, it is necessary to use, or at least refer to, material from the NCBI C Toolkit. Here are some links relevant to the C Toolkit:

-   [C Toolkit Documentation](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/SDKDOCS/INDEX.HTML)

-   [C Toolkit Queryable Source Browser](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/SB/hbr.html)

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Using NCBI C and C++ Toolkits together](#using-ncbi-c-and-c++-toolkits-together)

    -   [Overview](#overview)

    -   [Shared Sources](#shared-sources)

        -   [CONNECT Library](#connect-library)

        -   [ASN.1 Specifications](#asn1-specifications)

    -   [Run-Time Resources](#run-time-resources)

        -   [LOG and CNcbiDiag](#log-and-cncbidiag)

        -   [REG and CNcbiRegistry](#reg-and-cncbiregistry)

        -   [MT\_LOCK and CRWLock](#mtlock-and-crwlock)

        -   [CONNECT Library in C++ Code](#connect-library-in-c++-code)

        -   [C Toolkit diagnostics redirection](#c-toolkit-diagnostics-redirection)

        -   [CONNECT Library in C Code](#connect-library-in-c-code)

-   [Access to the C Toolkit source tree using CVS](#access-to-the-c-toolkit-source-tree-using-cvs)

    -   [CVS Source Code Retrieval for In-House Users with Read-Write Access](#cvs-source-code-retrieval-for-in-house-users-with-read-write-access)

        -   [Using CVS from Unix or Mac OS X](#using-cvs-from-unix-or-mac-os-x)

        -   [Using CVS from Windows](#using-cvs-from-windows)

Using NCBI C and C++ Toolkits together
--------------------------------------

<span class="nctnt highlight">Note</span>: Due to security issues, not all links on this page are accessible by users outside NCBI.

-   [Overview](#overview)

-   [Shared Sources](#shared-sources)

    -   [CONNECT Library](#connect-library)

    -   [ASN.1 Specifications](#asn1-specifications)

-   [Run-Time Resources](#run-time-resources)

    -   [LOG and CNcbiDiag](#log-and-cncbidiag)

    -   [REG and CNcbiRegistry](#reg-and-cncbiregistry)

    -   [MT\_LOCK and CRWLock](#mtlock-and-crwlock)

    -   [CONNECT Library in C++ Code](#connect-library-in-c++-code)

        -   [Setting LOG](#setting-log)

        -   [Setting REG](#setting-reg)

        -   [Setting MT-Locking](#setting-mt-locking)

        -   [Convenience call CONNECT\_Init()](#convenience-call-connectinit)

    -   [C Toolkit diagnostics redirection](#c-toolkit-diagnostics-redirection)

    -   [CONNECT Library in C Code](#connect-library-in-c-code)

        -   [Convenience call CONNECT\_Init()](#convenience-call-connectinit)

### Overview

When using both C and C++ Toolkits together in a single application, it is very important to understand that there are some resources shared between the two. This document describes how to safely use both Toolkits together, and how to gain from their cooperation.

### Shared Sources

To maintain a sort of uniformity and ease in source code maintenance, the <span class="nctnt ncbi-path">CONNECT</span> library is the first library of both Toolkits kept the same at the source code level. To provide data interoperability, ASN.1 specifications have to be identical in both Toolkits, too.

#### CONNECT Library

The [CONNECT](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect) library is currently the only C code that is kept identical in both Toolkits. The old API of the <span class="nctnt ncbi-path">CONNECT</span> library is still supported by means of a simple wrapper, which is only in the C Toolkit. There are two scripts that perform synchronization between C++ Toolkit and C Toolkit:

<span class="nctnt ncbi-path">sync\_c\_to\_cxx.pl</span> – This script copies the latest changes made in the C Toolkit (which is kept in the CVS repository) to the C++ Toolkit (kept in the Subversion repository). The following files are presently copied: <span class="nctnt ncbi-path">gicache.h</span> and <span class="nctnt ncbi-path">gicache.c</span>. Both are copied from the <span class="nctnt ncbi-path">distrib/network/sybutils/ctlib</span> CVS module to the <span class="nctnt ncbi-path">trunk/c++/src/objtools/data\_loaders/genbank/gicache</span> location in the Toolkit repository.

<span class="nctnt ncbi-path">sync\_cxx\_to\_c.pl</span> – This script copies files in the opposite direction: from the C++ Toolkit to the C Toolkit. Most of the files common to both Toolkits are synchronized by this script. Here’s the list of C source directories (CVS modules) that are currently copied from Subversion:<br/>- connect<br/>- ctools<br/>- algo/blast/core<br/>- algo/blast/composition\_adjustment<br/>- util/tables<br/>- util/creaders<br/>ASN files in the following CVS modules are also synchronized with Subversion:<br/>- network/medarch/client<br/>- network/taxon1/common<br/>- network/id1arch<br/>- network/id2arch<br/>- access<br/>- asn<br/>- biostruc<br/>- biostruc/cdd<br/>- biostruc/cn3d<br/>- tools<br/>- api<br/>- data

#### ASN.1 Specifications

Unlike the C source files in the <span class="nctnt ncbi-path">CONNECT</span> library, the ASN.1 data specifications are maintained within C Toolkit source structure, and have to be copied over to C++ Toolkit tree whenever they are changed.

However, the internal representations of ASN.1-based objects differ between the two Toolkits. If you need to convert an object from one representation to the other, you can use the template class [CAsnConverter\<\>](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CAsnConverter&d=C), defined in [ctools/asn\_converter.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/ctools/asn_converter.hpp).

### Run-Time Resources

The <span class="nctnt ncbi-path">CONNECT</span> library was written for use "as is" in the C Toolkit, but it must also be in the C++ Toolkit tree. Therefore, it cannot directly employ the utility objects offered by the C++ Toolkit such as message logging [CNcbiDiag](ch_core.html#ch_core.diag), registry [CNcbiRegistry](ch_core.html#ch_core.registry), and MT-locks [CRWLock](ch_core.html#ch_core.lock_classes). Instead, [these objects](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core.h) were replaced with helper objects coded entirely in C (as tables of function pointers and data).

On the other hand, throughout the code, the <span class="nctnt ncbi-path">CONNECT</span> library refers to predefined objects <span class="nctnt ncbi-var">g\_CORE\_Log</span> (so called <span class="nctnt ncbi-monospace">CORE C logger</span>) <span class="nctnt ncbi-var">g\_CORE\_Registry</span> (<span class="nctnt ncbi-monospace">CORE C registry</span>), and <span class="nctnt ncbi-var">g\_CORE\_Lock</span> (<span class="nctnt ncbi-monospace">CORE C MT-lock</span>), which actually are never initialized by the library, i.e. they are empty objects, which do nothing. It is an application's resposibility to replace these dummies with real working logger, registry, and MT-lock objects. There are two approaches, one for C and another for C++.

C programs can call [CORE\_SetREG()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetREG), [CORE\_SetLOG()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOG), and [CORE\_SetLOCK()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOCK) to set up the registry, logger, and MT-lock ([connect/ncbi\_util.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_util.h) must also be included). There are also convenience routines for <span class="nctnt ncbi-monospace">CORE logger</span>, like [CORE\_SetLOGFILE()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOGFILE), [CORE\_SetLOGFILE\_NAME()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOGFILE_NAME), which facilitate redirecting logging messages to either a C stream (<span class="nctnt ncbi-type">FILE\*</span>) or a named file.

In a C++ program, it is necessary to convert <span class="nctnt ncbi-monospace">native</span> C++ objects into their C equivalents, so that the C++ objects can be used where types [LOG](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=LOG), [REG](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=REG) or [MT\_LOCK](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=MT_LOCK) are expected. This is done using calls declared in [connect/ncbi\_core\_cxx.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core_cxx.hpp), as described later in this section.

#### <span class="nctnt ncbi-type">LOG</span> and <span class="nctnt ncbi-class">CNcbiDiag</span>

The <span class="nctnt ncbi-path">CONNECT</span> library has its own [logger](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=LOG), which has to be set by one of the routines declared in [connect/ncbi\_util.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_util.h): [CORE\_SetLOG()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOG), [CORE\_SetLOGFILE()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOGFILE) etc. On the other hand, the interface defined in [connect/ncbi\_core\_cxx.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core_cxx.hpp) provides the following C++ function to convert a logging stream of the NCBI C++ Toolkit into a <span class="nctnt ncbi-type">LOG</span> object:

    LOG LOG_cxx2c (void)

This function creates the <span class="nctnt ncbi-type">LOG</span> object on top of the corresponding C++ <span class="nctnt ncbi-class">CNcbiDiag</span> object, and then both C and C++ objects can be manipulated interchangeably, causing exactly the same effect on the underlying logger. Then, the returned C handle <span class="nctnt ncbi-type">LOG</span> can be subsequently used as a <span class="nctnt ncbi-monospace">CORE C logger</span> by means of <span class="nctnt ncbi-func">CORE\_SetLOG()</span>, as in the following nested calls:

    CORE_SetLOG(LOG_cxx2c());

#### REG and CNcbiRegistry

[connect/ncbi\_core\_cxx.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core_cxx.hpp) declares the following C++ function to bind C <span class="nctnt ncbi-type">REG</span> object to <span class="nctnt ncbi-class">CNcbiRegistry</span> used in C++ programs built with the use of the NCBI C++ Toolkit:

    REG REG_cxx2c (CNcbiRegistry* reg, bool pass_ownership = false)

Similarly to <span class="nctnt ncbi-monospace">CORE C logger</span> setting, the returned handle can be used later with [CORE\_SetREG()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetREG) declared in [connect/ncbi\_util.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_util.h) to set up the global registry object (<span class="nctnt ncbi-monospace">CORE C registry</span>).

#### MT\_LOCK and CRWLock

There is a function

    MT_LOCK MT_LOCK_cxx2c (CRWLock* lock, bool pass_ownership = false)

declared in [connect/ncbi\_core\_cxx.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core_cxx.hpp), which converts an object of class <span class="nctnt ncbi-class">CRWLock</span> into a C object <span class="nctnt ncbi-type">MT\_LOCK</span>. The latter can be used as an argument to [CORE\_SetLOCK()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOCK) for setting the global <span class="nctnt ncbi-monospace">CORE C MT-lock</span>, used by a low level code, written in C. Note that passing 0 as the lock pointer will effectively create a new internal <span class="nctnt ncbi-class">CRWLock</span> object, which will then be converted into <span class="nctnt ncbi-type">MT\_LOCK</span> and returned. This object gets automatically destroyed when the corresponding <span class="nctnt ncbi-type">MT\_LOCK</span> is destroyed. If the pointer to <span class="nctnt ncbi-class">CRWLock</span> is passed a non <span class="nctnt ncbi-var">NULL</span> value then the second argument can specify whether the resulting <span class="nctnt ncbi-type">MT\_LOCK</span> acquires the ownership of the lock, thus is able to delete the lock when destructing itself.

#### CONNECT Library in C++ Code

##### Setting LOG

To set up the <span class="nctnt ncbi-monospace">CORE C logger</span> to use the same logging format of messages and destination as used by <span class="nctnt ncbi-class">CNcbiDiag</span>, the following sequence of calls may be used:

    CORE_SetLOG(LOG_cxx2c());
    SetDiagTrace(eDT_Enable);
    SetDiagPostLevel(eDiag_Info);
    SetDiagPostFlag(eDPF_All);

##### Setting REG

To set the <span class="nctnt ncbi-monospace">CORE C registry</span> be the same as C++ registry <span class="nctnt ncbi-class">CNcbiRegistry</span>, the following call is necessary:

    CORE_SetREG(REG_cxx2c(cxxreg, true));

Here <span class="nctnt ncbi-var">cxxreg</span> is a <span class="nctnt ncbi-class">CNcbiRegistry</span> registry object created and maintained by a C++ application.

##### Setting MT-Locking

To set up a CORE lock, which is used throughout the low level code, including places of calls of non-reentrant library calls (if no reentrant counterparts were detected during configure process), one can place the following statement close to the beginning of the program:

    CORE_SetLOCK(MT_LOCK_cxx2c());

Note that the use of this call is extremely important in a multi-threaded environment.

##### Convenience call CONNECT\_Init()

Header file [connect/ncbi\_core\_cxx.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core_cxx.hpp) provides a convenience call, which sets all shared <span class="nctnt ncbi-path">CONNECT</span>-related resources discussed above for an application written within the C++ Toolkit framework (or linked solely against the libraries contained in the Toolkit):

    void CONNECT_Init(CNcbiRegistry* reg = NULL);

The call takes only one argument, an optional pointer to a registry, which is used by the application, and should also be considered by the <span class="nctnt ncbi-path">CONNECT</span> library. No registry will be used if <span class="nctnt ncbi-var">NULL</span> gets passed. The ownership of the registry is passed along. This fact should be noted by an application making extensive use of <span class="nctnt ncbi-path">CONNECT</span> in static classes, i.e. prior to or after main(), because the registry can get deleted before the <span class="nctnt ncbi-path">CONNECT</span> library stops using it. The call also ties <span class="nctnt ncbi-monospace">CORE C logger</span> to [CNcbiDiag](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CNcbiDiag), and privately creates a <span class="nctnt ncbi-monospace">CORE C MT-lock</span> object (on top of [CRWLock](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CRWLock)) for internal synchronization inside the library.

An example of how to use this call can be found in the test program [test\_ncbi\_conn\_stream.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/test/test_ncbi_conn_stream.cpp). It shows how to properly setup <span class="nctnt ncbi-monospace">CORE C logger</span>, <span class="nctnt ncbi-monospace">CORE C registry</span> and <span class="nctnt ncbi-monospace">CORE C MT-lock</span> so they will use the same data in the C and C++ parts of both the <span class="nctnt ncbi-path">CONNECT</span> library and the application code.

Another good source of information is the set of working application examples in [src/app/id1\_fetch](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/app/id1_fetch). <span class="nctnt highlight">Note:</span> In the examples, the convenience routine does not change logging levels or disable/enable certain logging properties. If this is desired, the application still has to use separate calls.

#### C Toolkit diagnostics redirection

In a C/C++ program linked against both NCBI C++ and NCBI C Toolkits the diagnostics messages (if any) generated by either Toolkit are not necessarily directed through same route, which may result in lost or garbled messages. To set the diagnostics destination be the same as <span class="nctnt ncbi-class">CNcbiDiag</span>'s one, and thus to guarantee that the messages from both Toolkits will be all stored sequentially and in the order they were generated, there is a call

    #include <ctools/ctools.h>
    void SetupCToolkitErrPost(void);

which is put in a specially designated directory [ctools](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/ctools) providing back links to the C Toolkit from the C++ Toolkit.

#### CONNECT Library in C Code

The <span class="nctnt ncbi-path">CONNECT</span> library in the C Toolkit has a header [connect/ncbi\_core\_c.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/connect/ncbi_core_c.h), which serves exactly the same purpose as <span class="nctnt ncbi-path">connect/ncbi\_core\_cxx.hpp</span>, described previously. It defines an API to convert native Toolkit objects, like logger, registry, and MT-lock into their abstract equivalents, [LOG](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=LOG), [REG](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=REG), and [MT\_LOCK](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=MT_LOCK), respectively, which are defined in [connect/ncbi\_core.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/connect/ncbi_core.h), and subsequently can used by the <span class="nctnt ncbi-path">CONNECT</span> library as <span class="nctnt ncbi-monospace">CORE C</span> objects.

Briefly, the calls are:

-   <span class="nctnt ncbi-code">LOG </span>[LOG\_c2c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=LOG_c2c)<span class="nctnt ncbi-code"> (void);</span> Create a logger <span class="nctnt ncbi-type">LOG</span> with all messages sent to it rerouted via the error logging facility used by the C Toolkit.

-   <span class="nctnt ncbi-code">REG </span>[REG\_c2c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=REG_c2c)<span class="nctnt ncbi-code"> (const char\* conf\_file);</span> Build a registry object <span class="nctnt ncbi-type">REG</span> from a named file <span class="nctnt ncbi-var">conf\_file</span>. Passing <span class="nctnt ncbi-var">NULL</span> as an argument causes the default Toolkit registry file to be searched for and used.

-   <span class="nctnt ncbi-code">MT\_LOCK </span>[MT\_LOCK\_c2c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=MT_LOCK_c2c)<span class="nctnt ncbi-code"> (</span>[TNlmRWlock](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=TNlmRWlock)<span class="nctnt ncbi-code"> lock, int/\*bool\*/ pass\_ownership);</span> Build an <span class="nctnt ncbi-type">MT\_LOCK</span> object on top of <span class="nctnt ncbi-type">TNlmRWlock</span> handle. Note that passing <span class="nctnt ncbi-var">NULL</span> effectively creates an internal handle, which is used as an underlying object. Ownership of the original handle can be passed to the resulting <span class="nctnt ncbi-type">MT\_LOCK</span> by setting the second argument to a non-zero value. The internally created handle always has its ownership passed along.

Exactly the same way as described in the previous section, all objects, resulting from the above functions, can be used to set up <span class="nctnt ncbi-monospace">CORE C logger</span>, <span class="nctnt ncbi-monospace">CORE C registry</span>, and <span class="nctnt ncbi-monospace">CORE MT-lock</span> of the <span class="nctnt ncbi-path">CONNECT</span> library using the API defined in [connect/ncbi\_util.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/connect/ncbi_util.h): [CORE\_SetLOG()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=CORE_SetLOG), [CORE\_SetREG()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=CORE_SetREG), and [CORE\_SetLOCK()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/ident?i=CORE_SetLOCK), respectively.

##### Convenience call CONNECT\_Init()

As an alternative to using per-object settings as shown in the previous paragraph, the following "all-in-one" call is provided:

    void CONNECT_Init (const char* conf_file);

This sets <span class="nctnt ncbi-monospace">CORE C logger</span> to go via Toolkit default logging facility, causes <span class="nctnt ncbi-monospace">CORE C registry</span> to be loaded from the named file (or from the Toolkit's default file if <span class="nctnt ncbi-var">conf\_file</span> passed <span class="nctnt ncbi-var">NULL</span>), and creates <span class="nctnt ncbi-monospace">CORE C MT-lock</span> on top of internally created <span class="nctnt ncbi-type">TNlmRWlock</span> handle, the ownership of which is passed to the <span class="nctnt ncbi-type">MT\_LOCK</span>.

<span class="nctnt highlight">Note:</span> Again, properties of the logging facility are not affected by this call, i.e. the selection of what gets logged, how, and where, should be controlled by using native C Toolkit's mechanisms defined in [ncbierr.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/C_DOC/lxr/source/corelib/ncbierr.h).

Access to the C Toolkit source tree Using CVS
---------------------------------------------

For a detailed description of the CVS utility see [the CVS online manual](http://www.cs.utah.edu/csinfo/texinfo/cvs/cvs_toc.html) or run the commands <span class="nctnt ncbi-cmd">"man cvs"</span> or <span class="nctnt ncbi-cmd">"cvs --help"</span> on your Unix workstation.

### CVS Source Code Retrieval for In-House Users with Read-Write Access

You must have a CVS account set up prior to using CVS - email <span class="oem_span">z/u4hktpuGujip5ust5upo5nv/</span> to get set up.

The C Toolkit CVS repository is available [online](http://intranet/cvsutils/index.cgi/distrib) and may be [searched](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/C_DOC/lxr/source) using LXR.

-   [Using CVS from Unix or Mac OS X](#using-cvs-from-unix-or-mac-os-x)

-   [Using CVS from Windows](#using-cvs-from-windows)

#### Using CVS from Unix or Mac OS X

To set up a CVS client on Unix or Mac OS X:

-   Set the <span class="nctnt ncbi-var">CVSROOT</span> environment variable to: <span class="nctnt ncbi-monospace">:pserver:${LOGNAME}@cvsvault:/src/NCBI/vault.ncbi</span>. Note that for NCBI Unix users, this may already be set if you specified <span class="nctnt ncbi-monospace">developer</span> for the <span class="nctnt ncbi-monospace">facilities</span> option in the <span class="nctnt ncbi-path">.ncbi\_hints</span> file in your home directory.

-   Run the command: <span class="nctnt ncbi-cmd">cvs login</span> You will be asked for a password (email <span class="oem_span">z/u4hktpuGujip5ust5upo5nv/</span> if you need the password). This command will record your login info into <span class="nctnt ncbi-path">~/.cvspass</span> file so you won't have to login into CVS in the future. <span class="nctnt highlight">Note:</span> You may need to create an empty <span class="nctnt ncbi-path">~/.cvspass</span> file before logging in as some CVS clients apparently just cannot create it for you. If you get an authorization error, then send e-mail with the errors to <span class="oem_span">jww4jvylGujip5ust5upo5nv/</span>.

-   If you have some other CVS snapshot which was checked out with an old value of <span class="nctnt ncbi-var">CVSROOT</span>, you should commit all your changes first, then delete completely the old snapshot dir and run: <span class="nctnt ncbi-cmd">cvs checkout</span> to get it with new <span class="nctnt ncbi-var">CVSROOT</span> value.

-   Now you are all set and can use all the usual CVS commands.

<span class="nctnt highlight">Note:</span> When you are in a directory that was created with <span class="nctnt ncbi-cmd">cvs checkout</span> by another person, a local <span class="nctnt ncbi-path">./CVS/</span> subdirectory is also created in that directory. In this case, the cvs command ignores the current value of the <span class="nctnt ncbi-var">CVSROOT</span> environment variable and picks up a value from <span class="nctnt ncbi-path">./CVS/Root</span> file. Here is an example of what this <span class="nctnt ncbi-path">Root</span> file looks like:

    :pserver:username@cvsvault:/src/NCBI/vault.ncbi

Here the *username* is the user name of the person who did the initial CVS checkout in that directory. So CVS picks up the credentials of the user who did the initial check-in and ignores the setting of the <span class="nctnt ncbi-var">CVSROOT</span> environment variable, and therefore the CVS commands that require authorization will fail. There are two possible solutions to this problem:

-   Create your own snapshot of this area using the <span class="nctnt ncbi-cmd">cvs get</span> command.

-   Impersonate the user who created the CVS directory by creating in the <span class="nctnt ncbi-path">~/.cvspass</span> file another string which is a duplicate of the existing one, and in this new string change the username to that of the user who created the directory. This hack will allow you to work with the CVS snapshot of the user who created the directory. However, this type of hack is not recommended for any long term use as you are impersonating another user.

#### Using CVS from Windows

The preferred CVS client is [TortoiseCVS](http://www.tortoisecvs.org/). If this is not installed on your PC, ask [PC Systems](http://jira.be-md.ncbi.nlm.nih.gov/secure/CreateIssue!default.jspa?pid=10371) to have it installed. Your TortoiseCVS installation should include both a CVS command-line client and integration into Windows Explorer.

To use TortoiseCVS as integrated into Windows Explorer:

-   Navigate to the directory where you want the source code to be put.

-   Right-click and select "CVS Checkout".

-   Set the CVSROOT text field to <span class="nctnt ncbi-monospace">:pserver:%USERNAME%@cvsvault:/src/NCBI/vault.ncbi</span> (where <span class="nctnt ncbi-monospace">%USERNAME%</span> is replaced with your Windows user name).

-   Set the module text field to the portion of the C Toolkit you want to retrieve. If you want the whole Toolkit, use <span class="nctnt ncbi-monospace">distrib</span>. If you want just one library, for example the CONNECT library, use <span class="nctnt ncbi-monospace">distrib/connect</span>. There are also non C Toolkit modules (you can see them [here](http://intranet/cvsutils/index.cgi/distrib)). You can work with those as well by using their names instead of <span class="nctnt ncbi-monospace">distrib</span> (e.g. <span class="nctnt ncbi-monospace">internal</span>).

-   Click OK. If you are asked for a password and don't know what to use, email <span class="oem_span">z/u4hktpuGujip5ust5upo5nv/</span>.

-   From the context menu (right-click) you can now perform CVS functions, such as updating, committing, tagging, diffing, etc.

-   You may also change global preferences (such as external tools) using the Preferences application available from the Start menu.

For command-line use, follow the [in-house Unix / Mac OS X instructions](#in-house-unix--mac-os-x-instructions) with these exceptions:

-   Make sure you have your "home" directory set up -- i.e. the environment variables <span class="nctnt ncbi-var">HOMEDRIVE</span> and <span class="nctnt ncbi-var">HOMEPATH</span> should be set. In NCBI, <span class="nctnt ncbi-var">HOMEDRIVE</span> usually set to <span class="nctnt ncbi-monospace">C:</span>, and <span class="nctnt ncbi-var">HOMEPATH</span> is usually set to something like <span class="nctnt ncbi-monospace">\\Documents and Settings\\%USERNAME%</span> (where <span class="nctnt ncbi-monospace">%USERNAME%</span> is replaced with your Windows user name).

-   Create an empty file named <span class="nctnt ncbi-path">.cvspass</span> in your "home" directory.

-   The CVS root needs to be specified.

    -   Either set an environment variable:<br/><span class="nctnt ncbi-cmd">%CVSROOT%=:pserver:%USERNAME%@cvsvault:/src/NCBI/vault.ncbi</span>

    -   or use a command-line argument for each CVS command:<br/><span class="nctnt ncbi-cmd">-d :pserver:%USERNAME%@cvsvault:/src/NCBI/vault.ncbi</span>

-   Open a command shell, verify the above environment variables are set properly, and execute the command "<span class="nctnt ncbi-cmd">cvs login</span>". You will be asked for a password (email <span class="oem_span">z/u4hktpuGujip5ust5upo5nv/</span> if you need the password). This command will record your login info in the <span class="nctnt ncbi-path">.cvspass</span> file so you won't have to log into CVS in the future. If you get an authorization error, send e-mail with the errors to <span class="oem_span">jww4jvylGujip5ust5upo5nv/</span>.


