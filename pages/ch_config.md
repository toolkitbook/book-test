---
layout: default
title: C++ Toolkit test
nav: pages/ch_config
---

<span class="label">4</span>Configure, Build, and Use the Toolkit
=================================================================

Last Update: November 13, 2014.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter describes in detail how to configure, build, and use the NCBI C++ Toolkit (or selected components of it) on supported platforms. See the [Getting Started](ch_start.html#ch_start.basic_install) chapter for a general overview of the process. A list of all supported platforms can be seen [here](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/public_releases/release_notes.html#release_notes.Platforms_OSs__compi).

<span class="nctnt highlight">Note:</span> Users insde NCBI who just want to use the Toolkit don't need to configure and build it - there are [various configurations](ch_proj.html#ch_proj.daily_builds) of the Toolkit prebuilt and ready to use. See the [new\_project](ch_proj.html#ch_proj.new_project_Starting) script for more information.

Configuring is the process of creating configuration files that define exactly what can be built and what options may be used in the build process. The created configuration files include C headers that define suitable preprocessor macros, as well makefiles (for Unix) or project solutions (for MS Visual C++ or for Xcode) used in the build step.

With some compilers that include an Integrated Development Environment (e.g. MS Visual C++), a top-level build target, called CONFIGURE, is available. On Unix-like systems it is necessary to execute a configuration script *configure* – sometimes via a special wrapper script that first performs some platform-specific pre-configuration steps and then runs the configuration process.

The configuration process defines the set of targets that can be built. It is up to the user to choose which of those targets to build and to choose the desired build options. For more details on the build system and the Makefiles created by the configuration process, see the chapter on [Working with Makefiles](ch_build.html).

Successful builds result in immediately usable libraries and applications, and generally there is no need for a separate installation step on any platform.

In addition to building the Toolkit libraries and applications, this chapter also discusses building test suites and sample applications. You might want to build and run a test suite if you are having trouble using the Toolkit and you aren’t sure if it is working properly. While it isn’t necessary to build a test suite to use the Toolkit, it can be useful for ensuring that the Toolkit has been properly configured and built. Building a sample application may be a good first step toward learning how to build your own applications.

### Chapter Outline

[General Information for All Platforms](#general-information-for-all-platforms)

-   [Choosing a Build Scope](#choosing-a-build-scope)

    -   [Project List Files](#project-list-files)

    -   [Reducing Build Scope with Project Tags](#reducing-build-scope-with-project-tags)

-   [Configure the Build](#configure-the-build)

    -   [Site-Specific Third Party Library Configuration](#site-specific-third-party-library-configuration)

    -   [Configuring with the Configuration GUI](#configuring-with-the-configuration-gui)

-   [Use the Toolkit](#use-the-toolkit)

-   [Supported Platforms](#supported-platforms)

[Unix](#unix)

-   [General Information for Unix Platforms](#general-information-for-unix-platforms)

    -   [Choosing a Build Scope with Unix](#choosing-a-build-scope-with-unix)

    -   [Configuring with Unix](#configuring-with-unix)

    -   [Building with Unix](#building-with-unix)

    -   [Using the Toolkit with Unix](#using-the-toolkit-with-unix)

-   [Special Considerations for Specific Unix Platforms](#special-considerations-for-specific-unix-platforms)

    -   [Linux / ICC](#linux--icc)

    -   [Cygwin / GCC](#cygwin--gcc)

[MS Windows](#ms-windows)

-   [MS Visual C++](#ms-visual-c++)

    -   [Choosing a Build Scope with Visual C++](#choosing-a-build-scope-with-visual-c++)

    -   [Configuring with Visual C++](#configuring-with-visual-c++)

    -   [Building with Visual C++](#building-with-visual-c++)

    -   [Using the Toolkit with Visual C++](#using-the-toolkit-with-visual-c++)

-   [Cygwin / GCC](#cygwin--gcc)

[Mac OS X](#mac-os-x)

-   [Xcode 3.0, 3.1](#xcode-30-31)

    -   [Choosing a Build Scope with Xcode 3.0 or Later](#choosing-a-build-scope-with-xcode-30-or-later)

    -   [Configuring with Xcode 3.0 or Later](#configuring-with-xcode-30-or-later)

    -   [Building with Xcode 3.0 or Later](#building-with-xcode-30-or-later)

-   [Xcode 1.0, 2.0](#xcode-10-20)

    -   [Build the Toolkit](#build-the-toolkit)

    -   [The Build Results](#the-build-results)

-   [Darwin / GCC](#darwin--gcc)

-   [Code Warrior](#code-warrior) (discontinued as of April 30, 2006)

General Information for All Platforms
-------------------------------------

Using the Toolkit on any platform requires these basic high-level steps:

-   [Prepare the development environment](ch_faq.html#ch_faq.How_do_I_prepare_my_development_e).

-   [Get the source files](ch_getcode_svn.html) from NCBI and place them in your working directory.

-   [Choose a build scope](#choose-a-build-scope).

-   [Configure the build](#configure-the-build).

-   Build.

-   [Use the Toolkit](#use-the-toolkit) from your application.

### Choosing a Build Scope

After [preparing the development environment](ch_faq.html#ch_faq.How_do_I_prepare_my_development_e), you'll need to choose a build scope. Choosing a build scope means deciding whether you want to build the entire Toolkit or just some portion of it. The build system includes methods on most platforms for building pre-defined scopes, such as just the core libraries and applications, the Genome Workbench, pre-defined lists of one or more projects, etc. Choosing a build scope must be done before configuring on some platforms. On other platforms it can be done either before or after configuring. See the section for your platform for more details on pre-defined build scope choices.

#### Project List Files

A very convenient way to explicitly define the build scope is to use a project list file. These files are simple line-oriented text files that essentially just list the set of projects you want included in (or excluded from) your build scope. Project list files can be referenced from various build-related tools including the <span class="nctnt ncbi-app">configure</span> script and the [Configuration GUI](#configuration-gui) (which pass the files to the build system); the [update\_projects](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh) script; and the [`prepare_release`](http://mini.ncbi.nlm.nih.gov/376) framework (NCBI only).

Project list files consist of two types of lines (ignoring blank lines) - [path lines](#path-lines), and [hash lines](#hash-lines). Thus, any non-blank line that doesn't begin with the hash character "<span class="nctnt ncbi-monospace">\#</span>" must be a path line.

##### Path Lines in Project List Files

Typically, most of the lines in project list files are path lines, which identify subtrees of the toolkit repository, for example:

    corelib
    connect/services
    objects

Each such "path" actually represents two subtrees:

-   a source subtree with <span class="nctnt ncbi-path">trunk/c++/src/</span> prepended to the given path; and

-   an include subtree with <span class="nctnt ncbi-path">trunk/c++/include/</span> prepended to the given path.

For example, the path <span class="nctnt ncbi-path">connect/services</span> results in the inclusion of:

-   the source subtree <span class="nctnt ncbi-path">trunk/c++/src/connect/services</span>; and

-   the include subtree <span class="nctnt ncbi-path">trunk/c++/include/connect/services</span>.

Note that subtrees are recursive by default. See the section on using a [trailing dollar sign](#trailing-dollar-sign) for making the selection non-recursive.

Similarly, paths beginning with <span class="nctnt ncbi-path">internal/</span> result in the inclusion of source and include subtrees from the internal codebase. For example, the path <span class="nctnt ncbi-path">internal/ilink</span> results in the inclusion of the source subtree <span class="nctnt ncbi-path">trunk/internal/c++/src/internal/ilink</span> and the include subtree <span class="nctnt ncbi-path">trunk/internal/c++/include/internal/ilink</span>.

The general syntax for path lines is represented by the following diagram:

<span>[![Image ch\_config\_lst\_grammar\_path\_line.png](/book-test/static/img/ch_config_lst_grammar_path_line.png)](/book-test/static/img/ch_config_lst_grammar_path_line.png "Click to see the full-resolution image")</span>

The various elements of the above syntax diagram are discussed in the following sections:

-   [Leading hyphen](#leading-hyphen)

-   [Leading dot-slash or caret-slash](#leading-dot-slash-or-caret-slash)

-   [Trailing dollar sign](#trailing-dollar-sign)

-   [Trailing update-only](#trailing-update-only)

-   [Regular expressions in project paths](#regular-expressions-in-project-paths)

##### Leading hyphen

Path lines may have a leading "<span class="nctnt ncbi-monospace">-</span>". If so, it means that the selected subtree(s) will be excluded, rather than included. For example:

    algo/blast/blastinput
    -algo/blast/blastinput/demo

results in excluding the <span class="nctnt ncbi-path">demo</span> subdirectory while including all the other subdirectories of <span class="nctnt ncbi-path">algo/blast/blastinput</span>.

<span class="nctnt highlight">Note:</span> Path lines might not be processed in the order they appear in the project list file. For example, the <span class="nctnt ncbi-app">prepare\_release</span> framework first builds a tree formed from all the "positive" path lines (those without leading hyphens), and then removes the subtrees specified by "negative" path lines (those with leading hyphens).

##### Leading dot-slash or caret-slash

Path lines may have a leading "<span class="nctnt ncbi-monospace">./</span>" or "<span class="nctnt ncbi-monospace">^/</span>". A leading "<span class="nctnt ncbi-monospace">./</span>" means that the given path will select only one subtree, with <span class="nctnt ncbi-path">trunk/c++/</span> prepended to the given path. A leading "<span class="nctnt ncbi-monospace">^/</span>" selects only one subtree, with <span class="nctnt ncbi-path">trunk/</span> prepended to the given path.

<span class="nctnt highlight">Note:</span> As of June, 2014, the only tool supporting this syntax feature is <span class="nctnt ncbi-app">prepare\_release</span>.

##### Trailing dollar sign

By default, all the contained files and subdirectories in a given path will be recursively selected. However, if the path has a trailing dollar sign "<span class="nctnt ncbi-monospace">$</span>", then no subdirectories will be selected.

Thus, the following lines:

    util$
    util/compress

select:

-   The files directly contained in:

    -   <span class="nctnt ncbi-path">trunk/c++/include/util/</span>

    -   <span class="nctnt ncbi-path">trunk/c++/src/util/</span>

-   All the files and subdirectories recursively contained in:

    -   <span class="nctnt ncbi-path">trunk/c++/include/util/compress/</span>

    -   <span class="nctnt ncbi-path">trunk/c++/src/util/compress/</span>

##### Trailing update-only

Path lines may have a trailing "<span class="nctnt ncbi-monospace">update-only</span>". If so, it means that the selected subtrees will be kept up-to-date with respect to the repository, but will not be built.

##### Regular expressions in project paths

Path components may contain limited regular-expression-like syntax. For example,

    internal/[^m].*/(app\|unit_test\|demo\|samples).*

selects all the sub-subdirectories beginning with <span class="nctnt ncbi-path">app</span>, <span class="nctnt ncbi-path">unit\_test</span>, <span class="nctnt ncbi-path">demo</span>, or <span class="nctnt ncbi-path">samples</span> that are contained in all internal subdirectories that begin with something other than '<span class="nctnt ncbi-monospace">m</span>'. Different tools may support regular expressions to different degrees. The special string <span class="nctnt ncbi-path">c++</span> in a path may be interpreted literally rather than as a regular expression.

<span class="nctnt highlight">Note:</span> Using regular expression characters early in a path can cause significant performance problems. For example, when the <span class="nctnt ncbi-app">prepare\_release</span> framework detects a regular expression character in a path component, it performs a recursive SVN list command and applies the regular expression to the results.

##### Hash Lines in Project List Files

Non-blank lines in a project list file must be either [path lines](#path-lines) or hash lines (i.e. start with the hash character "<span class="nctnt ncbi-monospace">\#</span>").

The primary purpose of hash lines is to enable two complementary goals:

-   path lines should be trivially recognizable (simply by the absence of a leading "<span class="nctnt ncbi-monospace">\#</span>"); and

-   it should be possible for tools to use the C preprocessor to build a master list of projects from project list files that contain <span class="nctnt ncbi-monospace">\#include</span> directives.

While the C preprocessor will only process hash lines, other tools may process both path lines and hash lines. For example, <span class="nctnt ncbi-app">prepare\_release</span> processes path lines and <span class="nctnt ncbi-monospace">\#include</span> directives.

To remain compatible with the C preprocessor while supporting processing by other tools, the project list file grammar encompasses four hash line constructs:

-   [include lines](#include-lines)

-   [project tag lines](#project-tag-lines)

-   [hashed path lines](#hashed-path-lines)

-   [comment blocks](#comment-blocks)

##### Include lines

Project list files may include others, using C syntax and semantics, for example:

    #include "../../projects/no_gbench.lst"

##### Project tag lines

Project tags may be used to filter the final set of selected projects (see the [Defining Project Tags](#defining-project-tags) and [Filtering with Project Tags](#filtering-with-project-tags) sections for more details).

The general syntax for project tag lines is represented by the following diagram:

<span>[![Image ch\_config\_lst\_grammar\_tags\_line.png](/book-test/static/img/ch_config_lst_grammar_tags_line.png)](/book-test/static/img/ch_config_lst_grammar_tags_line.png "Click to see the full-resolution image")</span>

For example:

    #define  TAGS  [demo test]

##### Hashed path lines

Occasionally a developer may wish to simply comment out, rather than delete, a path line in a project list file, for example:

    #internal/qidx$

Unfortunately, this will appear to the C preprocessor as an invalid preprocessor directive, and it will generate an error. As of June 2014, however, no tools that use project list files will report this as an error. The only indication of the error will be the terminal output, which will contain an error message similar to:

    /build/path/projects.pseudo.c:12:2: error: invalid preprocessing directive #internal

Therefore, if you don't mind error messages like this in your terminal output, you may be able to get away with hashed path lines. However, this is discouraged because future tools may detect the preprocessor error, and this construct may become deprecated. To be safe, you should use a comment block as described below.

##### Comment blocks

As discussed in the previous section, lines that begin with "<span class="nctnt ncbi-monospace">\#</span>", but are not valid C preprocessor directives, will result in an error. But it is also possible that lines beginnng with "<span class="nctnt ncbi-monospace">\#</span>" and containing general comments could actually be valid preprocessor directives - and they could alter the processing of the project list file.

For example, consider this project list file:

    some/project$
    #Here is rather silly block comment that demonstrates possible major problems
    #if simple hash comments are used - specifically, the build system will not
    #include anything after the above line - and it would generate an error for this <--
    #line if it got to it (and another on this <-- line)...

    #nothing else will get processed :-/
    #but it _would_ have if the "else" was the first word in the above line!
    another/project
    #include "still/more/projects"

Only the first path line will get processed because the C preprocessor will detect an error on the line:

    #if simple hash comments are used - specifically, the build system will not

and it won't find the end of the <span class="nctnt ncbi-monospace">\#if</span> clause.

Furthermore, current tools won't report this error, so if free-form block comments using hash lines were permitted, project list file errors could go undetected.

However, there are safe ways to use block comments in project list files - here are three:

    #if 0
    # Your comment here.
    # NOTE: Lines between the '#if 0' and '#endif' must begin with '#'
    #       (otherwise they'd be interpreted by various tools as path lines).
    # Also, unmatched quote characters will cause a preprocessor warning.
    #endif

    #define  LST_COMMENT  \
    # your \
    # comment \
    # here (again, all continuation lines must start with '#')

    #define  LST_COMMENT1  your
    #define  LST_COMMENT2  comment
    #define  LST_COMMENT3  here

#### Reducing Build Scope with Project Tags

The pre-defined build scopes mentioned [above](#above) may be unnecessarily broad for your task. You can reduce the build scope by using project tags.

There are two complementary parts to using project tags. First, project tags are [defined](#defined) and associated with selected projects. Second, a tag filter is [supplied](#supplied) to the configuration process. The configuration process then filters the list of projects that will be built, based on each project's tags and the supplied tag filter.

An important benefit of using project tags is that all dependencies for the projects that match the tag filter will be automatically deduced and added to the build list.

##### Defining Project Tags

All project tags must be defined in <span class="nctnt ncbi-path">src\\build-system\\project\_tags.txt</span> prior to use. Tag names should be easily recognizable and classifiable, like ‘<span class="nctnt ncbi-monospace">proj[\_subproj]</span>’, e.g. “<span class="nctnt ncbi-monospace">pubchem</span>” or “<span class="nctnt ncbi-monospace">pubchem\_openeye</span>”.

Once defined in <span class="nctnt ncbi-path">project\_tags.txt</span>, project tags can then be associated with any number of projects by using the <span class="nctnt ncbi-macro">PROJ\_TAG</span> macro in the <span class="nctnt ncbi-path">Makefile.in</span> or <span class="nctnt ncbi-path">Makefile.\*.{app\\|lib}</span> for the selected projects. Project tag definitions apply recursively to subprojects and subdirectories (similar to a [`REQUIRES`](ch_proj.html#ch_proj.proj_makefiles) definition), thereby removing the need to define tags in all makefiles in a subtree. Subprojects may define additional tags, or undefine inherited tags by prefixing a hyphen '<span class="nctnt ncbi-monospace">-</span>' to the tag.

The syntax for defining (or undefining) a project tag is:

    PROJ_TAG = [-]mytag1 [[-]mytag2...]

For example, if <span class="nctnt ncbi-path">Makefile.in</span> has this line:

    PROJ_TAG = foo bar

and a project beneath it in the tree hierarchy (say <span class="nctnt ncbi-path">Makefile.\*.app</span>) has this line:

    PROJ_TAG = xyz -bar

then the latter project's effective tag definition is:

    PROJ_TAG = foo xyz

##### Filtering with Project Tags

A tag filter can be constructed from one or more project tags – either as a single tag or as a Boolean expression of tags. Boolean expressions of tags can include grouping (parentheses) and the '<span class="nctnt ncbi-monospace">&&</span>' (AND), '<span class="nctnt ncbi-monospace">\\|\\|</span>" (OR), and '<span class="nctnt ncbi-monospace">!</span>' (NOT) operators, for example: <span class="nctnt ncbi-monospace">(core \\|\\| web) && !test</span>

<span class="nctnt highlight">Note:</span> An asterisk '<span class="nctnt ncbi-monospace">\*</span>' or an empty string can be used in place of a tag filter in the "Allowed project tags" field on the [Configuration tab](#configuration-tab) of the configuration GUI. These values are not filters, but simply indicate that all projects in the build scope will be passed to the configuration process without filtering.

The following places are searched in the order given for the tag filter to use (if any) in the configuration process:

1  
The "Allowed project tags" field in the configuration GUI (if the configuration GUI is being used).

2  
A tag filter definition line in a project list file (if one is being used).

a  
To use a project list file for configuration, either specify the project list file in the "Subtree, or LST file" field on the [Configuration tab](#configuration-tab) of the configuration GUI or use the <span class="nctnt ncbi-monospace">--with-projects=FILE</span> argument for the <span class="nctnt ncbi-cmd">configure</span> script.

b  
When one project list file includes another, only the original will be scanned for a filter. This applies to both interactive (i.e. with the configuration GUI) and non-interactive configuring.

c  
The syntax for the tag filter definition line in a project list file is: <span class="nctnt ncbi-monospace">\#define TAGS [ tag\_filter ]</span>

3  
For MSVC, the <span class="nctnt ncbi-macro">-projtag</span> option of the <span class="nctnt ncbi-macro">PTB\_FLAGS</span> macro in the <span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\static\\build\\UtilityProjects\\configure.\_</span> file for non-interactive configuring, or the same option in the <span class="nctnt ncbi-path">configure\_dialog.\_</span> file for interactive configuring.

If a significant tag filter (i.e. something besides an asterisk or empty field) is found in one of the above places, then that tag filter will be supplied to the configuration process. Otherwise, there will be no filtering of the projects.

### Configure the Build

Prior to configuring, users outside NCBI should make sure the paths to their third party libraries are [correctly specified](#correctly-specified).

For the configuration step you can specify whether to use static or dynamically-linked libraries; whether to generate multithread-safe code; whether to look for various third-party libraries at alternative locations; whether or not to include debugging information; etc.

Configuration can be done in one of three ways:

-   Using the [Configuration GUI](#configuration-gui).

-   Using a "native" IDE – [MSVC](#msvc) on Windows or [Xcode](#xcode) on Mac OS X.

-   Using the command-line on [Unix](#unix), [Cygwin/Windows](#cygwinwindows), or [Mac OS X](#mac-os-x).

#### Site-Specific Third Party Library Configuration

Users outside NCBI should check the file <span class="nctnt ncbi-path">src/build-system/config.site</span> to see if it correctly specifies the paths to their third party libraries. If not, it can be edited using <span class="nctnt ncbi-path">src/build-system/config.site.ex</span> as a guide.

<span class="nctnt highlight">Note:</span> The <span class="nctnt ncbi-cmd">configure --with-PACKAGE</span> options take precedence over the <span class="nctnt ncbi-path">config.site</span> and <span class="nctnt ncbi-var">PACKAGE\_PATH</span> settings.

#### Using the Configuration GUI

The configuration GUI can be launched from a command shell or from an IDE (MSVC or Xcode). It is Java-based and requires the [Java Platform Standard Edition](http://www.java.com/).

The following sections describe how to use the configuration GUI:

-   [Starting the configuration GUI](#starting-the-configuration-gui)

-   [Configuration tab](#configuration-tab)

-   [Advanced tab](#advanced-tab)

-   [Third party libraries tab](#third-party-libraries-tab)

-   [Projects tab](#projects-tab)

-   [Done tab](#done-tab)

See the [Unix](#unix), [Windows](#windows), and [Mac OS X](#mac-os-x) sections for OS-specific configuration information.

##### Starting the configuration GUI

To launch the configuration GUI:

-   From the command-line: <span class="nctnt ncbi-cmd">./configure --with-configure-dialog</span>

-   From the MSVS IDE: build the <span class="nctnt ncbi-macro">-CONFIGURE-DIALOG-</span> project

-   From the Xcode IDE: build the <span class="nctnt ncbi-macro">CONFIGURE-DIALOG</span> target

The configuration GUI has a "Wizard" style design – selections are made in a sequence of steps, followed by clicking the Next button. After each step additional tabs may be enabled, depending on the specific data. It opens with initial values set by the invoking program (the configure script for command-line invocation or the <span class="nctnt ncbi-app">project\_tree\_builder</span> program for IDE's).

##### Configuration tab

The Configuration tab looks like:

<span>[![Image ch\_config\_dlg\_cfg.png](/book-test/static/img/ch_config_dlg_cfg.png)](/book-test/static/img/ch_config_dlg_cfg.png "Click to see the full-resolution image")</span>

The Configuration tab allows you to:

-   Choose between static and dynamically-linked libraries.

-   Specify the subset of the Toolkit that you want to build, using either a path for a subtree (e.g. <span class="nctnt ncbi-path">src\\</span>) or a project list file (<span class="nctnt ncbi-path">\*.lst</span>) for specific projects. Clicking on the "..." button opens a file selection dialog, which can be used to navigate to the desired subtree or to select a project list file.

-   Specify one or more project tags (which will restrict the scope of the build to the specified projects). Clicking on the "..." button simply displays the valid choices for project tags (it isn't used for selecting tags). More than one project tag can be combined in a Boolean expression, for example:<br/><span class="nctnt ncbi-monospace">(code \\|\\| web) && !test</span>

-   Load a configuration from a file. This requires having previously saved a configuration, from the [Done tab](#done-tab). If you load a configuration from a file, the file path is shown in the "Originally loaded from" text field and the Reset button becomes enabled. Clicking the Reset button resets all configuration settings to the values that were used to invoke the configuration GUI.

##### Advanced tab

The Advanced tab looks like:

<span>[![Image ch\_config\_dlg\_adv\_less.png](/book-test/static/img/ch_config_dlg_adv_less.png)](/book-test/static/img/ch_config_dlg_adv_less.png "Click to see the full-resolution image")</span>

The Advanced tab allows you to:

-   View the current version of the IDE (currently only applicable to Windows / Microsoft Visual Studio).

-   View the current architecture (currently only applicable to Windows / Microsoft Visual Studio).

-   Specify the name of a solution file to generate. You can use this to create different solution files for different configurations.

-   Specify where to look for missing libraries. This can be used to change the build – for example, from <span class="nctnt ncbi-path">cxx.current</span> to <span class="nctnt ncbi-path">cxx.potluck</span>.

In addition, by clicking "more" you will see:

<span>[![Image ch\_config\_dlg\_adv\_more.png](/book-test/static/img/ch_config_dlg_adv_more.png)](/book-test/static/img/ch_config_dlg_adv_more.png "Click to see the full-resolution image")</span>

These additional options generally don't need to be changed, but they allow you to:

-   Exclude the "Build PTB" step from the configure process. This should be selected if the PTB (project tree builder) source is not available. Even if the PTB source is available, it usually makes sense to exclude building the PTB because building it will take longer and generally won't have a benefit.

-   Prevent whole-tree scanning for missing project dependencies. A project dependency may be missing if, for example, import\_project was used and the configuration was changed to something other than simply Debug or Release (e.g. DebugMT).

-   Use external libraries instead of missing in-tree ones.

-   Select a different project tree builder. In most cases this won't be needed, but it could be useful for tasks such as debugging the build system.

-   Select a different location to use as the root of the source tree.

##### Third party libraries tab

The Third party libraries tab looks like:

<span>[![Image ch\_config\_dlg\_third.png](/book-test/static/img/ch_config_dlg_third.png)](/book-test/static/img/ch_config_dlg_third.png "Click to see the full-resolution image")</span>

The Third party libraries tab allows you to:

-   Select a different location for third-party libraries.

-   Select a different location for the NCBI C Toolkit.

-   Add VTune configurations. If selected, new VTune configurations will be added to the list of available configurations – for example, VTune\_DebugDLL.

##### Projects tab

The Projects tab looks like:

<span>[![Image ch\_config\_dlg\_proj.png](/book-test/static/img/ch_config_dlg_proj.png)](/book-test/static/img/ch_config_dlg_proj.png "Click to see the full-resolution image")</span>

The Projects tab allows you to select exactly which applications and libraries will be built. If an item is not selected, but at least one selected item depends on it, then it will also be built. This provides a convenient way for developers to simply pick the top-level items to build.

The "-all" and "+all" buttons uncheck or check all the items in a column.

The Tags column allows you to quickly select all items having the selected project tag(s). Also, selecting items in the other columns will update the selection status of the tags column.

##### Done tab

The Done tab looks like:

<span>[![Image ch\_config\_dlg\_done.png](/book-test/static/img/ch_config_dlg_done.png)](/book-test/static/img/ch_config_dlg_done.png "Click to see the full-resolution image")</span>

The Done tab:

-   Reports whether the project was generated successfully.

-   Shows the path for the generated solution file.

-   Gives the option to save the configuration parameters. Once saved, the same parameters can be loaded again from the [Configuration tab](#configuration-tab).

-   Gives the option to start over and create a new set of configuration parameters.

-   Gives the option to close the tool, via the Finish button. Closing the tool will return you to the configuration process, which will continue based on the parameters set in the configuration GUI.

### Use the Toolkit

After choosing a build scope, configuring, and building the Toolkit, you can now use it. The Toolkit itself includes useful applications, demo programs, and sample code – in addition to the libraries you can use from your own applications. You can also build a suite of test applications and/or sample applications if desired.

### Supported Platforms

The term “platform” in this chapter has a specific meaning: the combination of operating system, architecture, and compiler. A supported platform is one for which the Toolkit has been configured, built, tested, and used by other applications.

The list of supported platforms may change with new releases. For the platforms supported in the release you are using, see the [Supported Platforms](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/public_releases/release_notes.html#release_notes.Platforms_OSs__compi) section in the release notes. Note that some platforms are only partially supported.

Unix
----

<span class="nctnt highlight">Note:</span> Please also see the [General Information for All Platforms](#general-information-for-all-platforms) section, as it contains relevant information that is not repeated here.

This section covers the following topics:

-   [General Information for Unix Platforms](#general-information-for-unix-platforms)

    -   [Choosing a Build Scope](#choosing-a-build-scope)

    -   [Configuring](#configuring)

    -   [Building](#building)

    -   [Using](#using)

-   [Special Considerations for Specific Unix Platforms](#special-considerations-for-specific-unix-platforms)

    -   [Linux / ICC](#linux--icc)

    -   [Cygwin / GCC](#cygwin--gcc)

### General Information for Unix Platforms

This section provides information on configuring, building, and using the Toolkit that is applicable to all Unix platforms. The section [Special Considerations for Specific Unix Platforms](#special-considerations-for-specific-unix-platforms) addresses platform-specific details.

Note, however, that the sections on specific platforms do not address the level of support for specific compilers. See the [Supported Platforms](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/public_releases/release_notes.html#release_notes.Platforms_OSs__compi) section in the release notes for information on partially supported compilers.

The following topics are discussed in this section:

-   [Choosing a Build Scope](#choosing-a-build-scope)

-   [Configuring](#configuring)

    -   [Configuration Script configure](#configuration-script-configure)

    -   [Structure of the Build Tree Produced by configure](#structure-of-the-build-tree-produced-by-configure)

    -   [Options for Fine-Tuning the configure Script](#options-for-fine-tuning-the-configure-script)

    -   [Quick Reconfiguration](#quick-reconfiguration)

-   [Building](#building)

    -   [General Principles for Building with Unix](#general-principles-for-building-with-unix)

    -   [Building Only Core Libraries and Applications](#building-only-core-libraries-and-applications)

    -   [Building GUI Libraries and Applications](#building-gui-libraries-and-applications)

    -   [Building the Genome Workbench](#building-the-genome-workbench)

    -   [Building the Entire Toolkit](#building-the-entire-toolkit)

-   [Using](#using)

    -   [Modify or Debug an Existing Toolkit Application](#modify-or-debug-an-existing-toolkit-application)

    -   [Modify or Debug an Existing Toolkit Library](#modify-or-debug-an-existing-toolkit-library)

#### Choosing a Build Scope with Unix

The Toolkit is very large and you may not want to retrieve and build the entire Toolkit if you don’t need to. Therefore, after [preparing the development environment](ch_faq.html#ch_faq.How_do_I_prepare_my_development_e) and [getting the source files](ch_getcode_svn.html), you'll need to choose a build scope. Several mechanisms are provided to enable working with only a portion of the Toolkit.

The first thing you can do is to limit the source code [retrieved from the repository](ch_getcode_svn.html):

-   using the shell script [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh); or

-   using the shell script [update\_projects](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh).

Next, you can limit what is built:

-   by configuring with the *--with-projects* option; or

-   by running *make* only within directories of interest; or

-   by building only a selected list of end targets using *flat makefile*

You can also choose between static and shared libraries - or build both. Building with static libraries will result in much larger applications and require much more disk space.

#### Configuring with Unix

The following topics are discussed in this section:

-   [Configuration Script configure](#configuration-script-configure)

-   [Structure of the Build Tree Produced by configure](#structure-of-the-build-tree-produced-by-configure)

-   [Options for Fine-Tuning the configure Script](#options-for-fine-tuning-the-configure-script)

    -   [Getting a Synopsis of Available Configuration Options](#getting-a-synopsis-of-available-configuration-options)

    -   [Debug vs. Release Configuration](#debug-vs-release-configuration)

    -   [Multi-Thread Safe Compilation and Linking with MT Libraries](#multi-thread-safe-compilation-and-linking-with-mt-libraries)

    -   [Building Shared Libraries (DLLs)](#building-shared-libraries-dlls)

    -   [Finer-grained Control of Projects: --with-projects](#finer-grained-control-of-projects---with-projects)

    -   [Building in the 64-bit mode](#building-in-the-64-bit-mode)

    -   [Localization for the System and Third-Party Packages](#localization-for-the-system-and-third-party-packages)

    -   [Naming the Build Tree](#naming-the-build-tree)

    -   [Hard-Coding Run-Time DLL Path into Executables and DLLs](#hard-coding-run-time-dll-path-into-executables-and-dlls)

    -   [Automatic Generation of Dependencies (for GNU make Only)](#automatic-generation-of-dependencies-for-gnu-make-only)

    -   [After-Configure User Callback Script](#after-configure-user-callback-script)

    -   [Tools and Flags](#tools-and-flags)

    -   [Prohibiting the Use of Some of the System and Third-party Packages](#prohibiting-the-use-of-some-of-the-system-and-third-party-packages)

    -   [Optional Projects](#optional-projects)

    -   [Miscellaneous: --without-exe, --without-execopy, --with-lib-rebuilds(=ask)](#miscellaneous---without-exe---without-execopy---with-lib-rebuilds=ask)

-   [Quick Reconfiguration](#quick-reconfiguration)

##### Configuration Script configure

Different build setups compile C++ (and even C!) code differently; they may vary in the OS standard and 3<sup>rd</sup>-party libraries and header files, completeness of the C++ implementation, and in compiler bugs. There are also different versions of *make* and other tools and different file naming conventions on different platforms.

Thus, configuration is needed to use the platform- and compiler-specific features. For this purpose, we are using a script produced by the GNU [autoconf](http://www.gnu.org/software/autoconf) utility to automatically generate the build-specific header file <span class="nctnt ncbi-path">ncbiconf.h</span> and makefiles that would work for the given platform.

The user performs configuration by merely running platform-independent (*sh, bash*) shell script ***configure*** (which we pre-generate in-house from the template <span class="nctnt ncbi-path">configure.ac</span> using [autoconf](http://www.gnu.org/software/autoconf)).

During the configuration process, many compiler features are tested, and the results of this testing are recorded in the configuration header <span class="nctnt ncbi-path">ncbiconf.h</span> by the means of C preprocessor variables. For example, the preprocessor variable <span class="nctnt ncbi-var">NO\_INCLASS\_TMPL</span> indicates whether the compiler supports template class methods. Also contained in the <span class="nctnt ncbi-path">ncbiconf.h</span> file are preprocessor variables used to define [sized integer](ch_core.html#ch_core.fixed_size_integers) and [BigScalar](ch_core.html#ch_core.big_scalar) types.

The ***configure*** script will create a [build tree](ch_start.html#ch_start.F2), a hierarchy of directories where object modules, libraries, and executables are to be built. It will also configure all <span class="nctnt ncbi-path">\*.in</span> template files located in the NCBI C++ source tree (<span class="nctnt ncbi-path">src/</span>) and deploy the resultant configured files in the relevant places of the build tree. This way, all platform- and compiler-specific tools and flags will be "frozen" inside the configured makefiles in the build tree. The <span class="nctnt ncbi-path">ncbiconf.h</span> (described above, also configured for the given compiler) will be put to the <span class="nctnt ncbi-path">inc/</span> sub-directory of the resultant build tree.

You can create as many build trees as needed. All build trees refer to the same [source tree](ch_start.html#ch_start.F1), but contain their own platform/compiler-specific <span class="nctnt ncbi-path">ncbiconf.h</span> header and/or different set of compilation/linking flags and tools ("frozen" in the makefiles, particularly in [Makefile.mk](ch_build.html#ch_build.build_make_macros)). This allows building libraries and executables using different compilers and/or flags, yet from the same source, and in a uniform way.

A configuration tool with a Java-based GUI is also available and can be launched from the command-line:

    ./configure --with-configure-dialog

Additional parameters can also be passed to configure, just as without the configuration GUI.

For more information on using the configuration GUI, see the [general section on configuring](#general-section-on-configuring).

##### Structure of the Build Tree Produced by configure

Each configuration process results in a new [build tree](ch_start.html#ch_start.F2). The top-level directories in the tree are:

<span class="nctnt ncbi-path">inc/</span> - contains the <span class="nctnt ncbi-path">ncbiconf.h</span> configuration header generated by the ***configure*** script.

<span class="nctnt ncbi-path">build/</span> - contains a hierarchy of directories that correspond to those in the <span class="nctnt ncbi-path">src/</span> (in NCBI C++ original sources). These directories will contain makefiles (<span class="nctnt ncbi-path">Makefile.\*</span>) generated by the ***configure*** script from the makefile templates (<span class="nctnt ncbi-path">Makefile.\*.in</span>) of the corresponding project located in the source tree. The resultant scripts and makefiles will keep references to the original NCBI C++ source directories. There is a "very special" file, [Makefile.mk](ch_build.html#ch_build.build_make_macros), that contains all configured tools, flags, and local paths. This file is usually included by other makefiles. All build results (object modules, libraries, and executables, as well as any auxiliary files and directories created during the build) will go exclusively into the *build tree* and not to the original NCBI C++ source directories. This allows for several build trees to use the same source code while compiling and linking with different flags and/or compilers.

<span class="nctnt ncbi-path">lib/</span> - contains the libraries built by the <span class="nctnt ncbi-path">build/</span>-located projects.

<span class="nctnt ncbi-path">bin/</span> - contains the executables built by the <span class="nctnt ncbi-path">build/</span>-located projects.

<span class="nctnt ncbi-path">status/</span> - contains:

-   <span class="nctnt ncbi-path">config.cache</span>, a cache file;

-   <span class="nctnt ncbi-path">config.log</span>, a log file;

-   <span class="nctnt ncbi-path">config.status</span>, a secondary configuration script produced by ***configure***;

-   <span class="nctnt ncbi-path">\*.enabled</span> files, with package and feature availability; and

-   <span class="nctnt ncbi-path">.\*.dep</span> files, with timestamps of the built Toolkit libraries.

##### Options for Fine-Tuning the configure Script

The configure script is highly customizable. The following sections describe some of the configuration options:

-   [Getting a Synopsis of Available Configuration Options](#getting-a-synopsis-of-available-configuration-options)

-   [Debug vs. Release Configuration](#debug-vs-release-configuration)

-   [Multi-Thread Safe Compilation and Linking with MT Libraries](#multi-thread-safe-compilation-and-linking-with-mt-libraries)

-   [Building Shared Libraries (DLLs)](#building-shared-libraries-dlls)

-   [Finer-grained Control of Projects: --with-projects](#finer-grained-control-of-projects---with-projects)

-   [Building in the 64-bit mode](#building-in-the-64-bit-mode)

-   [Localization for the System and Third-Party Packages](#localization-for-the-system-and-third-party-packages)

-   [Naming the Build Tree](#naming-the-build-tree)

-   [Hard-Coding Run-Time DLL Path into Executables and DLLs](#hard-coding-run-time-dll-path-into-executables-and-dlls)

-   [Automatic Generation of Dependencies (for GNU make Only)](#automatic-generation-of-dependencies-for-gnu-make-only)

-   [After-Configure User Callback Script](#after-configure-user-callback-script)

-   [Tools and Flags](#tools-and-flags)

-   [Prohibiting the Use of Some of the System and Third-party Packages](#prohibiting-the-use-of-some-of-the-system-and-third-party-packages)

-   [Optional Projects](#optional-projects)

-   [Miscellaneous: --without-exe, --without-execopy, --with-lib-rebuilds(=ask)](#miscellaneous---without-exe---without-execopy---with-lib-rebuilds=ask)

##### Getting Synopsis of Available Configuration Options

To get the full list of available configuration options, run <span class="nctnt ncbi-cmd">./configure --help</span>. The NCBI-specific options are at the end of the printout.

<span class="nctnt highlight">Note:</span> Do not use the "standard" configure options listed in the *"Directory and file names:"* section of the help printout (such as <span class="nctnt ncbi-cmd">--prefix= </span>*,* <span class="nctnt ncbi-cmd">--bindir=</span>, etc.) because these are usually not used by the NCBI ***configure*** script.

##### Debug vs. Release Configuration

The following two ***configure*** flags control whether to target for the *Debug* or *Release* version. These options (the default being *--with-debug*) control the appearance of preprocessor flags *-D\_DEBUG* and *-DNDEBUG* and compiler/linker flags *-g* and *-O*, respectively:

*--with-debug* -- engage *-D\_DEBUG* and *-g*, strip *-DNDEBUG* and *-O* (if not *--with-optimization*)

*--without-debug* -- strip *-D\_DEBUG* and *-g*, engage *-DNDEBUG* and *-O* (if not *--without-optimization*)

*--with-optimization* -- unconditionally engage *-DNDEBUG* and *-O*

*--without-optimization* -- unconditionally strip *-DNDEBUG* and *-O*

default: *--with-debug --without-optimization*

##### Multi-Thread Safe Compilation and Linking with MT Libraries

*--with-mt* - compile all code in an MT-safe manner; link with the system thread library.

*--without-mt* - compile with no regard to MT safety.

default: *--without-mt*

##### Building Shared Libraries (DLLs)

On the capable platforms, you can build libraries as *shared (dynamic)*.

*--with-dll --with-static* -- build libraries as both *dynamic* and *static*; however, if the library project makefile specifies [LIB\_OR\_DLL = lib](ch_proj.html#ch_proj.make_proj_lib), then build the library as *static* only, and if the library project makefile specifies [LIB\_OR\_DLL = dll](ch_proj.html#ch_proj.make_proj_lib), then build the library as *dynamic* only. Note that the resulting static libraries consist of position-independent objects.

*--with-dll* -- build libraries as *dynamic*; however, if the library project makefile specifies [LIB\_OR\_DLL = lib](ch_proj.html#ch_proj.make_proj_lib), then build the library as *static*

*--without-dll* -- always build *static* libraries, even if the library project makefile specifies [LIB\_OR\_DLL = dll](ch_proj.html#ch_proj.make_proj_lib)

default: build libraries as *static* (albeit with position-independent code); however, if the library project makefile specifies [LIB\_OR\_DLL = dll](ch_proj.html#ch_proj.make_proj_lib), then build the library as *dynamic*

See [this example](ch_proj.html#ch_proj.make_proj_lib) for more information about building shared libraries.

##### Finer-grained Control of Projects: --with-projects

If the above options aren't specific enough for you, you can also tell ***configure*** which projects you want to build by passing the flag *--with-projects=FILE*, where *FILE* contains a list of [extended regular expressions](http://www.opengroup.org/onlinepubs/007904975/basedefs/xbd_chap09.html) indicating which directories to build in. (See the [Project List Files](#project-list-files) section for a detailed discussion.) With this option, the *make* target <span class="nctnt ncbi-monospace">all\_p</span> will build all selected projects under the current directory. If there is a project that you want to keep track of but not automatically build, you can follow its name with "update-only". To **exclude** projects that would otherwise match, list them explicitly with an initial hyphen. (Exclusions can also be regular expressions rather than simple project names.) If no *FILE* argument is supplied then ***configure*** expects to find a project list file named "projects" in the top-level c++ directory.

For instance, a file containing the lines

    corelib$
    util
    serial
    -serial/test
    test update-only

would request a non-recursive build in <span class="nctnt ncbi-path">corelib</span> and a recursive build in <span class="nctnt ncbi-path">util</span>, and a recursive build in <span class="nctnt ncbi-path">serial</span> that skipped <span class="nctnt ncbi-path">serial/test</span>. It would also request keeping the <span class="nctnt ncbi-path">test</span> project up-to-date (for the benefit of the programs in <span class="nctnt ncbi-path">util/test</span>).

<span class="nctnt highlight">Note:</span> The flags listed [above](#above) still apply; for instance, you still need *--with-internal* to enable internal projects. However, [update\_projects](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh) can automatically take care of these for you.

Project list files may also define a project tag filter, with the syntax:

    #define TAGS [ tag_filter ]

See the section on [filtering with project tags](#filtering-with-project-tags) for more information.

##### Building in the 64-bit Mode

*--with-64* - compile all code and build executables in 64-bit mode.

default: depends on the platform; usually *--without-64* if both 32-bit and 64-bit build modes are available.

##### Localization for the System and Third-Party Packages

There is some configuration info that usually cannot be guessed or detected automatically, and thus in most cases it must be specified "manually" for the given local host's working environment. This is done by setting the localization environment variables (see [Table 2](#table-2)) in addition to the "generic" ones ([CC, CXX, CPP, AR, RANLIB, STRIP, CFLAGS, CXXFLAGS, CPPFLAGS, LDFLAGS, LIBS](#cc-cxx-cpp-ar-ranlib-strip-cflags-cxxflags-cppflags-ldflags-libs)).

Table 2. User-defined localization variables

| Name            | Default                              | Synopsis                                    |
|-----------------|--------------------------------------|---------------------------------------------|
| THREAD\_LIBS    | -lpthread                            | System thread library                       |
| NETWORK\_LIBS   | -lsocket -lnsl                       | System network libraries                    |
| MATH\_LIBS      | -lm                                  | System math library                         |
| KSTAT\_LIBS     | -lkstat                              | System kernel statistics library            |
| RPCSVC\_LIBS    | -lrpcsvc                             | System RPC services library                 |
| CRYPT\_LIBS     | -lcrypt[\_i]                         | System encrypting library                   |
| SYBASE\_PATH    | /netopt/Sybase/clients/current       | Path to Sybase package (but see note below) |
| FTDS\_PATH      | /netopt/Sybase/clients-mssql/current | Path to FreeTDS package                     |
| FASTCGI\_PATH   | $NCBI/fcgi-current                   | Path to the in-house FastCGI client lib     |
| FLTK\_PATH      | $NCBI/fltk                           | Path to the FLTK package                    |
| WXWIN\_PATH     | $NCBI/wxwin                          | Path to the wxWindows package               |
| NCBI\_C\_PATH   | $NCBI                                | Path to the NCBI C Toolkit                  |
| NCBI\_SSS\_PATH | $NCBI/sss/BUILD                      | Path to the NCBI SSS package                |
| NCBI\_GEO\_PATH | $NCBI/geo                            | Path to the NCBI GEO package                |
| SP\_PATH        | $NCBI/SP                             | Path to the SP package                      |
| NCBI\_PM\_PATH  | $NCBI/pubmed[64]                     | Path to the NCBI PubMed package             |
| ORBACUS\_PATH   | $NCBI/corba/OB-4.0.1                 | Path to the ORBacus CORBA package           |

Note: It is also possible to make configure look elsewhere for Sybase by means of --with-sybase-local[=DIR]. If you specify a directory, it will override SYBASE\_PATH; otherwise, the default will change to /export/home/sybase/clients/current, but SYBASE\_PATH will still take priority. Also, the option --with-sybase-new will change the default version of Sybase from 12.0 to 12.5 and adapt to its layout.

It is also possible to override WXWIN\_PATH by --with-wxwin=DIR, FLTK\_PATH by --\> --with-fltk=DIR, and ORBACUS\_PATH by --with-orbacus=DIR.

On the basis of [Table 2](#table-2), ***configure*** will derive the variables shown in [Table 3](#table-3) to use in the generated makefiles.

Table 3. Derived localization variables for makefiles

| Name               | Value                                                                                                             | Used to...                        |
|--------------------|-------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| THREAD\_LIBS       | $THREAD\_LIBS                                                                                                     | Link with system thread lib.      |
| NETWORK\_LIBS      | $NETWORK\_LIBS                                                                                                    | Link with system network libs.    |
| MATH\_LIBS         | $MATH\_LIBS                                                                                                       | Link with system math lib.        |
| KSTAT\_LIBS        | $KSTAT\_LIBS                                                                                                      | Link with system kernel stat lib. |
| RPCSVC\_LIBS       | $RPCSVC\_LIBS                                                                                                     | Link with system RPC lib.         |
| CRYPT\_LIBS        | $CRYPT\_LIBS                                                                                                      | Link with system encrypting lib.  |
| SYBASE\_INCLUDE    | -I$SYBASE\_PATH/include                                                                                           | \#include Sybase headers          |
| SYBASE\_LIBS       | -L$SYBASE\_PATH/lib[64] -lblk[\_r][64] -lct[\_r][64] -lcs[\_r][64] -ltcl[\_r][64] -lcomn[\_r][64] -lintl[\_r][64] | Link with Sybase libs.            |
| SYBASE\_DLLS       | -ltli[\_r][64]                                                                                                    | Sybase DLL-only libs              |
| FTDS\_INCLUDE      | -I$FTDS\_PATH/include                                                                                             | \#include FreeTDS headers         |
| FTDS\_LIBS         | -L$FTDS\_PATH/lib -lsybdb -ltds                                                                                   | Link with the FreeTDS API.        |
| FASTCGI\_INCLUDE   | -I$FASTCGI\_PATH/include[64]                                                                                      | \#include Fast-CGI headers        |
| FASTCGI\_LIBS      | -L$FASTCGI\_PATH/lib[64] -lfcgi or -L$FASTCGI\_PATH/altlib[64] -lfcgi                                             | Link with FastCGI lib.            |
| FLTK\_INCLUDE      | -I$FLTK\_PATH/include                                                                                             | \#include FLTK headers            |
| FLTK\_LIBS         | -L$FLTK\_PATH/[GCC-]{Release\\|Debug}[MT][64]/lib -lfltk ... -lXext -lX11 ... or -L$FLTK\_PATH/lib .....          | Link with FLTK libs.              |
| WXWIN\_INCLUDE     | -I$WXWIN\_PATH/include                                                                                            | \#include wxWindows headers       |
| WXWIN\_LIBS        | -L$WXWIN\_PATH/[GCC-]{Release\\|Debug}/lib -lwx\_gtk[d] -lgtk -lgdk -lgmodule -lglib or -L$WXWIN\_PATH/lib .....  | Link with wxWindows libs.         |
| NCBI\_C\_INCLUDE   | -I$NCBI\_C\_PATH/include[64]                                                                                      | \#include NCBI C Toolkit headers  |
| NCBI\_C\_LIBPATH   | -L$NCBI\_C\_PATH/lib[64] or -L$NCBI\_C\_PATH/altlib[64]                                                           | Path to NCBI C Toolkit libs.      |
| NCBI\_C\_ncbi      | -lncbi                                                                                                            | NCBI C Toolkit CoreLib            |
| NCBI\_SSS\_INCLUDE | -I$NCBI\_SSS\_PATH/include                                                                                        | \#include NCBI SSS headers        |
| NCBI\_SSS\_LIBPATH | -L$NCBI\_SSS\_PATH/lib/.... ....{Release\\|Debug}[GNU][64][mt]                                                    | Link with NCBI SSS libs.          |
| NCBI\_GEO\_INCLUDE | -I$NCBI\_GEO\_PATH/include                                                                                        | \#include NCBI GEO headers        |
| NCBI\_GEO\_LIBPATH | -L$NCBI\_GEO\_PATH/lib/.... ...[GCC-\\|KCC-\\|ICC-]{Release\\|Debug}[64]                                          | Link with NCBI GEO libs.          |
| SP\_INCLUDE        | -I$SP\_PATH/include                                                                                               | \#include SP headers              |
| SP\_LIBS           | -L$SP\_PATH/{Release\\|Debug}[MT][64] -lsp                                                                        | Link with the SP lib.             |
| NCBI\_PM\_PATH     | $NCBI\_PM\_PATH                                                                                                   | Path to the PubMed package.       |
| ORBACUS\_INCLUDE   | -I$ORBACUS\_PATH/include -I$ORBACUS\_PATH/{Release\\|Debug}[MT][64]/inc                                           | \#include ORBacus CORBA headers   |
| ORBACUS\_LIBPATH   | -L$ORBACUS\_PATH/{Release\\|Debug}[MT][64]/lib                                                                    | Link with ORBacus CORBA libs.     |

<span class="nctnt highlight">Note:</span> The file <span class="nctnt ncbi-path">src/build-system/config.site</span> may also be [edited](#edited) to simplify localization of third party libraries, especially for users outside NCBI.

##### Naming the Build Tree

The configuration process will produce the new *build tree* in a subdirectory of the root source directory. The default base name of this subdirectory will reflect the compiler name and a *Release/Debug* suffix, e.g., *GCC-Release/*. The default *build tree* name can be alternated by passing the following flags to the ***configure*** script:

*--without-suffix* - do not add *Release/Debug*, **MT**, and/or **DLL** suffix(es) to the *build tree* name. **Example:** *GCC/* instead of *GCC-ReleaseMT/*

*--with-hostspec* - add full host specs to the *build tree* name. **Example:** *GCC-Debug--i586-pc-linux-gnu/*

*--with-build-root=/home/foo/bar* - specify your own *build tree* path and name.

With *--with-build-root=*, you still can explicitly use *--with-suffix* and *--with-hostspec* to add suffix(s) to your *build tree* name in a manner described above.

**Example:** *--with-build-root=/home/foo/bar--with-mt --with-suffix* would deploy the new *build tree* in */home/foo/bar-DebugMT*.

There is also a special case with "*--with-build-root=."* for those who prefer to put object files, libraries, and executables in the same directory as the sources. But be advised that this will not allow you to configure other *build trees*.

##### Hard-Coding Run-Time DLL Paths into Executables and DLLs

To be able to run executables linked against dynamic libraries (DLLs), you have to specify the location (runpath) of the DLLs. It can be done by hard-coding (using linker flags such as*-R.....*) the *runpath* into the executables.

*--with-runpath* - hard-code the path to the *lib/* dir of the Toolkit *build tree*.

*--with-runpath=/foo/bar* - hard-code the path to the user-defined */foo/bar* dir.

*--without-runpath* - do not hard-code any *runpath*.

default: if *--without-dll* flag is specified, then act as if *--without-runpath* was specified; otherwise, engage the *--with-runpath* scenario.

The makefile macro <span class="nctnt ncbi-macro">ncbi\_runpath</span> will be set to the resulting runpath, if any.

<span class="nctnt highlight">Note:</span> When running an executable you also can use environment variable <span class="nctnt ncbi-var">$LD\_LIBRARY\_PATH</span> to specify the runpath, like this:

    env LD_LIBRARY_PATH="/home/USERNAME/c++/WorkShop6-ReleaseDLL/lib" \
    /home/USERNAME/c++/WorkShop6-ReleaseDLL/bin/coretest

**HINT:** The *--with-runpath=....* option can be useful to build production DLLs and executables, which are meant to use production DLLs. The latter are usually installed not in the <span class="nctnt ncbi-path">lib/</span> dir of your development tree (*build tree*) but at some well-known dir of your production site. Thus, you can do the development in a "regular" manner (i.e., in a *build tree* configured using only *--with-runpath*); then, when you want to build a production version (which is to use, let's say, DLLs installed in <span class="nctnt ncbi-path">"/some\_path/foo/ </span>*"*), you must reconfigure your C++ build tree with just the same options as before, plus *"--with-runpath=/some\_path/foo"*. Then rebuild the DLLs and executables and install them into production. Then re-reconfigure your *build tree* back with its original flags (without the "*--with-runpath* <span class="nctnt ncbi-path">=/some\_path/foo </span>*"*) and continue with your development cycle, again using local in-tree DLLs.

See [this example](ch_proj.html#ch_proj.make_proj_lib) for more information about building shared libraries.

##### Automatic Generation of Dependencies (for GNU make only)

*--with-autodep* - add build rules to automatically generate dependencies for the compiled C/C++ sources.

*--without-autodep* - do not add these rules.

default: detect if the *make* command actually calls GNU *make*; if it does, then *--with-autodep*, else *--with-autodep*

Also, you can always switch between these two variants "manually", after the configuration is done, by setting the value of the variable Rules in [Makefile.mk](ch_build.html#ch_build.build_make_macros) to either <span class="nctnt ncbi-var">rules</span> or <span class="nctnt ncbi-var">rules\_with\_autodep</span>.

<span class="nctnt highlight">Note:</span> You **must** use GNU *make* if you configured with *--with-autodep*, because in this case the makefiles would use very specific GNU *make* features!

##### After-Configure User Callback Script

You can specify your own script to call from the ***configure*** script after the configuration is complete:

*--with-extra-action="\<some\_action\>"*

where *\<some\_action\>* can be some script with parameters. The trick here is that in the *\<some\_action\>* string, all occurrences of "**{}**" will be replaced by the build dir name.

**Example:**

    configure --with-extra-action="echo foobar {}"

will execute (after the configuration is done):

    echo foobar /home/user/c++/GCC-Debug

##### Tools and Flags

There is a predefined set of tools and flags used in the build process. The user can customize these tools and flags by setting the environment variables shown in [Table 1](#table-1) for the ***configure*** script. For example, if you intend to debug the Toolkit with Insure++, you should run ***configure*** with <span class="nctnt ncbi-var">CC</span> and <span class="nctnt ncbi-var">CXX</span> set to <span class="nctnt ncbi-var">insure</span>.

[Later](ch_build.html#ch_build.build_make_macros), these tools and flags will be engaged in the makefile build rules, such as:

-   To compile C sources: <span class="nctnt ncbi-cmd">$(CC) -c $(CFLAGS) $(CPPFLAGS)....</span>

-   To compile C++ sources: <span class="nctnt ncbi-cmd">$(CXX) -c $(CXXFLAGS) $(CPPFLAGS)....</span>

-   To compose a library: <span class="nctnt ncbi-cmd">$(AR) libXXX.a xxx1.o xxx2.o xxx3.o .....$(RANLIB) libXXX.a</span>

-   To link an executable: <span class="nctnt ncbi-cmd">$(LINK) $(LDFLAGS) ..... $(LIBS)</span>

For more information on these and other variables, see [the GNU autoconf documentation](http://www.gnu.org/software/autoconf/manual/autoconf.html). The specified tools and flags will then be "frozen" inside the makefiles of *build tree* produced by this ***configure*** run.

##### Prohibiting the Use of Some of the System and Third-Party Packages

Some of the above system and third-party packages can be prohibited from use by using the following ***configure*** flags:

*--without-sybase* (Sybase)

*--without-ftds* (FreeTDS)

*--without-fastcgi* (FastCGI)

*--without-fltk* (FLTK)

*--without-wxwin* (wxWindows)

*--without-ncbi-c* (NCBI C Toolkit)

*--without-sssdb* (NCBI SSS DB)

*--without-sssutils* (NCBI SSS UTILS)

*--without-sss* (both *--without-sssdb* and *--without-sssutils*)

*--without-geo* (NCBI GEO)

*--without-sp* (NCBI SP)

*--without-pubmed* (NCBI PubMed)

*--without-orbacus* (ORBacus CORBA)

[and MANY more; ./configure --help \\| grep ‘\\-\\-without-‘ will give a current list for both this and the following heading.]

##### Optional Projects

You can control whether to build the following core packages using the following ***configure*** flags:

*--without-serial* -- do not build C++ ASN.1 serialization library and [datatool](ch_app.html#ch_app.datatool); see in <span class="nctnt ncbi-path">internal/c++/{ </span>[src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/serial) *\\|* [include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/serial)<span class="nctnt ncbi-path">}/serial</span> directories

*--without-ctools* -- do not build projects that use NCBI C Toolkit see in <span class="nctnt ncbi-path">internal/c++/{ </span>[src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/ctools) *\\|* [include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/ctools)<span class="nctnt ncbi-path">}/ctools</span> directories

*--without-gui* -- do not build projects that use wxWindows GUI package see in <span class="nctnt ncbi-path">internal/c++/{ </span>[src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/gui) *\\|* [include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/gui)<span class="nctnt ncbi-path">}/gui</span> directories

*--with-objects* -- generate and build libraries to serialize ASN.1 objects; see in <span class="nctnt ncbi-path">internal/c++/{ </span>[src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/objects) *\\|* [include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objects)<span class="nctnt ncbi-path">}/objects</span> directories

*--with-internal* -- build of internal projects is by default disabled on most platforms; see in <span class="nctnt ncbi-path">internal/c++/{ </span>[src](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/internal) *\\|* [include](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/include/internal)<span class="nctnt ncbi-path">}/internal</span> directories

##### Miscellaneous: --without-exe, --without-execopy, --with-lib-rebuilds(=ask)

*--without-exe* -- do not build the executables enlisted in the <span class="nctnt ncbi-var">APP\_PROJ</span>.

*--without-execopy* -- do not copy (yet build) the executables enlisted in the <span class="nctnt ncbi-var">APP\_PROJ</span>.

*--with-lib-rebuilds* -- when building an application, attempt to rebuild all of the libraries it uses in case they are out of date.

*--with-lib-rebuilds=ask* -- as above, but prompt before any needed rebuilds. (Do not prompt for libraries that are up to date.)

Here's a more detailed explanation of *--with-lib-rebuilds*: There are three modes of operation:

In the default mode (*--without-lib-rebuilds*), starting a build from within a subtree (such as internal) will not attempt to build anything outside of that subtree.

In the unconditional mode (*--with-lib-rebuilds*), building an application will make the system rebuild any libraries it requires that are older than their sources. This can be useful if you have made a change that affects everything under objects but your project only needs a few of those libraries; in that case, you can save time by starting the build in your project's directory rather than at the top level.

The conditional mode (*--with-lib-rebuilds=ask*) is like the unconditional mode, except that when the system discovers that a needed library is out of date, it asks you about it. You can then choose between keeping your current version (because you prefer it or because nothing relevant has changed) and building an updated version.

##### Quick Reconfiguration

Sometimes, you change or add configurables (<span class="nctnt ncbi-path">\*.in</span> files, such as *Makefile.in* meta-makefiles) in the *source tree*.

For the *build tree* to pick up these changes, go to the appropriate build directory and run the script ***reconfigure.sh***. It will automatically use just the same command-line arguments that you used for the original configuration of that *build tree*.

Run ***reconfigure.sh*** with argument:

*update* - if you did not add or remove any *configurables* in the *source tree* but only modified some of them.

*reconf* - if you changed, added, and/or removed any *configurables* in the *source tree*.

*recheck* - if you also suspect that your working environment (compiler features, accessibility of third-party packages, etc.) might have changed since your last (re)configuration of the *build tree* and, therefore, you do not want to use the cached check results obtained during the last (re)configuration.

**without arguments** - printout of script usage info.

**Example:**

    cd /home/foobar/c++/GCC-Debug/build
    ./reconfigure.sh reconf

Naturally, *update* is the fastest of these methods, *reconf* is slower, and *recheck* (which is an exact equivalent of re-running the ***configure*** script with the same command-line arguments as were provided during the original configuration) is the slowest.

#### Building with Unix

Following are some examples of how to build specific projects and some additional topics:

-   [General Principles for Building with Unix](#general-principles-for-building-with-unix)

-   [Building Only Core Libraries and Applications](#building-only-core-libraries-and-applications)

-   [Building GUI Libraries and Applications](#building-gui-libraries-and-applications)

-   [Building the Genome Workbench](#building-the-genome-workbench)

-   [Building the Entire Toolkit](#building-the-entire-toolkit)

##### General Principles for Building with Unix

Use this key for the examples in the “Building with Unix” sections:

|------------------------|-------------------------------------------------------------------------------------------|
| $YOUR\_WORK\_DIR       | your directory corresponding to the top-level c++ directory in the source tree            |
| $YOUR\_CONFIG\_OPTIONS | any optional configuration options you’ve chosen                                          |
| --with-flat-makefile   | creates a makefile that can build all or selected projects                                |
| --without-internal     | excludes NCBI-internal projects from the makefile                                         |
| --without-gui          | excludes FLTK-based projects from the makefile                                            |
| --with-gbench          | ensures that the makefile will contain everything necessary to build the Genome Workbench |
| GCC401-Debug           | will be replaced based on the compiler and configuration options you’re using             |
| gui/                   | selects the GUI libraries target in the flat makefile                                     |
| gui/app/               | selects the sub-tree containing the primary Genome Workbench executable and its helpers   |
| all\_r                 | selects a recursive build of all targets at this and lower levels in the source tree      |

The [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh) script builds a single project in the working directory while referencing the rest of a pre-built Toolkit for all other Toolkit components. For example, to build only the <span class="nctnt ncbi-path">app/id2\_fetch</span> application and have the rest of the pre-built Toolkit available, use these commands:

    mkdir $YOUR_WORK_DIR
    cd $YOUR_WORK_DIR
    import_project app/id2_fetch
    cd trunk/c++/src/app/id2_fetch
    make

The [update\_projects](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh) script builds a single project and all the components it depends on in the working directory, and does not reference or build any other Toolkit components. For example, to build only the <span class="nctnt ncbi-path">corelib</span> project, use these commands:

    mkdir $YOUR_WORK_DIR
    cd $YOUR_WORK_DIR
    update_projects corelib .

The <span class="nctnt ncbi-path">update\_projects</span> script will automatically retrieve updated source code and then prompt you for configuring, compiling, building tests, and running tests.

To run a test suite after building, use this additional command:

    make check_r

##### Building Only Core Libraries and Applications with Unix

    cd $YOUR_WORK_DIR
    ./configure –-without-gui -–without-internal $YOUR_CONFIG_OPTIONS
    cd GCC401-Debug/build
    make all_r

##### Building GUI Libraries and Applications with Unix

    cd $YOUR_WORK_DIR
    ./configure $YOUR_CONFIG_OPTIONS --with-flat-makefile
    cd GCC401-Debug/build
    make -f Makefile.flat gui/

##### Building the Genome Workbench with Unix

    cd $YOUR_WORK_DIR
    ./configure $YOUR_CONFIG_OPTIONS --with-flat-makefile --with-gbench
    cd GCC401-Debug/build
    make -f Makefile.flat gui/app/
    (cd gui/app/gbench_install && make)

##### Building the Entire Toolkit with Unix

    cd $YOUR_WORK_DIR
    ./configure $YOUR_CONFIG_OPTIONS
    cd GCC401-Debug/build
    make all_r

#### Using the Toolkit with Unix

This section discusses the following examples of how to use the Toolkit with Unix:

-   [Modify or Debug an Existing Toolkit Application](#modify-or-debug-an-existing-toolkit-application)

-   [Modify or Debug an Existing Toolkit Library](#modify-or-debug-an-existing-toolkit-library)

##### Modify or Debug an Existing Toolkit Application with Unix

If you want to modify or debug an application (e.g. <span class="nctnt ncbi-path">gi2taxid</span>) start with these commands:

    cd $YOUR_WORK_DIR
    import_project app/gi2taxid

You will be prompted to select a desired stability and configuration and then the script will create the include and src trees necessary to work on the chosen application. It will also create all the necessary makefiles to build the application. The makefiles will be configured to use the latest nightly build of the chosen stability and configuration to resolve all dependencies outside the chosen application.

You can now edit, build, and/or debug the application:

    cd trunk/c++/src/app/gi2taxid
    # if you want to make changes, edit the desired file(s)
    make all_r
    # if desired, debug using your favorite debugger

##### Modify or Debug an Existing Toolkit Library with Unix

If you want to modify or debug a library (e.g. <span class="nctnt ncbi-path">corelib</span>) start with these commands:

    cd $YOUR_WORK_DIR
    import_project corelib

You will be prompted to select a desired stability and configuration and then the script will create the include and src trees necessary to work on the chosen library. It will also create all the necessary makefiles to build the library. The makefiles will be configured to use the latest nightly build of the chosen stability and configuration to resolve all dependencies outside the chosen library.

You can now edit, build, and/or debug (via some application) the library:

    cd trunk/c++/src/corelib
    # if you want to make changes, edit the desired file(s)
    make all_r
    # if you want to debug the library, build a dependent application
    # then debug using your favorite debugger

### Special Considerations for Specific Unix Platforms

Most of the non-GCC compilers require special tools and additional mandatory flags to compile and link C++ code properly. That's why there are special scripts that perform the required non-standard, compiler-specific pre-initialization for the [tools and flags](#tools-and-flags) used before running ***configure***.

These wrapper scripts are located in the *compilers/* directory, and now we have such wrappers for the <span class="nctnt ncbi-monospace">SUN WorkShop</span> (<span class="nctnt ncbi-monospace">5.5</span> through <span class="nctnt ncbi-monospace">5.9)</span>, <span class="nctnt ncbi-monospace">GCC</span> and <span class="nctnt ncbi-monospace">ICC</span> compilers:

-   <span class="nctnt ncbi-monospace">WorkShop.sh</span> {32\\|64} [build\_dir] [--configure-flags]

-   <span class="nctnt ncbi-monospace">WorkShop55.sh</span> {32\\|64} [build\_dir] [--configure-flags]

-   <span class="nctnt ncbi-monospace">ICC.sh</span> [build\_dir] [--configure-flags]

Note that these scripts accept all regular ***configure*** flags and then pass them to the ***configure*** script.

The following topics are discussed in this section:

-   [Linux / ICC](#linux--icc)

-   [Cygwin / GCC](#cygwin--gcc)

#### Linux / ICC

To build a project on Linux / ICC, just follow the generic [Unix guidelines](#unix-guidelines) but instead of running the <span class="nctnt ncbi-path">./configure.sh</span> script you will need to run <span class="nctnt ncbi-path">compilers/unix/ICC.sh</span>.

#### Cygwin / GCC

To build a project on Cygwin / GCC, just follow the generic [Unix guidelines](#unix-guidelines) but instead of running the <span class="nctnt ncbi-path">./configure.sh</span> script you will need to run <span class="nctnt ncbi-path">compilers/cygwin/build.sh</span>.

MS Windows
----------

<span class="nctnt highlight">Note:</span> Please also see the [General Information for All Platforms](#general-information-for-all-platforms) section, as it contains relevant information that is not repeated here.

The following topics are discussed in this section:

-   [MS Visual C++](#ms-visual-c++)

    -   [Choosing a Build Scope](#choosing-a-build-scope)

    -   [Configuring](#configuring)

    -   [Building](#building)

    -   [Using](#using)

-   [Cygwin / GCC](#cygwin--gcc)

### MS Visual C++

The following topics are discussed in this section:

-   [Choosing a Build Scope](#choosing-a-build-scope)

-   [Configuring](#configuring)

    -   [Site-Specific Build Tree Configuration](#site-specific-build-tree-configuration)

    -   [Fine-Tuning with MSVC Project Files](#fine-tuning-with-msvc-project-files)

        -   [Excluding project from the build](#excluding-project-from-the-build)

        -   [Adding files to project](#adding-files-to-project)

        -   [Excluding files from project](#excluding-files-from-project)

        -   [Adjusting build tools settings](#adjusting-build-tools-settings)

        -   [Specifying custom build rules](#specifying-custom-build-rules)

    -   [DLL Configuration](#dll-configuration)

    -   [Fine-Tuning with Environment Variables](#fine-tuning-with-environment-variables)

-   [Building](#building)

    -   [Building a Custom Solution](#building-a-custom-solution)

    -   [Building External Libraries (Optional)](#building-external-libraries-optional)

    -   [The Build Results](#the-build-results)

-   [Using](#using)

    -   [Start a new project that uses the Toolkit](#start-a-new-project-that-uses-the-toolkit)

    -   [Start a new project in the Toolkit](#start-a-new-project-in-the-toolkit)

    -   [Modify or Debug an existing project in the Toolkit](#modify-or-debug-an-existing-project-in-the-toolkit)

#### Choosing a Build Scope with Visual C++

The Toolkit is very large and you may not want to retrieve and build the entire Toolkit if you don’t need to. Therefore, after [preparing the development environment](ch_faq.html#ch_faq.How_do_I_prepare_my_development_e) and [getting the source files](ch_getcode_svn.html), you'll need to choose a build scope. Several mechanisms are provided to enable working with only a portion of the Toolkit.

If you are interested in building only one project, you can limit the source code [retrieved from the repository](ch_getcode_svn.html):

-   using the shell script [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh); or

-   using the shell script [update\_projects](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh).

You can also limit what will be built by choosing a standard solution. Five standard solutions are provided to enable working only with selected portions of the Toolkit.

<span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\static\\build\\ncbi\_cpp.sln</span>

<span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\dll\\build\\ncbi\_cpp.sln</span>

<span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\static\\build\\gui\\ncbi\_gui.sln</span>

<span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\dll\\build\\gui\\ncbi\_gui.sln</span>

<span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\dll\\build\\gbench\\ncbi\_gbench.sln</span>

The first two solutions build console applications and required libraries only; the last three solutions build GUI applications.

You can also choose between static and shared libraries. Building with static libraries will result in much larger applications and require much more disk space. Using static libraries is not an option for the Genome Workbench.

If you want to build the entire Toolkit, choose

#### Configuring with Visual C++

Once you have [chosen a build scope](#chosen-a-build-scope), you are ready to configure.

If you used either the <span class="nctnt ncbi-path">import\_project</span> script or the <span class="nctnt ncbi-path">update\_projects</span> script then you don’t need to configure because both of those scripts use existing configurations.

If you chose a standard solution then you will need to configure. Each standard solution contains a special project called **-CONFIGURE-** which is used for generating a Visual Studio project file based on Unix-style makefile templates <span class="nctnt ncbi-path">src\\....\\Makefile.\*</span>

The Visual Studio specific configuration files are:

-   [src\\build-system\\Makefile.mk.in.msvc](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/Makefile.mk.in.msvc)

-   [src\\build-system\\project\_tree\_builder.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/project_tree_builder.ini)

-   [src\\....\\Makefile.\*.msvc](#src\\makefile*msvc)

Each of the standard solutions use a predefined list of projects to build, which is taken from <span class="nctnt ncbi-path">scripts\\projects\\\*.lst files</span>.

To configure and generate the project list, open the chosen solution, select the desired configuration, right click on the **-CONFIGURE-** project, and click 'Build'. This will rewrite the project file that Visual C++ is currently using, so you should see one or more dialog boxes similar to this:

<span>[![Image ch\_config\_proj\_mod\_reload.png](/book-test/static/img/ch_config_proj_mod_reload.png)](/book-test/static/img/ch_config_proj_mod_reload.png "Click to see the full-resolution image")</span>

<span class="nctnt highlight">Note:</span> At least one such dialog will typically appear *before* the configuration is complete. Therefore, you need to wait until you see the message:

    ******************************************************************************
    ==============  It is now safe to reload the solution:          ==============
    ==============  Please, close it and open again                 ==============
    ******************************************************************************

in the Output window before reloading. Once this message appears, you can either click "Reload" or click "Ignore" and then manually close and reopen the solution. The reloaded solution will list all configured projects.

A configuration tool with a Java-based GUI is also available and can be launched by building the **-CONFIGURE-DIALOG-** project. For more information on using the configuration GUI, see the [general section on configuring](#general-section-on-configuring).

The following topics discuss configuring with Visual C++ in more detail:

-   [Site-Specific Build Tree Configuration](#site-specific-build-tree-configuration)

-   [Fine-Tuning with MSVC Project Files](#fine-tuning-with-msvc-project-files)

    -   [Excluding a Project From the Build](#excluding-a-project-from-the-build)

    -   [Adding Files to a Project](#adding-files-to-a-project)

    -   [Excluding Files From a Project](#excluding-files-from-a-project)

    -   [Adjusting Build Tools Settings](#adjusting-build-tools-settings)

    -   [Specifying Custom Build Rules](#specifying-custom-build-rules)

-   [DLL Configuration](#dll-configuration)

-   [Fine-Tuning with Environment Variables](#fine-tuning-with-environment-variables)

##### Site-Specific Build Tree Configuration

File [project\_tree\_builder.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/project_tree_builder.ini) (see [Table 4](#table-4)) describes build and source tree configurations, contains information about the location of 3rd-party libraries and applications, and includes information used to resolve macro definitions found in the Unix-style makefile templates.

Table 4. Project Tree Builder INI file (Local Site)

| Section                          | Key                                                                                                                                                                     | Comments                                                                                                                                                                                    |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Configure]                      | ThirdPartyBasePath,<br/>ThirdParty\_\*<br/>ThirdPartyAppsBasePath<br/>ThirdParty\_C\_ncbi | Location of 3<sup>rd</sup> party libraries and applications                                                                                                                                 |
|              | ProvidedRequests<br/>StandardFeatures                                                                                                         | List of requirements from Unix makefiles that are always met                                                                                                                                |
|              | NotProvidedRequests                                                                                                                                                     | List of requirements from Unix makefiles that are never met. Projects with that require any one of these, will be excluded                                                                  |
|              | DefinesPath                                                                                                                                                             | Path to .h file that will contain HAVE\_XXXX definitions. The path is relative from the project tree root.                                                                                  |
|              | Defines                                                                                                                                                                 | List of HAVE\_XXXX preprocessor definitions.                                                                                                                                                |
|              | Macros                                                                                                                                                                  | List of optional macros. Definition of any such macro depends upon availability of Components                                                                                               |
|              | LibChoices                                                                                                                                                              | List of pairs \<libID\>/\<Component\>. If the third-party library \<Component\> is present, then this library will be used instead of the internal library \<libID\>.                       |
|              | ThirdPartyLibsBinPathSuffix                                                                                                                                             | Part of the naming convention for third-party DLLs installation makefile.                                                                                                                   |
|              | ThirdPartyLibsBinSubDir                                                                                                                                                 | Part of the third-party DLLs installation target location.                                                                                                                                  |
|              | ThirdPartyLibsToInstall                                                                                                                                                 | List of components, which DLLs will be automatically installed in the binary build directory.                                                                                               |
| [ProjectTree]                    | MetaData                                                                                                                                                                | Makefile.mk.in - in this file the project tree builder will be looking for the Unix project tree macro definitions.                                                                         |
|              | include                                                                                                                                                                 | include "include" branch of project tree                                                                                                                                                    |
|              | src                                                                                                                                                                     | src "src" branch                                                                                                                                                                            |
|              | dll                                                                                                                                                                     | Subdirectory with DLL Makefiles                                                                                                                                                             |
|              | compilers                                                                                                                                                               | compilers "compilers" branch                                                                                                                                                                |
|              | projects                                                                                                                                                                | scripts/projects "projects" branch                                                                                                                                                          |
|              |                                                                                                                                                     |                                                                                                                                                                         |
| [msvc\*]                         | Configurations                                                                                                                                                          | List of buid configurations that use static runtime libraries                                                                                                                               |
|              |                                                                                                                                                     | List of build configurations that use dynamic runtime libraries                                                                                                                             |
|              | msvc\_prj                                                                                                                                                               | Sub-branch of compilers branch for MSVC projects                                                                                                                                            |
|              | MakefilesExt                                                                                                                                                            | Extension of MSVC-specific makefiles                                                                                                                                                        |
|              | Projects                                                                                                                                                                | "build" sub-branch                                                                                                                                                                          |
|              | MetaMakefile                                                                                                                                                            | Master .msvc makefile - Makefile.mk.in.msvc                                                                                                                                                 |
| [LibChoicesIncludes]             | CMPRS\_INCLUDE et al.                                                                                                                                                   | Definition for the include directories for LibChoices.                                                                                                                                      |
| [Defines]                        |                                                                                                                                                     | Contains definition of macros from Unix makefiles that cannot be resolved otherwise                                                                                                         |
| [HAVE\_XXXX]                     | Component                                                                                                                                                               | List of the components to check. An empty list means that the component is always available. A non-empty list means that the component(s) must be checked on presentation during configure. |
|              |                                                                                                                                                     |                                                                                                                                                                         |
| [Debug],[DebugDLL],etc...        | debug                                                                                                                                                                   | TRUE means that the debug configuration will be created.                                                                                                                                    |
|              | runtimeLibraryOption                                                                                                                                                    | C++ Runtime library to use.                                                                                                                                                                 |
|              |                                                                                                                                                     |                                                                                                                                                                         |
| [NCBI\_C\_LIBS],[FLTK\_LIBS\_GL] | Component                                                                                                                                                               | List of libraries to use.                                                                                                                                                                   |
| [\<LIBRARY\>]                    | INCLUDE                                                                                                                                                                 | Include path to the library headers.                                                                                                                                                        |
|              | DEFINES                                                                                                                                                                 | Preprocessor definition for library usage.                                                                                                                                                  |
|              | LIBPATH                                                                                                                                                                 | Path to library.                                                                                                                                                                            |
|              | LIB                                                                                                                                                                     | Library files.                                                                                                                                                                              |
|              | CONFS                                                                                                                                                                   | List of supported configurations.                                                                                                                                                           |
| [DefaultLibs]                    | INCLUDE                                                                                                                                                                 | Default libraries will be added to each project. This section is to negotiate the differences in the default libraries on the Unix and Windows platforms. Same as for [\<LIBRARY\>].        |
|              | LIBPATH                                                                                                                                                                 | Same as for [\<LIBRARY\>].                                                                                                                                                                  |
|              | LIB                                                                                                                                                                     | Same as for [\<LIBRARY\>].                                                                                                                                                                  |
|              |                                                                                                                                                     |                                                                                                                                                                         |
| [Datatool]                       | datatool                                                                                                                                                                | ID of the datatool project. Some projects (with ASN or DTD sources) depend on the datatool.                                                                                                 |
|              | Location.App                                                                                                                                                            | Location of datatool executable for APP projects.                                                                                                                                           |
|              | Location.Lib                                                                                                                                                            | Location of datatool executable for LIB projects.                                                                                                                                           |
|              | CommandLine                                                                                                                                                             | Partial command line for datatool.                                                                                                                                                          |

Toolkit project makefiles can list (in a pseudo-macro entry called '<span class="nctnt ncbi-monospace">REQUIRES</span>') a set of requirements that must be met in order for the project to be built. For example, a project can be built only on Unix, or only in multi-thread mode, or if a specific external library is available. Depending on which of the requirements are met, the Toolkit configurator may exclude some projects in some (or all) build configurations or define preprocessor and/or makefile macros.

Some of the Toolkit projects can be built differently depending on the availability of non-Toolkit components. For them, there is a list of macros - defined in '<span class="nctnt ncbi-monospace">Defines</span>' entry - that define conditional compilation. To establish a link between such a macro and a specific component, the configuration file also has sections with the names of the macro. For each build configuration, project tree builder creates a header file (see '<span class="nctnt ncbi-monospace">DefinesPath</span>' entry) and defines these macros there depending on the availability of corresponding components.

Many of the requirements define dependency on components that are 3rd-party packages, such as BerkeleyDB. For each one of these there is a special section (e.g. [BerkeleyDB]) in [project\_tree\_builder.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/project_tree_builder.ini) that describes the path(s) to the <span class="nctnt ncbi-path">include</span> and <span class="nctnt ncbi-path">library</span> directories of the package, as well as the preprocessor definitions to compile with and the libraries to link against. The Toolkit configurator checks if the package's directories and libraries do exist, and uses this information when generating appropriate MSVS projects.

There are a few indispensable external components that have analogs in the Toolkit. If the external component is not found, the analog in the Toolkit is used. The '<span class="nctnt ncbi-monospace">LibChoices</span>' entry identifies such pairs, and '<span class="nctnt ncbi-monospace">LibChoiceIncludes</span>' provides additional include paths to the builtin headers.

<span class="nctnt highlight">Note:</span> There are some requirements which, when building for MS Visual Studio, are always or never met. These requirements are listed in '<span class="nctnt ncbi-monospace">ProvidedRequests</span>', '<span class="nctnt ncbi-monospace">StandardFeatures</span>', or '<span class="nctnt ncbi-monospace">NotProvidedRequests</span>' of the '<span class="nctnt ncbi-monospace">Configure</span>' section.

##### Fine-Tuning with MSVC Project Files

While default MSVS project settings are defined in the [Makefile.mk.in.msvc](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/Makefile.mk.in.msvc) file, each project can require additional MSVC-specific fine-tuning, such as compiler or linker options, additional source code, etc. These tune-ups can be specified in <span class="nctnt ncbi-path">Makefile.\<project\_name\>.[ </span>[lib](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/corelib/Makefile.corelib.lib.msvc)\\|[app](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/Makefile.dbapi_advanced_features.app.msvc)<span class="nctnt ncbi-path">].msvc</span> file located in the project source directory. All entries in such <span class="nctnt ncbi-path">\*.msvc</span> file are optional.

Any section name can have one or several optional suffixes, so it can take the following forms:

-   <span class="nctnt ncbi-monospace">SectionName</span>

-   <span class="nctnt ncbi-monospace">SectionName.CompilerVersion</span>

-   <span class="nctnt ncbi-monospace">SectionName.Platform</span>

-   <span class="nctnt ncbi-monospace">SectionName.[static\\|dll]</span>

-   <span class="nctnt ncbi-monospace">SectionName.[debug\\|release]</span>

-   <span class="nctnt ncbi-monospace">SectionName.CompilerVersion.[debug\\|release]</span>

-   <span class="nctnt ncbi-monospace">SectionName.[static\\|dll].[debug\\|release]</span>

-   <span class="nctnt ncbi-monospace">SectionName.[debug\\|release].ConfigurationName</span>

-   <span class="nctnt ncbi-monospace">SectionName.[static\\|dll].[debug\\|release].ConfigurationName</span>

|------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| <span class="nctnt ncbi-monospace">CompilerVersion</span>                                            | 1000 (i.e. MSVC 2010)                                  |
| <span class="nctnt ncbi-monospace">Platform</span>                                                   | Win32 or x64                                           |
| <span class="nctnt ncbi-monospace">static</span> or <span class="nctnt ncbi-monospace">dll</span>    | type of runtime libraries                              |
| <span class="nctnt ncbi-monospace">debug</span> or <span class="nctnt ncbi-monospace">release</span> | build configuration type                               |
| <span class="nctnt ncbi-monospace">ConfigurationName</span>                                          | build configuration name (e.g. DebugDLL, or ReleaseMT) |

Settings in sections with more detailed names (ones that appear later on this list) override ones in sections with less detailed names (ones that appear earlier).

<span class="nctnt highlight">Note:</span> After changing settings, you will need to [reconfigure](#reconfigure) and reload the solution for the change to take effect.

The following topics discuss further fine-tuning with MSVC project files:

-   [Excluding a Project From the Build](#excluding-a-project-from-the-build)

-   [Adding Files to a Project](#adding-files-to-a-project)

-   [Excluding Files From a Project](#excluding-files-from-a-project)

-   [Adjusting Build Tools Settings](#adjusting-build-tools-settings)

-   [Specifying Custom Build Rules](#specifying-custom-build-rules)

##### Excluding a Project From the Build

To exclude a project from the build, set the '<span class="nctnt ncbi-monospace">ExcludeProject</span>' entry in the '<span class="nctnt ncbi-monospace">Common</span>' section:

-   <span class="nctnt ncbi-monospace">[Common]</span>

-   <span class="nctnt ncbi-monospace">ExcludeProject=TRUE</span>

##### Adding Files to a Project

To add files to a project, add entries to the '<span class="nctnt ncbi-monospace">AddToProject</span>' section. The section can have the following entries:

-   <span class="nctnt ncbi-monospace">[AddToProject]</span>

-   <span class="nctnt ncbi-monospace">HeadersInInclude=</span>

-   <span class="nctnt ncbi-monospace">HeadersInSrc=</span>

-   <span class="nctnt ncbi-monospace">IncludeDirs=</span>

-   <span class="nctnt ncbi-monospace">LIB=</span>

-   <span class="nctnt ncbi-monospace">ResourceFiles=</span>

-   <span class="nctnt ncbi-monospace">SourceFiles=</span>

|------------------------------------------------------------|---------------------------------------------------------------------------|
| <span class="nctnt ncbi-monospace">HeadersInInclude</span> | override default list of headers from include directory                   |
| <span class="nctnt ncbi-monospace">HeadersInSrc</span>     | override default list of headers from source directory                    |
| <span class="nctnt ncbi-monospace">IncludeDirs</span>      | additional include directories (relative to the source directory)         |
| <span class="nctnt ncbi-monospace">LIB</span>              | additional C++ Toolkit libraries (without extension)                      |
| <span class="nctnt ncbi-monospace">ResourceFiles</span>    | MS Windows resource files                                                 |
| <span class="nctnt ncbi-monospace">SourceFiles</span>      | additional (usually MS Windows specific) source files (without extension) |

By default, all header files found in the project's include and source directories are added to the MSVS project. If that's not exactly what you need, the list of headers can be overridden using the '<span class="nctnt ncbi-monospace">HeadersInInclude</span>' and '<span class="nctnt ncbi-monospace">HeadersInSrc</span>' entries. There, file names should be entered with their extension; an exclamation mark means negation; and wildcards are allowed. For example, the entry:

<span class="nctnt ncbi-monospace">HeadersInInclude = \*.h file1.hpp !file2.h</span>

means "add all files with h extension, add <span class="nctnt ncbi-path">file1.hpp</span>, and do not add <span class="nctnt ncbi-path">file2.h</span>".

<span class="nctnt highlight">Note:</span> A single exclamation mark with no file name means "do not add any header files".

All directories given in the '<span class="nctnt ncbi-monospace">IncludeDirs</span>' entry should be specified relative to the source directory. (Absolute paths aren't supported, nor are paths containing special characters such as spaces or commas.) After [reconfiguring](#reconfiguring), these directories are saved in the <span class="nctnt ncbi-monospace">AdditionalIncludeDirectories</span> project property - now relative to <span class="nctnt ncbi-macro">$(ProjectDir)</span>. The following table illustrates this path conversion:

| IncludeDirs Path -<br/>specified relative to source directory | AdditionalIncludeDirectories Path -<br/>saved relative to $(ProjectDir) |
|-----------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-path">somedir</span>                                            | <span class="nctnt ncbi-path">..\\..\\..\\..\\..\\src\\$(SolutionName)\\somedir</span>            |
| <span class="nctnt ncbi-path">..\\\\somedir</span>                                      | <span class="nctnt ncbi-path">..\\..\\..\\..\\..\\src\\somedir</span>                             |
| <span class="nctnt ncbi-path">..\\\\..\\\\somedir</span>                                | <span class="nctnt ncbi-path">..\\..\\..\\..\\..\\somedir</span>                                  |
| <span class="nctnt ncbi-path">..\\\\..\\\\..\\\\somedir</span>                          | <span class="nctnt ncbi-path">..\\..\\..\\..\\..\\..\\somedir</span>                              |
| <span class="nctnt ncbi-path">..\\\\..\\\\..\\\\..\\\\somedir</span>, etc.              | <span class="nctnt ncbi-path">..\\..\\..\\..\\..\\..\\..\\somedir</span>, etc.                    |

Although '<span class="nctnt ncbi-monospace">IncludeDirs</span>' does not support absolute paths, it is possible to add absolute paths by changing the '<span class="nctnt ncbi-monospace">AdditionalOptions</span>' entry in the '<span class="nctnt ncbi-monospace">[Compiler]</span>' section (see [Build Tool Settings](#build-tool-settings)).

Here are some example entries for the '<span class="nctnt ncbi-monospace">AddToProject</span>' section:

    [AddToProject]
    HeadersInInclude = *.h
    HeadersInSrc = task_server.hpp server_core.hpp srv_sync.hpp \
                   srv_stat.hpp
    IncludeDirs=..\\..\\sra\\sdk\\interfaces
    LIB=xser msbuild_dataobj
    ResourceFiles=cn3d.rc
    SourceFiles = sysalloc

##### Excluding Files From a Project

To exclude files from a project, set the '<span class="nctnt ncbi-monospace">SourceFiles</span>' or '<span class="nctnt ncbi-monospace">LIB</span>' entries of the '<span class="nctnt ncbi-monospace">ExcludedFromProject</span>' section.

##### Adjusting Build Tools Settings

The build tools are '<span class="nctnt ncbi-monospace">Compiler</span>', '<span class="nctnt ncbi-monospace">Linker</span>', '<span class="nctnt ncbi-monospace">Librarian</span>', and '<span class="nctnt ncbi-monospace">ResourceCompiler</span>' - that is, the tools used by the MS Visual Studio build system. The names of available entries in any one of these sections can be found in the [Makefile.mk.in.msvc](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/find?string=Makefile.mk.in.msvc) file. For the meaning and possible values of these entries, see Microsoft's [VCProjectEngine reference](http://msdn.microsoft.com/en-us/library/ms168475.aspx), or the specific reference pages for the [VCCLCompilerTool](http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.vcprojectengine.vcclcompilertool.aspx), [VCLinkerTool](http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.vcprojectengine.vclinkertool.aspx), [VCLibrarianTool](http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.vcprojectengine.vclibrariantool.aspx), and [VCResourceCompilerTool](http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.vcprojectengine.vcresourcecompilertool.aspx) Interfaces.

Here are some example settings, with some illustrating how section name suffixes can be used:

    [Compiler]
    AdditionalOptions=/I\"\\\\server\\share\\absolute path with spaces\"

    [Compiler.release]
    Optimization=0
    EnableFunctionLevelLinking=FALSE
    GlobalOptimizations=FALSE

    [Compiler.900]
    PreprocessorDefinitions=UCS2;_CRT_SECURE_NO_DEPRECATE=1;
    [Compiler.900.release]
    PreprocessorDefinitions=UCS2;_SECURE_SCL=0;_CRT_SECURE_NO_DEPRECATE=1;


    [Linker]
    subSystem = 1
    GenerateManifest=true
    EmbedManifest=true
    AdditionalOptions=test1.lib test2.lib \\\\server\\share\\path_no_spaces\\test3.lib

    [Linker.debug]
    OutputFile = $(OutDir)\\python_ncbi_dbapi_d.pyd
    [Linker.release]
    OutputFile = $(OutDir)\\python_ncbi_dbapi.pyd

Relative paths specified in build tool settings are relative to <span class="nctnt ncbi-macro">$(ProjectDir)</span>.

<span class="nctnt highlight">Note:</span> '<span class="nctnt ncbi-monospace">AdditionalOptions</span>' entries are applied when the tool executes - they do not modify other project properties. For example, if you add an include path using '<span class="nctnt ncbi-monospace">AdditionalOptions</span>', it will not affect the '<span class="nctnt ncbi-monospace">AdditionalIncludeDirectories</span>' property, which is used by the IDE. In this case, Visual C++ will not be able to check syntax, lookup definitions, use IntelliSense, etc. for files in that location while you're editing - but they will compile normally. Therefore, use the '<span class="nctnt ncbi-monospace">AddToProject</span>' section (see [above](#above)) for include directories unless you must use an absolute path.

See the [Makefile.mk.in.msvc](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/Makefile.mk.in.msvc) file for the default MSVS project settings.

##### Specifying Custom Build Rules

To specify custom build rules for selected files in the project (usually non C++ files) use the '<span class="nctnt ncbi-monospace">CustomBuild</span>' section. It has a single entry, '<span class="nctnt ncbi-monospace">SourceFiles</span>', which lists one or more files to apply the custom build rules to. Then, create a section with the name of the file, and define the following entries there: '<span class="nctnt ncbi-monospace">Commandline</span>', '<span class="nctnt ncbi-monospace">Description</span>', '<span class="nctnt ncbi-monospace">Outputs</span>', and '<span class="nctnt ncbi-monospace">AdditionalDependencies</span>' - that is, the same entries as in the Custom Build Step of Microsoft Visual Studio project property pages. This data will then be inserted "as is" into the MSVS project file.

##### DLL Configuration

The Toolkit Unix-style makefile templates give a choice of building the library as dynamic or static (or both). However, it is often convenient to assemble a "bigger" DLL made of the sources of several static libraries.

In the Toolkit, such compound DLLs are described using a set of special makefiles in the <span class="nctnt ncbi-path">src/dl</span>l subdirectory. Each such file – <span class="nctnt ncbi-path">Makefile.\*.dll</span> – contains the following entries:

|--------------|---------------------------------------------------|
| DLL          | name of the compound DLL                          |
| HOSTED\_LIBS | names of the included static libraries            |
| DEPENDENCIES | dependencies on other static or dynamic libraries |
| CPPFLAGS     | additional compiler flags, specific for this DLL  |

##### Fine-Tuning with Environment Variables

It is possible to fine-tune the configuration process by using the following environment variables:

-   <span class="nctnt ncbi-var">PREBUILT\_PTB\_EXE</span>

-   <span class="nctnt ncbi-var">PTB\_PROJECT</span>

When the <span class="nctnt ncbi-var">PREBUILT\_PTB\_EXE</span> environment variable defines an existing file (e.g. <span class="nctnt ncbi-path">project\_tree\_builder.exe</span>), this EXE is used. Otherwise, the configuration process builds <span class="nctnt ncbi-app">project\_tree\_builder</span> using existing sources, and then uses this EXE. At NCBI, even when <span class="nctnt ncbi-var">PREBUILT\_PTB\_EXE</span> is not defined, the toolkit still tries to use an external <span class="nctnt ncbi-app">project\_tree\_builder</span> – to speed up the configuration. Normally, this is the most recent successfully built one. To disable such behavior, this variable should be defined and have the value <span class="nctnt ncbi-var">bootstrap</span>:

<span class="nctnt ncbi-var">PREBUILT\_PTB\_EXE=bootstrap</span>

The <span class="nctnt ncbi-var">PTB\_PROJECT</span> environment variable can be used to redefine the default project list. For example, it can be defined as follows:

<span class="nctnt ncbi-var">PTB\_PROJECT=scripts\\projects\\datatool\\project.lst</span>

#### Building with Visual C++

Once you have [chosen a build scope](#chosen-a-build-scope) and have [configured](#configured), you are ready to build. The configure process creates a solution containing all the projects you can build.

To build a library, application, sample, or any other project, simply choose your configuration (e.g. ReleaseDLL), right-click on the desired project, and select "Build". To build all projects in the solution, build the -**BUILD-ALL-** project.

<span class="nctnt highlight">Note:</span> Do not use the 'Build Solution' command because this would include building the **–CONFIGURE-** project, which would result in: (a) reconfiguring (which may not be necessary at the time), and more importantly (b) not building the remaining projects in the solution.

By the way, you can build a desired project by right-clicking on it and selecting build, but debugging applies only to the StartUp Project. To select a project for debugging, right-click the desired project and select "Set as StartUp Project".

Following are some additional build-related topics:

-   [Building a Custom Solution](#building-a-custom-solution)

-   [Building External Libraries (Optional)](#building-external-libraries-optional)

-   [The Build Results](#the-build-results)

##### Building a Custom Solution

This section deals with building a custom solution within the C++ Toolkit source tree. To build a custom solution outside the source tree, please see the section on using the [new\_project](ch_proj.html#ch_proj.new_project_Starting) script.

There is a template solution, <span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\user\\build\\ncbi\_user.sln</span>, that should help you build a customized solution. The project list for this solution is in <span class="nctnt ncbi-path">scripts\\projects\\ncbi\_user.lst</span>

<span class="nctnt highlight">Note:</span> Do not use this solution directly. Instead, make a new solution based on the template:

1  
Make copies of the <span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\user\\</span> subtree and the <span class="nctnt ncbi-path">scripts\\projects\\ncbi\_user.lst</span> file (keep the copies in the same folders as the originals).

2  
Rename the subtree, solution file, and project list file appropriately, for example to <span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\project\_name\\, compilers\\msvc1000\_prj\\project\_name\\build\\project\_name.sln</span>, and <span class="nctnt ncbi-path">scripts\\projects\\project\_name.lst</span>.

3  
In the folder <span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\project\_name\\build\\UtilityProjects\\</span>, use a text editor to edit <span class="nctnt ncbi-path">\_CONFIGURE\_.vcproj</span>, and <span class="nctnt ncbi-path">\_CONFIGURE\_DIALOG\_.vcproj</span>. Change all instances of "<span class="nctnt ncbi-path">ncbi\_user</span>" to "<span class="nctnt ncbi-path">project\_name</span>".

4  
In the same folder, also edit <span class="nctnt ncbi-path">configure.\_</span>, and <span class="nctnt ncbi-path">configure\_dialog.\_</span>:

a  
Change all instances of "<span class="nctnt ncbi-path">ncbi\_user</span>" to "<span class="nctnt ncbi-path">project\_name</span>".

b  
By default, the solution uses static runtime libraries. If you want to use DLL's, also add the <span class="nctnt ncbi-monospace">'-dll</span>' option to the '<span class="nctnt ncbi-monospace">set PTB\_FLAGS=</span>' line.

c  
By default, the solution uses a project list file. If you don't want to use a project list file (e.g. if you want to use a project tag filter instead), also change the '<span class="nctnt ncbi-monospace">set PTB\_PROJECT\_REQ=</span>' line to the appropriate subtree, e.g. '<span class="nctnt ncbi-monospace">set PTB\_PROJECT\_REQ=src\\cgi\\</span>'.

d  
If you want to use a project tag filter, add the <span class="nctnt ncbi-monospace">'-projtag</span>' option to the '<span class="nctnt ncbi-monospace">set PTB\_FLAGS=</span>' line, e.g. '<span class="nctnt ncbi-monospace">set PTB\_FLAGS=-projtag "core && !test"</span>'. See the section on [reducing build scope](#reducing-build-scope) for more information on using project tags.

5  
If your new project will use a project list file, edit <span class="nctnt ncbi-path">scripts\\projects\\project\_name.lst</span> to identify the required project folders.

6  
Your custom solution can now be built. Open the solution file <span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\project\_name\\build\\project\_name.sln</span>, [configure](#configure), and [build](#build).

Note that the project directory, <span class="nctnt ncbi-path">msvc1000\_prj</span>, may be different for your version of Visual C++.

##### Building External Libraries (Optional)

Some of the NCBI C++ Toolkit projects make use of the [NCBI C Toolkit](ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/2008/Mar_17_2008/NCBI_C_Toolkit/README) (not to be confused with the NCBI C++ Toolkit) and/or freely distributed [3rd-party packages](ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/2008/Mar_17_2008/ThirdParty/README) (such as BerkeleyDB, LibZ, FLTK, etc.).

At NCBI, these libraries are already installed, and their locations are [hard coded](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/project_tree_builder.ini) in the C++ Toolkit configuration files. If you are outside of NCBI, you may need to build and install these libraries before building the C++ Toolkit.

Alternatively, the source code for the [NCBI C Toolkit](ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/2008/Mar_17_2008/NCBI_C_Toolkit/README) and the [3rd-party packages](ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/2008/Mar_17_2008/ThirdParty/README) can be downloaded from the NCBI FTP site and built - ideally, in all available configurations.

If you do not have the external libraries already installed, you can download, build, and install the NCBI C Toolkit and the freely distributed 3rd-party packages. The source code for the NCBI C Toolkit and the freely distributed 3rd-party packages can be downloaded from the NCBI FTP site and built in all available configurations. Refer to the documentation on the specific packages you wish to install for more information.

##### The Build Results

The built Toolkit applications and libraries will be put, respectively, to:

<span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\{static\\|dll}\\bin\\\<config\_name\></span>

<span class="nctnt ncbi-path">compilers\\msvc1000\_prj\\{static\\|dll}\\lib\\\<config\_name\></span>

Note that the project directory, <span class="nctnt ncbi-path">msvc1000\_prj</span>, may be different for your version of Visual C++.

<span class="nctnt highlight">Note:</span> If the PTB (project tree builder) stage fails, the build may be stuck in a "locked" state. To "unlock" it, delete the file "<span class="nctnt ncbi-path">\_\_configure.lock</span>" from the project directory.

#### Using the Toolkit with Visual C++

This section dissusses the following examples of how to use the Toolkit with Windows:

-   [Start a New Project That Uses the Toolkit](#start-a-new-project-that-uses-the-toolkit)

-   [Start a New Project in the Toolkit](#start-a-new-project-in-the-toolkit)

-   [Modify or Debug an Existing Project in the Toolkit](#modify-or-debug-an-existing-project-in-the-toolkit)

##### Start a New Project That Uses the Toolkit

To use an already built C++ Toolkit (with all its build settings and configured paths), use the <span class="nctnt ncbi-path">new\_project</span> script to create a new project:

    new_project <name> <type> [builddir] [flags]

where:

|------------|--------------------------------------------------------------------------------------|
| \<name\>   | is the name of the project to create                                                 |
| \<type\>   | is one of the predefined project types                                               |
| [builddir] | is the location of the C++ Toolkit libraries                                         |
| [flags]    | selects a recursive build of all targets at this and lower levels in the source tree |

For example, if the Toolkit is built in the <span class="nctnt ncbi-path">U:\\cxx</span> folder, then this command:

    new_project test app U:\cxx\compilers\msvc1000_prj

-   creates a new local build tree;

-   puts the project source files into the <span class="nctnt ncbi-path">\\src\\name</span> folder;

-   puts the header files into <span class="nctnt ncbi-path">name\\include\\name</span>;

-   puts the Visual Studio project file into <span class="nctnt ncbi-path">name\\compilers\\msvc1000\_prj\\static\\build\\name</span>; and

-   puts the solution file into <span class="nctnt ncbi-path">name\\compilers\\msvc1000\_prj\\static\\build</span>.

To add new source files or libraries to the project, edit <span class="nctnt ncbi-path">name\\src\\name\\Makefile.name.app</span> makefile template, then rebuild the **-CONFIGURE-** project of the solution.

##### Start a New Project in the Toolkit with Visual C++

Follow the regular Unix-style guidelines for [adding a new project](ch_proj.html#ch_proj.start_new_proj) to the Toolkit.

Then, build the **-CONFIGURE-** project and reload the solution.

To start a new project that will become part of the Toolkit, create the makefile template first. For applications it must be named <span class="nctnt ncbi-path">Makefile.\< project\_name\>.app</span>; for libraries - <span class="nctnt ncbi-path">Makefile.\<project\_name\>.lib</span>. If it is a new folder in the source tree, you will also need to create <span class="nctnt ncbi-path">Makefile.in</span> file in the new folder, to specify to the configuration system what should be built in the new folder. Also, the new folder must be listed in the <span class="nctnt ncbi-monospace">SUB\_PROJ</span> section of the parent folder's <span class="nctnt ncbi-path">Makefile.in</span>. Finally, make sure your new project folder is listed in the appropriate project list file in <span class="nctnt ncbi-path">scripts\\projects\\\*.lst</span>. It can be either a subdirectory of an already listed directory, or a new entry in the list.

##### Modify or Debug an Existing Project in the Toolkit with Visual C++

Within NCBI, the <span class="nctnt ncbi-path">import\_project</span> script can be used to work on just a few projects and avoid retrieving and building the whole source tree. For example, to work on the '<span class="nctnt ncbi-path">corelib</span>' subtree, run:

    import_project corelib

The script will create the build tree, copy (or extract from the repository) relevant files, and create Visual Studio project files and a solution which references pre-built Toolkit libraries installed elsewhere. Then, you can modify and/or debug the project as desired.

Here's an example showing all the steps needed to build and debug the COBALT test application using <span class="nctnt ncbi-path">import\_project</span> with Visual C++ (you should be able to apply the approach of this example to your project by changing some names):

1  
In the Windows command-line prompt, run:<br/><span class="nctnt ncbi-cmd">import\_project algo/cobalt</span><br/>This will prepare a Visual Studio solution and open Visual Studio. There, build "<span class="nctnt ncbi-path">cobalt\_unit\_test.exe</span>". It's all 32-bit by default, even though your Windows is 64-bit.<br/>(Agree to map "S:" disk if you want to see debug info from the pre-built libraries.)

2  
Copy your "data" dir from:<br/><span class="nctnt ncbi-path">imported\_projects\\src\\algo\\cobalt\\unit\_test\\data</span><br/>to:<br/><span class="nctnt ncbi-path">imported\_projects\\compilers\\msvc1000\_prj\\static\\build\\algo\\cobalt\\unit\_test\\data</span>

3  
Debug it (right-click on it, and choose Debug).

If this doesn't work (for whatever reasons) on your own PC, you're welcome to use the communal PC servers (via Remote Desktop):

<http://intranet.ncbi.nlm.nih.gov/wiki-private/CxxToolkit/index.cgi/Software_Development#Software_Development9>

### Cygwin / GCC

To build the project with Cygwin / GCC, just follow the generic [Unix guidelines](#unix-guidelines), noting any [special considerations](#special-considerations).

Mac OS X
--------

<span class="nctnt highlight">Note:</span> Please also see the [General Information for All Platforms](#general-information-for-all-platforms) section, as it contains relevant information that is not repeated here.

This section covers the following topics:

-   [Xcode 3.0, 3.1](#xcode-30-31)

    -   [Choosing a Build Scope](#choosing-a-build-scope)

    -   [Configuring](#configuring)

    -   [Building](#building)

-   [Xcode 1.0, 2.0](#xcode-10-20)

    -   [Build the Toolkit](#build-the-toolkit)

    -   [The Build Results](#the-build-results)

-   [Darwin / GCC](#darwin--gcc)

-   [CodeWarrior](#codewarrior)

### Xcode 3.0, 3.1

Starting with Xcode build system version 3.0, the NCBI C++ Toolkit uses a new approach to configuring and building the toolkit with Mac OS X. The goal is to make the build process match the build process of Microsoft Visual C++ as closely as possible.

The following topics are discussed in this section:

-   [Choosing a Build Scope](#choosing-a-build-scope)

-   [Configuring](#configuring)

    -   [Site-Specific Build Tree Configuration](#site-specific-build-tree-configuration)

    -   [Dynamic Libraries Configuration](#dynamic-libraries-configuration)

    -   [Fine-Tuning Xcode Target Build Settings](#fine-tuning-xcode-target-build-settings)

    -   [Adding Files to Target](#adding-files-to-target)

    -   [Specifying a Custom Build Script](#specifying-a-custom-build-script)

-   [Building](#building)

    -   [Building 3<sup>rd</sup>-Party Libraries (Optional)](#building-3rd-party-libraries-optional)

    -   [Building from a Command-Line](#building-from-a-command-line)

    -   [The Build Results](#the-build-results)

#### Choosing a Build Scope with Xcode 3.0 or Later

The Toolkit is very large and you may not want to retrieve and build the entire Toolkit if you don’t need to. Therefore, after [preparing the development environment](ch_faq.html#ch_faq.How_do_I_prepare_my_development_e) and [getting the source files](ch_getcode_svn.html), you'll need to choose a build scope. Several mechanisms are provided to enable working with only a portion of the Toolkit.

The first thing you can do is to limit the source code [retrieved from the repository](ch_getcode_svn.html):

-   using the shell script [import\_project](ch_getcode_svn.html#ch_getcode_svn.import_project_sh); or

-   using the shell script [update\_projects](ch_getcode_svn.html#ch_getcode_svn.update_projects_sh).

Next, you can limit what will be built by choosing one of the five standard projects:

<span class="nctnt ncbi-path">compilers/xcode30\_prj/static/ncbi\_cpp.xcodeproj</span>

<span class="nctnt ncbi-path">compilers/xcode30\_prj/dll/ncbi\_cpp\_dll.xcodeproj</span>

<span class="nctnt ncbi-path">compilers/xcode30\_prj/static/ncbi\_gui.xcodeproj</span>

<span class="nctnt ncbi-path">compilers/xcode30\_prj/dll/ncbi\_gui\_dll.xcodeproj</span>

<span class="nctnt ncbi-path">compilers/xcode30\_prj/dll/ncbi\_gbench\_dll.xcodeproj</span>

The first two projects build console applications and required libraries only; the last three projects build GUI applications:

Note that the project directory, <span class="nctnt ncbi-path">xcode30\_prj</span>, may be different for your version of Xcode.

#### Configuring with Xcode 3.0 or Later

Once you have [chosen a build scope](#chosen-a-build-scope), you are ready to configure.

Each standard project contains a single special target called **CONFIGURE**. Building **CONFIGURE** first builds an application called project tree builder (PTB) and then runs that application. PTB overwrites the current standard project file with a new project that contains all the other Xcode build targets. The new build targets are based on Unix-style makefile templates (<span class="nctnt ncbi-path">src/.../Makefile.\*</span>) and are specified by predefined lists of projects in <span class="nctnt ncbi-path">scripts/projects/\*.lst</span> files.

When **CONFIGURE** is built, a dialog will pop up stating that the project file has been overwritten by an external process (the external process is the PTB). Reload the project to ensure that it is loaded correctly. Then any or all of the other targets can be built.

A configuration tool with a Java-based GUI is also available and can be launched by building the **CONFIGURE-DIALOG** target. For more information on using the configuration GUI, see the [general section on configuring](#general-section-on-configuring).

You may build any of the five standard projects. The projects in the <span class="nctnt ncbi-path">static</span> directory build libraries and applications using static Toolkit libraries, the other three use dynamic libraries.

To build a specific target, make it an active one and invoke the **Build** command in the Xcode workspace. To build all project targets, build the **BUILD\_ALL** target.

Additional configuration files include:

-   <span class="nctnt ncbi-path">src/build-system/project\_tree\_builder.ini</span>

-   <span class="nctnt ncbi-path">src/build-system/Makefile.mk.in.xcode</span>

-   <span class="nctnt ncbi-path">src/.../Makefile.\*.xcode</span>

Modifying <span class="nctnt ncbi-path">project\_tree\_builder.ini</span> is described below in the section [Site-Specific Build Tree Configuration](#site-specific-build-tree-configuration).

Modifying <span class="nctnt ncbi-path">Makefile.mk.in.xcode</span> and <span class="nctnt ncbi-path">Makefile.\*.xcode</span> is described below in the section [Fine-Tuning Xcode Target Build Settings](#fine-tuning-xcode-target-build-settings).

The following topics discuss additional information about configuring with Xcode:

-   [Site-Specific Build Tree Configuration](#site-specific-build-tree-configuration)

-   [Dynamic Libraries Configuration](#dynamic-libraries-configuration)

-   [Fine-Tuning Xcode Target Build Settings](#fine-tuning-xcode-target-build-settings)

-   [Adding Files to Target](#adding-files-to-target)

-   [Specifying a Custom Build Script](#specifying-a-custom-build-script)

##### Site-Specific Build Tree Configuration

The build tree configuration can be tailored to your site by modifying the file [src/build-system/project\_tree\_builder.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/project_tree_builder.ini) (see [Table 4](#table-4)). For example, you may need to change the location of 3<sup>rd</sup>-party libraries to match your systems. Or you may need to specify conditions under which a certain project is excluded from the build.

<span class="nctnt ncbi-path">project\_tree\_builder.ini</span> describes build and source tree configurations; contains information about the location of 3rd-party libraries and applications; and includes information used to resolve macro definitions found in the Unix-style makefile templates.

Toolkit project makefiles can list a set of requirements that must be met in order for the project to be built. These requirements are specified in the pseudo-macro **REQUIRES**. For example, a project can be built only on Unix, or only in multi-thread mode, or only if a specific external library is available. Depending on which of the requirements are met, the Toolkit configuration tool may exclude some projects in some (or all) build configurations, preprocessor defines, and/or makefile macros.

Some of the Toolkit projects can be built differently depending on the availability of non-Toolkit components. For those projects, there is a list of macros - defined in the '<span class="nctnt ncbi-monospace">Defines</span>' entry - that define conditional compilation. Each of these macros also has its own section in <span class="nctnt ncbi-path">project\_tree\_builder.ini</span> that links the macro to a specific component. Using the '<span class="nctnt ncbi-monospace">Defines</span>' entry and the associated macro sections, a project can be linked to a list of components. For each build configuration, project tree builder creates a header file (see '<span class="nctnt ncbi-monospace">DefinesPath</span>' entry) and defines these macros there depending on the availability of the corresponding components.

Many of the requirements define dependencies on 3rd-party packages, such as BerkeleyDB. For each one of these there is a special section (e.g. [BerkeleyDB]) in [project\_tree\_builder.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/project_tree_builder.ini) that describes the path(s) to the <span class="nctnt ncbi-path">include</span> and <span class="nctnt ncbi-path">library</span> directories of the package, as well as the preprocessor definitions to compile with and the libraries to link against. The Toolkit configurator checks if the package's directories and libraries do exist, and uses this information when generating appropriate projects.

There are a few indispensable external components that have analogs in the Toolkit. If external libraries for these components are not available then the internal analog can be used. The '<span class="nctnt ncbi-monospace">LibChoices</span>' entry identifies such pairs, and '<span class="nctnt ncbi-monospace">LibChoiceIncludes</span>' provides additional include paths to the built-in headers.

<span class="nctnt highlight">Note:</span> There may be some requirements which are always or never met. These requirements are listed in the '<span class="nctnt ncbi-monospace">ProvidedRequests</span>', '<span class="nctnt ncbi-monospace">StandardFeatures</span>', or '<span class="nctnt ncbi-monospace">NotProvidedRequests</span>' entries of the '<span class="nctnt ncbi-monospace">Configure</span>' section.

##### Dynamic Libraries Configuration

The Toolkit Unix-style makefile templates give a choice of building the library as dynamic or static (or both). However, it is often convenient to assemble "bigger" dynamic libraries made of the sources of several static libraries.

In the Toolkit, such compound libraries are described using a set of special makefiles in <span class="nctnt ncbi-path">src/dl</span>l subdirectory. Each such file – <span class="nctnt ncbi-path">Makefile.\*.dll</span> – contains the following entries:

-   DLL – the name of the compound dynamic library;

-   HOSTED\_LIBS – the names of the static libraries to be assembled into the compound dynamic library;

-   DEPENDENCIES – dependencies on other static or dynamic libraries; and

-   CPPFLAGS – additional compiler flags, specific for this dynamic library.

##### Fine-Tuning Xcode Target Build Settings

While default build settings are defined in the <span class="nctnt ncbi-path">Makefile.mk.in.xcode</span> file, it is possible to redefine some of them in special tune-up files – <span class="nctnt ncbi-path">Makefile.\<project\_name\>.{lib\\|app}.xcode</span> – located in the project source directory. All entries in the tune-up files are optional.

Section names in the tune-up files can have one or more optional suffixes and can take any of the following forms:

-   <span class="nctnt ncbi-monospace">SectionName</span>

-   <span class="nctnt ncbi-monospace">SectionName.CompilerVersion</span>

-   <span class="nctnt ncbi-monospace">SectionName.Platform</span>

-   <span class="nctnt ncbi-monospace">SectionName.[static\\|dll]</span>

-   <span class="nctnt ncbi-monospace">SectionName.[debug\\|release]</span>

-   <span class="nctnt ncbi-monospace">SectionName.CompilerVersion.[debug\\|release]</span>

-   <span class="nctnt ncbi-monospace">SectionName.[static\\|dll].[debug\\|release]</span>

-   <span class="nctnt ncbi-monospace">SectionName.[debug\\|release].ConfigurationName</span>

-   <span class="nctnt ncbi-monospace">SectionName.[static\\|dll].[debug\\|release].ConfigurationName</span>

Here, '<span class="nctnt ncbi-monospace">static</span>' or '<span class="nctnt ncbi-monospace">dll</span>' means the type of runtime libraries that a particular build uses; '<span class="nctnt ncbi-monospace">debug</span>' or '<span class="nctnt ncbi-monospace">release</span>' means the type of the build configuration; and '<span class="nctnt ncbi-monospace">ConfigurationName</span>' means the name of the build configuration, for example DebugDLL or ReleaseMT.

Settings in sections with more detailed names (ones that appear later on this list) override ones in sections with less detailed names (ones that appear earlier).

##### Adding Files to Target

This information should be entered in the '<span class="nctnt ncbi-monospace">AddToProject</span>' section. The section can have the following entries:

-   <span class="nctnt ncbi-monospace">[AddToProject]</span>

-   <span class="nctnt ncbi-monospace">SourceFiles=</span>

-   <span class="nctnt ncbi-monospace">IncludeDirs=</span>

-   <span class="nctnt ncbi-monospace">LIB=</span>

-   <span class="nctnt ncbi-monospace">HeadersInInclude=</span>

-   <span class="nctnt ncbi-monospace">HeadersInSrc=</span>

The '<span class="nctnt ncbi-monospace">SourceFiles</span>' entry lists additional (usually OSX specific) source files for the project. Source file entries should not include file name extensions. The '<span class="nctnt ncbi-monospace">IncludeDirs</span>' entry lists additional include directories, and the '<span class="nctnt ncbi-monospace">LIB</span>' entry lists additional libraries for the project.

By default, all header files found in the project's include and source directories are added to the Xcode target. If that's not exactly what you need though, then the default set of headers to be added to the target can be altered using the '<span class="nctnt ncbi-monospace">HeadersInInclude</span>' and '<span class="nctnt ncbi-monospace">HeadersInSrc</span>' entries. Unlike the '<span class="nctnt ncbi-monospace">SourceFiles</span>' entry, file names in these entries should include their extension. Use an exclamation mark to exclude files that would otherwise be included. Wildcards are allowed. For example, the following entry

<span class="nctnt ncbi-monospace">HeadersInInclude = \*.h file1.hpp !file2.h</span>

means "add all files with the <span class="nctnt ncbi-path">.h</span> extension, add <span class="nctnt ncbi-path">file1.hpp</span>, and do not add <span class="nctnt ncbi-path">file2.h</span>".

<span class="nctnt highlight">Note:</span> A single exclamation mark with no file name means "do not add any header files".

##### Specifying a Custom Build Script

For a particular target, it is possible to specify a custom build script which will run in addition to the standard build operation. This could be used, for example, to copy application resource files once the build is completed. Xcode will automatically incorporate the custom script into the standard build process.

In the appropriate <span class="nctnt ncbi-path">Makefile.\*.xcode</span> customization file, define a section called ‘<span class="nctnt ncbi-monospace">CustomScript</span>’. It has one mandatory entry – <span class="nctnt ncbi-monospace">Script</span>, and three optional ones:

-   <span class="nctnt ncbi-monospace">Input</span> – a list of input files;

-   <span class="nctnt ncbi-monospace">Output</span> – a list of output files; and

-   <span class="nctnt ncbi-monospace">Shell</span> – which shell to use (the default is ‘/bin/sh’).

#### Building with Xcode 3.0 or Later

Once you have [chosen a build scope](#chosen-a-build-scope) and have [configured](#configured), you are ready to build.

<span class="nctnt highlight">Note:</span> Some projects may require using [3rd-party libraries](#3rd-party-libraries).

Select the desired project and build it. To build all projects, select the **BUILD-ALL** project.

Following are some examples of how to build specific projects and some additional topics:

-   [Building 3<sup>rd</sup>-Party Libraries (Optional)](#building-3rd-party-libraries-optional)

-   [Building from a Command-Line](#building-from-a-command-line)

-   [The Build Results](#the-build-results)

##### Build 3<sup>rd</sup>-Party Libraries (optional)

Some of the NCBI C++ Toolkit projects make use of the [NCBI C Toolkit](ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/2008/Mar_17_2008/NCBI_C_Toolkit/README) (not to be confused with the NCBI C++ Toolkit) and/or freely distributed 3rd-party packages (such as BerkeleyDB, LibZ, FLTK, etc.).

At NCBI, these libraries are already installed, and their locations are [hard coded](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/build-system/project_tree_builder.ini) in the C++ Toolkit configuration files. If you are outside of NCBI, you may need to build and install these libraries before building the C++ Toolkit.

If you do not have the external libraries already installed, you can download, build, and install the NCBI C Toolkit and the freely distributed 3rd-party packages. The source code for the NCBI C Toolkit and the freely distributed 3rd-party packages can be downloaded from the NCBI FTP site and built in all available configurations. Refer to the documentation on the specific packages you wish to install for more information.

##### Building from a Command-Line with Xcode 3.0 or Later

From the command-line, you can either build exactly as [under Unix](#under-unix), or you can build for Xcode.

To configure for Xcode, first run <span class="nctnt ncbi-cmd">configure</span> in the Xcode project directory (run <span class="nctnt ncbi-cmd">configure --help</span> to see available options):

    cd compilers/xcode30_prj
    ./configure

Once you have configured for Xcode, you can either open and work in the Xcode IDE or build from the command-line.

To build from the command-line, run <span class="nctnt ncbi-cmd">make all\_r</span>. Optionally build the testsuite with <span class="nctnt ncbi-cmd">make check\_r</span>.

    make all_r
    make check_r

##### The Build Results

Applications and libraries produced by the build will be put, respectively, into:

-   <span class="nctnt ncbi-path">compilers/xcode30\_prj/{static\\|dll}/bin/\<ConfigurationName\></span>

-   <span class="nctnt ncbi-path">compilers/xcode30\_prj/{static\\|dll}/lib/\<ConfigurationName\></span>

### Xcode 1.0, 2.0

For versions of Xcode earlier than 3.0 the handmade scripts have to be used.

The following topics are discussed in this section:

-   [Build the Toolkit](#build-the-toolkit)

-   [The Build Results](#the-build-results)

#### Build the Toolkit

Open, build and run a project file in <span class="nctnt ncbi-path">compilers/xCode</span>.

This GUI tool generates a new NCBI C++ Toolkit Xcode project. It allows you to:

-   Choose which Toolkit libraries and applications to build.

-   Automatically download and install all 3rd-party libraries.

-   Specify third-party installation directories.

#### The Build Results

The above process results in the Toolkit applications and libraries being put into the output directory selected by the user.

Apple Xcode versions 2.0 and above support build configurations. We use the default names Debug and Release, so the built applications will go to, for example:

-   <span class="nctnt ncbi-path">\<output\_dir\>/bin/Debug/Genome Workbench.app</span>, or

-   <span class="nctnt ncbi-path">\<output\_dir\>/bin/Release/Genome Workbench.app</span>

Apple Xcode versions before 2.0 do not support build configurations, so the build results will always go to:

-   <span class="nctnt ncbi-path">\<output\_dir\>/bin/Genome Workbench.app</span>

Most libraries are built as Mach-O dynamically linked and shared (<span class="nctnt ncbi-path">.dylib</span>) and go to:

-   <span class="nctnt ncbi-path">\<output\_dir\>/lib</span>

Genome Workbench plugins are built as Mach-O bundles (also with <span class="nctnt ncbi-path">.dylib</span> extension) and get placed inside Genome Workbench application bundle:

-   <span class="nctnt ncbi-path">\<output\_dir\>/Genome Workbench.app/Contents/MacOS/plugins</span>

### Darwin / GCC

To build the project with Darwin / GCC, just follow the generic [Unix guidelines](#unix-guidelines).

### CodeWarrior

The support for CodeWarrior was dropped in 2005.


