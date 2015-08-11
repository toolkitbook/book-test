---
layout: default
title: C++ Toolkit test
nav: pages/ch_getcode_svn
---

<span class="label">3</span>Retrieve the Source Code (FTP and Subversion)
=========================================================================

Created: April 1, 2003; Last Update: October 1, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

The first step in working with the C++ Toolkit is getting the source code, which can be either downloaded from anonymous FTP or checked out from a Subversion repository. This chapter describes both methods and the use of utility scripts that can help getting only the necessary source code components.

If you are interested in downloading source code from the C Toolkit instead of the C++ Toolkit, please see [Access to the C Toolkit source tree Using CVS](ch_res.html#ch_res.Access_to_the_C_Tool).

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Public Access to the Source Code via FTP](#public-access-to-the-source-code-via-ftp)

-   [Read-Only Access to the Source Code via Subversion](#read-only-access-to-the-source-code-via-subversion)

-   [Read-Write Access to the Source Code via Subversion (NCBI only)](#read-write-access-to-the-source-code-via-subversion-ncbi-only)

    -   [NCBI Source Tree Contents](#ncbi-source-tree-contents)

    -   [Source Code Retrieval under Unix](#source-code-retrieval-under-unix)

        -   [Retrieval of the C++ Toolkit Source Code Tree](#retrieval-of-the-c++-toolkit-source-code-tree)

            -   [Checking Out the Development NCBI C++ Toolkit Source Tree](#checking-out-the-development-ncbi-c++-toolkit-source-tree)

            -   [Checking Out the Production NCBI C++ Toolkit Source Tree](#checking-out-the-production-ncbi-c++-toolkit-source-tree)

            -   [svn\_core: Retrieving core components](#svncore-retrieving-core-components)

            -   [import\_project: Retrieve Source for an Existing Project](#importproject-retrieve-source-for-an-existing-project)

            -   [update\_core: Update the Portable and Core Components](#updatecore-update-the-portable-and-core-components)

            -   [update\_projects: Check out and Update Sources of Selected Projects](#updateprojects-check-out-and-update-sources-of-selected-projects)

    -   [Source Code Retrieval under MS Windows](#source-code-retrieval-under-ms-windows)

    -   [Source Code Retrieval under Mac OS X](#source-code-retrieval-under-mac-os-x)

-   [Source Tree Structure Summary](#source-tree-structure-summary)

Public Access to the Source Code via FTP
----------------------------------------

-   [FTP Download Now](ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/CURRENT)

-   **Available FTP Archives**: Select the archive for your system. When the dialog box appears, choose the destination in your file system for the downloaded archive. <span class="nctnt highlight">Note:</span> With some browsers, you may need to right-click-and-hold with your mouse and use the <span class="nctnt ncbi-monospace">'Save Link As...', 'Copy to Folder...'</span>, or similar options from the drop-down menu to properly save the archive. For a current list of the source code archives for different operating system/compiler combinations consult the current Release Notes available at <ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/CURRENT/RELEASE_NOTES.html>

-   **Unpack the Source Archive**

    -   *Unix and Macintosh Systems*<br/>The Unix distributions have been archived using the standard <span class="nctnt ncbi-app">tar</span> command and compressed using <span class="nctnt ncbi-app">gzip</span>. When unpacked, all files will be under the directory <span class="nctnt ncbi-path">ncbi\_cxx--\<version\_number\></span>, which will be created in the current directory. (<span class="nctnt highlight">Caution:</span> If <span class="nctnt ncbi-path">ncbi\_cxx--\<version\_number\></span> already exists, <span class="nctnt ncbi-app">tar</span> extraction will overwrite existing files.) To unpack the archive: <span class="nctnt ncbi-cmd">gunzip -c ncbi\_cxx--\*.tar.gz \\| tar xvf -</span>

    -   *Windows Systems*<br/>The Microsoft Windows versions of the source distribution have been prepared as self-extracting executables. By default a sub-folder <span class="nctnt ncbi-path">ncbi\_cxx--\<version\_number \></span> will be created in the current folder to contain the extracted source. If <span class="nctnt ncbi-path">ncbi\_cxx--\<version\_number \></span> already exists in the folder where the executable is launched, user confirmation is required before files are overwritten. To actually perform the extraction, do one of the following:

        -   Run the executable from a command shell. This will create the sub-folder in the shell's current directory, even if the executable is located somewhere else.

        -   Double-click on the archive's icon to create <span class="nctnt ncbi-path">ncbi\_cxx--\<version\_number \></span> in the current folder.

        -   Right-click on the archive's icon, and select <span class="nctnt ncbi-monospace">'Extract to...'</span> to unpack the archive to a user-specified location in the filesystem.

Read-Only Access to the Source Code via Subversion
--------------------------------------------------

The following options for read-only access to the C++ Toolkit Subversion repository are available to the public:

-   Checking out the source tree directly from the repository (e.g. svn co <http://anonsvn.ncbi.nlm.nih.gov/repos/v1/trunk/c++>).

-   Browsing the repository with an HTTP browser (e.g. <http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++>).

-   Accessing the repository with a WebDAV client (also using <http://anonsvn.ncbi.nlm.nih.gov/repos/v1/trunk/c++> – although some clients may require dav:// instead of http://).

Read-Write Access to the Source Code via Subversion (NCBI only)
---------------------------------------------------------------

<span class="nctnt highlight">Note:</span> This section discusses read-write access to the Subversion repository, which is only available to users inside NCBI. For public access, see the section on [read-only access](#read-only-access).

Subversion client installation and usage instructions are available on separate pages for [UNIX](#unix), [MS Windows](#ms-windows), and [Mac OS](#mac-os) systems.

For a detailed description of the Subversion Version Control System please download the book ["Version Control with Subversion"](http://svnbook.red-bean.com/) or run the command <span class="nctnt ncbi-cmd">svn help</span> on your workstation for quick reference.

The following is an outline of the topics presented in this section. Select the instructions appropriate for your **development** environment.

-   [NCBI Source Tree Contents](#ncbi-source-tree-contents)

-   [Source Code Retrieval under Unix](#source-code-retrieval-under-unix)

    -   [Retrieval of the C++ Toolkit Source Tree](#retrieval-of-the-c++-toolkit-source-tree)

        -   [Checking Out the Development NCBI C++ Toolkit Source Tree](#checking-out-the-development-ncbi-c++-toolkit-source-tree)

        -   [Checking Out the Production NCBI C++ Toolkit Source Tree](#checking-out-the-production-ncbi-c++-toolkit-source-tree)

        -   [svn\_core: Retrieving core components](#svncore-retrieving-core-components)

        -   [import\_project: Retrieve Source for an Existing Project](#importproject-retrieve-source-for-an-existing-project)

        -   [update\_core: Update the Portable and Core Components](#updatecore-update-the-portable-and-core-components)

        -   [update\_projects: Check out and Update Sources of Selected Projects](#updateprojects-check-out-and-update-sources-of-selected-projects)

-   [Source Code Retrieval under MS Windows](#source-code-retrieval-under-ms-windows)

-   [Source Code Retrieval under Mac OS X](#source-code-retrieval-under-mac-os-x)

### NCBI Source Tree Contents

The NCBI C++ Toolkit Subversion repository contains all source code, scripts, utilities, tools, tests and documentation required to build the Toolkit on the major classes of operating systems (<span class="nctnt ncbi-monospace">Unix</span>, <span class="nctnt ncbi-monospace">MS Windows</span> and <span class="nctnt ncbi-monospace">Mac OS</span>).

### Source Code Retrieval under Unix

#### Retrieval of the C++ Toolkit Source Code Tree

This section discusses the methods of checking out the entire source tree or just the necessary portions. An important point to note is that the entire NCBI C++ tree is very big because it contains a lot of internal projects. There are also numerous platform-specific files, and even platform-specific sub-trees, which you will never need unless you work on those platforms. Therefore it is frequently sufficient, and in fact, usually advisable, to retrieve only the files of interest using shell scripts. The relevant scripts are located in <span class="nctnt ncbi-path">/am/ncbiapdata/bin</span>, but the best way to get them into your <span class="nctnt ncbi-var">PATH</span> is to make sure you have <span class="nctnt ncbi-monospace">developer</span> in the <span class="nctnt ncbi-monospace">facilities</span> line of your <span class="nctnt ncbi-path">~/.ncbi\_hints</span> file.

The following sections discuss the checkout process in more detail:

-   [Checking Out the Development NCBI C++ Toolkit Source Tree](#checking-out-the-development-ncbi-c++-toolkit-source-tree)

-   [Checking Out the Production NCBI C++ Toolkit Source Tree](#checking-out-the-production-ncbi-c++-toolkit-source-tree)

-   [svn\_toolkit\_tree: Quickly checking out the whole Toolkit source tree](#svntoolkittree-quickly-checking-out-the-whole-toolkit-source-tree-)

-   [svn\_core: Retrieving core components](#svncore-retrieving-core-components)

-   [import\_project: Retrieve Source for an Existing Project](#importproject-retrieve-source-for-an-existing-project)

-   [update\_core: Update the Portable and Core Components](#updatecore-update-the-portable-and-core-components)

-   [update\_projects: Check out and update Source of Selected Projects](#updateprojects-check-out-and-update-source-of-selected-projects)

<span class="nctnt highlight">Note:</span> To facilitate the creation of a new project, use the script [new\_project](ch_proj.html#ch_proj.new_proj_struct) which generates new directories and makefiles for the new project from templates, but does not involve checking out files.

##### Checking Out the Development NCBI C++ Toolkit Source Tree

You can check out the entire development NCBI C++ source tree from the repository to your local directory (e.g., <span class="nctnt ncbi-path">foo/c++/</span>) just by running:

    cd foo
    svn checkout https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/c++

For internal projects use:

    cd foo
    svn checkout https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/internal/c++

<span class="nctnt highlight">Caution: </span>Be aware that sources checked out through the development source tree have the latest sources and are different from the public release that is done at periodic intervals. These sources are relatively unstable "development" sources, so they are not guaranteed to work properly or even compile. Use these sources at your own risk (and/or to apply patches to stable releases).The sources are usually better by the end of day and especially by the end of the week (like Sunday evening).

##### Checking Out the Production NCBI C++ Toolkit Source Tree

Besides the development NCBI C++ source tree, there is the C++ Toolkit "production" source tree that has been added to the public Subversion repository. This tree contains stable snapshots of the "development" C++ Toolkit tree. Please note that these sources are lagging behind, sometimes months behind the current snapshot of the sources.

You can check out the entire "production" NCBI C++ source tree from the public repository to your local directory by running:

    svn co https://svn.ncbi.nlm.nih.gov/repos/toolkit/production/latest/c++

This repository path corresponds to the latest production build of the Toolkit. If you want to check out sources for an older production build, please specify the exact date of that build, for example:

    svn co https://svn.ncbi.nlm.nih.gov/repos/toolkit/production/20031212/c++

where <span class="nctnt ncbi-monospace">20031212</span> is the date when this specific build was originated. You can easily find out the available production builds by running

    svn ls https://svn.ncbi.nlm.nih.gov/repos/toolkit/production

This command will print directories under <span class="nctnt ncbi-path">production/</span>, which correspond to the production builds.

##### <span class="nctnt ncbi-app">svn\_toolkit\_tree</span>: Quickly checking out the whole Toolkit source tree

Usage:

    svn_toolkit_tree <archive> <new_dir>

Checking out the whole Toolkit source tree using a Subversion client can take 15 minutes or more. However, the script <span class="nctnt ncbi-app">svn\_toolkit\_tree</span> produces the same result in under a minute. The <span class="nctnt ncbi-app">svn\_toolkit\_tree</span> script combines a daily archive with an update of the working copy to bring it up-to-date. This produces the same set of files and revisions as running <span class="nctnt ncbi-cmd">svn checkout</span>, but in much less time. Besides speed, the differences between using a Subversion client and the <span class="nctnt ncbi-app">svn\_toolkit\_tree</span> script include:

-   The <span class="nctnt ncbi-app">svn\_toolkit\_tree</span> script may not be compatible with your Subversion client. If your client is older than the version used to create the archive, you may not be able to access the archive.

-   The <span class="nctnt ncbi-app">svn\_toolkit\_tree</span> script will not accept the name of an existing directory.

The archives that were available at the time of this writing (October 2014) were:

| Archive                                                 | Corresponding C++ Toolkit tree                                                       |
|---------------------------------------------------------|--------------------------------------------------------------------------------------|
| production                                              | https://svn.ncbi.nlm.nih.gov/repos/toolkit/production/candidates/production.HEAD/c++ |
| trial                                                   | https://svn.ncbi.nlm.nih.gov/repos/toolkit/production/candidates/trial/c++           |
| trunk                                                   | https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/internal/c++                        |
| trunk-core<br/>(or just core) | https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/c++                                 |

Run the script with no arguments to find the most up-to-date list of supported archives.

For example, to retrieve the current TRUNK version of the "core" part of the C++ Toolkit tree (the part without the GUI and INTERNAL projects), run:

    $ svn_toolkit_tree core cpp
    /net/snowman/vol/projects/ncbisoft/toolkit_trees/trunk-core.tar.gz -> cpp
    Updating cpp...

    $ ls cpp
    compilers  configure  doc  include  scripts  src

##### <span class="nctnt ncbi-app">svn\_core</span>: Retrieving core components

Usage:

    svn_core <dir> <branch> [--export] ... [--<platform>]

The NCBI C++ Toolkit has many features and extensions beyond the core of portable functionality. However, one often wants to obtain a set of core sources that is free of non-portable elements, and the <span class="nctnt ncbi-app">svn\_core</span> script performs this task across the range of supported platforms. Options to the basic command allow the developer to further tailor the retrieved sources by including (or excluding) certain portions of the Toolkit.

For usage help, run <span class="nctnt ncbi-app">svn\_core</span> without arguments.

<span class="nctnt highlight">Note: </span><span class="nctnt ncbi-app">svn\_core</span> is not available on Windows.

[Table 1](#table-1) describes the arguments of <span class="nctnt ncbi-app">svn\_core</span>. Only the target directory and SVN branch arguments are mandatory.

Table 1. <span class="nctnt ncbi-app">svn\_core</span> Arguments

| Argument                                               | Description                                                                                                                                                                                                                                                                                                                                                                                                                              | Permitted Values                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|--------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-cmd">\<dir\></span>            | Path to where the source tree will be checked out. This argument is **required**.                                                                                                                                                                                                                                                                                                                                                        | A valid writable directory name (must not exist already); name cannot start with "-".                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| <span class="nctnt ncbi-cmd">\<branch\></span>         | Which branch of the source tree to check out. This argument is **required**.                                                                                                                                                                                                                                                                                                                                                             | <span class="nctnt ncbi-cmd">core</span> - <span class="nctnt ncbi-path">toolkit/trunk/c++</span><br/><span class="nctnt ncbi-cmd">development</span> - <span class="nctnt ncbi-path">toolkit/trunk/internal/c++</span><br/><span class="nctnt ncbi-cmd">production</span> - <span class="nctnt ncbi-path">toolkit/production/candidates/trial/c++</span><br/><span class="nctnt ncbi-cmd">prod-head</span> - <span class="nctnt ncbi-path">toolkit/production/candidates/production.HEAD/c++</span><br/><span class="nctnt ncbi-cmd">frozen-head</span> - <span class="nctnt ncbi-path">toolkit/production/candidates/frozen.HEAD/c++</span><br/><span class="nctnt ncbi-cmd">trial</span> - <span class="nctnt ncbi-path">toolkit/production/candidates/trial/c++</span><br/><span class="nctnt ncbi-cmd">release</span> - <span class="nctnt ncbi-path">toolkit/release/public/current/c++</span><br/><span class="nctnt ncbi-cmd">gbench</span> - <span class="nctnt ncbi-path">gbench/branches/1.1</span><br/><span class="nctnt ncbi-cmd">gbench2</span> - <span class="nctnt ncbi-path">gbench/trunk</span><br/>(See [c++-branches.txt](https://svn.ncbi.nlm.nih.gov/viewvc/toolkit/trunk/internal/scripts/build/c%2B%2B-branches.txt?view=markup) for an up-to-date list of branches.) |
| <span class="nctnt ncbi-cmd">--date </span>            | Check out as at the start of the specified timestamp. If the <span class="nctnt ncbi-cmd">--date</span> flag is missing, today’s date and current time are used.                                                                                                                                                                                                                                                                         | A date in a format acceptable to the <span class="nctnt ncbi-cmd">svn -r</span> argument, for example <span class="nctnt ncbi-cmd">--date="2013-03-29 19:49:48 +0000"</span>. (Do not include curly braces and quote the timestamp if it contains spaces.) See the [Revision Dates](http://svnbook.red-bean.com/en/1.6/svn.tour.revs.specifiers.html#svn.tour.revs.dates) section in the Subversion manual for details.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| <span class="nctnt ncbi-cmd">--export</span>           | Get a clean source tree without <span class="nctnt ncbi-path">.svn</span> directories.                                                                                                                                                                                                                                                                                                                                                   | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--\<platform\></span>     | Obtain sources for the specified platform(s).                                                                                                                                                                                                                                                                                                                                                                                            | <span class="nctnt ncbi-cmd">--unix</span> - Unix systems;<br/><span class="nctnt ncbi-cmd">--msvc</span> - Microsoft Visual C++ environment;<br/><span class="nctnt ncbi-cmd">--mac</span> - Macintosh systems;<br/><span class="nctnt ncbi-cmd">--cygwin</span> - Cygwin UNIX environment for Windows;<br/><span class="nctnt ncbi-cmd">--all</span> - all platforms.<br/>If no value is supplied, <span class="nctnt ncbi-cmd">--all</span> is used.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| <span class="nctnt ncbi-cmd">--with-ctools</span>      | Check out core projects responsible for working together with the NCBI C Toolkit (the <span class="nctnt ncbi-path">ctools</span> directory). This option is effective by default unless <span class="nctnt ncbi-cmd">--without-ctools</span> is used.                                                                                                                                                                                   | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--without-ctools</span>   | Do not check out core projects responsible for working together with the NCBI C Toolkit (the <span class="nctnt ncbi-path">ctools</span> directory).                                                                                                                                                                                                                                                                                     | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--with-gui</span>         | Check out core projects responsible for providing cross-platform graphic user interface capability (the <span class="nctnt ncbi-path">gui</span> directory). This option is effective by default unless <span class="nctnt ncbi-cmd">--without-gui</span> is used.                                                                                                                                                                       | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--without-gui</span>      | No not check out core projects responsible for providing cross-platform graphic user interface capability (the <span class="nctnt ncbi-path">gui</span> directory).                                                                                                                                                                                                                                                                      | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--with-internal</span>    | Check out a selection of NCBI-internal core projects. See [Table 4](#table-4) for a detailed list of affected directories.                                                                                                                                                                                                                                                                                                               | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--without-internal</span> | Do not check out NCBI-internal core projects.                                                                                                                                                                                                                                                                                                                                                                                            | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--with-objects</span>     | Check out the <span class="nctnt ncbi-path">objects</span>, <span class="nctnt ncbi-path">objmgr</span>, and <span class="nctnt ncbi-path">objtools</span> directories and generate serialization code from the ASN.1 specifications. If this flag is not present, those directories are still checked out (unless overridden by the <span class="nctnt ncbi-cmd">--without-objects</span> flag) but no serialization code is generated. | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| <span class="nctnt ncbi-cmd">--without-objects</span>  | Do not check out the <span class="nctnt ncbi-path">objects</span>, <span class="nctnt ncbi-path">objmgr</span>, and <span class="nctnt ncbi-path">objtools</span> directories or generate ASN.1 serialization code. (On Unix platforms the code generation can be done later, during the build.)                                                                                                                                         | n/a                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |

Some directories are always checked out, regardless of command-line arguments. These are shown in [Table 2](#table-2). (All paths are relative to the repository path <span class="nctnt ncbi-path">https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/c++</span>.)

Table 2. List of the directories that are always checked out

| Checked out directories                                               | Recursive?                              |
|-----------------------------------------------------------------------|-----------------------------------------|
| <span class="nctnt ncbi-path">(include\\|src)</span>                  | <span class="nctnt highlight">no</span> |
| <span class="nctnt ncbi-path">(include\\|src)/algo</span>             | yes                                     |
| <span class="nctnt ncbi-path">src/app</span>                          | yes                                     |
| <span class="nctnt ncbi-path">src/build-system</span>                 | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/cgi</span>              | yes                                     |
| <span class="nctnt ncbi-path">include/common</span>                   | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/connect</span>          | <span class="nctnt highlight">no</span> |
| <span class="nctnt ncbi-path">(include\\|src)/connect/ext</span>      | yes                                     |
| <span class="nctnt ncbi-path">include/connect/impl</span>             | yes                                     |
| <span class="nctnt ncbi-path">src/connect/test</span>                 | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/connect/services</span> | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/corelib</span>          | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/db</span>               | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/dbapi</span>            | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/html</span>             | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/misc</span>             | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/sample</span>           | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/serial</span>           | yes                                     |
| <span class="nctnt ncbi-path">include/test</span>                     | yes                                     |
| <span class="nctnt ncbi-path">(include\\|src)/util</span>             | yes                                     |
| <span class="nctnt ncbi-path">scripts</span>                          | yes                                     |

Other directories may or may not be checked out, depending on the <span class="nctnt ncbi-cmd">\<branch\></span> and <span class="nctnt ncbi-cmd">\<platform\></span> options. These are shown in [Table 3](#table-3).

Table 3. Directories that may be checked out depending on branch and platform options

| Checked out directories                                      | Recursive?                              | Options                                                                  |
|--------------------------------------------------------------|-----------------------------------------|--------------------------------------------------------------------------|
| <span class="nctnt ncbi-path">compilers</span>               | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = all                   |
| <span class="nctnt ncbi-path">compilers</span>               | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">\<platform\></span> != all                  |
| <span class="nctnt ncbi-path">compilers/cygwin</span>        | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = cygwin                |
| <span class="nctnt ncbi-path">compilers/msvc1000\_prj</span> | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = msvc                  |
| <span class="nctnt ncbi-path">compilers/unix</span>          | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = cygwin or mac or unix |
| <span class="nctnt ncbi-path">compilers/xCode</span>         | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = max                   |
| <span class="nctnt ncbi-path">compilers/xcode90\_prj</span>  | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = mac                   |
| <span class="nctnt ncbi-path">doc</span>                     | yes                                     | <span class="nctnt ncbi-cmd">\<branch\></span> = development             |
| <span class="nctnt ncbi-path">include/connect/daemons</span> | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = all or unix           |
| <span class="nctnt ncbi-path">src/check</span>               | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> != mac                  |
| <span class="nctnt ncbi-path">src/connect/daemons</span>     | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = all or unix           |
| <span class="nctnt ncbi-path">src/connect/mitsock</span>     | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = mac                   |
| <span class="nctnt ncbi-path">src/dll</span>                 | yes                                     | <span class="nctnt ncbi-cmd">\<platform\></span> = all or mac or msvc    |

Still other directories may be checked out depending on the <span class="nctnt ncbi-cmd">--with/--without-\<feature\></span> options. These are shown in [Table 4](#table-4).

Table 4. Directories that may be checked out depending on --with/--without options

| Checked out directories                                                              | Recursive?                              | Options                                                                                                         |
|--------------------------------------------------------------------------------------|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-path">(include\\|src)/ctools</span>                          | yes                                     | <span class="nctnt ncbi-cmd">--with-ctools</span> or not <span class="nctnt ncbi-cmd">--without-ctools</span>   |
| <span class="nctnt ncbi-path">(include\\|src)/gui</span>                             | yes                                     | <span class="nctnt ncbi-cmd">--with-gui</span> or not <span class="nctnt ncbi-cmd">--without-gui</span>         |
| <span class="nctnt ncbi-path">(include\\|src)/internal</span>                        | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/algo</span>                   | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/algo/id\_mapper</span>        | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/align\_model</span>           | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">include/internal/asn\_cache</span>                     | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">src/internal/asn\_cache</span>                         | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">src/internal/asn\_cache/lib</span>                     | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/blast</span>                  | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/blast/DistribDbSupport</span> | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/contigdb</span>               | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">src/internal/demo</span>                               | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/ID</span>                     | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/ID/utils</span>               | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/mapview</span>                | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/mapview/objects</span>        | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/mapview/util</span>           | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/myncbi</span>                 | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">include/internal/objects</span>                        | <span class="nctnt highlight">no</span> | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/objects</span>                         | yes                                     | <span class="nctnt ncbi-cmd">--with-objects</span> or not <span class="nctnt ncbi-cmd">--without-objects</span> |
| <span class="nctnt ncbi-path">(include\\|src)/objmgr</span>                          | yes                                     | <span class="nctnt ncbi-cmd">--with-objects</span> or not <span class="nctnt ncbi-cmd">--without-objects</span> |
| <span class="nctnt ncbi-path">(include\\|src)/objtools</span>                        | yes                                     | <span class="nctnt ncbi-cmd">--with-objects</span> or not <span class="nctnt ncbi-cmd">--without-objects</span> |
| <span class="nctnt ncbi-path">src/internal/objects</span>                            | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/sra</span>                    | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">src/internal/test</span>                               | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/txclient</span>               | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/txserver</span>               | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |
| <span class="nctnt ncbi-path">(include\\|src)/internal/txxmldoc</span>               | yes                                     | <span class="nctnt ncbi-cmd">--with-internal</span>                                                             |

##### <span class="nctnt ncbi-app">import\_project</span>: Retrieve Source for an Existing Project

Usage:

    import_project [options] <SVN_relative_tree_path> [builddir]

The <span class="nctnt ncbi-app">import\_project</span> script imports the source from a single project, and configures the resulting tree.

In many cases, you work on your own project which is a part of the NCBI C++ tree, and you do not want to check out, update and rebuild the entire NCBI C++ tree. Instead, you just want to use headers and libraries of the pre-built NCBI C++ Toolkit to build your project.

The shell script <span class="nctnt ncbi-app">import\_project</span> will check out your project's <span class="nctnt ncbi-path">src</span> and <span class="nctnt ncbi-path">include</span> directories from the repository and create temporary makefiles based on the project's [customized makefiles](ch_start.html#ch_start.make_templates). The new makefiles will also contain a reference to the pre-built NCBI C++ Toolkit.

For example:

    import_project serial/datatool

will check out the <span class="nctnt ncbi-path">datatool</span> project from the NCBI C++ tree (<span class="nctnt ncbi-path">trunk/c++/{src,include}/serial/datatool/</span>), and create a makefile <span class="nctnt ncbi-path">Makefile.datatool\_app</span> that uses the project's customized makefile <span class="nctnt ncbi-path">Makefile.datatool.app</span>. Now you can just go to the created working directory <span class="nctnt ncbi-path">c++/src/serial/datatool/</span> and build the application <span class="nctnt ncbi-app">datatool</span> using:

    make -f Makefile.datatool_app

##### <span class="nctnt ncbi-app">update\_core</span>: Update the Portable and Core Components

Usage:

    update_core [--no-projects] [<dirs>]

Once you have obtained the core C++ Toolkit sources, with <span class="nctnt ncbi-app">svn\_core</span> or otherwise, the local copies will become out of sync with the master SVN repository contents when other developers commit their changes. <span class="nctnt ncbi-app">update\_core</span> will update your local core source tree with any changed files without the side-effect of simultaneously checking out non-core portions of the tree. Subdirectory \*<span class="nctnt ncbi-path">/internal</span> does not get updated by this script.

The <span class="nctnt ncbi-cmd">--no-projects</span> switch excludes any <span class="nctnt ncbi-monospace">Windows</span> or <span class="nctnt ncbi-monospace">MacOS</span> project files from the update. Specifically, those subdirectory names of the form <span class="nctnt ncbi-path">\*\_prj</span> are skipped during the update when this flag is set.

The list <span class="nctnt ncbi-cmd">[\<dirs\>]</span>, when present, identifies the set of directories relative to the current directory to update. The default list of updated directories is:

-   <span class="nctnt ncbi-path">.</span>

-   <span class="nctnt ncbi-path">compilers</span>

-   <span class="nctnt ncbi-path">doc</span>

-   <span class="nctnt ncbi-path">include</span>

-   <span class="nctnt ncbi-path">scripts</span>

-   <span class="nctnt ncbi-path">src</span>

Note that the default list is not pushed onto a user-supplied list of directories.

##### <span class="nctnt ncbi-app">update\_projects</span>: Check out and update Source of Selected Projects

Usage:

    update_projects <project-list> [<directory>]

Script <span class="nctnt ncbi-app">update\_projects</span> facilitates the original retrieval and subsequent updates of selected parts of the Toolkit tree. Because the source code and makefiles are distributed over more than one subdirectory under repository path <span class="nctnt ncbi-path">trunk/c++</span>, this script assembles the set of required files and places them in your local C++ source tree.

The projects to be retrieved (or updated) must be specified in the command line as the <span class="nctnt ncbi-cmd">\<project-list\></span> parameter. Its value can be either of the following:

-   Explicit specification of the pathname of the project listing file. This project listing file can contain project directory names as well as references to other project listings and must be formatted according to the simple [syntax used by the configure script](ch_config.html#ch_config.ch_configwith_projec).

-   Specify one of the standard project names. Standard projects are those whose project listing files are located in one of the system directories, which are <span class="nctnt ncbi-path">trunk/c++/scripts/projects</span> and <span class="nctnt ncbi-path">trunk/c++/scripts/internal/projects</span>. When a project name is specified on the command line, the “.lst” extension is added to it and the resulting file name is searched for in the above mentioned system directories.

The parameter to <span class="nctnt ncbi-app">update\_projects</span> indicates the target directory where the sources will be checked out to and where the project will be configured and built. This parameter is optional and is set to the current directory by default.

### Source Code Retrieval under MS Windows

1  
In NCBI, the SVN clients must be set up and ready to use. Ask Systems if you don’t have the client installed on your workstation. If you are working outside of NCBI, then you can download the latest version of Subversion from <http://subversion.tigris.org/servlets/ProjectDocumentList?folderID=91>. Run the Subversion installer and follow the instructions. The latest version may not come with an executable installer though. In this case, please unpack the <span class="nctnt ncbi-type">zip</span> archive with the latest Subversion binaries to a local directory, for example <span class="nctnt ncbi-path">C:\\Program Files\\svn-win32-1.4.2</span>. Change the <span class="nctnt ncbi-var">PATH</span> environment variable so that it points to the <span class="nctnt ncbi-path">bin</span> subdirectory under your Subversion installation directory, for example <span class="nctnt ncbi-monospace">set PATH=%PATH%;C:\\Program Files\\svn-win32-1.4.2\\bin</span>

2  
Start your favorite command shell. Change current directory to the designated working directory. At the command prompt, type:<span class="nctnt ncbi-cmd">svn co https://svn.ncbi.nlm.nih.gov/repos/toolkit/trunk/c++</span>

3  
Modify source files as required. Refer to [Svnbook](http://svnbook.red-bean.com) for the documentation on particular Subversion commands. Monitor your changes using <span class="nctnt ncbi-cmd">svn diff</span>, synchronize your working copy with the trunk using <span class="nctnt ncbi-cmd">svn update</span>, and finally commit them using <span class="nctnt ncbi-cmd">svn commit</span>.

The rest should be the same as when using Subversion under UNIX systems. See [Source Code Retrieval under Unix](#source-code-retrieval-under-unix).

### Source Code Retrieval under Mac OS X

Download and install the latest Subversion binaries for MacOSX from [http://subversion.tigris.org/](http://subversion.tigris.org).

The rest should be the same as when using Subversion under UNIX systems. See [Source Code Retrieval under Unix](#source-code-retrieval-under-unix).

Source Tree Structure Summary
-----------------------------

To summarize the [Getting Started](ch_start.html#ch_start.source_organization) page, the [source tree](ch_start.html#ch_start.F1) is organized as follows:

-   The top-level has configuration files and the directories <span class="nctnt ncbi-path">include/, src/, scripts/, compilers/</span> and <span class="nctnt ncbi-path">doc/</span>

-   The <span class="nctnt ncbi-path">src</span> and <span class="nctnt ncbi-path">include</span> directories contain "projects" as subdirectories. Projects may contain sub-projects in a hierarchical fashion.

-   <span class="nctnt ncbi-path">src/</span> additionally contains <span class="nctnt ncbi-monospace">makefile</span> and <span class="nctnt ncbi-monospace">meta-makefile</span> templates.

-   Projects contain "[modules](ch_proj.html#ch_proj.new_modules)" and various customized [makefiles and meta-makefiles](ch_start.html#ch_start.make_templates) to control their compilation.


