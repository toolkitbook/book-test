---
layout: default
title: C++ Toolkit test
nav: pages/ch_start
---

<span class="label">2</span>Getting Started
===========================================

Last Update: September 27, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This section is intended as a bird's-eye view of the Toolkit for new users, and to give quick access to important reference links for experienced users. It lays out the general roadmap of tasks required to get going, giving links to take the reader to detailed discussions and supplying a number of simple, concrete test applications.

<span class="nctnt highlight">Note:</span> Much of this material is platform-neutral, although the discussion is platform-centric. Users would also benefit from reading the instructions specific to those systems and, where applicable, how to use Subversion (SVN) with [MS Windows](ch_getcode_svn.html#ch_getcode_svn.windows) and [Mac OS](ch_getcode_svn.html#ch_getcode_svn.mac).

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Quick Start](#quick-start)

-   [Example Applications](#example-applications)

-   [Example Libraries](#example-libraries)

-   [Source Tree Availability](#source-tree-availability)

    -   [FTP Availability](#ftp-availability)

    -   [SVN Availability](#svn-availability)

    -   [Availability via Shell Scripts](#availability-via-shell-scripts)

-   [Source Tree Contents](#source-tree-contents)

    -   [Top-Level Source Organization](#top-level-source-organization)

    -   [The Core NCBI C++ Toolkit](#the-core-ncbi-c++-toolkit)

    -   [Source Tree for Individual Projects](#source-tree-for-individual-projects)

    -   [The Makefile Templates](#the-makefile-templates)

    -   [The New Module Stubs](#the-new-module-stubs)

-   [Decide Where You Will Work (in-tree, in a subtree, out-of-tree)](#decide-where-you-will-work-in-tree-in-a-subtree-out-of-tree)

-   [Basic Installation and Configuration Considerations](#basic-installation-and-configuration-considerations)

-   [Basics of Using the C++ Toolkit](#basics-of-using-the-c++-toolkit)

    -   [Compiling and Linking with make](#compiling-and-linking-with-make)

    -   [Makefile Customization](#makefile-customization)

    -   [Basic Toolkit Coding Infrastructure](#basic-toolkit-coding-infrastructure)

    -   [Key Classes](#key-classes)

    -   [The Object Manager and datatool](#the-object-manager-and-datatool)

    -   [Debugging and Diagnostic Aids](#debugging-and-diagnostic-aids)

    -   [Coding Standards and Guidelines](#coding-standards-and-guidelines)

-   [Noteworthy Files](#noteworthy-files)

Quick Start
-----------

A good deal of the complication and tedium of getting started has thankfully been wrapped by a number of shell scripts. They facilitate a 'quick start' whether starting anew or within an existing Toolkit work environment. ('Non-quick starts' sometimes cannot be avoided, but they are considered [elsewhere](ch_proj.html#ch_proj.new_proj_struct).)

-   **Get the Source Tree (see** [Figure 1](#figure-1)**)**

    -   Retrieve via SVN ([in-house](ch_getcode_svn.html#ch_getcode_svn.code_retrieval) \\| [public](ch_getcode_svn.html#ch_getcode_svn.external)), **or**

    -   Download [via FTP](ch_getcode_svn.html#ch_getcode_svn.ftp_download), **or**

    -   Run [svn\_core](ch_getcode_svn.html#ch_getcode_svn.core_sh) *(requires a SVN repository containing the C++ Toolkit; for NCBI users)*

-   **Configure the build tree (see** [Figure 2](#figure-2)**)**

    -   Use the [configure](ch_config.html#ch_config.Running_the_configur) script, **or**

    -   Use a compiler-specific [wrapper script](ch_config.html#ch_config.Special_Consideratio) (e.g. <span class="nctnt ncbi-app">compilers/unix/\*.sh</span>).

-   **Build the C++ Toolkit from** <span class="nctnt ncbi-monospace">makefiles</span> **and** <span class="nctnt ncbi-monospace">meta-makefiles</span>(if required)

    -   <span class="nctnt ncbi-cmd">make all\_r</span> for a recursive make, **or**

    -   <span class="nctnt ncbi-cmd">make all</span> to make only targets for the current directory.

-   **Work on your new or existing application or library** the scripts [new\_project](ch_proj.html#ch_proj.new_proj_struct) and (for an existing Toolkit project) [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) help to set up the appropriate <span class="nctnt ncbi-monospace">makefiles</span> and/or source.

[![Figure 1. NCBI C++ Source Tree](/book-test/static/img/src_tree.gif)](/book-test/static/img/src_tree.gif "Click to see the full-resolution image")

Figure 1. NCBI C++ Source Tree

[![Figure 2. NCBI C++ Build Tree](/book-test/static/img/bld_tree.gif)](/book-test/static/img/bld_tree.gif "Click to see the full-resolution image")

Figure 2. NCBI C++ Build Tree

In a nutshell, that's all it takes to get up and running. The download, configuration, installation and build actions are shown for two cases in this sample.

The last item, employing the Toolkit in a project, completely glosses over the substantial issue of how to use the installed Toolkit. Where does one begin to look to identify the functionality to solve your particular problem, or indeed, to write the simplest of programs? "[Basics of Using the C++ Toolkit](#basics-of-using-the-c++-toolkit)" will deal with those issues. Investigate these and other topics with the set of [sample applications](#sample-applications). See [Examples](ch_demo.html) for further cases that employ specific features of the NCBI C++ Toolkit.

Example Applications
--------------------

The suite of application examples below highlight important areas of the Toolkit and can be used as a starting point for your own development. Note that you may generate the sample application code by running the [new\_project](ch_proj.html#ch_proj.new_proj_struct) script for that application. The following examples are now available:

-   [app/basic](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/basic/) - This example builds two applications: a generic application (<span class="nctnt ncbi-path">basic\_sample</span>) to demonstrate the use of [key Toolkit classes](#key-toolkit-classes), and an example program (<span class="nctnt ncbi-path">multi\_command</span>) that accepts multiple command line forms.

-   [app/alnmgr](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/alnmgr/) - Creates an alignment manager application.

-   [app/asn](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/asn/) - Creates a library based on an ASN.1 specification, and a test application.

-   [app/blast](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/blast/) - Creates an application that uses BLAST.

-   [app/cgi](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/cgi/) - Creates a Web-enabled CGI application.

-   [app/dbapi](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/) - Creates a database application.

-   [app/eutils](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/eutils/) - Creates an eUtils client application.

-   [app/lds](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/lds/) - Creates an application that uses local data storage (LDS).

-   [app/netcache](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/netcache/) - Creates an application that uses [NetCache](ch_app.html#ch_app.ncbi_netcache_service).

-   [app/netschedule](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/netschedule/) - Creates an NCBI [GRID](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/GRID) application that uses [NetSchedule](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/NetSchedule).

-   [app/objects](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/objects/) - Creates an application that uses ASN.1 objects.

-   [app/objmgr](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/objmgr/) - The Toolkit manipulates biological data objects in the context of an <span class="nctnt ncbi-monospace">Object Manager</span> class (<span class="nctnt ncbi-class">CObjectManager</span>). This example creates an application that uses the object manager.

-   [app/sdbapi](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/sdbapi/) - Creates a database application that uses <span class="nctnt ncbi-lib">SDBAPI</span>.

-   [app/serial](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/serial/) - Creates a dozen applications that demonstrate using serial library hooks, plus a handful of other applications that demonstrate other aspects of the serial library.

-   [app/soap/client](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/soap/client/) - Creates a SOAP client application.

-   [app/soap/server](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/soap/server/) - Creates a SOAP server application.

-   [app/unit\_test](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/unit_test/) - Creates an NCBI unit test application.

To build an example use its accompanying <span class="nctnt ncbi-path">Makefile</span>.

Example Libraries
-----------------

The following example libraries can be created with [new\_project](ch_proj.html#ch_proj.new_proj_struct) and used as a starting point for a new library:

-   [lib/basic](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/lib/basic/) - Creates a trivial library (it finds files in <span class="nctnt ncbi-var">PATH</span>) for demonstrating the basics of the build system for libraries. This example library includes a simple test application.

-   [lib/asn](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/lib/asn_lib/) - Creates an ASN.1 object project.

-   [lib/dtd](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/lib/dtd/) - Creates an XML DTD project.

-   [lib/xsd](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/lib/xsd/) - Creates an XML Schema project.

Source Tree Availability
------------------------

The source tree is available through FTP, SVN and by running special scripts. The following subsections discuss these topics in more detail:

-   [FTP Availability](#ftp-availability)

-   [SVN Availability](#svn-availability)

-   [Availability via Shell Scripts](#availability-via-shell-scripts)

### FTP Availability

The Toolkit source is available via ftp at [ftp://ftp.ncbi.nih.gov/toolbox/ncbi\_tools++/CURRENT/](ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/CURRENT), and the archives available, with unpacking instructions, are listed on the [download page](ch_getcode_svn.html). If you plan to modify the Toolkit source in any way with the ftp code, it is strongly advised that it be placed under a source code control system (preferably SVN) so that you can rollback to an earlier revision without having to ftp the entire archive once again.

### SVN Availability

NCBI users can obtain the source tree directly from the internal [SVN repository](ch_getcode_svn.html#ch_getcode_svn.code_retrieval).

A [read-only repository](ch_getcode_svn.html#ch_getcode_svn.external) is also available to the public.

### Availability via Shell Scripts

For NCBI users, the various shell scripts in <span class="nctnt ncbi-path">$NCBI/c++/scripts</span> tailor the working codebase and can prepare the work environment for new projects. Except where noted, an active Toolkit SVN repository is required, and obviously in all cases a version of the Toolkit must be accessible.

-   <span class="nctnt ncbi-app">svn\_core</span>. Details on [svn\_core](ch_getcode_svn.html#ch_getcode_svn.core_sh) are discussed in a later chapter.

-   <span class="nctnt ncbi-app">import\_project</span>. Details on [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) are discussed in a later chapter.

-   <span class="nctnt ncbi-app">new\_project</span>. Details on [new\_project](ch_proj.html#ch_proj.new_proj_struct) are discussed in a later chapter.

-   <span class="nctnt ncbi-app">update\_projects</span>. Details on [update\_core](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh) and [update\_projects](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh) are covered in later chapter.

Source Tree Contents
--------------------

The following topics are discussed in this section:

-   [Top-Level Source Organization](#top-level-source-organization)

-   [The Core NCBI C++ Toolkit](#the-core-ncbi-c++-toolkit)

-   [Source Tree for Individual Projects](#source-tree-for-individual-projects)

-   [The Makefile Templates](#the-makefile-templates)

-   [The New Module Stubs](#the-new-module-stubs)

### Top-Level Source Organization

The NCBI C++ Toolkit <span class="nctnt ncbi-monospace">source tree</span> (see [Figure 1](#figure-1)) is organized as follows:

-   <span class="nctnt ncbi-path">src/</span> -- a hierarchical directory tree of [NCBI C++ projects](ch_proj.html#ch_proj.start_new_proj). Contained within <span class="nctnt ncbi-path">src</span> are all source files (<span class="nctnt ncbi-path">\*.cpp, \*.c</span>), along with private header files (<span class="nctnt ncbi-path">\*.hpp, \*.h</span>), makefiles (<span class="nctnt ncbi-path">Makefile.\*</span>, including [Makefile.mk](ch_build.html#ch_build.build_make_macros)), scripts (<span class="nctnt ncbi-path">\*.sh</span>), and occasionally some project-specific data

-   <span class="nctnt ncbi-path">include/</span> -- a hierarchical directory tree whose structure mirrors the <span class="nctnt ncbi-path">src</span> directory tree. It contains only public header files (<span class="nctnt ncbi-path">\*.hpp, \*.h</span>).

<span class="nctnt highlight">Example:</span><span class="nctnt ncbi-path">include/corelib/</span> contains public headers for the sources located in <span class="nctnt ncbi-path">src/corelib/</span>

-   <span class="nctnt ncbi-path">scripts/</span> -- auxiliary scripts, including those to help manage interactions with the NCBI SVN code repository, such as [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh), [new\_project](ch_proj.html#ch_proj.new_project_Starting), and [svn\_core](ch_getcode_svn.html#ch_getcode_svn.core_sh).

-   files for platform-specific [configuration and installation](ch_config.html#ch_config.Running_the_configur):

    -   <span class="nctnt ncbi-path">compilers/</span> -- directory containing [compiler-specific configure wrappers](ch_config.html#ch_config.Special_Consideratio) (<span class="nctnt ncbi-path">unix/\*.sh</span>) and miscellaneous resources and build scripts for [MS Windows](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/common/config/ncbiconf_msvc.h)/[MacOS](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/common/config/ncbiconf_xcode.h) platforms

    -   <span class="nctnt ncbi-app">configure</span> -- a multi-platform configuration shell script (generated from template <span class="nctnt ncbi-path">configure.ac</span> using [autoconf](http://www.gnu.org/software/autoconf))

    -   various scripts and template files used by <span class="nctnt ncbi-app">configure</span>, [autoconf](http://www.gnu.org/software/autoconf)

-   <span class="nctnt ncbi-app">doc/</span> -- NCBI C++ documentation, including a library reference, configuration and installation instructions, example code and guidelines for **everybody** writing code for the NCBI C++ Toolkit.

### The Core NCBI C++ Toolkit

The 'core' libraries of the Toolkit provide users with a highly portable set of functionality. The following projects comprise the portable core of the Toolkit:

> <span class="nctnt ncbi-lib">corelib connect cgi html util</span>

Consult the library reference (Part 3 of this book) for further details.

### Source Tree for Individual Projects

For the overall NCBI C++ source tree structure see [Top-Level Source Organization](#top-level-source-organization) above.

An individual project contains the set of source code and/or scripts that are required to build a Toolkit library or executable. In the NCBI source tree, projects are identified as sub-trees of the <span class="nctnt ncbi-path">src</span>, and <span class="nctnt ncbi-path">include</span> directories of the main C++ tree root. For example, <span class="nctnt ncbi-path">corelib </span>and <span class="nctnt ncbi-path">objects/objmgr</span> are both projects. However, note that a project's code exists in two sibling directories: the public headers in <span class="nctnt ncbi-path">include/</span> and the source code, private headers and <span class="nctnt ncbi-monospace">makefiles</span> in <span class="nctnt ncbi-path">src</span>.

The contents of each project's [source tree](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src) are:

-   <span class="nctnt ncbi-path">\*.cpp, \*.hpp</span> -- project's source files and private headers

-   <span class="nctnt ncbi-path">Makefile.in</span> -- a [meta-makefile](ch_build.html#ch_build.makefiles_meta) to specify which local projects (described in <span class="nctnt ncbi-path">Makefile.\*.in</span>) and sub-projects(located in the project subdirectories) must be built

-   <span class="nctnt ncbi-path">Makefile.\*.lib, Makefile.\*.app</span> -- <span class="nctnt ncbi-monospace">customized makefiles</span> to build a library or an application

-   <span class="nctnt ncbi-path">Makefile.\*</span> -- "free style" makefiles

-   sub-project directories (if any)

### The Makefile Templates

Each project is built by customizing a set of generic <span class="nctnt ncbi-monospace">makefiles</span>. These generic <span class="nctnt ncbi-monospace">makefile</span> templates (<span class="nctnt ncbi-path">Makefile.\*.in</span>) are found in <span class="nctnt ncbi-path">src</span> and help to control the assembly of the entire Toolkit via recursive builds of the individual projects. (The usage of these <span class="nctnt ncbi-monospace">makefiles</span> and other configurations issues are [summarized below](#summarized-below) and detailed on the [Working with Makefiles](ch_build.html) page.)

-   <span class="nctnt ncbi-path">Makefile.in</span> -- makefile to perform a recursive build in all project subdirectories

-   <span class="nctnt ncbi-path">Makefile.meta.in</span> -- included by all makefiles that provide both local and recursive builds

-   <span class="nctnt ncbi-path">Makefile.mk.in</span> -- included by all makefiles; sets a lot of configuration variables

-   <span class="nctnt ncbi-path">Makefile.lib.in</span> -- included by all makefiles that perform a "standard" library build, when building only static libraries.

-   <span class="nctnt ncbi-path">Makefile.dll.in</span> -- included by all makefiles that perform a "standard" library build, when building only shared libraries.

-   <span class="nctnt ncbi-path">Makefile.both.in</span> -- included by all makefiles that perform a "standard" library build, when building both static and shared libraries.

-   <span class="nctnt ncbi-path">Makefile.lib.tmpl.in</span> -- serves as a template for the project <span class="nctnt ncbi-monospace">customized makefiles</span> (<span class="nctnt ncbi-path">Makefile.\*.lib[.in]</span>) that perform a "standard" library build

-   <span class="nctnt ncbi-path">Makefile.app.in</span> -- included by all makefiles that perform a "standard" application build

-   <span class="nctnt ncbi-path">Makefile.app.tmpl.in</span> -- serves as a template for the project <span class="nctnt ncbi-monospace">customized makefiles</span> (<span class="nctnt ncbi-path">Makefile.\*.app[.in]</span>) that perform a "standard" application build

-   <span class="nctnt ncbi-path">Makefile.rules.in, Makefile.rules\_with\_autodep.in</span> -- instructions for building object files; included by most other makefiles

### The New Module Stubs

A Toolkit module typically consists of a header (<span class="nctnt ncbi-path">\*.hpp</span>) and a source (<span class="nctnt ncbi-path">\*.cpp</span>) file. Use the [stubs](ch_proj.html#ch_proj.new_modules) provided, which include boilerplate such as the NCBI disclaimer and SVN revision information, to easily start a new module. You may also consider using the [sample code](#sample-code) described above for your new module.

Decide Where You Will Work (in-tree, in a subtree, out-of-tree)
---------------------------------------------------------------

Depending on how you plan to interact with the NCBI C++ Toolkit source tree, the Toolkit has mechanisms to streamline how you [create and manage](ch_proj.html) projects. The simplest case is to work [out-of-tree](ch_proj.html#ch_proj.outside_tree) in a private directory. This means that you are writing new code that needs only to link with pre-built Toolkit libraries. If your project requires the source for a limited set of Toolkit projects it is often sufficient to work [in a subtree](ch_proj.html#ch_proj.work_sub_tree) of the Toolkit source distribution.

Most users will find it preferable and fully sufficient to work in a subtree or a private directory. Certain situations and users (particularly Toolkit developers) do require access to the full Toolkit source tree; in such instances one must work [in-tree](ch_proj.html#ch_proj.inside_tree).

Basic Installation and Configuration Considerations
---------------------------------------------------

<span class="nctnt highlight">Note: </span>Much of this discussion is Unix-centric. Windows and Mac users would also benefit from reading the instructions specific to those systems.

The configuration and installation process is automated with the <span class="nctnt ncbi-app">configure</span> script and its [wrappers](ch_config.html#ch_config.Special_Consideratio) in the <span class="nctnt ncbi-path">compilers</span> directory. These scripts handle the compiler- and platform-dependent Toolkit settings and create the build<span class="nctnt ncbi-monospace"> tree</span> (see [Figure 2](#figure-2)) skeleton. The configured <span class="nctnt ncbi-monospace">build tree</span>, located in <span class="nctnt ncbi-path">\<builddir\></span>, is populated with customized <span class="nctnt ncbi-monospace">meta-makefile</span>, headers and source files. Most system-dependence has been isolated in the <span class="nctnt ncbi-path">\<builddir\>/inc/ncbiconf.h</span> header. By running <span class="nctnt ncbi-cmd">make all\_r</span> from <span class="nctnt ncbi-path">\<builddir\></span>, the full Toolbox is built for the target platform and compiler combination.

Summarized below are some basic ways to control the installation and configuration process. More comprehensive documentation can be found at [config.html](ch_config.html).

-   **A Simple Example Build**

-   [configure Options](ch_config.html#ch_config.ch_configget_synopsi) View the list of options by running<br/><span class="nctnt ncbi-cmd">./configure --help</span>

-   **Enable/Disable Debugging**

-   [Building Shared and/or Static Libraries](ch_config.html#ch_config.Building_Shared_Libr) Shared libraries (DLL's) can be used in Toolkit executables and libraries for a number of tested configurations. Note that to link with the shared libraries at run time a valid [runpath](ch_config.html#ch_config.ch_configconfightml_) must be specified. For a detailed example, including information on building shared libraries, see [this example](ch_proj.html#ch_proj.make_proj_lib).

-   If you are outside NCBI, make sure the paths to your third party libraries are correctly specified. See [Site-Specific Third Party Library Configuration](ch_config.html#ch_config.SiteSpecific_Third_Party_Libra) for details.

-   [Influencing configure via Environment Variables](ch_config.html#ch_config.ch_configconfig_flag) Several environment variables control the tools and flags employed by <span class="nctnt ncbi-app">configure</span>. The [generic ones](ch_config.html#ch_config.ref_TableToolsAndFlags) are: <span class="nctnt ncbi-var">CC, CXX, CPP, AR, RANLIB, STRIP, CFLAGS, CXXFLAGS, CPPFLAGS, LDFLAGS, LIBS</span>. In addition, you may manually set various [localization environment variables](ch_config.html#ch_config.ch_configlocalizatio).

-   [Multi-Thread Safe Compilation](ch_config.html#ch_config.ch_configmt_safe_com)

-   **Controlling Builds of Optional Projects** You may selectively build or not build one of the optional projects ("serial", "ctools", "gui", "objects", "internal") with <span class="nctnt ncbi-app">configure</span> flags. If an optional project is not configured into your distribution, it can be added later using the [import\_projects](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) script.

-   [Adjust the Configuration of an Existing Build](ch_config.html#ch_config.ch_configpre_built_h) If you need to update or change the configuration of an existing build, use the <span class="nctnt ncbi-app">reconfigure.sh</span> or <span class="nctnt ncbi-app">relocate.sh</span> script.

-   [Working with Multiple build trees](ch_config.html#ch_config.Configuration_and_In) Managing builds for a variety of platforms and/or compiler environments is straightforward. The configure/install/build cycle has been designed to support the concurrent development of multiple builds from the same source files. This is accomplished by having independent <span class="nctnt ncbi-monospace">build trees</span> that exist as sibling directories. Each build is configured according to its own set of configuration options and thus produces distinct libraries and executables. All builds are nonetheless constructed from the same source code in $NCBI/<span class="nctnt ncbi-path">c++/{src, include}</span>.

Table 1. Environment variables that affect the configure / build process

| Name          | Default                                                                       | Synopsis ([see also](ch_build.html#ch_build.T5))                                                      |
|---------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| AR            | ar cru                                                                        | Librarian                                                                                             |
| CC            | gcc, cc                                                                       | C compiler                                                                                            |
| CFLAGS        | -g [or/and/nor](ch_config.html#ch_config.Debug_vs_Release_Con) -O             | C compiler flags                                                                                      |
| CONFIG\_SHELL | /bin/sh                                                                       | Command interpreter to use in the configuration scripts and makefiles (it must be compatible with sh) |
| CPP           | $CC -E                                                                        | C preprocessor                                                                                        |
| CPPFLAGS      | -D\_DEBUG [or/and/nor](ch_config.html#ch_config.Debug_vs_Release_Con)-DNDEBUG | C/C++ preprocessor flags                                                                              |
| CXX           | c++, g++, gcc, CC, cxx, cc++                                                  | C++ compiler, also being used as a linker                                                             |
| CXXCPP        | $CXX -E                                                                       | C++ preprocessor                                                                                      |
| CXXFLAGS      | -g [or/and/nor](ch_config.html#ch_config.Debug_vs_Release_Con) -O             | C++ compiler flags                                                                                    |
| LDFLAGS       | None                                                                          | Linker flags                                                                                          |
| LIBS          | None                                                                          | Libraries to link to every executable                                                                 |
| STRIP         | strip                                                                         | To discard symbolic info                                                                              |

Basics of Using the C++ Toolkit
-------------------------------

The following topics are discussed in this section:

-   [Compiling and Linking with make](#compiling-and-linking-with-make)

-   [Makefile Customization](#makefile-customization)

-   [Basic Toolkit Coding Infrastructure](#basic-toolkit-coding-infrastructure)

-   [Key Classes](#key-classes)

-   [The Object Manager and datatool](#the-object-manager-and-datatool)

-   [Debugging and Diagnostic Aids](#debugging-and-diagnostic-aids)

-   [Coding Standards and Guidelines](#coding-standards-and-guidelines)

### Compiling and Linking with <span class="nctnt ncbi-app">make</span>

The NCBI C++ Toolkit uses the standard Unix utility <span class="nctnt ncbi-app">make</span> to build libraries and executable code, using instructions found in <span class="nctnt ncbi-monospace">makefiles</span>. More [details on compiling and linking with make can be found in a later chapter](ch_build.html).

To initiate compilation and linking, run <span class="nctnt ncbi-app">make</span>:

    make -f <Makefile_Name> [<target_name>]

When run from the top of the build tree, this command can make the entire tree (with target <span class="nctnt ncbi-cmd">all\_r</span>). If given within a specific project subdirectory it can be made to target just that project. The Toolkit has in its <span class="nctnt ncbi-path">src </span>directory templates (e.g., <span class="nctnt ncbi-path">Makefile.\*.in</span>) for [makefiles](ch_build.html#ch_build.build_proj_makefiles) and [meta-makefiles](ch_build.html#ch_build.makefiles_meta) that define common file locations, compiler options, environment settings, and [standard make targets](ch_build.html#ch_build.build_make_proj_target). Each Toolkit project has a specialized [meta-makefile](#meta-makefile) in its <span class="nctnt ncbi-path">src</span> directory. The relevant <span class="nctnt ncbi-monospace">meta-makefile</span> templates for a project, e.g., <span class="nctnt ncbi-path">Makefile.in</span>, are customized by <span class="nctnt ncbi-app">configure</span> and placed in its <span class="nctnt ncbi-monospace">build tree</span>. For new projects, whether [in](ch_proj.html#ch_proj.inside_tree) or [out](ch_proj.html#ch_proj.outside_tree) of the C++ Toolkit tree, the programmer must provide either <span class="nctnt ncbi-monospace">makefiles</span> or <span class="nctnt ncbi-monospace">meta-makefiles</span>.

### Makefile Customization

Fortunately, for the common situations where a [script](ch_getcode_svn.html#ch_getcode_svn.code_retrieval) was used to set up your source, or if you are working in the C++ Toolkit source tree, you will usually have correctly customized <span class="nctnt ncbi-monospace">makefiles</span> in each project directory of the build tree. For other cases, particularly [when using the new\_project](ch_proj.html#ch_proj.new_proj_struct) script, some measure of user customization may be needed. The more frequent customizations involve (see "[Working with Makefiles](ch_proj.html#ch_proj.proj_makefiles)" or "[Project makefiles"](ch_proj.html#ch_proj.proj_makefiles) for a full discussion):

-   [meta-makefile macros](ch_proj.html#ch_proj.inside_make_meta): <span class="nctnt ncbi-var">APP\_PROJ, LIB\_PROJ, SUB\_PROJ, USR\_PROJ</span> Lists of applications, libraries, sub-projects, and user projects, respectively, to make.

-   [Library and](ch_proj.html#ch_proj.make_proj_lib)[Application macros](ch_proj.html#ch_proj.make_proj_app): <span class="nctnt ncbi-var">APP, LIB, LIBS, OBJ, SRC</span> List the application name to build, Toolkit library(ies) to make or include, non-Toolkit library(ies) to link, object files to make, and source to use, respectively.

-   [Compiler Flag Macros](ch_build.html#ch_build.build_make_macros): <span class="nctnt ncbi-var">CFLAGS, CPPFLAGS, CXXFLAGS, LDFLAGS</span> Include or override C compiler, C/C++ preprocessor, C++ compiler, and linker flags, respectively. Many more localization macros are also [available for use](ch_config.html#ch_config.ch_configlocalizatio).

-   [Altering the Active Version of the Toolkit](ch_proj.html#ch_proj.new_project_Starting) You can change the active version of NCBI C++ toolkit by manually setting the variable <span class="nctnt ncbi-var">$(builddir)</span> in <span class="nctnt ncbi-path">Makefile.foo\_[app\\|lib]</span> to the desired toolkit path, e.g.: <span class="nctnt ncbi-code">builddir = $(NCBI)/c++/GCC-Release/build</span>.<br/>Consult [this list](ch_proj.html#ch_proj.outside_dir_loc) or, better, look at the output of '<span class="nctnt ncbi-cmd">ls -d $NCBI/c++/\*/build</span>' to see those pre-built Toolkit builds available on your system.

### Basic Toolkit Coding Infrastructure

Summarized below are some features of the global Toolkit infrastructure that users may commonly employ or encounter.

-   ***The NCBI Namespace Macros*** The header [ncbistl.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbistl.hpp) defines three principal namespace macros: <span class="nctnt ncbi-macro">NCBI\_NS\_STD</span>, <span class="nctnt ncbi-macro">NCBI\_NS\_NCBI</span> and <span class="nctnt ncbi-macro">NCBI\_USING\_NAMESPACE\_STD</span>. Respectively, these refer to the standard C++ <span class="nctnt ncbi-code">std::</span> namespace, a local NCBI namespace <span class="nctnt ncbi-code">ncbi::</span> for Toolkit entities, and a namespace combining the names from <span class="nctnt ncbi-macro">NCBI\_NS\_STD</span> and <span class="nctnt ncbi-macro">NCBI\_NS\_NCBI</span>.

-   ***Using the NCBI Namespaces*** Also in [ncbistl.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbistl.hpp) are the macros <span class="nctnt ncbi-macro">BEGIN\_NCBI\_SCOPE</span> and <span class="nctnt ncbi-macro">END\_NCBI\_SCOPE</span>. These bracket code blocks which define names to be included in the NCBI namespace, and are invoked in nearly all of the Toolkit headers (see example). To use the NCBI namespace in a code block, place the <span class="nctnt ncbi-macro">USING\_NCBI\_SCOPE</span> macro before the block references its first unqualified name. This macro also allows for unqualified use of the <span class="nctnt ncbi-code">std::</span> namespace. Much of the Toolkit source employs this macro (see example), although it is possible to define and work with other namespaces.

-   ***Configuration-Dependent Macros and*** <span class="nctnt ncbi-path">ncbiconf.h</span><span class="nctnt ncbi-code"> \#ifdef</span> tests for the configuration-dependent macros, for example <span class="nctnt ncbi-macro">\_DEBUG</span> or <span class="nctnt ncbi-macro">NCBI\_OS\_UNIX</span>, etc., are used throughout the Toolkit for conditional compilation and accommodate your environment's requirements. The <span class="nctnt ncbi-app">configure</span> script defines many of these macros; the resulting <span class="nctnt ncbi-code">\#define</span>'s appear in the [ncbiconf.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/find?string=ncbiconf.h) header and is found in the <span class="nctnt ncbi-path">\<builddir\>/inc</span> directory. It is not typically included explicitly by the programmer, however. Rather, it is included by other basic Toolkit headers (e.g., <span class="nctnt ncbi-path">ncbitype.h, ncbicfg.h, ncbistl.hpp</span>) to pick up configuration-specific features.

-   ***NCBI Types (***<span class="nctnt ncbi-path">ncbitype.h, ncbi\_limits.[h\\|hpp]</span>***)*** To promote code portability developers are strongly encouraged to use these standard C/C++ types whenever possible as they are ensured to have well-defined behavior throughout the Toolkit. Also see the current [type-use rules](ch_core.html#ch_core.types_policy). The [ncbitype.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbitype.h) header provides a set of [fixed-size integer types](ch_core.html#ch_core.fixed_size_integers) for special situations, while the <span class="nctnt ncbi-path">ncbi\_limits.[</span>[h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbi_limits.h)<span class="nctnt ncbi-path">\\| </span>[hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbi_limits.h)<span class="nctnt ncbi-path">]</span> headers set numeric limits for the supported types.

-   ***The*** <span class="nctnt ncbi-path">ncbistd.hpp</span> ***header*** The NCBI C++ standard \#include's and \#defin'itions are found in [ncbistd.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbistd.hpp), which provides the interface to many of the basic Toolkit modules. The explicit NCBI headers included by <span class="nctnt ncbi-path">ncbistd.hpp</span> are: <span class="nctnt ncbi-path">ncbitype.h, ncbistl.hpp, ncbistr.hpp, ncbidbg.hpp, ncbiexpt.hpp</span> and <span class="nctnt ncbi-path">ncbi\_limits.h</span>.

-   ***Portable Stream Handling*** Programmers can ensure portable stream and buffer I/O operations by using the NCBI C++ Toolkit stream wrappers, <span class="nctnt ncbi-code">typedef</span>'s and <span class="nctnt ncbi-code">\#define</span>'s declared in the [ncbistre.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbistre.hpp). For example, always use <span class="nctnt ncbi-class">CNcbiIstream</span> instead of <span class="nctnt ncbi-code">YourFavoriteNamespace::istream</span> and favor <span class="nctnt ncbi-class">NcbiCin</span> over <span class="nctnt ncbi-class">cin</span>. A variety of classes that perform case-conversion and other manipulations in conjunction with NCBI streams and buffers are also available. See the [source](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/corelib/ncbistre.hpp) for details.

-   ***Use of the C++ STL (Standard Template Library) in the Toolkit*** The Toolkit employs the STL's set of template container classes, algorithms and iterators for managing collections of objects. Being standardized interfaces, coding with them provides portability. However, one drawback is the inability of STL containers to deal with reference objects, a problem area the Toolkit's [CRef and CObject classes](ch_core.html#ch_core.CRef) largely remedy.

-   ***Serializable Objects, the ASN.1 Data Types and*** <span class="nctnt ncbi-app">datatool</span> The [ASN.1 data model](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/SDKDOCS/DATAMODL.HTML) for biological data underlies all of the C and C++ Toolkit development at NCBI. The C++ Toolkit represents the ASN.1 data types as [serializable objects](ch_proj.html#ch_proj.ser_object), that is, objects able to save, restore, or transmit their state. This requires knowledge of an object's type and as such a <span class="nctnt ncbi-class">CTypeInfo</span> object is provided in each class to encapsulate [type information](ch_ser.html#ch_ser.typeinfo.html).<br/>Additionally, [object stream](ch_ser.html#ch_ser.objstream.html) classes (<span class="nctnt ncbi-class">CObject[IO]Stream</span>, and subclasses) have been designed specifically to perform data object serialization. The nuts-and-bolts of doing this has been documented on the [Processing Serial Data](ch_ser.html#ch_ser.asn.html) page, with additional information about the contents and parsing of ASN.1-derived objects in [Traversing a Data Structure](ch_ser.html#ch_ser.traverse.html).Each of the serializable objects appears in its own subdirectory under <span class="nctnt ncbi-path">[</span>[src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/objects)<span class="nctnt ncbi-path">\\| </span>[include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objects)<span class="nctnt ncbi-path">]/objects</span>. These <span class="nctnt ncbi-path">objects/\*</span> projects are configured differently from the rest of the Toolkit, in that header and source files are auto-generated from the ASN.1 specifications by the [datatool](ch_app.html#ch_app.datatool) program. The --with-objects flag to <span class="nctnt ncbi-app">configure</span> also directs a build of the [user classes](ch_proj.html#ch_proj.base_classes) for the serializable objects.

### Key Classes

For reference, we list some of the fundamental classes used in developing applications with the Toolkit. Some of these classes [are described elsewhere](ch_core.html#ch_core.writing_simple_app), but consult the library reference (Part 3 of this book) and the [source browser](ch_browse.html) for complete details.

-   [CNcbiApplication](ch_core.html#ch_core.writing_simple_app) (abstract class used to define the basic functionality and behavior of an NCBI application; **this application class effectively supersedes the C-style main() function**)

-   [CArgDescriptions, CArgs, and CArgValue](ch_core.html#ch_core.cmd_line_args) (command-line argument processing)

-   [CNcbiEnvironment](ch_core.html#ch_core.CNcbiEnvironment) (store, access, and modify environment variables)

-   [CNcbiRegistry](ch_core.html#ch_core.registry) (load, access, modify and store runtime information)

-   [CNcbiDiag](ch_core.html#ch_core.diag) (error handling for the Toolkit; )

-   [CObject](ch_core.html#ch_core.CObject) (base class for objects requiring a reference count)

-   [CRef](ch_core.html#ch_core.smart_ptrs) (a reference-counted smart pointer; particularly useful with STL and template classes)

-   [CObject[IO]Stream](ch_ser.html#ch_ser.objstream.html) (serialized data streams)

-   [CTypeInfo and CObjectTypeInfo](ch_ser.html#ch_ser.typeinfo.html) (Runtime Object Type Information; extensible to [user-defined types](ch_ser.html#ch_ser.usrtypeinfo.html))

-   [CObjectManager, etc.](ch_objmgr.html) (classes for working with biological sequence data)

-   [CCgiApplication, etc.](ch_cgi.html#ch_cgi.cg_develop_apps) (classes to create CGI and Fast-CGI applications and handle [CGI Diagnostics](ch_cgi.html#ch_cgi.cgi_diag.html))

-   [CNCBINode, etc.](ch_html.html#ch_html.webpgs.html) (classes representing HTML tags and Web page content)

-   [Iterator Classes](ch_ser.html#ch_ser.iterators.html) (easy traversal of collections and containers)

-   [Exception Handling](ch_debug.html#ch_debug.excep_cpp_toolkit) (classes, macros and tracing for exceptions)

### The Object Manager and <span class="nctnt ncbi-app">datatool</span>

The [datatool](ch_app.html#ch_app.datatool) processes the ASN.1 specifications in the <span class="nctnt ncbi-path">src/objects/directories</span> and is the C++ Toolkit's analogue of the C Toolkit's <span class="nctnt ncbi-app">asntool</span>. The goal of <span class="nctnt ncbi-app">datatool</span> is to generate the class definitions corresponding to each ASN.1 defined data entity, including all required [type information](ch_ser.html#ch_ser.typeinfo.html). As ASN.1 allows data to be selected from one of several types in a [choice element](ch_ser.html#ch_ser.choice.html), care must be taken to handle such cases.

The [Object Manager](ch_objmgr.html) is a C++ Toolkit library whose goal is to transparently download data from the GenBank database, investigate bio sequence data structure, and retrieve sequence data, descriptions and annotations. In the library are [classes](ch_objmgr.html#ch_objmgr.om_def.html) such as <span class="nctnt ncbi-class">CDataLoader</span> and <span class="nctnt ncbi-class">CDataSource</span> which manage global and local accesses to data, <span class="nctnt ncbi-class">CSeqVector</span> and <span class="nctnt ncbi-class">CSeqMap</span> objects to find and manipulate sequence data, a number of [specialized iterators](ch_objmgr.html#ch_objmgr.om_def.html_Iterators) to parse descriptions and annotations, among others. The <span class="nctnt ncbi-class">CObjectManager</span> and <span class="nctnt ncbi-class">CScope</span> classes provide the foundation of the library, managing data objects and coordinating their interactions.

[Jump-start](ch_objmgr.html#ch_objmgr.om_start.html) and [Object Manager FAQ](ch_objmgr.html#ch_objmgr.om_faq.html) are all available to help new users.

### Debugging and Diagnostic Aids

The Toolkit has a number of methods for catching, reporting and handling coding bugs and exceptional conditions. During development, a [debug mode](ch_debug.html#ch_debug.debug_mode_internal) exists to allow for assertions, traces and message posting. The standard C++ exception handling (which should be used as much as possible) has been extended by a pair of [NCBI exception classes](ch_debug.html#ch_debug.excep_cpp_toolkit), <span class="nctnt ncbi-class">CErrnoException</span> and <span class="nctnt ncbi-class">CParseException</span> and additional associated macros. [Diagnostics](ch_core.html#ch_core.diag), including an [ERR\_POST macro](ch_core.html#ch_core.ERR_POST) available for routine error posting, have been built into the Toolkit infrastructure.

For more detailed and extensive reporting of an object's state (including the states of any contained objects), a special [debug dump interface](ch_debug.html#ch_debug.debug_dump) has been implemented. All objects derived from the <span class="nctnt ncbi-class">CObject</span> class, which is in turn derived from the abstract base class [CDebugDumpable](ch_debug.html#ch_debug.debug_architecture), automatically have this capability.

### Coding Standards and Guidelines

All C++ source in the Toolkit has a [well-defined coding style](ch_style.html#ch_style.prog_style) which shall be used for new contributions and is highly encouraged for all user-developed code. Among these standards are

-   [variable naming conventions](ch_style.html#ch_style.naming_conv) (for types, constants, class members, etc.)

-   [using namespaces and the NCBI name scope](ch_style.html#ch_style.naming_prefix)

-   [code indentation](ch_style.html#ch_style.code_indentation) (4-space indentation, **no** tab symbols)

-   [declaring and defining classes and functions](ch_style.html#ch_style.class_decl)

Noteworthy Files
----------------

A list of important files is given in [Table 1](#table-1).

Table 1. Noteworthy Files

| Filename (relative to $NCBI/c++)                                                                                                                                                                                                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-app">compilers/\*/\<compiler\_name\>.sh</span>                                                                                                                                                                                             | Use the <span class="nctnt ncbi-app">configure</span> shell script, or one of its [compiler-specific wrappers](ch_config.html#ch_config.Special_Consideratio), to fully configure and install all files required to build the Toolkit.                                                                                                                                                                                                                                                                                                                                                 |
| <span class="nctnt ncbi-app">import\_project</span>                                                                                                                                                                                                                | Import only an existing Toolkit project into an independent subtree of your current Toolkit source tree. (Requires a SVN source repository.)                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| <span class="nctnt ncbi-app">update\_{core\\|projects}</span>                                                                                                                                                                                                      | Update your local copy of either the [core](#core) Toolkit or set of specified projects. (Requires a SVN source repository.)                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| <span class="nctnt ncbi-app">new\_project</span>                                                                                                                                                                                                                   | Set up a new project outside of the NCBI C++ Toolkit tree to access pre-built version of the Toolkit libraries. Sample code can be requested to serve as a template for the new module.                                                                                                                                                                                                                                                                                                                                                                                                |
| <span class="nctnt ncbi-path">src/\<project\_dir\>/Makefile.in</span><br/><span class="nctnt ncbi-path">src/\<project\_dir\>/</span><br/><span class="nctnt ncbi-path"> Makefile.\<project\>.{app, lib}</span> | Customized <span class="nctnt ncbi-monospace">meta-makefile</span> template and the corresponding datafile to provide project-specific source dependencies, libraries, compiler flags, etc. This information is accessed by <span class="nctnt ncbi-app">configure</span> to build a projects's <span class="nctnt ncbi-monospace">meta-makefile</span> (see below).                                                                                                                                                                                                                   |
| <span class="nctnt ncbi-path">doc/framewrk.{cpp\\|hpp}</span>                                                                                                                                                                                                      | Basic templates for source and header files that can be used when starting a new module. Includes common headers, the NCBI disclaimer and SVN keywords in a standard way.                                                                                                                                                                                                                                                                                                                                                                                                              |
| <span class="nctnt ncbi-path">CHECKOUT\_STATUS</span>                                                                                                                                                                                                              | This file summarizes the local source tree structure that was obtained when using one of the shell scripts in <span class="nctnt ncbi-path">scripts</span>. (Requires a SVN source repository.)                                                                                                                                                                                                                                                                                                                                                                                        |
| **Build-specific Files (relative to $NCBI/c++/\<builddir\>)**                                                                                                                                                                                                      | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| <span class="nctnt ncbi-path">Makefile</span><br/><span class="nctnt ncbi-path">Makefile.mk</span><br/><span class="nctnt ncbi-path">Makefile.meta</span>                                                      | These are the primary <span class="nctnt ncbi-monospace">makefiles</span> used to build the entire Toolkit (when used recursively). They are customized for a specific build from the corresponding <span class="nctnt ncbi-path">\*.in</span> templates in <span class="nctnt ncbi-path">$NCBI/c++/src</span>. <span class="nctnt ncbi-path">Makefile</span> is the master, top-level file, <span class="nctnt ncbi-path">Makefile.mk</span> sets many make and shell variables and <span class="nctnt ncbi-path">Makefile.meta</span> is where most of the make targets are defined. |
| <span class="nctnt ncbi-path">\<project\_dir\>/Makefile</span><br/><span class="nctnt ncbi-path">\<project\_dir\>/</span><br/><span class="nctnt ncbi-path"> Makefile.\<project\>\_{app, lib}</span>           | Project-specific custom <span class="nctnt ncbi-monospace">meta-makefile</span> and <span class="nctnt ncbi-monospace">makefiles</span>, respectively, configured from templates in the <span class="nctnt ncbi-path">src/</span> hierarchy and any pertinent <span class="nctnt ncbi-path">src/\<project\_dir\>/Makefile.\<project\>.{app, lib} files (see REF TO OLD ANCHOR: get\_started.html\_ref\_TmplMetaMake\<secref rid="get\_started.html\_ref\_ImptFiles"\>above\</secref\>).</span>                                                                                         |
| <span class="nctnt ncbi-path">inc/ncbiconf.h</span>                                                                                                                                                                                                                | Header that <span class="nctnt ncbi-code">\#define</span>'s many of the build-specific constants required by the Toolkit. This file is auto-generated by the <span class="nctnt ncbi-app">configure</span> script, and some pre-built versions do exist in <span class="nctnt ncbi-path">compilers</span>.                                                                                                                                                                                                                                                                             |
| <span class="nctnt ncbi-app">reconfigure.sh</span>                                                                                                                                                                                                                 | Update the build tree due to changes in or the addition of configurable files (\*.in files, such as <span class="nctnt ncbi-path">Makefile.in</span> or the <span class="nctnt ncbi-monospace">meta-makefiles</span>) to the source tree.                                                                                                                                                                                                                                                                                                                                              |
| <span class="nctnt ncbi-app">relocate.sh</span>                                                                                                                                                                                                                    | Adjust paths to this build tree and the relevant source tree.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| <span class="nctnt ncbi-path">corelib/ncbicfg.c</span>                                                                                                                                                                                                             | Define and manage the runtime path settings. This file is auto-generated by the <span class="nctnt ncbi-app">configure</span> script.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| <span class="nctnt ncbi-path">status/config.{cache\\|log\\|status}</span>                                                                                                                                                                                          | These files provide information on <span class="nctnt ncbi-app">configure</span>'s construction of the build tree, and the cache of build settings to expedite future changes.                                                                                                                                                                                                                                                                                                                                                                                                         |


