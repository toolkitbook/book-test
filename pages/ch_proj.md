---
layout: default
title: C++ Toolkit test
nav: pages/ch_proj
---

<span class="label">6</span>Project Creation and Management
===========================================================

Last Update: December 2, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter discusses the setup procedures for starting a new project such as the location of makefiles, header files, source files, etc. It also discusses the SVN tree structure and how to use SVN for tracking your code changes, and how to manage the development environment.

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Starting New Projects](#starting-new-projects)

    -   [New Projects: Location and File Structure](#new-projects-location-and-file-structure-)

        -   [new\_project: Starting a New Project outside the C++ Toolkit Tree](#newproject-starting-a-new-project-outside-the-c++-toolkit-tree)

        -   [Creating a New Project Inside the C++ Toolkit Tree](#creating-a-new-project-inside-the-c++-toolkit-tree)

    -   [Projects and the Toolkit's SVN Tree Structure](#projects-and-the-toolkit's-svn-tree-structure)

    -   [Creating source and include SVN dirs for a new C++ project](#creating-source-and-include-svn-dirs-for-a-new-c++-project)

    -   [Starting New Modules](#starting-new-modules-)

    -   [Meta-makefiles (to provide multiple and/or recursive builds)](#meta-makefiles-to-provide-multiple-andor-recursive-builds)

    -   [Project makefiles](#project-makefiles)

        -   [Example 1: Customized makefile to build a library](#example-1-customized-makefile-to-build-a-library)

        -   [Example 2: Customized makefile to build an application](#example-2-customized-makefile-to-build-an-application)

        -   [Example 3: User-defined makefile to build... whatever](#example-3-user-defined-makefile-to-build-whatever)

    -   [An example of the NCBI C++ makefile hierarchy ("corelib/")](#an-example-of-the-ncbi-c++-makefile-hierarchy-corelib)

-   [Managing the Work Environment](#managing-the-work-environment)

    -   [Obtaining the Very Latest Builds](#obtaining-the-very-latest-builds)

    -   [Working in a separate directory](#working-in-a-separate-directory)

        -   [Setting up Directory Location](#setting-up-directory-location-)

        -   [The Project's Makefile](#the-project's-makefile-)

        -   [Testing your setup](#testing-your-setup-)

    -   [Working Independently In a C++ Subtree](#working-independently-in-a-c++-subtree)

    -   [Working within the C++ source tree](#working-within-the-c++-source-tree)

        -   [Checkout the source tree and configure a build directory](#checkout-the-source-tree-and-configure-a-build-directory)

        -   [The project's directories and makefiles](#the-project's-directories-and-makefiles)

        -   [Makefile.in meta files](#makefilein-meta-files)

        -   [An example meta-makefile and its associated project makefiles](#an-example-meta-makefile-and-its-associated-project-makefiles)

        -   [Executing make](#executing-make)

        -   [Custom project makefile: Makefile.myProj](#custom-project-makefile-makefilemyproj)

        -   [Library project makefile: Makefile.myProj.lib](#library-project-makefile-makefilemyprojlib)

        -   [Application project makefile: Makefile.myProj.app](#application-project-makefile-makefilemyprojapp)

        -   [Defining and running tests](#defining-and-running-tests)

        -   [The configure scripts](#the-configure-scripts)

    -   [Working with the serializable object classes](#working-with-the-serializable-object-classes)

        -   [Serializable Objects](#serializable-objects)

        -   [Locating and browsing serializable objects in the C++ Toolkit](#locating-and-browsing-serializable-objects-in-the-c++-toolkit)

        -   [Base classes and user classes](#base-classes-and-user-classes)

        -   [Adding methods to the user classes](#adding-methods-to-the-user-classes)

            -   [Checking out source code, configuring the working environment, building the libraries](#checking-out-source-code-configuring-the-working-environment-building-the-libraries).

            -   [Adding methods](#adding-methods)

Starting New Projects
---------------------

The following assumes that you have all of the necessary Toolkit components. If you need to obtain part or the Toolkit's entire source tree, consult the [FTP instructions](ch_getcode_svn.html#ch_getcode_svn.ftp_download) or [SVN checkout procedures](ch_getcode_svn.html#ch_getcode_svn.code_retrieval). Please visit the [Getting Started](ch_start.html) page for a broad overview of the NCBI C++ Toolkit and its use.

The following topics are discussed in this section:

-   [New Projects: Location and File Structure](#new-projects-location-and-file-structure)

    -   [new\_project: Starting a New Project outside the C++ Toolkit Tree](#newproject-starting-a-new-project-outside-the-c++-toolkit-tree)

    -   [Creating a New Project Inside the C++ Toolkit Tree](#creating-a-new-project-inside-the-c++-toolkit-tree)

-   [Projects and the Toolkit's SVN Tree Structure](#projects-and-the-toolkit's-svn-tree-structure)

-   [Creating source and include SVN dirs for a new C++ project](#creating-source-and-include-svn-dirs-for-a-new-c++-project)

-   [Starting New Modules](#starting-new-modules-)

-   [Meta-makefiles (to provide multiple and/or recursive builds)](#meta-makefiles-to-provide-multiple-andor-recursive-builds)

-   [Project makefiles](#project-makefiles)

    -   [Example 1: Customized makefile to build a library](#example-1-customized-makefile-to-build-a-library)

    -   [Example 2: Customized makefile to build an application](#example-2-customized-makefile-to-build-an-application)

    -   [Example 3: User-defined makefile to build... whatever](#example-3-user-defined-makefile-to-build-whatever)

-   [An example of the NCBI C++ makefile hierarchy ("corelib/")](#an-example-of-the-ncbi-c++-makefile-hierarchy-corelib)

### New Projects: Location and File Structure

Before creating the new project, you must decide if you need to work within a C++ source tree (or subtree) or merely need to link with the Toolkit libraries and work in a separate directory. The later case is simpler and allows you to work independently in a private directory, but it is not an option if the Toolkit source, headers, or <span class="nctnt ncbi-monospace">makefiles</span> are to be directly used or altered during the new project's development.

-   [Work in the Full Toolkit Source Tree](#work-in-the-full-toolkit-source-tree)

-   [Work in a Toolkit Subtree](#work-in-a-toolkit-subtree)

-   [Work in a Separate Directory](#work-in-a-separate-directory)

Regardless of where you build your new project, it must adopt and maintain a particular structure. Specifically, each project's [source tree](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src) relative to <span class="nctnt ncbi-path">$NCBI/c++</span> should contain:

-   <span class="nctnt ncbi-path">include/\*.hpp</span> -- project's public headers

-   <span class="nctnt ncbi-path">src/\*.{cpp, hpp}</span> -- project's source files and private headers

-   <span class="nctnt ncbi-path">src/Makefile.in</span> -- a [meta-makefile](#meta-makefile) template to specify which local projects (described in <span class="nctnt ncbi-path">Makefile.\*.in</span>) and sub-projects (located in the project subdirectories) must be built

-   <span class="nctnt ncbi-path">src/Makefile.\<project\_name\>.{lib, app}[.in]</span> -- one or more <span class="nctnt ncbi-monospace">customized makefiles</span> to build a library or an application

-   <span class="nctnt ncbi-path">src/Makefile.\*[.in]</span> -- "free style" makefiles (if any)

-   sub-project directories (if any)

The following topics are discussed in this section:

-   [new\_project: Starting a New Project outside the C++ Toolkit Tree](#newproject-starting-a-new-project-outside-the-c++-toolkit-tree)

-   [Creating a New Project Inside the C++ Toolkit Tree](#creating-a-new-project-inside-the-c++-toolkit-tree)

#### <span class="nctnt ncbi-app">new\_project</span>: Starting a New Project outside the C++ Toolkit Tree

Script usage:

    new_project <name> <type>[/<subtype>] [builddir]

<span class="nctnt highlight">NOTE</span>: in NCBI, you can (and should) invoke common scripts simply by name - i.e. without path or extension. The proper script located in the pre-built NCBI C++ toolkit directory will be invoked.

This script will create a startup makefile for a new project which uses the NCBI C++ Toolkit (and possibly the C Toolkit as well). Replace <span class="nctnt ncbi-cmd">\<type\></span> with <span class="nctnt ncbi-cmd">lib</span> for libraries or <span class="nctnt ncbi-cmd">app</span> for applications.

Sample code will be included in the project directory for new applications. Different samples are available for <span class="nctnt ncbi-cmd">type=app[/basic]</span> (a command-line argument demo application based on the [corelib](ch_core.html) library), <span class="nctnt ncbi-cmd">type=app/cgi</span> (for a [CGI or Fast-CGI](ch_cgi.html) application), <span class="nctnt ncbi-cmd">type=app/objmgr</span> (for an application using the <span class="nctnt ncbi-monospace">Object Manager</span>), <span class="nctnt ncbi-cmd">type=app/objects</span> (for an application using ASN.1 objects), and many others.

You will need to slightly edit the resultant makefile to:

-   specify the name of your library (or application)

-   specify the list of source files going to it

-   modify some preprocessor, compiler, etc. flags, if needed

-   modify the set of additional libraries to link to it (if it's an application), if needed

For example:

    new_project foo app/basic

creates a model makefile <span class="nctnt ncbi-path">Makefile.foo\_app</span> to build an application using tools and flags hard-coded in <span class="nctnt ncbi-path">$NCBI/c++/Debug/build/Makefile.mk</span>, and headers from <span class="nctnt ncbi-path">$NCBI/c++/include/</span>. The file <span class="nctnt ncbi-path">/tmp/foo/foo.cpp</span> is also created; you can either replace this with your own <span class="nctnt ncbi-path">foo.cpp</span> or modify its sample code as required.

Now, after specifying the application name, list of source files, etc., you can just go to the created working directory <span class="nctnt ncbi-path">foo/</span> and build your application using:

    make -f Makefile.foo_app

You can easily change the active version of NCBI C++ Toolkit by manually setting variable <span class="nctnt ncbi-var">$(builddir)</span> in the file <span class="nctnt ncbi-path">Makefile.foo\_app</span> to the desired Toolkit path, e.g.,

    builddir = $(NCBI)/c++.current/GCC-Release/build

<span class="nctnt highlight">Note:</span> On Unix, the default build is the production build - because <span class="nctnt ncbi-path">$(NCBI)/c++</span> is a symbolic link to <span class="nctnt ncbi-path">$(NCBI)/c++.production</span>. On Windows, the default build is the current build. Windows builds have the form: <span class="nctnt ncbi-path">\\\\snowman\\win-coremake\\Lib\\Ncbi\\CXX\_Toolkit\\msvc10\\cxx.stable</span>

In many cases, you work on your own project which **is a part** of the NCBI C++ tree, and you do not want to check out, update and rebuild the whole NCBI C++ tree. Instead, you just want to use headers and libraries of the pre-built NCBI C++ Toolkit to build your project. In these cases, use the [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) script instead of <span class="nctnt ncbi-path">new\_project</span>.

<span class="nctnt highlight">Note for users inside NCBI:</span> To be able to view debug information in the Toolkit libraries for Windows builds, you will need to have the <span class="nctnt ncbi-cmd">S:</span> drive mapped to <span class="nctnt ncbi-path">\\\\snowman\\win-coremake\\Lib</span>. By default, <span class="nctnt ncbi-path">new\_project</span> will make this mapping for you if it's not already done.

#### Creating a New Project Inside the C++ Toolkit Tree

To create your new project (e.g., "bar\_proj") directories in the NCBI C++ Toolkit source tree (assuming that the entire NCBI C++ Toolkit has been [checked out](ch_getcode_svn.html#ch_getcode_svn.chkout_complete_tree) to directory <span class="nctnt ncbi-path">foo/c++/</span>):

    cd foo/c++/include && mkdir bar_proj && svn add bar_proj
    cd foo/c++/src     && mkdir bar_proj && svn add bar_proj

From there, you can now [add](#add) and edit your project C++ files.

NOTE: remember to add this new project directory to the <span class="nctnt ncbi-var">$(SUB\_PROJ)</span> list of the upper level [meta-makefile](ch_build.html#ch_build.makefiles_meta) configurable template (e.g., for this particular case, to <span class="nctnt ncbi-path">foo/c++/src/Makefile.in</span>).

### Projects and the Toolkit's SVN Tree Structure

(For the overall NCBI C++ SVN tree structure see [SVN details](ch_getcode_svn.html#ch_getcode_svn.source_tree).)

Even if you work outside of the C++ tree, it is necessary to understand how the Toolkit uses <span class="nctnt ncbi-monospace">makefiles</span>, <span class="nctnt ncbi-monospace">meta-makefiles</span>, and <span class="nctnt ncbi-monospace">makefile templates</span>, and the SVN tree structure.

The standard SVN location for NCBI C++/STL projects is <span class="nctnt ncbi-path">$SVNROOT/internal/c++/</span>. Public header files (<span class="nctnt ncbi-path">\*.hpp, \*.inl</span>) of all projects are located below the <span class="nctnt ncbi-path">$SVNROOT/internal/c++/include/</span> directory. <span class="nctnt ncbi-path">$SVNROOT/internal/c++/src/</span> directory has just the same hierarchy of subdirectories as <span class="nctnt ncbi-path">.../include/</span>, and its very top level contains:

-   Templates of generic makefiles (<span class="nctnt ncbi-path">Makefile.\*.in</span>):

    -   <span class="nctnt ncbi-path">Makefile.in</span> -- makefile to perform a recursive build in all project subdirectories

    -   <span class="nctnt ncbi-path">Makefile.meta.in</span> -- included by all makefiles that provide both local and recursive builds

    -   <span class="nctnt ncbi-path">Makefile.lib.in</span> -- included by all makefiles that perform a "standard" library build, when building only static libraries.

    -   <span class="nctnt ncbi-path">Makefile.dll.in</span> -- included by all makefiles that perform a "standard" library build, when building only shared libraries.

    -   <span class="nctnt ncbi-path">Makefile.both.in</span> -- included by all makefiles that perform a "standard" library build, when building both static and shared libraries.

    -   <span class="nctnt ncbi-path">Makefile.lib.tmpl.in</span> -- serves as a template for the project <span class="nctnt ncbi-monospace">customized makefiles</span> (<span class="nctnt ncbi-path">Makefile.\*.lib[.in]</span>) that perform a "standard" library build

    -   <span class="nctnt ncbi-path">Makefile.app.in</span> -- included by all makefiles that perform a "standard" application build

    -   <span class="nctnt ncbi-path">Makefile.lib.tmpl.in</span> -- serves as a template for the project <span class="nctnt ncbi-monospace">customized makefiles</span> (<span class="nctnt ncbi-path">Makefile.\*.app[.in]</span>) that perform a "standard" application build

    -   <span class="nctnt ncbi-path">Makefile.rules.in, Makefile.rules\_with\_autodep.in</span> -- instructions for building object files; included by most other makefiles

    -   <span class="nctnt ncbi-path">Makefile.mk.in</span> -- included by all makefiles; sets a lot of configuration variables

-   The contents of each project are detailed [above](#above). If your project is to become part of the Toolkit tree, you need to ensure that all <span class="nctnt ncbi-monospace">makefiles</span> and <span class="nctnt ncbi-path">Makefile\*.in</span> templates are available so the master <span class="nctnt ncbi-monospace">makefiles</span> can properly configure and build it (see "[Meta-Makefiles](#meta-makefiles)" and "[Project Makefiles"](#project-makefiles) below). You will also need to [prepare SVN directories](#prepare-svn-directories) to hold the new source and header files.

### Creating source and include SVN dirs for a new C++ project

To create your new project (e.g., "bar\_proj") directories in the NCBI C++ SVN tree to directory <span class="nctnt ncbi-path">foo/c++/</span>):

    cd foo/c++/include && mkdir bar_proj && SVN add -m "Project Bar" bar_proj
    cd foo/c++/src     && mkdir bar_proj && SVN add -m "Project Bar" bar_proj

Now you can [add](#add) and edit your project C++ files in there.

<span class="nctnt highlight">NOTE:</span> remember to add this new project directory to the <span class="nctnt ncbi-var">$(SUB\_PROJ)</span> list of the upper level [meta-makefile](#meta-makefile) configurable template (e.g., for this particular case, to <span class="nctnt ncbi-path">foo/c++/src/Makefile.in</span>).

### Starting New Modules

Projects in the NCBI C++ Toolkit consist of “modules”, which are most often a pair of source <span class="nctnt ncbi-path">(\*.cpp</span>) and header (<span class="nctnt ncbi-path">\*.hpp</span>) files. To help create new modules, template source and header files may be used, or you may modify the sample code generated by the script [new\_project](#newproject). The template source and header files are <span class="nctnt ncbi-path">.../doc/public/framewrk.cpp</span> and <span class="nctnt ncbi-path">.../doc/public/framewrk.hpp</span>. The template files contain a standard startup framework so that you can just cut-and-paste them to start a new module (just don't forget to replace the "framewrk" stubs by your new module name).

-   Header file (\*.hpp) -- API for the external users. Ideally, this file contains only (well-commented) declarations and inline function implementations for the public interface. No less, and no more.

-   Source file (\*.cpp) -- Definitions of non-inline functions and internally used things that should not be included by other modules.

On occasion, a second private header file is required for good encapsulation. Such second headers should be placed in the same directory as the module source file.

Each and every source file **must** include the NCBI disclaimer and (preferably) Subversion keywords (e.g. $Id$). Then, the header file must be protected from double-inclusion, and it must define any inlined functions.

### Meta-makefiles (to provide multiple and/or recursive builds)

All projects from the NCBI C++ hierarchy are tied together by a set of <span class="nctnt ncbi-monospace">meta-makefiles</span> which are present in all project source directories and provide a uniform and easy way to perform both local and recursive builds. See more detail on the [Working with Makefiles](ch_build.html) page. A typical <span class="nctnt ncbi-monospace">meta-makefile</span> template (e.g. <span class="nctnt ncbi-path">Makefile.in</span> in your <span class="nctnt ncbi-path">foo/c++/src/bar\_proj/</span> dir) looks like that:

    # Makefile.bar_u1, Makefile.bar_u2 ...
    USR_PROJ = bar_u1 bar_u2 ...
    # Makefile.bar_l1.lib, Makefile.bar_l2.lib ...
    LIB_PROJ = bar_l1 bar_l2 ...
    # Makefile.bar_a1.app, Makefile.bar_a2.app ...
    APP_PROJ = bar_a1 bar_l2 ...
    SUB_PROJ = app sub_proj1 sub_proj2
    srcdir = @srcdir@
    include @builddir@/Makefile.meta

This template separately specifies instructions for user, library and application projects, along with a set of three sub-projects that can be made. The mandatory final two lines <span class="nctnt ncbi-code">"srcdir = @srcdir@ ; include @builddir@/Makefile.meta"</span> define the [standard build targets](ch_build.html#ch_build.std_build_targets).

### Project makefiles

Just like the configurable template <span class="nctnt ncbi-path">Makefile.meta.in</span> is used to ease and standardize the writing of [meta-makefiles](#meta-makefiles), so there are templates to help in the creation of "regular" project makefiles to build a library or an application. These auxiliary template makefiles are described on the "[Working with Makefiles](ch_build.html#ch_build.makefiles_hierarch)" page and listed [above](#above). The <span class="nctnt ncbi-app">configure</span>'d versions of these templates get put at the very top of a <span class="nctnt ncbi-monospace">build tree</span>.

In addition to the <span class="nctnt ncbi-monospace">meta-makefile</span> that must be defined for each project, a customized makefile <span class="nctnt ncbi-path">Makefile.\<project\_name\>.[app\\|lib]</span> must also be provided. The following three sections give examples of <span class="nctnt ncbi-monospace">customized makefiles</span> for a [library](#library) and an [application](#application), along with a case where a [user-defined](#user-defined) <span class="nctnt ncbi-monospace">makefile</span> is required.

You have great latitude in specifying optional packages, features and projects in <span class="nctnt ncbi-monospace">makefiles</span>. The macro <span class="nctnt ncbi-var">REQUIRES</span> in the examples is one way to allows you access them. See the "<span class="nctnt ncbi-monospace">Working with Makefiles</span>" page for a [complete list](ch_build.html#ch_build.packages_opt); the configuration page gives the corresponding [configure options](ch_config.html#ch_config.ch_configprohibit_sy).

The following examples are discussed in this section:

-   [Example 1: Customized makefile to build a library](#example-1-customized-makefile-to-build-a-library)

-   [Example 2: Customized makefile to build an application](#example-2-customized-makefile-to-build-an-application)

-   [Example 3: User-defined makefile to build... whatever](#example-3-user-defined-makefile-to-build-whatever)

#### Example 1: Customized makefile to build a library

Here is an example of a <span class="nctnt ncbi-monospace">customized makefile</span> to build either a library <span class="nctnt ncbi-lib">libxmylib.a</span> or a shared library <span class="nctnt ncbi-lib">libxmylib.so</span> from two source files <span class="nctnt ncbi-path">xmy\_src1.cpp</span> and <span class="nctnt ncbi-path">xmy\_src2.c</span>, and one pre-compiled object file <span class="nctnt ncbi-path">some\_obj1.o</span>. To make the example even more realistic, we assume that the said source files include headers from the NCBI C Toolkit.

    LIB      = xmylib
    SRC      = xmy_src1 xmy_src2
    OBJ      = some_obj1
    REQUIRES = xrequirement

    # For a static library, this is optional (but can be used to override --with-dll):
    LIB_OR_DLL = lib
    # To encourage building a shared library (for capable platforms), this is required:
    LIB_OR_DLL = dll
    # You can also encourage building both a static and a shared version of the library:
    LIB_OR_DLL = both

    # To resolve external references for shared libraries:
    #   if using --with-dll or --with-gbench
    DLL_DLIB = xncbi
    #   else
    DLL_LIB = xncbi

    # To require that a library must have all externals resolved:
    DLL_UNDEF_FLAGS = $(FORBID_UNDEF)

    # For third-party references:
    LIBS = $(NETWORK_LIBS)

    CFLAGS   = $(ORIG_CFLAGS)   -abc -DFOOBAR_NOT_CPLUSPLUS
    CXXFLAGS = $(FAST_CXXFLAGS) -xyz
    CPPFLAGS = $(ORIG_CPPFLAGS) -UFOO -DP1_PROJECT -I$(NCBI_C_INCLUDE)

Here are the steps the build process follows:

-   Skip building this library if <span class="nctnt ncbi-monospace">xrequirement</span> ([an optional package or project](ch_build.html#ch_build.packages_opt)) is disabled or unavailable.

-   Compile <span class="nctnt ncbi-path">xmy\_src1.cpp</span> using the C++ compiler <span class="nctnt ncbi-var">$(CXX)</span> with the flags <span class="nctnt ncbi-cmd">$(FAST\_CXXFLAGS) -xyz $(CPPFLAGS)</span>, which are the C++ flags for faster code, some additional flags specified by the user, and the original preprocessor flags.

-   Compile <span class="nctnt ncbi-path">xmy\_src2.c</span> using the C compiler <span class="nctnt ncbi-var">$(CC)</span> with the flags <span class="nctnt ncbi-cmd">$(ORIG\_CFLAGS) -abc -DFOOBAR\_NOT\_CPLUSPLUS $(CPPFLAGS)</span>, which are the original C flags, some additional flags specified by the user, and the original preprocessor flags.

-   If building a static library: Using <span class="nctnt ncbi-var">$(AR)</span> and <span class="nctnt ncbi-var">$(RANLIB)</span>, compose the library <span class="nctnt ncbi-lib">libxmylib.a</span> from the resultant object files, plus the pre-compiled object file <span class="nctnt ncbi-path">some\_obj1.o</span>.

-   If building a shared library: Using <span class="nctnt ncbi-var">$(LINK\_DLL)</span>, compose the library <span class="nctnt ncbi-lib">libxmylib.so</span> from the resultant object files, plus the pre-compiled object file <span class="nctnt ncbi-path">some\_obj1.o</span>.

-   Copy <span class="nctnt ncbi-lib">libxmylib.\*</span> to the top-level <span class="nctnt ncbi-path">lib/</span> directory of the <span class="nctnt ncbi-monospace">build tree</span> (for the later use by other projects).

This <span class="nctnt ncbi-monospace">customized makefile</span> should be referred to as <span class="nctnt ncbi-path">xmylib</span> in the <span class="nctnt ncbi-var">LIB\_PROJ</span> macro of the relevant [meta-makefile](#meta-makefile). As usual, [Makefile.mk](ch_build.html#ch_build.build_make_macros) will be implicitly included.

For information about run-time linking with shared libraries, see [this section](ch_config.html#ch_config.ch_configconfightml_).

#### Example 2: Customized makefile to build an application

Here is an example of a <span class="nctnt ncbi-monospace">customized makefile</span> to build the application <span class="nctnt ncbi-app">my\_exe</span> from three source files, <span class="nctnt ncbi-path">my\_main.cpp</span>, <span class="nctnt ncbi-path">my\_src1.cpp</span>, and <span class="nctnt ncbi-path">my\_src2.c</span>. To make the example even more realistic, we assume that the said source files include headers from the NCBI SSS DB packages, and the target executable uses the NCBI C++ libraries [libxmylib.\*](#libxmylib*) and <span class="nctnt ncbi-lib">libxncbi.\*</span>, plus NCBI SSS DB, SYBASE, and system network libraries. We assume further that the user would prefer to link statically against libxmylib if building the toolkit as both shared and static libraries (<span class="nctnt ncbi-cmd">configure --with-dll --with-static ...</span>), but is fine with a shared libxncbi.

    APP      = my_exe
    SRC      = my_main my_src1 my_src2
    OBJ      = some_obj
    LIB      = xmylib$(STATIC) xncbi
    REQUIRES = xrequirement
    CPPFLAGS = $(ORIG_CPPFLAGS) $(NCBI_SSSDB_INCLUDE)
    LIBS     = $(NCBI_SSSDB_LIBS) $(SYBASE_LIBS) $(NETWORK_LIBS) $(ORIG_LIBS)

-   Skip building this library if <span class="nctnt ncbi-monospace">xrequirement</span> ([an optional package or project](ch_build.html#ch_build.packages_opt)) is disabled or unavailable.

-   Compile <span class="nctnt ncbi-path">my\_main.cpp</span> and <span class="nctnt ncbi-path">my\_src1.cpp</span> using the C++ compiler <span class="nctnt ncbi-var">$(CXX)</span> with the flags <span class="nctnt ncbi-var">$(CXXFLAGS)</span> (see Note below).

-   Compile <span class="nctnt ncbi-path">my\_src2.c</span> using the C compiler <span class="nctnt ncbi-var">$(CC)</span> with the flags <span class="nctnt ncbi-var">$(CFLAGS)</span> (see Note below).

-   Using <span class="nctnt ncbi-var">$(CXX)</span> as a linker, build an executable <span class="nctnt ncbi-app">my\_exe</span> from the object files <span class="nctnt ncbi-path">my\_main.o, my\_src1.o, my\_src2.o</span>, the precompiled object file <span class="nctnt ncbi-path">some\_obj.o</span>, NCBI C++ Toolkit libraries [libxmylib.a](#libxmyliba) and <span class="nctnt ncbi-lib">libxncbi.\*</span>, and NCBI SSS DB, SYBASE, and system network libraries (see Note below).

-   Copy the application to the top-level <span class="nctnt ncbi-path">bin/</span> directory of the <span class="nctnt ncbi-monospace">build tree</span> (for later use by other projects).

<span class="nctnt highlight">Note:</span> Since we did not redefine <span class="nctnt ncbi-var">CFLAGS</span>, <span class="nctnt ncbi-var">CXXFLAGS</span>, or <span class="nctnt ncbi-var">LDFLAGS</span>, their default values <span class="nctnt ncbi-var">ORIG\_\*FLAGS</span> (obtained during the <span class="nctnt ncbi-monospace">build tree </span>[configuration](ch_config.html#ch_config.Configuration_and_In)) will be used.

This <span class="nctnt ncbi-monospace">customized makefile</span> should be referred to as <span class="nctnt ncbi-app">my\_exe</span> in the <span class="nctnt ncbi-var">APP\_PROJ</span> macro of the relevant [meta-makefile](#meta-makefile). Note also, that the [Makefile.mk](ch_build.html#ch_build.build_make_macros) will be implicitly included.

#### Example 3: User-defined makefile to build... whatever

In some cases, we may need more functionality than the <span class="nctnt ncbi-monospace">customized makefiles</span> (designed to build libraries and applications) can provide.

So, if you have a "regular" non-customized <span class="nctnt ncbi-monospace">user makefile</span>, and you want to make from it, then you must enlist this <span class="nctnt ncbi-monospace">user makefile</span> in the <span class="nctnt ncbi-var">USR\_PROJ</span> macro of the project's [meta-makefile](#meta-makefile).

Now, during the project build (and before any <span class="nctnt ncbi-monospace">customized makefiles</span> are processed), your makefile will be called with one of the [standard make targets](ch_build.html#ch_build.build_make_proj_target) from the project's build directory. Additionally, the [builddir](ch_build.html#ch_build.T1) and [srcdir](ch_build.html#ch_build.T1) macros will be passed to your makefile (via the <span class="nctnt ncbi-app">make</span> command line).

In most cases, it is necessary to know your "working environment"; i.e., tools, flags and paths (those that you use in your <span class="nctnt ncbi-monospace">customized makefiles</span>). This can be easily done by including [Makefile.mk](ch_build.html#ch_build.build_make_macros) in your makefile.

Shown below is a real-life example of a <span class="nctnt ncbi-monospace">user makefile</span>:

-   build an auxiliary application using the customized makefile <span class="nctnt ncbi-path">Makefile.hc\_gen\_obj.app</span> (this part is a tricky one...)

-   use the resultant application <span class="nctnt ncbi-path">$(bindir)/hc\_gen\_obj</span> to generate the source and header files <span class="nctnt ncbi-path">humchrom\_dat.[ch]</span> from the data file <span class="nctnt ncbi-path">humchrom.dat</span>

-   use the script <span class="nctnt ncbi-app">$(top\_srcdir)/scripts/if\_diff.sh</span> to replace the previous copies (if any) of <span class="nctnt ncbi-path">humchrom\_dat.[ch]</span> with the newly generated versions if and only if the new versions are different (or there were no old versions).

And, of course, it provides build rules for all the [standard make targets](ch_build.html#ch_build.build_make_proj_target).

    File $(top_srcdir)/src/internal/humchrom/Makefile.hc_gen_obj:
    # Build a code generator for hard-coding the chrom data into
    # an obj file
    # Generate header and source "humchrom_dat.[ch]" from data
    # file "humchrom.dat"
    # Deploy the header to the compiler-specific include dir
    # Compile source code
    #################################
    include $(builddir)/Makefile.mk
    BUILD__HC_GEN_OBJ = $(MAKE) -f "$(builddir)/Makefile.app.tmpl" \
    srcdir="$(srcdir)" TMPL="hc_gen_obj" $(MFLAGS)
    all_r: all
    all: build_hc_gen_obj humchrom_dat.dep
    purge_r: purge
    purge:   x_clean
        $(BUILD__HC_GEN_OBJ) purge
    clean_r: clean
    clean:   x_clean
        $(BUILD__HC_GEN_OBJ) clean
    x_clean:
        -rm -f humchrom_dat.h
        -rm -f humchrom_dat.c
    build_hc_gen_obj:
        $(BUILD__HC_GEN_OBJ) all
    humchrom_dat.dep: $(srcdir)/data/humchrom.dat $(bindir)/hc_gen_obj
        -cp -p humchrom_dat.c humchrom_dat.save.c
        $(bindir)/hc_gen_obj -d $(srcdir)/data/humchrom.dat
            -f humchrom_dat
        $(top_srcdir)/scripts/if_diff.sh "mv" humchrom_dat.h
            $(incdir)/humchrom_dat.h
        -rm humchrom_dat.h
        $(top_srcdir)/scripts/if_diff.sh "mv" humchrom_dat.c
           humchrom_dat.save.c
        mv humchrom_dat.save.c humchrom_dat.c
        touch humchrom_dat.dep

### An example of the NCBI C++ makefile hierarchy ("corelib/")

See also the [source](ch_start.html#ch_start.F1) and [build](ch_start.html#ch_start.F2) hierarchy charts.

<span class="nctnt ncbi-path">c++/src/Makefile.in</span>:

    SUB_PROJ = corelib cgi html @serial@ @internal@
    include @builddir@/Makefile.meta

<span class="nctnt ncbi-path">c++/src/corelib/Makefile.in</span>:

    LIB_PROJ = corelib
    SUB_PROJ = test
    srcdir = @srcdir@
    include @builddir@/Makefile.meta

<span class="nctnt ncbi-path">c++/src/corelib/Makefile.corelib.lib</span>:

    SRC = ncbidiag ncbiexpt ncbistre ncbiapp ncbireg ncbienv ncbistd
    LIB = xncbi

<span class="nctnt ncbi-path">c++/src/corelib/test/Makefile.in</span>:

    APP_PROJ = coretest
    srcdir = @srcdir@
    include @builddir@/Makefile.meta

Managing the Work Environment
-----------------------------

The following topics are discussed in this section:

-   [Obtaining the Very Latest Builds](#obtaining-the-very-latest-builds)

-   [Working in a separate directory](#working-in-a-separate-directory)

    -   [Setting up Directory Location](#setting-up-directory-location-)

    -   [The Project's Makefile](#the-project's-makefile-)

    -   [Testing your setup](#testing-your-setup-)

-   [Working Independently In a C++ Subtree](#working-independently-in-a-c++-subtree)

-   [Working within the C++ source tree](#working-within-the-c++-source-tree)

    -   [Checkout the source tree and configure a build directory](#checkout-the-source-tree-and-configure-a-build-directory)

    -   [The project's directories and makefiles](#the-project's-directories-and-makefiles)

    -   [Makefile.in meta files](#makefilein-meta-files)

    -   [An example meta-makefile and its associated project makefiles](#an-example-meta-makefile-and-its-associated-project-makefiles)

    -   [Executing make](#executing-make)

    -   [Custom project makefile: Makefile.myProj](#custom-project-makefile-makefilemyproj)

    -   [Library project makefile: Makefile.myProj.lib](#library-project-makefile-makefilemyprojlib)

    -   [Application project makefile: Makefile.myProj.app](#application-project-makefile-makefilemyprojapp)

    -   [Defining and running tests](#defining-and-running-tests)

    -   [The configure scripts](#the-configure-scripts)

-   [Working with the serializable object classes](#working-with-the-serializable-object-classes)

    -   [Serializable Objects](#serializable-objects)

    -   [Locating and browsing serializable objects in the C++ Toolkit](#locating-and-browsing-serializable-objects-in-the-c++-toolkit)

    -   [Base classes and user classes](#base-classes-and-user-classes)

    -   [Adding methods to the user classes](#adding-methods-to-the-user-classes)

        -   [Checking out source code, configuring the working environment, building the libraries.](#checking-out-source-code-configuring-the-working-environment-building-the-libraries)

        -   [Adding methods](#adding-methods)

### Obtaining the Very Latest Builds

Each new nightly build is available in the <span class="nctnt ncbi-path">$NCBI/c++.by-date/{date}</span> subdirectory. This is done regardless of whether the build succeeds or not.

There are defined symlinks into this directory tree. They include:

-   <span class="nctnt ncbi-path">$NCBI/c++</span> - Symbolic link to <span class="nctnt ncbi-path">$NCBI/c++.production</span>.

-   <span class="nctnt ncbi-path">$NCBI/c++.potluck</span> - The most recent nightly build. It contains whatever libraries and executables have managed to build, and it can miss some of the libraries and/or executables. Use it if you desperately need yesterday's bug fix and do not care of the libraries which are missing.

-   <span class="nctnt ncbi-path">$NCBI/c++.metastable</span> - The most recent nightly build for which the compilation (but not necessarily the test suite) succeeded in all configurations on the given platform. Please note that some projects, including the entire "<span class="nctnt ncbi-path">gui</span>" tree, are considered expendable due to their relative instability and therefore not guaranteed to be present.

-   <span class="nctnt ncbi-path">$NCBI/c++.current</span> - Symbolic link to <span class="nctnt ncbi-path">$NCBI/c++.metastable</span>.

-   <span class="nctnt ncbi-path">$NCBI/c++.stable</span> - The most recent nightly build for which the nightly build (INCLUDING the gui projects) succeeded AND the test suite passed all critical tests on the given platform. This would be the preferred build most of the time for the developers whose projects make use of the actively developed C++ Toolkit libraries. It is usually relatively recent (usually no more than 1 or 2 weeks behind), and at the same time quite stable.

-   <span class="nctnt ncbi-path">$NCBI/c++.frozen</span> - A "production candidate" build made out of the production codebase. There are usually two such builds made for each version of production codebase -- one is for the original production build, and another (usually made in about 2 months after the original production build) is the follow-up bugfix build.

-   <span class="nctnt ncbi-path">$NCBI/c++.production</span> - The most recent production snapshot. This is determined based on general stability of the Toolkit and it is usually derived off the codebase of one of the prior "<span class="nctnt ncbi-path">c++.stable</span>" builds. Its codebase is the same for all platforms and configurations. It is installed only on the major NCBI development platforms (Linux, MS-Windows, and MacOS). It is the safest bet for long-term development. It changes rarely, once in 1 to 3 months. Also, unlike all other builds mentioned here it is guaranteed to be accessible for at least a year (or more), and its DLLs are installed on all (including production) Linux hosts.

-   <span class="nctnt ncbi-path">$NCBI/c++.prod-head</span> - This build is for NCBI developers to quickly check their planned stable component commits using [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh). It is based on the repository path <span class="nctnt ncbi-path">toolkit/production/candidates/production.HEAD</span> – which is the HEAD SVN revision of the C++ Stable Components on which the latest c++.production build was based. It is available on 64-bit Linux.

-   <span class="nctnt ncbi-path">$NCBI/c++.trial</span> - This build is for NCBI developers to quickly check their planned stable component commits using [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh). It is based on the repository path <span class="nctnt ncbi-path">toolkit/production/candidates/trial</span> – which is usually a codebase for the upcoming production build. It is available on 64-bit Linux.

### Working in a separate directory

The following topics are discussed in this section:

-   [Setting up Directory Location](#setting-up-directory-location-)

-   [The Project's Makefile](#the-project's-makefile-)

-   [Testing your setup](#testing-your-setup-)

#### Setting up Directory Location

There are two topics relevant to writing an application using the NCBI C++ Toolkit:

-   Where to place the source and header files for the project

-   How to create a makefile which can link to the correct C++ libraries

What you put in your makefile will depend on where you define your working directory. In this discussion, we assume you will be working **outside** the NCBI C++ tree, say in a directory called <span class="nctnt ncbi-path">newproj</span>. This is where you will write both your source and header files. The first step then, is to create the new working directory and use the [new\_project](#newproject) script to install a makefile there:

    mkdir newproj
    new_project newproj app $NCBI/c++/GCC-Debug/build
        Created a model makefile "/home/user/newproj/Makefile.newproj_app".

The syntax of the script command is:

    new_project <project_name> <app \| lib> [builddir]

where: - <span class="nctnt ncbi-cmd">project\_name</span> is the name of the directory you will be working in - <span class="nctnt ncbi-cmd">app (lib)</span> is used to indicate whether you will be building an application or a library - <span class="nctnt ncbi-cmd">builddir</span> (optional) specifies what version of the pre-built NCBI C++ Toolkit libraries to link to

Several build environments have been pre-configured and are available for developing on various platforms using different compilers, in either **debug** or **release** mode. These environments include custom-made configuration files, makefile templates, and links to the appropriate pre-built C++ Toolkit libraries. To see a list of the available environments for the platform you are working on, use: <span class="nctnt ncbi-cmd">ls -d $NCBI/c++/\*/build</span>. For example, on <span class="nctnt ncbi-monospace">Solaris</span>, the build directories currently available are shown in [Table 1](#table-1).

Table 1. Build Directories

| Directory                                                                      | Compiler     | Version                     |
|--------------------------------------------------------------------------------|--------------|-----------------------------|
| <span class="nctnt ncbi-path">/netopt/ncbi\_tools/c++/Debug/build </span>      | Sun Workshop | Debug                       |
| <span class="nctnt ncbi-path">/netopt/ncbi\_tools/c++/Debug64/build </span>    | Sun Workshop | Debug (64 bit)              |
| <span class="nctnt ncbi-path">/netopt/ncbi\_tools/c++/DebugMT/build </span>    | Sun Workshop | Debug (Multi-thread safe)   |
| <span class="nctnt ncbi-path">/netopt/ncbi\_tools/c++/Release/build </span>    | Sun Workshop | Release                     |
| <span class="nctnt ncbi-path">/netopt/ncbi\_tools/c++/ReleaseMT/build </span>  | Sun Workshop | Release (Multi-thread safe) |
| <span class="nctnt ncbi-path">/netopt/ncbi\_tools/c++/GCC-Debug/build </span>  | GCC          | Debug                       |
| <span class="nctnt ncbi-path">/netopt/ncbi\_tools/c++/GCC-Release/build</span> | GCC          | Release                     |

In the example above, we specified the GNU compiler debug environment: <span class="nctnt ncbi-path">$NCBI/c++/GCC-Debug/build</span>. For a list of currently supported compilers, see the [release notes](https://ncbiconfluence.ncbi.nlm.nih.gov/display/CXX/Release+Notes#ReleaseNotes-release_notes.Platforms_OSs__compi). Running the <span class="nctnt ncbi-app">new\_project</span> script will generate a ready-to-use makefile in the directory you just created. For a more detailed description of this and other scripts to assist you in the set-up of your working environment, see [Starting a new C++ project](#starting-a-new-c++-project).

#### The Project's Makefile

The file you just created with the above script will be called <span class="nctnt ncbi-path">Makefile.newproj\_app</span>. In addition to other things, you will see definitions for: - <span class="nctnt ncbi-var">$(builddir) </span>- a path to the build directory specified in the last argument to the above script - <span class="nctnt ncbi-var">$(srcdir) </span>- the path to your current working directory (".") - <span class="nctnt ncbi-var">$(APP) </span>- the application name - <span class="nctnt ncbi-var">$(OBJ) </span>- the names of the object modules to build and link to the application - <span class="nctnt ncbi-var">$(LIB)</span> - specific libraries to link to in the NCBI C++ Toolkit - <span class="nctnt ncbi-var">$(LIBS) </span>- all other libraries to link to (outside the C++ Toolkit)

<span class="nctnt ncbi-path">$(builddir)/lib</span> specifies the library path (-L), which in this case points to the GNU debug versions of the NCBI C++ Toolkit libraries. $(LIB) lists the individual libraries in this path that you will be linking to. Minimally, this should include <span class="nctnt ncbi-path">xncbi</span> - the library which implements the foundational classes for the C++ tools. Additional library names (e.g. <span class="nctnt ncbi-path">xhtml, xcgi, </span>etc.) can be added here.

Since the shell script assumes you will be building a single executable with the same name as your working directory, the application is defined simply as <span class="nctnt ncbi-app">newproj</span>. Additional targets to build can be added in the area indicated towards the end of the file. The list of objects (OBJ) should include the names (without extensions) of all source files for the application (APP). Again, the script makes the simplest assumption, i.e. that there is a single source file named <span class="nctnt ncbi-path">newproj.cpp</span>. Additional source names can be added here.

#### Testing your setup

For a very simple application, this makefile is ready to be run. Try it out now, by creating the file <span class="nctnt ncbi-path">newproj.cpp</span>:

    // File name: newproj.cpp
    #include <iostream>
    using namespace std;
    int main() {
    cout << "Hello again, world" << endl;
    }

and running:

    make -f Makefile.newproj_app

Of course, it wasn't necessary to set up the directories and makefiles to accomplish this much, as this example does not use any of the C++ classes or resources defined in the NCBI C++ Toolkit. But having accomplished this, you are now prepared to write an actual application, such as described in [Writing a simple application project](ch_core.html#ch_core.writing_simple_app)

Most real applications will at a minimum, require that you <span class="nctnt ncbi-code">\#include </span>[ncbistd.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbistd.hpp) in your header file. In addition to defining some basic NCBI C++ Toolkit objects and templates, this header file in turn includes other header files that define the C Toolkit data types, NCBI namespaces, debugging macros, and exception classes. A set of [template](#template) files are also provided for your use in developing new applications.

### Working Independently In a C++ Subtree

An alternative to developing a new project from scratch is to work within a subtree of the main NCBI C++ source tree so as to utilize the header, source, and make files defined for that subtree. One way to do this would be to check out the entire source tree and then do all your work within the selected subtree(s) only. A better solution is to create a new working directory and check out only the relevant subtrees into that directory. This is somewhat complicated by the distributed organization of the C++ SVN tree: header files are (recursively) contained in an <span class="nctnt ncbi-path">include</span> subtree, while source files are (recursively) contained in a <span class="nctnt ncbi-path">src</span> subtree. Thus, multiple checkouts may be required to set things up properly, and the customized makefiles (<span class="nctnt ncbi-path">Makefile.\*.app</span>) will need to be modified. The shell script [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) will do all of this for you. The syntax is:

    import_project subtree_name [builddir]

where:

-   <span class="nctnt ncbi-var">subtree\_name</span> is the path to a selected directory inside <span class="nctnt ncbi-path">[internal/]c++/src/</span>

-   <span class="nctnt ncbi-var">builddir</span> (optional) specifies what version of the pre-built NCBI C++ Toolkit libraries to link to.

As a result of executing this shell script, you will have a new directory created with the pathname <span class="nctnt ncbi-path">./[internal/]c++/</span> whose structure contains "slices" of the original SVN tree. Specifically, you will find:

    ./[internal/]c++/include/subtree_name
    ./[internal/]c++/src/subtree_name

The <span class="nctnt ncbi-path">src</span> and <span class="nctnt ncbi-path">include</span> directories will contain all of the requested subtree's source and header files along with any hierarchically defined subdirectories. In addition, the script will create new makefiles with the suffix *\*\_app*. These makefiles are generated from the original [customized makefiles](#customized-makefiles) (<span class="nctnt ncbi-path">Makefile.\*.app</span>) located in the original <span class="nctnt ncbi-path">src</span> subtrees. The customized makefiles were designed to work only in conjunction with the build directories in the larger NCBI C++ tree; the newly created makefiles can be used directly in your new working directories.

You can re-run <span class="nctnt ncbi-app">import\_project</span> to add multiple projects to your tree.

<span class="nctnt highlight">Note:</span> If you'd like to import both internal and public projects into a single tree, you'll need to use the <span class="nctnt ncbi-cmd">-topdir</span> option, which will locate the public project within the internal tree, for example:

    import_project internal/demo/misc/xmlwrapp
    import_project -topdir trunk/internal/c++ misc/xmlwrapp
    pushd trunk/internal/c++/src/misc/xmlwrapp
    make
    popd
    pushd trunk/internal/c++/src/internal/demo/misc/xmlwrapp
    make

In this case, your public projects will be located in the <span class="nctnt ncbi-path">internal</span> tree. You must build in each imported subtree, in order from most-dependent to least-dependent so that the imported libraries will be linked to rather than the pre-built libraries.

The NCBI C++ Toolkit project directories, along with the libraries they implement and the logical modules they entail, are summarized in the [Library Reference](part3.html).

Two project directories, <span class="nctnt ncbi-path">internal </span>and <span class="nctnt ncbi-path">objects</span>, may have some subdirectories for which the [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) script does not work normally, if at all. The <span class="nctnt ncbi-path">internal </span>subdirectories are used for in-house development, and the author of a given project may customize the project for their own needs in a way that is incompatible with [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh). The <span class="nctnt ncbi-path">objects</span> subdirectories are used as the original repositories for ASN.1 specifications (which are available for use in your application as described in the section [Processing ASN.1 Data](ch_ser.html#ch_ser.asn.html)), and subsequently, for writing the object definitions and implementations created by the <span class="nctnt ncbi-app">datatool</span> program. Again, these projects can be altered in special ways and some may not be compatible with [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh). Generally, however, [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) should work well with most of these projects.

### Working within the C++ source tree

The following topics are discussed in this section:

-   [Checkout the source tree and configure a build directory](#checkout-the-source-tree-and-configure-a-build-directory)

-   [The project's directories and makefiles](#the-project's-directories-and-makefiles)

-   [Makefile.in meta files](#makefilein-meta-files)

-   [An example meta-makefile and its associated project makefiles](#an-example-meta-makefile-and-its-associated-project-makefiles)

-   [Executing make](#executing-make)

-   [Custom project makefile: Makefile.myProj](#custom-project-makefile-makefilemyproj)

-   [Library project makefile: Makefile.myProj.lib](#library-project-makefile-makefilemyprojlib)

-   [Application project makefile: Makefile.myProj.app](#application-project-makefile-makefilemyprojapp)

-   [Defining and running tests](#defining-and-running-tests)

-   [The configure scripts](#the-configure-scripts)

Most users will find that working in a <span class="nctnt ncbi-monospace">checked-out</span> subtree or a private directory is preferable to working directly in the C++ source tree. There are two good reasons to avoid doing so:

-   Building your own versions of the extensive libraries can be very time-consuming.

-   There is no guarantee that the library utilities your private code links to have not become obsolete.

This section is provided for those developers who must work within the source tree. The [Library Reference](part3.html) provides more complete and technical discussion of the topics reviewed here, and many links to the relevant sections are provided. This page is provided as an overview of material presented in the Library Reference and on the [Working with Makefiles](ch_build.html) pages.

#### Checkout ([\*](ch_getcode_svn.html#ch_getcode_svn.source_tree)) the source tree and configure a build directory

To checkout full Toolkit tree:

<span class="nctnt ncbi-cmd">svn co https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/internal/c++ c++</span>

or, if you don't need internal projects:

<span class="nctnt ncbi-cmd">svn co https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/c++ c++</span>

Once you have done so, you will need to run one of the <span class="nctnt ncbi-app">configure</span> scripts in the Toolkit's root directory. For example, to configure your environment to work with the <span class="nctnt ncbi-app">gcc</span> compiler (on any platform), just run: <span class="nctnt ncbi-app">./configure</span>.

Users working under Windows should consult the [MS Visual C++](ch_config.html#ch_config.MS_Windows) section in the chapter on [Configuring and Building the Toolkit](ch_config.html).

The [configure script](ch_config.html#ch_config.Configuration_and_In) is a multi-platform configuration shell script (generated from <span class="nctnt ncbi-path">configure.in</span> using <span class="nctnt ncbi-app">autoconf</span>). Here are some pointers to sections that will help you configure the build environment:

-   [Wrapper scripts](ch_config.html#ch_config.Special_Consideratio) supporting various platforms

-   [Optional configuration flags](ch_config.html#ch_config.Running_the_configur)

The <span class="nctnt ncbi-app">configure</span> script concludes with a message describing how to build the C++ Toolkit libraries. If your application will be working with ASN.1 data, use the <span class="nctnt ncbi-cmd">--with-objects</span> flag in running the <span class="nctnt ncbi-app">configure</span> script, so as to populate the <span class="nctnt ncbi-path">include/objects</span> and <span class="nctnt ncbi-path">src/objects</span> subdirectories and build the <span class="nctnt ncbi-path">objects</span> libraries. The <span class="nctnt ncbi-path">objects</span> directories and libraries can also be updated separately from the rest of the compilation, by executing <span class="nctnt ncbi-app">make</span> inside the <span class="nctnt ncbi-path">build/objects</span> directory. Prior to doing so however, you should always verify that your <span class="nctnt ncbi-path">build/bin</span> directory contains the latest version of <span class="nctnt ncbi-app">datatool</span>.

#### The project's directories and makefiles

To start a new project ("<span class="nctnt ncbi-path">myProj</span>"), you should begin by creating both a <span class="nctnt ncbi-path">src</span> and an <span class="nctnt ncbi-path">include</span> subtree for that project inside the C++ tree. In general, all header files that will be accessed by multiple source modules outside the project directory should be placed in the <span class="nctnt ncbi-path">include</span> directory. Header files that will be used solely inside the project's <span class="nctnt ncbi-path">src</span> directory should be placed in the <span class="nctnt ncbi-path">src</span> directory, along with the implementation files.

In addition to the C++ source files, the <span class="nctnt ncbi-path">src</span> subtrees contain <span class="nctnt ncbi-monospace">meta-makefiles</span> named <span class="nctnt ncbi-path">Makefile.in</span>, which are used by the <span class="nctnt ncbi-app">configure</span> script to generate the corresponding makefiles in the <span class="nctnt ncbi-path">build</span> subtrees. Figure 1 shows slices of the directory structure reflecting the correspondences between the <span class="nctnt ncbi-monospace">meta-makefiles</span> in the <span class="nctnt ncbi-path">src</span> subtrees and makefiles in the <span class="nctnt ncbi-path">build</span> subtrees. Figure 2 is a sketch of the entire C++ tree in which these directories are defined.

During the configuration process, each of the <span class="nctnt ncbi-monospace">meta-makefiles</span> in the top-level of the <span class="nctnt ncbi-path">src</span> tree is translated into a corresponding makefile in the top-level of the <span class="nctnt ncbi-path">build</span> tree. Then, for each project directory containing a <span class="nctnt ncbi-path">Makefile.in</span>, the <span class="nctnt ncbi-app">configure</span> script will: (1) create a corresponding subdirectory of the same name in the <span class="nctnt ncbi-path">build</span> tree if it does not already exist, and (2) generate a corresponding makefile in the project's <span class="nctnt ncbi-path">build</span> subdirectory. The contents of the project's <span class="nctnt ncbi-path">Makefile.in</span> in the <span class="nctnt ncbi-path">src</span> subdirectory determine what is written to the project's makefile in the <span class="nctnt ncbi-path">build</span> subdirectory. Project subdirectories that do not contain a <span class="nctnt ncbi-path">Makefile.in</span> file are ignored by the <span class="nctnt ncbi-app">configure</span> script.

Thus, you will also need to create a <span class="nctnt ncbi-monospace">meta-makefile</span> in the newly created <span class="nctnt ncbi-path">src/myProj</span> directory before configuring your <span class="nctnt ncbi-path">build</span> directory to include the new project. The <span class="nctnt ncbi-app">configure</span> script will then create the corresponding subtree in the <span class="nctnt ncbi-path">build</span> directory, along with a new makefile generated from the <span class="nctnt ncbi-path">Makefile.in</span> you created. See Makefile Hierarchy (Chapter 4, [Figure 1](ch_build.html#ch_build.F1)) and [Figure 1](#figure-1).

[![Figure 1. Meta makefiles and the makefiles they generate](/book-test/static/img/cpptree.jpg)](/book-test/static/img/cpptree.jpg "Click to see the full-resolution image")

Figure 1. Meta makefiles and the makefiles they generate

#### Makefile.in meta files

The meta-makefile <span class="nctnt ncbi-path">myProj/Makefile.in</span> should define at least one of the following macros:

-   <span class="nctnt ncbi-var">USR\_PROJ</span> (optional) - a list of names for user-defined makefiles.<br/>This macro is provided for the usage of ordinary stand-alone makefiles which do not utilize the <span class="nctnt ncbi-app">make</span> commands contained in additional makefiles in the top-level <span class="nctnt ncbi-path">build</span> directory. Each <span class="nctnt ncbi-monospace">p\_i</span> listed in <span class="nctnt ncbi-monospace">USR\_PROJ = p\_1 ... p\_N</span> must have a corresponding [Makefile.p\_i](#makefilepi) in the project's source directory. When <span class="nctnt ncbi-app">make</span> is executed, the <span class="nctnt ncbi-app">make</span> directives contained in these files will be executed directly to build the targets as specified.

-   <span class="nctnt ncbi-var">LIB\_PROJ</span> (optional) - a list of names for library makefiles.<br/>For each library <span class="nctnt ncbi-monospace">l\_i</span> listed in <span class="nctnt ncbi-monospace">LIB\_PROJ = l\_1 ... l\_N</span>, you must have created a corresponding project makefile named [Makefile.l\_i.lib](#makefilelilib) in the project's source directory. When <span class="nctnt ncbi-app">make</span> is executed, these library project makefiles will be used along with <span class="nctnt ncbi-path">Makefile.lib</span> and <span class="nctnt ncbi-path">Makefile.lib.tmpl</span> (located in the top-level of the <span class="nctnt ncbi-path">build</span> tree) to build the specified libraries.

-   <span class="nctnt ncbi-var">APP\_PROJ</span> (optional) - a list of names for application makefiles.<br/>Similarly, each application (<span class="nctnt ncbi-monospace">p1, p2, ..., pN</span>) listed under <span class="nctnt ncbi-var">APP\_PROJ</span> must have a corresponding project makefile named [Makefile.p\*.app](#makefilep*app) in the project's source directory. When <span class="nctnt ncbi-app">make</span> is executed, these application project makefiles will be used along with <span class="nctnt ncbi-path">Makefile.app</span> and <span class="nctnt ncbi-path">Makefile.app.tmpl</span> to build the specified executables.

-   <span class="nctnt ncbi-var">SUB\_PROJ</span> (optional) - a list of names for subproject directories (used on recursive makes).<br/>The <span class="nctnt ncbi-var">SUB\_PROJ</span> macro is used to recursively define <span class="nctnt ncbi-app">make</span> targets; items listed here define the subdirectories rooted in the project's source directory where <span class="nctnt ncbi-app">make</span> should also be executed.

The <span class="nctnt ncbi-path">Makefile.in</span> meta file in the project's source directory defines a kind of road map that will be used by the <span class="nctnt ncbi-app">configure</span> script to generate a makefile (<span class="nctnt ncbi-path">Makefile</span>) in the corresponding directory of the <span class="nctnt ncbi-path">build</span> tree. <span class="nctnt ncbi-path">Makefile.in</span> does *not* participate in the actual execution of <span class="nctnt ncbi-app">make</span>, but rather, defines what will happen at that time by directing the <span class="nctnt ncbi-app">configure</span> script in the creation of the <span class="nctnt ncbi-path">Makefile</span> that **will** be executed (see also the description of [Makefile targets](ch_build.html#ch_build.std_build_targets)).

#### An example meta-makefile and its associated project makefiles

A simple example should help to make this more concrete. Assuming that <span class="nctnt ncbi-path">myProj</span> is used to develop an application named <span class="nctnt ncbi-path">myProj</span>, <span class="nctnt ncbi-path">myProj/Makefile.in</span> should contain the following:

    ####### Example: src/myProj/Makefile.in
    APP_PROJ = myProj
    srcdir = @srcdir@
    include @builddir@/Makefile.meta

The last two lines in <span class="nctnt ncbi-path">Makefile.in</span> should always be exactly as shown here. These two lines specify <span class="nctnt ncbi-app">make</span> variable templates using the <span class="nctnt ncbi-var">@var\_name@</span> syntax. When generating the corresponding makefile in the <span class="nctnt ncbi-path">build</span> directory, the <span class="nctnt ncbi-app">configure</span> script will substitute each identifier name bearing that notation with full path definitions.

The corresponding makefile in <span class="nctnt ncbi-path">build/myProj</span> generated by the <span class="nctnt ncbi-app">configure</span> script for this example will then contain:

    ####### Example: myBuild/build/myProj/Makefile
    # Generated automatically from Makefile.in by configure.
    APP_PROJ = myProj
    srcdir = /home/zimmerma/internal/c++/src/myProj
    include /home/zimmerma/internal/c++/myBuild/build/Makefile.meta

As demonstrated in this example, the <span class="nctnt ncbi-var">@srcdir@</span> and <span class="nctnt ncbi-var">@builddir@</span> aliases in the makefile template have been replaced with absolute paths in the generated makefile, while the definition of <span class="nctnt ncbi-var">APP\_PROJ</span> is copied verbatim.

The only build target in this example is <span class="nctnt ncbi-path">myProj</span>. <span class="nctnt ncbi-path">myProj</span> is specified as an application - not a library - because it is listed under <span class="nctnt ncbi-var">APP\_PROJ</span> rather than under <span class="nctnt ncbi-var">LIB\_PROJ</span>. Accordingly, there must also be a file named <span class="nctnt ncbi-path">Makefile.myProj.app</span> in the <span class="nctnt ncbi-path">src/myProj</span> directory. A project's application makefile specifies:

-   <span class="nctnt ncbi-var">APP</span> - the name to be used for the resulting executable

-   <span class="nctnt ncbi-var">OBJ</span> - a list of object files to use in the compilation

-   <span class="nctnt ncbi-var">LIB</span> - a list of NCBI C++ Toolkit libraries to use in the linking

-   <span class="nctnt ncbi-var">LIBS</span> - a list of other libraries to use in the linking

There may be any number of application or library makefiles for the project, Each application should be listed under <span class="nctnt ncbi-var">APP\_PROJ</span> and each library should be listed under <span class="nctnt ncbi-var">LIB\_PROJ</span> in <span class="nctnt ncbi-path">Makefile.in</span>. A suitable application makefile for this simple example might contain just the following text:

    ####### Example: src/myProj/Makefile.myProj.app
    APP = myProj
    OBJ = myProj
    LIB = xncbi

In this simple example, the <span class="nctnt ncbi-var">APP\_PROJ</span> definition in <span class="nctnt ncbi-path">Makefile.in</span> is identical to the definitions of both <span class="nctnt ncbi-var">APP</span> and <span class="nctnt ncbi-var">OBJ</span> in <span class="nctnt ncbi-path">Makefile.myProj.app</span>. This is not always the case, however, as the <span class="nctnt ncbi-var">APP\_PROJ</span> macro is used to define which makefiles in the <span class="nctnt ncbi-path">src</span> directory should be used during compilation, while <span class="nctnt ncbi-var">APP</span> defines the name of the resulting executable and <span class="nctnt ncbi-var">OBJ</span> specifies the names of object files. (Project makefiles for applications are described in more detail [below](#below).)

#### Executing make

Given these makefile definitions, executing <span class="nctnt ncbi-cmd">make all\_r</span> in the <span class="nctnt ncbi-path">build</span> project subdirectory indirectly causes <span class="nctnt ncbi-path">build/Makefile.meta</span> to be executed, which sets the following chain of events in motion:

1  
For each <span class="nctnt ncbi-path">proj\_name</span> listed in [USR\_PROJ](#usrproj), <span class="nctnt ncbi-path">Makefile.meta</span> first tests to see if <span class="nctnt ncbi-path">Makefile.proj\_name</span> is available in the current <span class="nctnt ncbi-path">build</span> directory, and if so, executes:<br/><br/><span class="nctnt ncbi-cmd">make -f Makefile.proj\_name builddir="$(builddir)"</span><br/><span class="nctnt ncbi-cmd">srcdir="$(srcdir)" $(MFLAGS)</span><br/><br/>Otherwise, <span class="nctnt ncbi-path">Makefile.meta</span> assumes the required makefile is in the project's source directory, and executes:<br/><br/><span class="nctnt ncbi-cmd">make -f $(srcdir)/Makefile.proj\_name builddir="$(builddir)" srcdir="$(srcdir)" $(MFLAGS)</span><br/><br/>In either case, the important thing to note here is that the commands contained in the project's makefiles are executed directly and are **not** combined with additional makefiles in the top-level <span class="nctnt ncbi-path">build</span> directory. The aliased <span class="nctnt ncbi-var">srcdir</span>, <span class="nctnt ncbi-var">builddir</span>, and <span class="nctnt ncbi-var">MFLAGS</span> are still available and can be referred to inside <span class="nctnt ncbi-path">Makefile.proj\_name</span>. By default, the resulting libraries and executables are written to the <span class="nctnt ncbi-path">build</span> directory only.<br/>

2  
For each <span class="nctnt ncbi-lib">lib\_name</span> listed in [LIB\_PROJ](#libproj),<br/><br/><span class="nctnt ncbi-cmd">make -f $(builddir)/Makefile.lib.tmpl</span><br/><br/>is executed. This in turn specifies that <span class="nctnt ncbi-path">$(builddir)/Makefile.mk</span>, <span class="nctnt ncbi-path">$(srcdir)/Makefile.lib\_name.lib</span>, and <span class="nctnt ncbi-path">$(builddir)/Makefile.lib</span> should be included in the generated makefile commands that actually get executed. The resulting libraries are written to the <span class="nctnt ncbi-path">build</span> subdirectory and copied to the <span class="nctnt ncbi-path">lib</span> subtree.

3  
For each <span class="nctnt ncbi-app">app\_name</span> listed in [APP\_PROJ](#appproj),<br/><br/><span class="nctnt ncbi-cmd">make -f $(builddir)/Makefile.app.tmpl</span><br/><br/>is executed. This in turn specifies that <span class="nctnt ncbi-path">$(builddir)/Makefile.mk</span>, <span class="nctnt ncbi-path">$(srcdir)/Makefile.app\_name.app</span>, and <span class="nctnt ncbi-path">$(builddir)/Makefile.app</span> should be included in the generated makefile commands that actually get executed. The resulting executables are written to the <span class="nctnt ncbi-path">build</span> subdirectory and copied to the <span class="nctnt ncbi-path">bin</span> subtree.

4  
For each <span class="nctnt ncbi-path">dir\_name</span> listed in [SUB\_PROJ](#subproj) (on <span class="nctnt ncbi-cmd">make all\_r</span>),<br/><br/><span class="nctnt ncbi-cmd">cd dir\_name</span><br/><span class="nctnt ncbi-cmd">make all\_r</span><br/><br/>is executed. Steps (1) - (3) are then repeated in the project subdirectory.

More generally, for each subdirectory listed in <span class="nctnt ncbi-var">SUB\_PROJ</span>, the <span class="nctnt ncbi-app">configure</span> script will create a relative subdirectory inside the new <span class="nctnt ncbi-path">build</span> project directory, and generate the new subdirectory's <span class="nctnt ncbi-path">Makefile</span> from the corresponding meta-makefile in the <span class="nctnt ncbi-path">src</span> subtree. Note that each subproject directory must also contain its own <span class="nctnt ncbi-path">Makefile.in</span> along with the corresponding project makefiles. The recursive make commands, <span class="nctnt ncbi-cmd">make all\_r, make clean\_r</span>, and <span class="nctnt ncbi-cmd">make purge\_r</span> all refer to this definition of the subprojects to define what targets should be recursively built or removed.

#### Custom project makefile: Makefile.myProj ([\*](#*))

As described, regular makefiles contained in the project's <span class="nctnt ncbi-path">src</span> directory will be invoked from the <span class="nctnt ncbi-path">build</span> directory if their suffixes are specified in the <span class="nctnt ncbi-var">USR\_PROJ</span> macro. This macro is originally defined in the project's <span class="nctnt ncbi-path">src</span> directory in the <span class="nctnt ncbi-path">Makefile.in</span> meta file, and is propagated to the corresponding <span class="nctnt ncbi-path">Makefile</span> in the <span class="nctnt ncbi-path">build</span> directory by the <span class="nctnt ncbi-app">configure</span> script.

For example, if <span class="nctnt ncbi-monospace">USR\_PROJ = myProj</span> in the <span class="nctnt ncbi-path">build</span> directory's <span class="nctnt ncbi-path">Makefile</span>, executing <span class="nctnt ncbi-app">make</span> will cause <span class="nctnt ncbi-path">Makefile.myProj</span> (the project makefile) to be executed. This project makefile may be located in either the current <span class="nctnt ncbi-path">build</span> directory **or** the corresponding <span class="nctnt ncbi-path">src</span> directory. In either case, although the makefile is executed directly, references to the source or object files (contained in the project makefile) must give complete paths to those files. In the first case, <span class="nctnt ncbi-app">make</span> is invoked as: <span class="nctnt ncbi-cmd">make -f Makefile.myProj</span>, so the makefile is located in the current working (<span class="nctnt ncbi-path">build</span>) directory but the source files are not. In the second case, <span class="nctnt ncbi-app">make</span> is invoked as:

<span class="nctnt ncbi-cmd">make -f $(srcdir)/Makefile.myProj</span>,

so both the project makefile **and** the source files are non-local. For example:

    ####### Makefile.myProj
    include $(NCBI)/ncbi.mk
    # use the NCBI default compiler for this platform
    CC = $(NCBI_CC)
    # along with the default include
    INCPATH = $(NCBI_INCDIR)
    # and library paths
    LIBPATH = $(NCBI_LIBDIR)
    all: $(srcdir)/myProj.c
        $(CC) -o myProj $(srcdir)/myProj.c $(NCBI_CFLAGS) -I($INCPATH) \
        -L($LIBPATH) -lncbi
        cp -p myProj $(builddir)/bin
    clean:
        -rm  myProj myProj.o
    purge: clean
        -rm  $(builddir)/bin/myProj

will cause the C program <span class="nctnt ncbi-path">myProj</span> to be built directly from <span class="nctnt ncbi-path">Makefile.myProj</span> using the default C compiler, library paths, include paths, and compilation flags defined in <span class="nctnt ncbi-path">ncbi.mk</span>. The executables and libraries generated from the targets specified in <span class="nctnt ncbi-var">USR\_PROJ</span> are by default written to the current <span class="nctnt ncbi-path">build</span> directory only. In this example however, they are also explicitly copied to the <span class="nctnt ncbi-path">bin</span> directory, and accordingly, the <span class="nctnt ncbi-cmd">purge</span> directives also remove the copied executable.

#### Library project makefile: Makefile.myProj.lib ([\*](#*))

<span class="nctnt ncbi-path">Makefile.lib\_name.lib</span> should contain the following macro definitions:

-   <span class="nctnt ncbi-var">$(SRC)</span> - the names of all source files to compile and include in the library

-   <span class="nctnt ncbi-var">$(OBJ)</span> - the names of any pre-compiled object files to include in the library

-   <span class="nctnt ncbi-var">$(LIB)</span> - the name of the library being built

In addition, any of the make variables defined in <span class="nctnt ncbi-path">build/Makefile.mk</span>, such as <span class="nctnt ncbi-var">$CPPFLAGS</span>, <span class="nctnt ncbi-var">$LINK</span>, etc., can be referred to and/or redefined in the project makefile, e.g.:

    CFLAGS   = $(ORIG_CFLAGS)   -abc -DFOOBAR_NOT_CPLUSPLUS
    CXXFLAGS = $(ORIG_CXXFLAGS) -xyz
    CPPFLAGS = $(ORIG_CPPFLAGS) -UFOO -DP1_PROJECT -I$(NCBI_C_INCLUDE)
    LINK     = purify $(ORIG_LINK)

For an example from the Toolkit, see [Makefile.corelib.lib](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/Makefile.corelib.lib).

For a more detailed example, including information about shared libraries, see [example 1 above](#example-1-above).

#### Application project makefile: Makefile.myProj.app ([\*](#*))

<span class="nctnt ncbi-path">Makefile.app\_name.app</span> should contain the following macro definitions:

-   <span class="nctnt ncbi-var">$(SRC)</span> - the names of the object modules to build and link to the application

-   <span class="nctnt ncbi-var">$(OBJ)</span> - the names of any pre-compiled object files to include in the linking

-   <span class="nctnt ncbi-var">$(LIB)</span> - specific libraries in the NCBI C++ Toolkit to include in the linking

-   <span class="nctnt ncbi-var">$(LIBS)</span> - all other libraries to link to (outside the C++ Toolkit)

-   <span class="nctnt ncbi-var">$(APP)</span> - the name of the application being built

For example, if C Toolkit libraries should also be included in the linking, use:

    LIBS = $(NCBI_C_LIBPATH) -lncbi $(ORIG_LIBS)

The project's application makefile can also redefine the compiler and linker, along with other flags and tools affecting the build process, as described above for <span class="nctnt ncbi-path">Makefile.\*.lib</span> files. For an example from the Toolkit, see [Makefile.coretest.app](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/test/Makefile.coretest.app), and for a documented example, see [example 2 above](#example-2-above).

#### Defining and running tests

The definition and execution of unit tests is controlled by the <span class="nctnt ncbi-macro">CHECK\_CMD</span> macro in the test application's makefile, <span class="nctnt ncbi-path">Makefile.app\_name.app</span>. If this macro is not defined (or commented out), then no test will be executed. If <span class="nctnt ncbi-macro">CHECK\_CMD</span> is defined, then the test it specifies will be included in the automated test suite and can also be invoked independently by running "<span class="nctnt ncbi-cmd">make check</span>".

To include an application into the test suite it is necessary to add just one line into its makefile <span class="nctnt ncbi-path">Makefile.app\_name.app</span>:

    CHECK_CMD =

or

    CHECK_CMD = command line to run application test

For the first form, where no command line is specified by the <span class="nctnt ncbi-macro">CHECK\_CMD</span> macro, the program specified by the makefile variable <span class="nctnt ncbi-macro">APP</span> will be executed (without any parameters).

For the second form: If your application is executed by a script specified in a <span class="nctnt ncbi-macro">CHECK\_CMD</span> command line, and it doesn't read from <span class="nctnt ncbi-monospace">STDIN</span>, then the script should invoke it like this:

    $CHECK_EXEC app_name arg1 arg2 ...

If your application *does* read from <span class="nctnt ncbi-monospace">STDIN</span>, then <span class="nctnt ncbi-macro">CHECK\_CMD</span> scripts should invoke it like this:

    $CHECK_EXEC_STDIN app_name arg1 arg2 ...

<span class="nctnt highlight">Note:</span> Applications / scripts in the <span class="nctnt ncbi-macro">CHECK\_CMD</span> definition should **not** use "<span class="nctnt ncbi-cmd">.</span>", for example:

    $CHECK_EXEC ./app_name arg1 arg2 ... # Do not prefix app_name with ./

Scripts invoked via <span class="nctnt ncbi-macro">CHECK\_CMD</span> should pass an exit code to the testing framework via the <span class="nctnt ncbi-var">exitcode</span> variable, for example:

    exitcode=$?

If your test program needs additional files (for example, a configuration file, data files, or helper scripts referenced in <span class="nctnt ncbi-macro">CHECK\_CMD</span>), then set <span class="nctnt ncbi-macro">CHECK\_COPY</span> to point to them:

    CHECK_COPY = file1 file2 dir1 dir2

Before the tests are run, all specified files and directories will be copied to the build or special check directory (which is platform-dependent). Note that all paths to copied files and directories must be relative to the application source directory.

By default, the application's execution time is limited to 200 seconds. You can set a new limit using:

    CHECK_TIMEOUT = <time in seconds>

If application continues execution after specified time, it will be terminated and test marked as FAILED.

If you'd like to get nightly test results automatically emailed to you, add your email address to the <span class="nctnt ncbi-macro">WATCHERS</span> macro in the makefile. Note that the <span class="nctnt ncbi-macro">WATCHERS</span> macro has replaced the <span class="nctnt ncbi-macro">CHECK\_AUTHORS</span> macro which had a similar purpose.

For information about using Boost for unit testing, see the "[Boost Unit Test Framework](ch_boost.html)" chapter.

#### The configure scripts

A number of [compiler-specific wrappers](ch_config.html#ch_config.Special_Consideratio) for different platforms are described in the chapter on [configuring and building](ch_config.html). Each of these wrappers performs some pre-initialization for the tools and flags used in the <span class="nctnt ncbi-app">configure</span> script before running it. The compiler-specific wrappers are in the [c++/compilers](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/compilers) directory. The <span class="nctnt ncbi-app">configure</span> script serves two very different types of function: (1) it tests the selected compiler and environment for a multitude of features and generates <span class="nctnt ncbi-code">\#include</span> and <span class="nctnt ncbi-code">\#define</span> statements accordingly, and (2) it reads the <span class="nctnt ncbi-path">Makefile.in</span> files in the <span class="nctnt ncbi-path">src</span> directories and creates the corresponding <span class="nctnt ncbi-path">build</span> subtrees and makefiles accordingly.

Frequently during development it is necessary to make minor adjustments to the <span class="nctnt ncbi-path">Makefile.in</span> files, such as adding new projects or subprojects to the list of targets. In these contexts however, the compiler, environment, and source directory structures remain unchanged, and <span class="nctnt ncbi-app">configure</span> is actually doing much more work than is necessary. In fact, there is even some risk of failing to re-create the same configuration environment if the user does not exactly duplicate the same set of configure flags when re-running <span class="nctnt ncbi-app">configure</span>. In these situations, it is preferable to run an auxiliary script named [config.status](ch_config.html#ch_config.ch_configpre_built_h), located at the top level of the <span class="nctnt ncbi-path">build</span> directory in a subdirectory named <span class="nctnt ncbi-path">status</span>.

In contrast, changes to the <span class="nctnt ncbi-path">src</span> directory structure, or the addition/deletion of <span class="nctnt ncbi-path">Makefile.in</span> files, all require re-running the <span class="nctnt ncbi-app">configure</span> script, as these actions require the creation/deletion of subdirectories in the <span class="nctnt ncbi-path">build</span> tree and/or the creation/deletion of the associated <span class="nctnt ncbi-path">Makefile</span> in those directories.

### Working with the serializable object classes

The following topics are discussed in this section:

-   [Serializable Objects](#serializable-objects)

-   [Locating and browsing serializable objects in the C++ Toolkit](#locating-and-browsing-serializable-objects-in-the-c++-toolkit)

-   [Base classes and user classes](#base-classes-and-user-classes)

-   [Adding methods to the user classes](#adding-methods-to-the-user-classes)

    -   [Checking out source code, configuring the working environment, building the libraries](#checking-out-source-code-configuring-the-working-environment-building-the-libraries)

    -   [Adding methods](#adding-methods)

#### Serializable Objects

All of the ASN.1 data types defined in the C Toolkit have been re-implemented in the C++ Toolkit as serializable objects. Header files for these classes can be found in the [include/objects](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objects) directories, and their implementations are located in the [src/objects](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/objects) directories. and

The implementation of these classes as serializable objects has a number of implications. It must be possible to use expressions like: <span class="nctnt ncbi-code">instream \>\> myObject</span> and <span class="nctnt ncbi-code">outstream \<\< myObject, </span>where specializations are entailed for the <span class="nctnt ncbi-monospace">serial format</span> of the iostreams (ASN.1, XML, etc.), as well as for the internal structure of the object. The C++ Toolkit deploys several [object stream classes](ch_ser.html#ch_ser.objstream.html) that specialize in various formats, and which know how to access and apply the [type information](ch_ser.html#ch_ser.typeinfo.html)that is associated with the serializable object.

The type information for each class is defined in a separate static [CTypeInfo](ch_ser.html#ch_ser.typeinfo.html_ctypeinfo_ref) object, which can be accessed by all instances of that class. This is a very powerful device, which allows for the implementation of many features generally found only in languages which have built-in class reflection. Using the Toolkit's serializable objects will require some familiarity with the usage of this type information, and several sections of this manual cover these topics (see [Runtime Object Type Information](ch_ser.html#ch_ser.typeinfo.html) for a general discussion).

#### Locating and browsing serializable objects in the C++ Toolkit

The top level of the [include/objects](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objects) subtree is a set of subdirectories, where each subdirectory includes the public header files for a separately compiled library. Similarly, the [src/objects](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/objects) subtree includes a set of subtrees containing the source files for these libraries. Finally, your <span class="nctnt ncbi-path">build/objects</span> directory will contain a corresponding set of build subtrees where these libraries are actually built.

If you checked out the entire C++ SVN tree, you may be surprised to find that initially, the <span class="nctnt ncbi-path">include/objects</span> subtrees are empty, and the subdirectories in the <span class="nctnt ncbi-path">src/objects</span> subtree contain only ASN.1 modules. This is because both the header files and source files are auto-generated from the ASN.1 specifications by the [datatool](ch_app.html#ch_app.datatool) program. As described in [Working within the C++ source tree](#working-within-the-c++-source-tree), you can build everything by running <span class="nctnt ncbi-cmd">make all\_r</span> in the build directory.

<span class="nctnt highlight">Note:</span> If you would like to have the <span class="nctnt ncbi-lib">objects</span> libraries built locally, you **must** use the <span class="nctnt ncbi-cmd">--with-objects</span> flag when running the <span class="nctnt ncbi-app">configure</span> script.

You can also access the pre-generated serializable objects in the public area, using the source browsers to locate the objects you are particularly interested in. For example, if you are seeking the new class definition for the <span class="nctnt ncbi-monospace">Bioseq struct</span> defined in the C Toolkit, you can search for the <span class="nctnt ncbi-class">CBioseq</span> class, using either the [LXR](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident) identifier search tool, or the Doxygen [class hierarchy](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/hierarchy.html) browser. Starting with the name of the data object as it appears in the ASN.1 module, two simple rules apply in deriving the new C++ class name:

-   The one letter 'C' (for class) prefix should precede the ASN.1 name

-   All hyphens ('-') should be replaced by underscores ('\_')

For example, <span class="nctnt ncbi-monospace">Seq-descr </span>becomes <span class="nctnt ncbi-class">CSeq\_descr</span>.

#### Base classes and user classes

The classes whose names are derived in this manner are called the <span class="nctnt ncbi-monospace">user classes</span>, and each also has a corresponding <span class="nctnt ncbi-monospace">base class </span>implementation. The name of the <span class="nctnt ncbi-monospace">base class</span> is arrived at by appending <span class="nctnt ncbi-monospace">"\_Base"</span> to the <span class="nctnt ncbi-monospace">user class</span> name. Most of the <span class="nctnt ncbi-monospace">user classes </span>are empty wrapper classes that do not bring any new functionality or data members to the inherited <span class="nctnt ncbi-monospace">base class</span>; they are simply provided as a platform for development. In contrast, the <span class="nctnt ncbi-monospace">base classes</span> are **not** intended for public use (other than browsing), and should never be modified.

More generally, the <span class="nctnt ncbi-monospace">base classes</span> should *never* be instantiated or accessed directly in an application. The relation between the two source files and the classes they define reflects a general design used in developing the object libraries: the base class files are auto-generated by [datatool](ch_app.html#ch_app.datatool) according to the ASN.1 specifications in the <span class="nctnt ncbi-path">src/objects</span> directories; the inherited class files (the so-called <span class="nctnt ncbi-monospace">user classes</span>) are intended for developers who can extend these classes to support features above and beyond the ASN.1 specifications.

Many applications will involve a "tangled hierarchy" of these objects, reflecting the complexity of the real world data that they represent. For example, a <span class="nctnt ncbi-class">CBioseq\_set</span> contains a list of <span class="nctnt ncbi-class">CSeq\_entry</span> objects, where each <span class="nctnt ncbi-class">CSeq\_entry</span> is, in turn, a [choice](ch_ser.html#ch_ser.choice.html) between a <span class="nctnt ncbi-class">CBioseq</span> and a <span class="nctnt ncbi-class">CBioseq\_set</span>.

Given the potential for this complexity of interactions, a critical design issue becomes how one can ensure that methods which may have been defined only in the <span class="nctnt ncbi-monospace">user class</span> will be available for all instances of that class. In particular, these instances may occur as contained elements of another object which is compiled in a different library. These inter-object dependencies are the motivation for the <span class="nctnt ncbi-monospace">user classes</span>. As shown in [Figure 2](#figure-2), all references to external objects which occur inside the <span class="nctnt ncbi-monospace">base classes</span>, access external <span class="nctnt ncbi-monospace">user classes</span>, so as to include any methods which may be defined only in the <span class="nctnt ncbi-monospace">user classes</span>:

[![Figure 2. Example of complex relationships between base classes and user classes](/book-test/static/img/user_base.gif)](/book-test/static/img/user_base.gif "Click to see the full-resolution image")

Figure 2. Example of complex relationships between base classes and user classes

In most cases, adding non-virtual methods to a <span class="nctnt ncbi-monospace">user class</span> will **not** require re-compiling any libraries except the one which defines the modified object. Note however, that adding non-static data members and/or <span class="nctnt ncbi-code">virtual</span> methods to the <span class="nctnt ncbi-monospace">user classes </span>**will change** the class layouts, and in these cases only, will entail recompiling any external library objects which access these classes.

#### Adding methods to the user classes

<span class="nctnt highlight">Note:</span> This section describes the steps currently required to add new methods to the <span class="nctnt ncbi-monospace">user classes</span>. It is subject to change, and there is no guarantee the material here is up-to-date. In general, it is not recommended practice to add methods to the <span class="nctnt ncbi-monospace">user classes</span>, unless your purpose is to extend these classes across all applications as part of a development effort.

The following topics are discussed in this section:

-   [Checking out source code, configuring the working environment, building the libraries.](#checking-out-source-code-configuring-the-working-environment-building-the-libraries)

-   [Adding methods](#adding-methods)

##### Checking out source code, configuring the working environment, building the libraries

-   Create a working directory (e.g. <span class="nctnt ncbi-path">Work</span>) and check out the C++ tree to that directory:, using either SVN checkout or the shell script, [`svn_core`](ch_getcode_svn.html#ch_getcode_svn.core_sh).

-   [Configure](ch_config.html#ch_config.Running_the_configur) the environment to work inside this tree using one of the configure scripts, according to the platform you will be working on. Be sure to include the <span class="nctnt ncbi-cmd">--with-objects</span> flag in invoking the <span class="nctnt ncbi-app">configure</span> script.

-   Build the <span class="nctnt ncbi-lib">xncbi</span>, <span class="nctnt ncbi-lib">xser</span> and <span class="nctnt ncbi-lib">xser</span> libraries, and run <span class="nctnt ncbi-cmd">datatool</span> to create the objects header and source files, and build all of the object module libraries:

<!-- -->

    # Build the core library
    cd path_to_compile_dir/build/corelib
    make
    # Build the util library
    cd path_to_compile_dir/build/util
    make
    # might as well build datatool and avoid possible version skew cd path_to_compile_dir/build/serial make all_r
    # needed for a few projects
    cd path_to_compile_dir/build/connect
    make
    cd path_to_compile_dir/build/objects
    make all_r

<br/>Here <span class="nctnt ncbi-var">path\_to\_compile\_dir</span> is set to the compile work directory which depends on the compiler settings (e.g: <span class="nctnt ncbi-path">~/Work/internal/GCC-Debug</span>). In addition to creating the header and source files, using <span class="nctnt ncbi-cmd">make all\_r </span>(instead of just <span class="nctnt ncbi-app">make</span>) will build all the libraries. All libraries that are built are also copied to the <span class="nctnt ncbi-path">lib</span> dir, e.g.:<span class="nctnt ncbi-path">~/Work/internal/c++/GCC-Debug/lib</span>. Similarly, all executables (such as <span class="nctnt ncbi-app">asn2asn</span>) are copied to the bin dir, e.g.: <span class="nctnt ncbi-path">~/Work/internal/c++/GCC-Debug/bin</span>.

You are now ready to edit the user class files and add methods.

##### Adding methods

As an example, suppose that we would like to add a method to the <span class="nctnt ncbi-class">CSeq\_inst</span> class to calculate sequence length, e.g.:<span class="nctnt ncbi-func">CSeq\_inst::CalculateLength()</span>. We begin by adding a declaration of this method to the public section of the user class definition in <span class="nctnt ncbi-path">Seq\_inst.hpp</span>:

    class CSeq_inst : public CSeq_inst_Base
    {
    public:
        CSeq_inst(void);
        ~CSeq_inst(void);
        static CSeq_inst* New(void)
            {
                return new CSeq_inst(eCanDelete);
            }
        int CalculateLength() const;
    protected:
        CSeq_inst(ECanDelete);
    };

and in the source file, <span class="nctnt ncbi-path">Seq\_inst.cpp</span>, we implement

    int CSeq_inst::CalculateLength() const
    {
        // implementation goes here
    }

These files are in the [include/objects/seq](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objects/seq) and [src/objects/seq](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/objects/seq) subdirectories, respectively. Once you have made the modifications to the files, you need to recompile the <span class="nctnt ncbi-lib">seq</span> library, <span class="nctnt ncbi-lib">libseq.a</span>, i.e.:

    cd path_to_compile_dir/GCC-Debug/build/objects/seq
    make

Here <span class="nctnt ncbi-var">path\_to\_compile\_dir </span>is set to the compile work directory which depends on the compiler settings (e.g: <span class="nctnt ncbi-path">~/Work/internal/GCC-Debug</span>). The new method can now be invoked from within a <span class="nctnt ncbi-class">CBioseq</span> object as: <span class="nctnt ncbi-code">myBioseq.GetInst().CalculateLength().</span>

The key issue that determines whether or not you will need to rebuild any <span class="nctnt ncbi-monospace">external</span> libraries that use the modified user class involves the class layout in memory. All of the external libraries which reference the object refer to the class layout that existed prior to the changes you have made. Thus, if your modifications do **not** affect the class layout, you do not have to rebuild any external libraries. Changes that *do* affect memory mapping include:

-   The addition of new, non-static data members

-   The addition of virtual methods

If you have added either of the above to the <span class="nctnt ncbi-monospace">user class</span>, then you will need to identify all external objects which use your object, and recompile the libraries in which these objects are defined.


