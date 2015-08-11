---
layout: default
title: C++ Toolkit test
nav: pages/ch_dbapi
---

<span class="label">10</span>Database Access Support
====================================================

Last Update: April 2, 2015.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter covers the C++ Toolkit support for database access using:

-   the [SDBAPI](#sdbapi) database access library [[include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/dbapi/simple/) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/simple/)];

-   the [DBAPI](#dbapi) database access library [[include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/dbapi/) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/)];

-   the [BDB](#bdb) wrapper [[include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/db/bdb/) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/db/bdb/)]; or

-   the [SQLite](#sqlite) wrapper [[include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/db/sqlite/) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/db/sqlite/)].

DBAPI was the traditional database access library interface, but has been superseded by SDBAPI - the "simplified" interface. They have considerable overlap, but they are fully not compatible and each has at least one feature the other doesn't, so you must pick one. For example, DBAPI allows a choice of underlying drivers and access to the underlying data source while SDBAPI does not. On the other hand, SDBAPI supports bookmarking blobs while DBAPI doesn't. In the vast majority of cases SDBAPI will be both more than adequate and easier to use and maintain. Therefore, SDBAPI should be the first choice unless there's a compelling reason to use DBAPI.

The DBAPI library provides the underlying [user-layer](#user-layer) and [driver](#driver) API for the NCBI database connectivity project. The project's goal is to provide access to various relational database management systems (RDBMS) with a single uniform user interface. Consult the detailed documentation for details of the [supported DBAPI drivers](#supported-dbapi-drivers).

The "BDB wrapper" is part of the NCBI C++ Toolkit and serves as a high-level interface to the open source Berkeley DB library. The BDB wrapper is architecturally different from the DBAPI library and does not follow its design - rather, it is compatible with Berkeley DB v. 4.1 and higher. The primary purpose of the Berkeley DB library is to provide tools to work with flatfile, federated databases. Thus, the BDB wrapper is intended for use by software developers who need small-footprint, high-performance database capabilities with zero administration. The database in this case becomes tightly integrated with the application code. For more information about Berkeley DB, see the official [documentation](http://www.oracle.com/us/products/database/berkeley-db/overview/index.html).

The "SQLite wrapper" is part of the NCBI C++ Toolkit and serves as a high-level interface to the open source SQLite library. The SQLite wrapper is architecturally different from the DBAPI library and does not follow its design - rather, it is compatible with SQLite v. 3.6.14 and higher. The primary purpose of the SQLite library is to provide small, fast, and reliable in-process full-SQL database access. The SQLite wrapper provides convenient wrappers for SQLite-related objects and most commonly used functions. The wrapper requires SQLite 3.6.14 or higher with the asynchronous VFS extension and assumes that no SQLite calls are made except via the wrapper itself. For more information about SQLite, see the official [documentation](http://sqlite.org/docs.html).

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Security](#security)

    -   [Preventing SQL Injection](#preventing-sql-injection)

    -   [Using Kerberos with DBAPI](#using-kerberos-with-dbapi)

-   [SDBAPI / DBAPI Feature Comparison](#sdbapi--dbapi-feature-comparison)

-   [The SDBAPI Library](#the-sdbapi-library)

    -   [SDBAPI Overview](#sdbapi-overview)

    -   [Includes and Linkage](#includes-and-linkage)

    -   [Connections](#connections)

    -   [Executing Basic Queries](#executing-basic-queries)

    -   [Stored Procedures and Parameters](#stored-procedures-and-parameters)

    -   [Retrieving Results](#retrieving-results)

    -   [Getting a Stored Procedure Return Value](#getting-a-stored-procedure-return-value)

    -   [Meta-Data Accessors](#meta-data-accessors)

    -   [Working with NULL](#working-with-null)

    -   [Using Transactions](#using-transactions)

    -   [Using Cursors](#using-cursors)

-   [The DBAPI Library](#the-dbapi-library)

    -   [DBAPI User-Layer Reference](#dbapi-user-layer-reference)

        -   [DBAPI Overview](#dbapi-overview)

        -   [Object Hierarchy](#object-hierarchy)

        -   [Includes](#includes)

        -   [Objects](#objects)

        -   [Object Life Cycle](#object-life-cycle)

        -   [CVariant Type](#cvariant-type)

        -   [Choosing the Driver](#choosing-the-driver)

        -   [Data Source and Connections](#data-source-and-connections)

        -   [Main Loop](#main-loop)

        -   [Input and Output Parameters](#input-and-output-parameters)

        -   [Stored Procedures](#stored-procedures)

        -   [Cursors](#cursors)

        -   [Working with BLOBs](#working-with-blobs)

        -   [Updating BLOBs Using Cursors](#updating-blobs-using-cursors)

        -   [Using Bulk Insert](#using-bulk-insert)

        -   [Diagnostic Messages](#diagnostic-messages)

        -   [Trace Output](#trace-output)

    -   [DBAPI Driver Reference](#dbapi-driver-reference)

        -   [Overview](#overview)

        -   [The driver architecture](#the-driver-architecture)

        -   [Sample program](#sample-program)

        -   [Error handling](#error-handling)

        -   [Driver context and connections](#driver-context-and-connections)

        -   [Driver Manager](#driver-manager)

        -   [Text and Image Data Handling](#text-and-image-data-handling)

        -   [Results loop](#results-loop)

    -   [Supported DBAPI drivers](#supported-dbapi-drivers)

        -   [FreeTDS (TDS ver. 7.0)](#freetds-tds-ver-70)

        -   [Sybase CTLIB](#sybase-ctlib)

        -   [ODBC](#odbc)

        -   [MySQL Driver](#mysql-driver)

-   [The BDB Wrapper](#the-bdb-wrapper)

-   [The SQLite Wrapper](#the-sqlite-wrapper)

-   [Database Load-Balancing (DBLB)](#database-load-balancing-dblb)

    -   [Setting up Load-Balancing of Database Servers](#setting-up-load-balancing-of-database-servers)

    -   [Using Database Load-Balancing from C++](#using-database-load-balancing-from-c++)

    -   [Load-Balanced Database Access via Python and Perl](#load-balanced-database-access-via-python-and-perl)

    -   [Advantages of using DBLB](#advantages-of-using-dblb)

    -   [How it works (by default)](#how-it-works-by-default)

Security
--------

### Preventing SQL Injection

This section is not intended to cover all the important aspects of security because a lot of good information on the topic is already published elsewhere. Please use other resources to find the security-related information needed for your work.

However, it's worth pointing out a couple of the most important ways to protect against SQL injection:

1  
Never construct a SQL statement from user-supplied input if the same functionality can be achieved by passing the user input to stored procedures or parameterized SQL.

2  
If constructing a SQL statement from user-supplied input cannot be avoided, then you MUST sanitize the user input.

The following sample programs illustrates how to protect against SQL injection for basic SQL statements using SDBAPI and DBAPI:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/sdbapi/sdbapi_simple.cpp>

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/dbapi_simple.cpp>

See the [Security FAQ](ch_faq.html#ch_faq.Security) for more information.

### Using Kerberos with DBAPI

Within NCBI, individual users (i.e. not service accounts) can use Kerberos with DBAPI, provided the following conditions are met:

1  
The database must allow them to connect using Kerberos. (Email <span class="oem_mark">dbhelp@ncbi.nlm.nih.gov</span> if you need help with this.)

2  
DBAPI must be configured to enable Kerberos.

a  
Either the <span class="nctnt ncbi-var">NCBI\_CONFIG\_\_DBAPI\_\_CAN\_USE\_KERBEROS</span> environment variable must be set to <span class="nctnt ncbi-monospace">true</span>; or

b  
the <span class="nctnt ncbi-monospace">can\_use\_kerberos</span> entry in the <span class="nctnt ncbi-monospace">dbapi</span> section of the application configuration file must be set to <span class="nctnt ncbi-monospace">true</span>.

3  
Their Kerberos ticket must not be expired.

4  
They must pass an empty string for the user name.

This is also covered in the [DBAPI section](ch_libconfig.html#ch_libconfig.DBAPI) of the Library Configuration chapter.

SDBAPI / DBAPI Feature Comparison
---------------------------------

SDBAPI is mostly a wrapper over DBAPI, and as such it introduces some overhead - some of which is unavoidable. SDBAPI and DBAPI each have some features that aren't available in the other, but SDBAPI is usually more than sufficient for 95% of real-life tasks (and it makes them easier to code too). So, in most cases you should use SDBAPI.

The following features are only available in DBAPI:

-   Choice of driver (e.g. CTLIB, ODBC, etc.).

-   Support for cursors.

-   Writing BLOBs to streams.

The following features are only available in SDBAPI:

-   Bookmarking BLOBs.

The following table compares the implementation of various features in DBAPI and SDBAPI:

| Feature                                                     | SDBAPI                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | DBAPI                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| sample code                                                 | [src/sample/app/sdbapi](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/sdbapi/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | [src/sample/app/dbapi](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| available drivers                                           | FTDS                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | CTLIB, FTDS, MYSQL, ODBC                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| cursor support                                              | no                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | yes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| writing BLOBs to streams                                    | no                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | yes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| bookmarking BLOBs                                           | yes                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | no                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| access to stored procedure parameters                       | only by name                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | by name or position (note: if possible, prefer using names over positions because using positions creates maintenance difficulties)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| makefile <span class="nctnt ncbi-monospace">REQUIRES</span> | <span class="nctnt ncbi-monospace">REQUIRES = dbapi FreeTDS</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | <span class="nctnt ncbi-monospace">\# choose driver, e.g. FreeTDS, BerkeleyDB, or SQLITE3</span><br/><span class="nctnt ncbi-monospace">REQUIRES = dbapi FreeTDS</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| makefile <span class="nctnt ncbi-monospace">LIB</span>      | <span class="nctnt ncbi-monospace">LIB = $(SDBAPI\_LIB) xconnect xutil xncbi</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | <span class="nctnt ncbi-monospace">LIB = ncbi\_xdbapi\_ftds $(FTDS\_LIB) \\</span><br/><span class="nctnt ncbi-monospace"> dbapi\_util\_blobstore$(STATIC) dbapi$(STATIC) dbapi\_driver$(STATIC) \\</span><br/><span class="nctnt ncbi-monospace"> $(XCONNEXT) xconnect $(COMPRESS\_LIBS) xutil xncbi</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| makefile <span class="nctnt ncbi-monospace">LIBS</span>     | <span class="nctnt ncbi-monospace">LIBS = $(SDBAPI\_LIBS) $(NETWORK\_LIBS) $(DL\_LIBS) $(ORIG\_LIBS)</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | <span class="nctnt ncbi-monospace">LIBS = $(FTDS\_LIBS) $(CMPRS\_LIBS) $(NETWORK\_LIBS) $(DL\_LIBS) $(ORIG\_LIBS)</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| includes                                                    | <span class="nctnt ncbi-code">\<dbapi/simple/sdbapi.hpp\></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | <span class="nctnt ncbi-code">// "new" DBAPI</span><br/><span class="nctnt ncbi-code">\#include \<dbapi/dbapi.hpp\></span><br/><br/><span class="nctnt ncbi-code">// "old" DBAPI</span><br/><span class="nctnt ncbi-code">\#include \<dbapi/driver/dbapi\_conn\_factory.hpp\> // CTrivialConnValidator</span><br/><span class="nctnt ncbi-code">\#include \<dbapi/driver/dbapi\_svc\_mapper.hpp\> // DBLB\_INSTALL\_DEFAULT</span><br/><span class="nctnt ncbi-code">\#include \<dbapi/driver/drivers.hpp\> // DBAPI\_RegisterDriver\_FTDS</span><br/><span class="nctnt ncbi-code">\#include \<dbapi/driver/exception.hpp\> // CDB\_UserHandler</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| to set up a connection                                      | <span class="nctnt ncbi-code">// Specify connection parameters.</span><br/><span class="nctnt ncbi-code">CSDB\_ConnectionParam db\_params;</span><br/><span class="nctnt ncbi-code">db\_params.Set(CSDB\_ConnectionParam::eUsername, m\_User);</span><br/><span class="nctnt ncbi-code">db\_params.Set(CSDB\_ConnectionParam::ePassword, m\_Password);</span><br/><span class="nctnt ncbi-code">db\_params.Set(CSDB\_ConnectionParam::eService, m\_Service);</span><br/><span class="nctnt ncbi-code">db\_params.Set(CSDB\_ConnectionParam::eDatabase, m\_DbName);</span><br/><br/><span class="nctnt ncbi-code">// Connect to the database.</span><br/><span class="nctnt ncbi-code">m\_Db = CDatabase(db\_params);</span><br/><span class="nctnt ncbi-code">m\_Db.Connect();</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | <span class="nctnt ncbi-code">// Use load balancing if available.</span><br/><span class="nctnt ncbi-code">DBLB\_INSTALL\_DEFAULT();</span><br/><br/><span class="nctnt ncbi-code">// Explicitly register a driver.</span><br/><span class="nctnt ncbi-code">DBAPI\_RegisterDriver\_FTDS();</span><br/><br/><span class="nctnt ncbi-code">CDriverManager& dm(CDriverManager::GetInstance());</span><br/><br/><span class="nctnt ncbi-code">// Create a data source - the root object for all other</span><br/><span class="nctnt ncbi-code">// objects in the library.</span><br/><span class="nctnt ncbi-code">m\_Ds = dm.CreateDs("ftds");</span><br/><br/><span class="nctnt ncbi-code">// Setup diagnostics.</span><br/><span class="nctnt ncbi-code">m\_Logstream.open(m\_LogFileName.c\_str());</span><br/><span class="nctnt ncbi-code">m\_Ds-\>SetLogStream(&m\_Logstream);</span><br/><br/><span class="nctnt ncbi-code">// Add a message handler for 'context-wide' error messages (not bound</span><br/><span class="nctnt ncbi-code">// to any particular connection); let DBAPI own it.</span><br/><span class="nctnt ncbi-code">I\_DriverContext\* drv\_context = m\_Ds-\>GetDriverContext();</span><br/><span class="nctnt ncbi-code">drv\_context-\>PushCntxMsgHandler(</span><br/><span class="nctnt ncbi-code"> new CErrHandler("General", &m\_Logstream),</span><br/><span class="nctnt ncbi-code"> eTakeOwnership);</span><br/><br/><span class="nctnt ncbi-code">// Add a 'per-connection' message handler to the stack of default</span><br/><span class="nctnt ncbi-code">// handlers which are inherited by all newly created connections;</span><br/><span class="nctnt ncbi-code">// let DBAPI own it.</span><br/><span class="nctnt ncbi-code">drv\_context-\>PushDefConnMsgHandler(</span><br/><span class="nctnt ncbi-code"> new CErrHandler("Connection", &m\_Logstream),</span><br/><span class="nctnt ncbi-code"> eTakeOwnership);</span><br/><br/><span class="nctnt ncbi-code">// Configure this context.</span><br/><span class="nctnt ncbi-code">drv\_context-\>SetLoginTimeout(10);</span><br/><span class="nctnt ncbi-code">// default query timeout for client/server comm for all connections</span><br/><span class="nctnt ncbi-code">drv\_context-\>SetTimeout(15);</span><br/><br/><span class="nctnt ncbi-code">// Create connection.</span><br/><span class="nctnt ncbi-code">m\_Conn = m\_Ds-\>CreateConnection();</span><br/><span class="nctnt ncbi-code">if (m\_Conn == NULL) {</span><br/><span class="nctnt ncbi-code"> ERR\_POST\_X(1, Fatal \<\< "Could not create connection.");</span><br/><span class="nctnt ncbi-code">}</span><br/><br/><span class="nctnt ncbi-code">// Validate connection. When using load balancing, this will interpret</span><br/><span class="nctnt ncbi-code">// the "server" name as a service, then use the load balancer to find</span><br/><span class="nctnt ncbi-code">// servers, then try in succession until a successful login to the</span><br/><span class="nctnt ncbi-code">// given database is successful.</span><br/><span class="nctnt ncbi-code">CTrivialConnValidator val(m\_DbName);</span><br/><span class="nctnt ncbi-code">m\_Conn-\>ConnectValidated(val, m\_User, m\_Password, m\_Service, m\_DbName);</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| to execute a stored procedure                               | <span class="nctnt ncbi-code">CQuery query = m\_Db.NewQuery();</span><br/><span class="nctnt ncbi-code">query.SetParameter("@max\_id", 5);</span><br/><span class="nctnt ncbi-code">query.SetParameter("@max\_fl", 5.1f);</span><br/><span class="nctnt ncbi-code">query.SetParameter("@num\_rows", 0, eSDB\_Int4, eSP\_InOut);</span><br/><span class="nctnt ncbi-code">query.ExecuteSP(proc\_name);</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | <span class="nctnt ncbi-code">ICallableStatement \*cstmt = conn-\>PrepareCall("ProcName");</span><br/><span class="nctnt ncbi-code">Uint1 byte = 1;</span><br/><span class="nctnt ncbi-code">cstmt-\>SetParam(CVariant("test"), "@test\_input");</span><br/><span class="nctnt ncbi-code">cstmt-\>SetParam(CVariant::TinyInt(&byte), "@byte");</span><br/><span class="nctnt ncbi-code">cstmt-\>SetOutputParam(CVariant(eDB\_Int), "@result");</span><br/><span class="nctnt ncbi-code">cstmt-\>Execute();</span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| to retrieve results                                         | <span class="nctnt ncbi-code">// Print the results.</span><br/><span class="nctnt ncbi-code">//</span><br/><span class="nctnt ncbi-code">// NOTE: For database APIs, array-like indexes are 1-based, not 0-based!</span><br/><span class="nctnt ncbi-code">//</span><br/><span class="nctnt ncbi-code">NcbiCout \<\< "int\_val fl\_val" \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code">ITERATE(CQuery, row, query.SingleSet()) {</span><br/><span class="nctnt ncbi-code"> NcbiCout</span><br/><span class="nctnt ncbi-code"> \<\< row[1].AsInt4() \<\< " "</span><br/><span class="nctnt ncbi-code"> \<\< row[2].AsFloat() \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code">}</span><br/><br/><span class="nctnt ncbi-code">// Confirm having read all results.</span><br/><span class="nctnt ncbi-code">query.VerifyDone();</span><br/><br/><span class="nctnt ncbi-code">// Print the number of result rows.</span><br/><span class="nctnt ncbi-code">NcbiCout</span><br/><span class="nctnt ncbi-code"> \<\< "Number of rows: "</span><br/><span class="nctnt ncbi-code"> \<\< query.GetParameter("@num\_rows").AsInt4() \<\< NcbiEndl;</span> | <span class="nctnt ncbi-code">// Retrieve and display the data.</span><br/><span class="nctnt ncbi-code">while (stmt-\>HasMoreResults()) {</span><br/><span class="nctnt ncbi-code"> // Use an auto\_ptr to manage resultset lifetime.</span><br/><span class="nctnt ncbi-code"> // NOTE: Use it with caution. When the wrapped parent object</span><br/><span class="nctnt ncbi-code"> // goes out of scope, all child objects are destroyed</span><br/><span class="nctnt ncbi-code"> // (which isn't an issue for this demo but could be for</span><br/><span class="nctnt ncbi-code"> // other applications).</span><br/><span class="nctnt ncbi-code"> auto\_ptr\<IResultSet\> rs(stmt-\>GetResultSet());</span><br/><br/><span class="nctnt ncbi-code"> // Sometimes the results have no rows - and that's ok.</span><br/><span class="nctnt ncbi-code"> if ( ! stmt-\>HasRows() ) {</span><br/><span class="nctnt ncbi-code"> LOG\_POST\_X(1, Info \<\< "No rows.");</span><br/><span class="nctnt ncbi-code"> continue;</span><br/><span class="nctnt ncbi-code"> }</span><br/><br/><span class="nctnt ncbi-code"> switch (rs-\>GetResultType()) {</span><br/><br/><span class="nctnt ncbi-code"> case eDB\_StatusResult:</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< "\\nStatus results:" \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> while (rs-\>Next()) {</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " Status: " \<\< rs-\>GetVariant(1).GetInt4()</span><br/><span class="nctnt ncbi-code"> \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> }</span><br/><span class="nctnt ncbi-code"> break;</span><br/><br/><span class="nctnt ncbi-code"> case eDB\_ParamResult:</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< "\\nParameter results:" \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> while (rs-\>Next()) {</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " Parameter: "</span><br/><span class="nctnt ncbi-code"> \<\< rs-\>GetVariant(1).GetInt4() \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> }</span><br/><span class="nctnt ncbi-code"> break;</span><br/><br/><span class="nctnt ncbi-code"> case eDB\_RowResult: {</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< "\\nRow results:" \<\< NcbiEndl;</span><br/><br/><span class="nctnt ncbi-code"> const IResultSetMetaData\* rsMeta = rs-\>GetMetaData();</span><br/><br/><span class="nctnt ncbi-code"> // Print column headers.</span><br/><span class="nctnt ncbi-code"> for (unsigned i = 1; i \<= rsMeta-\>GetTotalColumns(); ++i) {</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " " \<\< rsMeta-\>GetName(i);</span><br/><span class="nctnt ncbi-code"> }</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> for (unsigned i = 1; i \<= rsMeta-\>GetTotalColumns(); ++i) {</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " " \<\< string(rsMeta-\>GetName(i).size(), '=');</span><br/><span class="nctnt ncbi-code"> }</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< NcbiEndl;</span><br/><br/><span class="nctnt ncbi-code"> while (rs-\>Next()) {</span><br/><span class="nctnt ncbi-code"> // Print a row of data.</span><br/><span class="nctnt ncbi-code"> for (unsigned i = 1; i \<= rsMeta-\>GetTotalColumns(); ++i) {</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " " \<\< rs-\>GetVariant(i).GetString();</span><br/><span class="nctnt ncbi-code"> }</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> }</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " ---------------" \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " Row count: " \<\< stmt-\>GetRowCount()</span><br/><span class="nctnt ncbi-code"> \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> break;</span><br/><span class="nctnt ncbi-code"> }</span><br/><br/><span class="nctnt ncbi-code"> // These types aren't used in this demo, but might be in</span><br/><span class="nctnt ncbi-code"> // your code.</span><br/><span class="nctnt ncbi-code"> case eDB\_ComputeResult:</span><br/><span class="nctnt ncbi-code"> case eDB\_CursorResult:</span><br/><span class="nctnt ncbi-code"> LOG\_POST\_X(1, Warning \<\< "Unhandled results type:"</span><br/><span class="nctnt ncbi-code"> \<\< rs-\>GetResultType());</span><br/><span class="nctnt ncbi-code"> break;</span><br/><br/><span class="nctnt ncbi-code"> // Any other type means this code is out-of-date.</span><br/><span class="nctnt ncbi-code"> default:</span><br/><span class="nctnt ncbi-code"> ERR\_POST\_X(1, Critical \<\< "Unexpected results type:"</span><br/><span class="nctnt ncbi-code"> \<\< rs-\>GetResultType());</span><br/><span class="nctnt ncbi-code"> }</span><br/><span class="nctnt ncbi-code">}</span><br/><span class="nctnt ncbi-code">// The stored procedure will return a status.</span><br/><span class="nctnt ncbi-code">NcbiCout \<\< "\\nStored procedure returned status: "</span><br/><span class="nctnt ncbi-code"> \<\< cstmt-\>GetReturnStatus() \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code">string msgs = m\_Ds-\>GetErrorInfo();</span><br/><span class="nctnt ncbi-code">if ( ! msgs.empty() ) {</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " Errors:" \<\< NcbiEndl;</span><br/><span class="nctnt ncbi-code"> NcbiCout \<\< " " \<\< msgs \<\< NcbiEndl;</span> |

The SDBAPI Library
------------------

### SDBAPI Overview

The following snippet is a simple example of executing static SQL using the NCBI simplified database API (SDBAPI):

    // Specify connection parameters.
    CSDB_ConnectionParam    db_params;
    db_params.Set(CSDB_ConnectionParam::eUsername, m_User);
    db_params.Set(CSDB_ConnectionParam::ePassword, m_Password);
    db_params.Set(CSDB_ConnectionParam::eService,  m_Service);
    db_params.Set(CSDB_ConnectionParam::eDatabase, m_DbName);

    // Connect to the database.
    CDatabase  db(db_params);
    db.Connect();

    // Execute a query.
    CQuery query = db.NewQuery("select title from Journal");
    query.Execute();
    ITERATE(CQuery, qit, query) {
        NcbiCout << qit["title"].AsString() << NcbiEndl; 
    }

<span class="nctnt highlight">Note:</span> Load balancing is performed automatically and transparently with SDBAPI - there's no need to call <span class="nctnt ncbi-func">DBLB\_INSTALL\_DEFAULT()</span>.

See the SDBAPI sample programs for more example code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/sdbapi/>

The following sections cover specific aspects of SDBAPI:

-   [Includes and Linkage](#includes-and-linkage)

-   [Connections](#connections)

-   [Executing Basic Queries](#executing-basic-queries)

-   [Stored Procedures and Parameters](#stored-procedures-and-parameters)

-   [Retrieving Results](#retrieving-results)

-   [Getting a Stored Procedure Return Value](#getting-a-stored-procedure-return-value)

-   [Meta-Data Accessors](#meta-data-accessors)

-   [Working with NULL](#working-with-null)

-   [Using Transactions](#using-transactions)

-   [Using Cursors](#using-cursors)

### Includes and Linkage

SDBAPI requires only one header:

    #include <dbapi/simple/sdbapi.hpp>

For proper linkage, merge the following into your LIB and LIBS lines:

    LIB  = $(SDBAPI_LIB) xconnect xutil xncbi
    LIBS = $(SDBAPI_LIBS) $(NETWORK_LIBS) $(DL_LIBS) $(ORIG_LIBS)

### Connections

To establish a database connection, first specify the connection parameters, then create the database object using those parameters, then call the <span class="nctnt ncbi-func">Connect()</span> method, for example:

    // Specify connection parameters.
    CSDB_ConnectionParam    db_params;
    db_params.Set(CSDB_ConnectionParam::eUsername, m_User);
    db_params.Set(CSDB_ConnectionParam::ePassword, m_Password);
    db_params.Set(CSDB_ConnectionParam::eService,  m_Service);
    db_params.Set(CSDB_ConnectionParam::eDatabase, m_DbName);

    // Connect to the database.
    m_Db = CDatabase(db_params);
    m_Db.Connect();

Additional parameters are available for connection pooling, timeouts, etc. Please see the <span class="nctnt ncbi-class">CSDB\_ConnectionParam</span> [class reference](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCSDB__ConnectionParam.html) for the up-to-date list.

After making the connection, it is recommended to set the connection session parameters, for example:

    string sql("SET ANSI_NULLS ON\n"
               "SET ANSI_PADDING ON\n"
               "SET ANSI_WARNINGS ON\n"
               "SET ARITHABORT ON\n"
               "SET CONCAT_NULL_YIELDS_NULL ON\n"
               "SET QUOTED_IDENTIFIER ON\n"
               "SET NUMERIC_ROUNDABORT OFF");
    CQuery query = m_Db.NewQuery(sql);
    query.Execute();

It may also be appropriate to set, <span class="nctnt ncbi-monospace">TEXTSIZE</span>, depending on your project.

### Executing Basic Queries

Once a [connection](#connection) is established, executing a basic query can be as simple as:

    CQuery query = m_Db.NewQuery("your SQL statement goes here");
    query.Execute();

<span class="nctnt highlight">Note:</span> There are security factors to be considered when executing queries. See the [security section](#security-section), and use the following guidelines when deciding which method to use:

1  
If stored procuedures can be used, then use them. This increases both security and performance. Plus, this practice could facilitate testing and documentation.

2  
Otherwise, if the SQL statement does not depend on dynamic values, then use static SQL.

3  
Otherwise, if parameterized SQL can be used, then use it.

4  
Otherwise, as a last resort, use dynamic SQL. If user-supplied data is used to construct the statement, then you **MUST** sanitize the user-supplied data. Even if the data does not come from an outside user, it's still a good idea to sanitize all dynamic data unless there's a compelling reason not to, in which case that fact should be prominently documented in comments adjacent to the dynamic SQL creation code. See the [Security FAQ](ch_faq.html#ch_faq.Security) for more information.

### Stored Procedures and Parameters

The following settings should be made before executing a store procedure:

|----------------------------------------------------------------------|-----------------------------------------------|
| <span class="nctnt ncbi-monospace">ANSI\_NULLS</span>                | <span class="nctnt ncbi-monospace">ON</span>  |
| <span class="nctnt ncbi-monospace">ANSI\_PADDING</span>              | <span class="nctnt ncbi-monospace">ON</span>  |
| <span class="nctnt ncbi-monospace">ANSI\_WARNINGS</span>             | <span class="nctnt ncbi-monospace">ON</span>  |
| <span class="nctnt ncbi-monospace">ARITHABORT</span>                 | <span class="nctnt ncbi-monospace">ON</span>  |
| <span class="nctnt ncbi-monospace">CONCAT\_NULL\_YIELDS\_NULL</span> | <span class="nctnt ncbi-monospace">ON</span>  |
| <span class="nctnt ncbi-monospace">QUOTED\_IDENTIFIER</span>         | <span class="nctnt ncbi-monospace">ON</span>  |
| <span class="nctnt ncbi-monospace">NUMERIC\_ROUNDABORT</span>        | <span class="nctnt ncbi-monospace">OFF</span> |

It may also be appropriate to set, <span class="nctnt ncbi-monospace">TEXTSIZE</span>, depending on your project.

Note that <span class="nctnt ncbi-var">ANSI\_NULLS</span> and <span class="nctnt ncbi-var">QUOTED\_IDENTIFIER</span> must be set when the stored procedure is created because they can't be changed at run-time. The other settings can be changed at run-time but typically they're not, so it's efficient to set them when the [connection](#connection) is created.

For input parameters, simply pass the name and value to <span class="nctnt ncbi-func">SetParameter()</span>; for input/output parameters, also pass the database field type and the value <span class="nctnt ncbi-var">eSP\_InOut</span>; for <span class="nctnt ncbi-monospace">NULL</span> parameters call <span class="nctnt ncbi-func">SetNullParameter()</span>. For example:

    // input only
    query.SetParameter("@id", 123);
    query.SetNullParameter("@create_time", eSDB_DateTime);

    // input/output
    query.SetParameter("@tolower", "aBcD", eSDB_String, eSP_InOut); 
    query.SetNullParameter("@result", eSDB_Int, eSP_InOut);

The stored procedure is invoked by calling <span class="nctnt ncbi-func">ExecuteSP()</span>:

    CQuery query = m_Db.NewQuery();
    query.SetParameter("@max_id", 5);
    query.ExecuteSP(proc_name);

### Retrieving Results

It's best to reference columns by name, unless there's some compelling reason not to, because referencing columns by index is less maintanable. <span class="nctnt highlight">Note:</span> For the Toolkit database APIs, array-like indexes are 1-based, not 0-based. In other words, <span class="nctnt ncbi-func">operator[]()</span> starts at one for rows and columns.

For simple queries or stored procedures that return a single result set:

    query.ExecuteSP(sproc_name);
    ITERATE(CQuery, qit, query.SingleSet()) {
        NcbiCout << qit["release_id"].AsInt4() << NcbiEndl;
        // or, if there's a reason not to use column names
        NcbiCout << qit[1].AsInt4() << NcbiEndl;
    }

For stored procedures that return multiple result sets, SDBAPI supports two behavioral modes: either merging all result sets into one - chosen by calling <span class="nctnt ncbi-code">query.SingleSet()</span> - or returning them separately - chosen by calling <span class="nctnt ncbi-code">query.MultiSet()</span>. If neither <span class="nctnt ncbi-func">SingleSet()</span> nor <span class="nctnt ncbi-func">MultiSet()</span> is called, then SDBAPI chooses a default mode. The default was formerly MultiSet mode, but it has been switched to SingleSet mode with the release stable components 15.

Retrieving multiple result sets in SingleSet mode is accomplished exactly as if there was only one result set, as shown above. To retrieve multiple result sets in MultiSet mode, use <span class="nctnt ncbi-code">query.HasMoreResultSets()</span>:

    while (query.HasMoreResultSets()) {
        ITERATE(CQuery, qit, query) {
            // do something with the current result set row
        }
    }

It is possible to verify that all data rows have been read, by using <span class="nctnt ncbi-func">VerifyDone()</span>:

    ITERATE(CQuery, qit, query) {
        // do something with the current result set row
    }
    query.VerifyDone(); // throws if any rows remain unread

Similarly, if a certain number or range of rows is expected, you can verify that the total number of rows matches the expected number or range (with no rows remaining):

    query.RequireRowCount(5);                // e.g. "expect exactly five rows";
    query.RequireRowCount(100, kMax_Auto);   // or, "expect at least one hundred rows"
    ITERATE(CQuery, qit, query) {
        // do something with the current result set row
    }
    query.VerifyDone(); // throws if the expected number of rows wasn't retrieved

<span class="nctnt highlight">Note: </span><span class="nctnt ncbi-func">VerifyDone()</span> is not a simple, read-only informational method. In its quest to determine if any result sets or rows remain unread, it will read and purge any unread rows. Furthermore, it doesn't return <span class="nctnt ncbi-type">bool</span> to indicate whether all expected records have been read. Instead, it throws an exception (after reading and purging) if all records had not been read prior to its call, or if the number of read rows was unexpected per <span class="nctnt ncbi-func">RequireRowCount()</span>.

### Getting a Stored Procedure Return Value

To get the return value from a stored procedure, simply call <span class="nctnt ncbi-func">GetStatus()</span>.

### Meta-Data Accessors

A handful of accessors are available for query meta-data:

-   <span class="nctnt ncbi-func">GetColumnName()</span> -- Gets the name of the given column in the current result set.

-   <span class="nctnt ncbi-func">GetColumnType()</span> -- Gets the type of the given column in the current result set.

-   <span class="nctnt ncbi-func">GetRowCount()</span> -- Gets the number of rows read after a statement is executed.

-   <span class="nctnt ncbi-func">GetStatus()</span> -- Gets the value returned from a stored procedure.

-   <span class="nctnt ncbi-func">GetTotalColumns()</span> -- Gets the number of columns in the current result set.

<span class="nctnt highlight">Note:</span> These accessors are intended to be used after retrieving the results - they will result in exceptions if called before all data is read. The API doesn't include any accessors for the same information prior to reading the data.

### Working with NULL

SDBAPI, as a "simple" database API, does not provide a separate <span class="nctnt ncbi-monospace">NULL</span> value. To set an input parameter to <span class="nctnt ncbi-monospace">NULL</span>:

    query.SetNullParameter("@some_param", eSDB_Int);

To determine if a output parameter result is <span class="nctnt ncbi-monospace">NULL</span>:, call <span class="nctnt ncbi-func">IsNull()</span>:

    ITERATE(CQuery, qit, query) {
        if (qit["id"].IsNull()) {
            // handle NULL
        } else {
            x = qit["id"].AsInt4(); // OK to use the converted value
        }

<span class="nctnt highlight">Note:</span> The <span class="nctnt ncbi-func">As\*()</span> methods will return default values (e.g. zero or an empty string) when the actual result value is <span class="nctnt ncbi-monospace">NULL</span>. The only way to know if a <span class="nctnt ncbi-monospace">NULL</span> result was converted is to call <span class="nctnt ncbi-func">IsNull()</span>. Therefore, if you need to distinguish between <span class="nctnt ncbi-monospace">NULL</span> and default values, you **must** call <span class="nctnt ncbi-func">IsNull()</span>.

### Using Transactions

SDBAPI does not provide any special API support for transactions, so simply run, for example:

    m_Db.NewQuery("BEGIN TRAN").Execute();

### Using Cursors

SDBAPI does not support cursors. If you need cursors, you must use [DBAPI](#dbapi).

The DBAPI Library
-----------------

### DBAPI User-Layer Reference

#### DBAPI Overview

DBAPI is a consistent, object-oriented programming interface to multiple back-end databases. It encapsulates leading relational database vendors' APIs and is universal for all applications regardless of which database is used. It frees developers from dealing with the low-level details of a particular database vendor's API, allowing them to concentrate on domain-specific issues and build appropriate data models. It allows developers to write programs that are reusable with many different types of relational databases and to drill down to the native database APIs for added control when needed.

DBAPI has open SQL interface. It takes advantage of database-specific features to maximize performance and allows tight control over statements and their binding and execution semantics.

DBAPI has "Native" Access Modules for Sybase, Microsoft SQL Server, SQLITE, and ODBC. It provides native, high-performance implementations for supported vendor databases. It allows porting to other databases with minimal code changes.

DBAPI is split into low-layer and user-layer.

In addition, a simplified C++ API ([SDBAPI](#sdbapi)) layer is provided for cases where the full DBAPI feature set is not required.

See the [DBAPI configuration parameters reference](ch_libconfig.html#ch_libconfig.DBAPI) for details on configuring the DBAPI library.

See the DBAPI sample programs for example code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/>

The following sections cover specific aspects of DBAPI in detail.

-   [Object Hierarchy](#object-hierarchy)

-   [Includes](#includes)

-   [Objects](#objects)

-   [Object Life Cycle](#object-life-cycle)

-   [CVariant Type](#cvariant-type)

-   [Choosing the Driver](#choosing-the-driver)

-   [Data Source and Connections](#data-source-and-connections)

-   [Main Loop](#main-loop)

-   [Input and Output Parameters](#input-and-output-parameters)

-   [Stored Procedures](#stored-procedures)

-   [Cursors](#cursors)

-   [Working with BLOBs](#working-with-blobs)

-   [Updating BLOBs Using Cursors](#updating-blobs-using-cursors)

-   [Using Bulk Insert](#using-bulk-insert)

-   [Diagnostic Messages](#diagnostic-messages)

-   [Trace Output](#trace-output)

#### Object Hierarchy

See [Figure 1](#figure-1).

[![Figure 1. Object Hierarchy](/book-test/static/img/dbapi_user.gif)](/book-test/static/img/dbapi_user.gif "Click to see the full-resolution image")

Figure 1. Object Hierarchy

#### Includes

For most purposes it is sufficient to include one file in the user source file: <span class="nctnt ncbi-path">dbapi.hpp</span>.

    #include <dbapi/dbapi.hpp>

For static linkage the following include file is also necessary:

    #include <dbapi/driver/drivers.hpp>

#### Objects

All objects are returned as pointers to their respective interfaces. The null (0) value is valid, meaning that no object was returned.

#### Object Life Cycle

In general, any child object is valid only in the scope of its parent object. This is because most of the objects share the same internal structures. There is no need to delete every object explicitly, as all created objects will be deleted upon program exit. Specifically, all objects are managed by the static <span class="nctnt ncbi-class">CDriverManager</span> object, and will be destroyed when <span class="nctnt ncbi-class">CDriverManager</span> is destroyed. It is possible to delete any object from the framework and it is deleted along with all derived objects. For example, when an [IConnection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIConnection.html) object is deleted, all derived [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html), [ICallableStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classICallableStatement.html) and [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html) objects will be deleted too. If the number of the objects (for instance [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)) is very high, it is recommended to delete them explicitly or enclose in the <span class="nctnt ncbi-type">auto\_ptr\<...\></span> template. For each object a <span class="nctnt ncbi-func">Close()</span> method is provided. It disposes of internal resources, required for the proper library cleanup, but leaves the framework intact. After calling <span class="nctnt ncbi-func">Close()</span> the object becomes invalid. This method may be necessary when the database cleanup and framework cleanup are performed in different places of the code.

#### CVariant Type

The [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) type is used to represent any database data type (except BLOBs). It is an object, not a pointer, so it behaves like a primitive C++ type. Basic comparison operators are supported (==, !=, \< ) for identical internal types. If types are not identical, [CVariantException](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariantException.html) is thrown. [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) has a set of getters to extract a value of a particular type, e.g. <span class="nctnt ncbi-func">GetInt4()</span>, <span class="nctnt ncbi-func">GetByte()</span>,<span class="nctnt ncbi-func"> GetString()</span>, etc. If <span class="nctnt ncbi-func">GetString()</span> is called for a different type, like <span class="nctnt ncbi-type">DateTime</span> or <span class="nctnt ncbi-type">integer</span> it tries to convert it to a string. If it doesn't succeed, [CVariantException](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariantException.html) is thrown. There is a set of factory methods (static functions) for creating [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) objects of a particular type, such as <span class="nctnt ncbi-func">CVariant::BigInt()</span>, <span class="nctnt ncbi-func">CVariant::SmallDateTime()</span>, <span class="nctnt ncbi-func">CVariant::VarBinary()</span> etc. For more details please see the comments in [variant.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/dbapi/variant.hpp) file.

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_object.cpp>

#### Choosing the Driver

There are two drivers for working with different SQL servers on different platforms. The ones presently implemented are "<span class="nctnt ncbi-lib">ftds</span>" (MS SQL, Sybase, cross platform) and "<span class="nctnt ncbi-lib">ctlib</span>" (Sybase). For static linkage these drivers should be registered manually; for dynamic linkage this is not necessary. The [CDriverManager](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDriverManager.html) object maintains all registered drivers. Manual registration is done with the RegisterDriver functions:

    DBAPI_RegisterDriver_FTDS();
    DBAPI_RegisterDriver_CTLIB();

<span class="nctnt highlight">Note:</span> FTDS is the primary driver and should be used unless there's a very specific reason why CTLIB must be used. Also, CTLIB is limited to Linux and Solaris connecting to Sybase.

As of July, 2014, there were a couple of issues with FTDS to be aware of:

-   When using FTDS to connect to SQL Server, there are some limitations in updating LOB-fields which participate in replication.

-   When using FTDS to connect to Sybase Open Server, you must explicitly set TDS version to 5.0, otherwise the connect operation will hang. Also, explicitly configuring the packet size setting to 3584 (7 \* 512) has historically been helpful.

More details on [recommended drivers](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Recommended_DBAPI_drivers) are available to users inside NCBI.

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_context.cpp>

#### Data Source and Connections

The [IDataSource](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIDataSource.html) interface defines the database platform. To create an object implementing this interface, use the method <span class="nctnt ncbi-func">CreateDs(const string& driver)</span>. An [IDataSource](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIDataSource.html) can create objects represented by an [IConnection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIConnection.html) interface, which is responsible for the connection to the database. It is highly recommended to specify the database name as an argument to the <span class="nctnt ncbi-func">CreateConnection()</span> method, or use the <span class="nctnt ncbi-func">SetDatabase()</span> method of a [CConnection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCConnection.html) object instead of using a regular SQL statement. In the latter case, the library won't be able to track the current database.

    IDataSource *ds = dm.CreateDs("ctlib");
    IConnection *conn = ds->CreateConnection();
    conn->Connect("user", "password", "server", "database");
    IStatement *stmt = conn->CreateStatement();

Every additional call to [IConnection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIConnection.html)<span class="nctnt ncbi-func">::CreateStatement()</span> results in cloning the connection for each statement. These connections inherit the same default database, which was specified in the <span class="nctnt ncbi-func">Connect()</span> or <span class="nctnt ncbi-func">SetDatabase()</span> method. Thus if the default database was changed by calling <span class="nctnt ncbi-func">SetDatabase()</span>, all subsequent cloned connections created by <span class="nctnt ncbi-func">CreateStatement()</span> will inherit this particular default database.

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/dbapi_simple.cpp>

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_connection.cpp>

#### Main Loop

The library simulates the main result-retrieving loop of the Sybase client library by using the [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html)<span class="nctnt ncbi-func">::HasMoreResults()</span> method:

    stmt->Execute("select * from MyTable");
    while( stmt->HasMoreResults() ) {
        if( stmt->HasRows() ) {
            IResultSet *rs = stmt->GetResultset();

            // Retrieve results, if any

            while( rs->Next() ) {
                int col1 = rs->GetVariant(1).GetInt4();
                ...
            }
        }
    }

This method should be called until it returns <span class="nctnt ncbi-monospace">false</span>, which means that no more results are available. It returns as soon as a result is ready. The type of the result can be obtained by calling the [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::GetResultType()</span> method. Supported result types are <span class="nctnt ncbi-var">eDB\_RowResult, eDB\_ParamResult, eDB\_ComputeResult, eDB\_StatusResult, eDB\_CursorResult</span>. The method [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html)<span class="nctnt ncbi-func">::GetRowCount()</span> returns the number of updated or deleted rows.

The [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html)<span class="nctnt ncbi-func">::ExecuteUpdate()</span> method is used for SQL statements that do not return rows:

    stmt->ExecuteUpdate("update...");
    int rows = stmt->GetRowCount();

The method [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html)<span class="nctnt ncbi-func">::GetResultSet()</span> returns an [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html) object. The method [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::Next()</span> actually does the fetching, so it should be always called first. It returns <span class="nctnt ncbi-monospace">false</span> when no more fetch data is available. All column data, except Image and Text is represented by a single [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) object. The method [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::GetVariant()</span> takes one parameter, the column number. Column numbers start with 1.

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/dbapi_simple.cpp>

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_stmt.cpp>

#### Input and Output Parameters

The method [ICallableStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classICallableStatement.html)<span class="nctnt ncbi-func">::SetParam(const CVariant& v, const string& name)</span> is used to pass parameters to stored procedures and dynamic SQL statements. To ensure the correct parameter type it is recommended to use [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) type factories (static methods) to create a [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) of the required internal type. There is no internal representation for the BIT parameter type, please use TinyInt of Int types with 0 for <span class="nctnt ncbi-monospace">false</span> and 1 for <span class="nctnt ncbi-monospace">true</span> respectively. Here are a few examples: [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::Int4(Int4 \*p)</span>, [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::TinyInt(UInt1 \*p)</span>, [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::VarChar(const char \*p, size\_t len )</span> etc.

There are also corresponding constructors, like [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::CVariant(Int4 v)</span>, [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::CVariant(const string& s)</span>, ..., but the user must ensure the proper type conversion in the arguments, and not all internal types can be created using constructors.

Output parameters are set by the [ICallableStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classICallableStatement.html)<span class="nctnt ncbi-func">::SetOutputParam(const CVariant& v, const string& name)</span> method, where the first argument is a null [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) of a particular type, e.g. <span class="nctnt ncbi-code">SetOutputParam(CVariant(eDB\_SmallInt),"@arg")</span>.

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/dbapi_simple.cpp>

#### Stored Procedures

The [ICallableStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classICallableStatement.html) object is used for calling stored procedures. First get the object itself by calling [IConnection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIConnection.html)<span class="nctnt ncbi-func">::PrepareCall()</span>. Then set any parameters. If the parameter name is empty, the calls to <span class="nctnt ncbi-func">SetParam()</span> should be in the exact order of the actual parameters. Retrieve all results in the main loop. Get the status of the stored procedure using the [ICallableStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classICallableStatement.html)<span class="nctnt ncbi-func">::GetReturnStatus()</span> method.

    ICallableStatement *cstmt = conn->PrepareCall("ProcName");
    Uint1 byte = 1;
    cstmt->SetParam(CVariant("test"), "@test_input");
    cstmt->SetParam(CVariant::TinyInt(&byte), "@byte");
    cstmt->SetOutputParam(CVariant(eDB_Int), "@result");
    cstmt->Execute();
    while(cstmt->HasMoreResults()) {
        if( cstmt->HasRows() ) {
            IResultSet *rs = cstmt->GetResultSet();
            switch( rs->GetResultType() ) {
                case eDB_RowResult:
                    while(rs->Next()) {

                    // retrieve row results

                    }
                    break;
                case eDB_ParamResult:
                    while(rs->Next()) {

                    // Retrieve parameter row

                    }
                    break;
            }
        }
    }

    // Get status
    int status = cstmt->GetReturnStatus();

It is also possible to use [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html) interface to call stored procedures using standard SQL language call. The difference from [ICallableStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classICallableStatement.html) is that there is no <span class="nctnt ncbi-func">SetOutputParam()</span> call. The output parameter is passed with a regular <span class="nctnt ncbi-func">SetParam()</span> call having a *non-null* [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) argument. There is no <span class="nctnt ncbi-func">GetReturnStatus()</span> call in [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html), so use the result type filter to get it - although note that result sets with type <span class="nctnt ncbi-monospace">eDB\_StatusResult</span> are not always guaranteed to be returned when using the [IStatement](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIStatement.html) interface.

    sql = "exec SampleProc @id, @f, @o output";
    stmt->SetParam(CVariant(5), "@id");
    stmt->SetParam(CVariant::Float(&f), "@f");
    stmt->SetParam(CVariant(5), "@o");
    stmt->Execute(sql);
    while(stmt->HasMoreResults()) {
        IResultSet *rs = stmt->GetResultSet();

        if( rs == 0 )
            continue;

        switch( rs->GetResultType() ) {
        case eDB_ParamResult:
            while( rs->Next() ) {
                NcbiCout << "Output param: "
                         << rs->GetVariant(1).GetInt4()
                         << NcbiEndl;
            }
            break;
        case eDB_StatusResult:
            while( rs->Next() ) {
                NcbiCout << "Return status: "
                         << rs->GetVariant(1).GetInt4()
                         << NcbiEndl;
            }
            break;
        case eDB_RowResult:
            while( rs->Next() ) {
                if( rs->GetVariant(1).GetInt4() == 2121 ) {
                    NcbiCout << rs->GetVariant(2).GetString() << "\|"
                             << rs->GetVariant(3).GetString() << "\|"
                             << rs->GetVariant(4).GetString() << "\|"
                             << rs->GetVariant(5).GetString() << "\|"
                             << rs->GetVariant(6).GetString() << "\|"
                             << rs->GetVariant(7).GetString() << "\|"
                             << NcbiEndl;
                } else {
                    NcbiCout << rs->GetVariant(1).GetInt4() << "\|"
                             << rs->GetVariant(2).GetFloat() << "\|"
                             << rs->GetVariant("date_val").GetString() << "\|"
                             << NcbiEndl;
                }
            }
            break;
        }
    }
    stmt->ClearParamList();

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/dbapi/dbapi_simple.cpp>

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_proc.cpp>

#### Cursors

The library currently supports basic cursor features such as setting parameters and cursor update and delete operations.

    ICursor *cur = conn->CreateCursor("table_cur",
                                      "select ... for update of ...");
    IResultSet *rs = cur->Open();
    while(rs->Next()) {
        cur->Update(table, sql_statement_for_update);
    }
    cur->Close();

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_cursor.cpp>

#### Working with BLOBs

Due to the possibly very large size, reading and writing BLOBs requires special treatment. During the fetch the contents of the whole column must be read before advancing to the next one. That's why the columns of type IMAGE and TEXT are not bound to the corresponding variables in the resultset and all subsequent columns are not bound either. So it is recommended to put the BLOB columns at the end of the column list. There are several ways to read BLOBs, using [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::Read()</span>, [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::GetBlobIStream()</span>, and [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::GetBlobReader()</span> methods. The first is the most efficient; it reads data into a supplied buffer until it returns 0 bytes read. The next call will read from the next column. The second method implements the STL istream interface. After each successful column read you should get another istream for the next column. The third implements the C++ Toolkit <span class="nctnt ncbi-func">IReader</span> interface. If the data size is small and double buffering is not a performance issue, the BLOB columns can be bound to [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) variables by calling [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::BindBlobToVariant(true)</span>. In this case the data should be read using [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::Read()</span> and [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::GetBlobSize()</span>. To write BLOBs there are also several options. To pass a BLOB as a SQL parameter you should store it in a [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html) using [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::Append()</span> and [CVariant](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCVariant.html)<span class="nctnt ncbi-func">::Truncate()</span> methods. To store a BLOB in the database you should initialize this column first by writing a zero value (<span class="nctnt ncbi-var">0x0</span>) for an IMAGE type or a space value (<span class="nctnt ncbi-monospace">' '</span>) for a TEXT type. After that you can open a regular [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html) or [ICursor](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classICursor.html) and for each required row update the BLOB using [IResultSet](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIResultSet.html)<span class="nctnt ncbi-func">::GetBlobOStream()</span>. NOTE: this call opens an additional connection to the database.

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_lob.cpp>

#### Updating BLOBs Using Cursors

It is recommended to update BLOBs using cursors, because no additional connections are opened and this is the only way to work with ODBC drivers.

    ICursor *blobCur = conn->CreateCursor("test",
                 "select id, blob from BlobSample for update of blob");
    IResultSet *blobRs = blobCur->Open();
    while(blobRs->Next()) {
        ostream& out = blobCur->GetBlobOStream(2, blob.size());
        out.write(buf, blob.size());
        out.flush();
    }

Note that <span class="nctnt ncbi-func">GetBlobOStream()</span> takes the column number as the first argument and this call is invalid until the cursor is open.

#### Using Bulk Insert

Bulk insert is useful when it is necessary to insert big amounts of data. The <span class="nctnt ncbi-func">IConnection::CreateBulkInsert()</span> takes one parameter, the table name. The number of columns is determined by the number of <span class="nctnt ncbi-func">Bind()</span> calls. The <span class="nctnt ncbi-func">CVariant::Truncate(size\_t len)</span> method truncates the internal buffer of CDB\_Text and CDB\_Image object from the end of the buffer. If no parameter specified, it erases the whole buffer.

    NcbiCout << "Initializing BlobSample table..." << NcbiEndl;
    IBulkInsert *bi = conn->CreateBulkInsert(tbl_name);
    CVariant col1 = CVariant(eDB_Int);
    CVariant col2 = CVariant(eDB_Text);
    bi->Bind(1, &col1);
    bi->Bind(2, &col2);
    for(int i = 0; i < ROWCOUNT; ++i ) {
        string im = "BLOB data " + NStr::IntToString(i);
        col1 = i;
        col2.Truncate();
        col2.Append(im.c_str(), im.size());
        bi->AddRow();
    }
    bi->Complete();

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_bcp.cpp>

#### Diagnostic Messages

The DBAPI library is integrated with the C++ Toolkit diagnostic and tracing facility. By default all client and server messages are handled by the Toolkit's standard message handler. However it is possible to redirect the DBAPI-specific messages to a single <span class="nctnt ncbi-class">CDB\_MultiEx</span> object and retrieve them later at any time. There are two types of redirection, per data source and per connection. The redirection from a data source is enabled by calling <span class="nctnt ncbi-func">IDataSource::SetLogStream(0)</span>. After the call all client- and context-specific messages will be stored in the <span class="nctnt ncbi-func">IDataSource</span> object. The <span class="nctnt ncbi-func">IDataSource::GetErrorInfo()</span> method will return the string representation of all accumulated messages and clean up the storage. The <span class="nctnt ncbi-func">IDataSource::GetErrorAsEx()</span> will return a pointer to the underlying <span class="nctnt ncbi-class">CDB\_MultiEx</span> object. Retrieving information and cleaning up is left to the developer. Do NOT delete this object. The connection-specific redirection is controlled by calling <span class="nctnt ncbi-func">IConnection::MsgToEx(boolean enable)</span> method. This redirection is useful; for instance, to temporarily disable default messages from the database server. The <span class="nctnt ncbi-func">IConnection::GetErrorInfo()</span> and <span class="nctnt ncbi-func">IConnection::GetErrorAsEx()</span> methods work in the same manner as for the <span class="nctnt ncbi-func">IDataSource</span>

Related sample code:

-   <http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/test/dbapi_unit_test_msg.cpp>

#### Trace Output

The DBAPI library uses the Toolkit-wide DIAG\_TRACE environment variable to do the debug output. To enable it set it to any value. If you have any problems with the DBAPI please include the trace output into your email.

### DBAPI Driver Reference

The following sections cover low-level access to the various RDBMSs:

-   [Overview](#overview)

-   [The driver architecture](#the-driver-architecture)

-   [Sample program](#sample-program)

-   [Error handling](#error-handling)

-   [Driver context and connections](#driver-context-and-connections)

-   [Driver Manager](#driver-manager)

-   [Text and Image Data Handling](#text-and-image-data-handling)

-   [Results loop](#results-loop)

#### Overview

SDBAPI clients can only use [FreeTDS](#freetds), but DBAPI clients must choose the lower-level driver.

The NCBI DBAPI driver library describes and implements a set of objects needed to provide a uniform low-level access to the various relational database management systems (RDBMS). The basic driver functionality is the same as in most other RDBMS client APIs. It allows opening a connection to a server, executing a command (query) on this connection and getting the results back. The main advantage of using the driver is that you don't have to change your own upper-level code if you need to move from one RDBMS client API to another.

The driver can use two different methods to access the particular RDBMS. If the RDBMS provides a client library for the given computer system (e.g. Sun/Solaris), then the driver uses that library. If no such client library exists, then the driver connects to an RDBMS through a special gateway server which is running on a computer system where such a library does exist.

#### The driver architecture

There are two major groups of the driver's objects: the RDBMS-independent objects, and the objects which are specific to a particular RDBMS. The only RDBMS-specific object which user should be aware of is a "Driver Context". The "Driver Context" is effectively a "Connection" factory. The only way to make a connection to the server is to call the <span class="nctnt ncbi-func">Connect()</span> method of a "Driver Context" object. So, before doing anything with an RDBMS, you need to create at least one driver context object. All driver contexts implement the same interface defined in [I\_DriverContext](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classI__DriverContext.html) class. If you are working on a library which could be used with more than one RDBMS, the driver context should not be created by the library. Instead, the library API should include a pointer to [I\_DriverContext](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=I_DriverContext&d=C) so an existing driver context can be passed in.

There is no "real" factory for driver contexts because it's not always possible to statically link the RDBMS libraries from different vendors into the same binary. Most of them are written in C and name collisions do exist. The [Driver Manager](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=C_DriverMgr&d=C) helps to overcome this problem. It allows creating a mixture of statically linked and dynamically loaded drivers and using them together in one executable.

The driver context creates the connection which is RDBMS-specific, but before returning it to the caller it puts it into an RDBMS-independent "envelope", [CDB\_Connection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Connection.html). The same is true for the commands and for the results - the user gets the pointer to the RDBMS-independent "envelope object" instead of the real one. It is the caller's responsibility to delete those objects. The life spans of the real object and the envelope object are not necessarily the same.

Once you have the connection object, you could use it as a factory for the different types of commands. The command object in turn serves as a factory for the results. The connection is always single threaded, that means that you have to execute the commands and process their results sequentially one by one. If you need to execute the several commands in parallel, you can do it using multiple connections.

Another important part of the driver is error and message handling. There are two different mechanisms implemented. The first one is exceptions. All exceptions which could be thrown by the driver are inherited from the single base class [CDB\_Exception](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Exception.html). Drivers use the exception mechanism whenever possible, but in many cases the underlying client library uses callbacks or handlers to report error messages rather than throwing exceptions. The driver supplies a handler's stack mechanism to manage these cases.

To send and to receive the data through the driver you have to use the driver provided datatypes. The collection of the datatypes includes: [one](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__TinyInt.html), [two](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__SmallInt.html), [four](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Int.html) and [eight](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__BigInt.html) byte integers; [float](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Float.html) and [double](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Double.html); [numeric](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Numeric.html); [char](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Char.html), [varchar](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__VarChar.html), [binary](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Binary.html), [varbinary](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__VarBinary.html); [datetime](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__DateTime.html) and [smalldatetime](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__SmallDateTime.html); [text](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Text.html) and [image](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Image.html). All datatypes are derived from a single base class [CDB\_Object](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Object.html).

#### Sample program

This program opens one connection to the server and selects the database names and the date when each database was created (assuming that table "sysdatabases" does exist). In this example the string "XXX" should be replaced with the real driver name.

    #include <iostream>
    #include <dbapi/driver/public.hpp>
    #include <dbapi/driver/exception.hpp>
    /* Here, XXXlib has to be replaced with the real name, e.g. "ctlib" */
    #include <dbapi/driver/XXXlib/interfaces.hpp>
    USING_NCBI_SCOPE;
    int main()
    {
        try { // to be sure that we are catching all driver related exceptions
            // We need to create a driver context first
            // In real program we have to replace CXXXContext with something real
            CXXXContext my_context;
            // connecting to server "MyServer"
            // with user name "my_user_name" and password "my_password"
            CDB_Connection* con = my_context.Connect("MyServer", "my_user_name",
                                                     "my_password", 0);
            // Preparing a SQL query
            CDB_LangCmd* lcmd =
                con->LangCmd("select name, crdate from sysdatabases");
            // Sending this query to a server
            lcmd->Send();
            CDB_Char dbname(64);
            CDB_DateTime crdate;
            // the result loop
            while(lcmd->HasMoreResults()) {
                CDB_Result* r= lcmd->Result();
                // skip all but row result
                if (r == 0  \|\|  r->ResultType() != eDB_RowResult) {
                    delete r;
                    continue;
                }
                // printing the names of selected columns
                NcbiCout << r->ItemName(0) << " \t\t\t"
                         << r->ItemName(1) << NcbiEndl;
                // fetching the rows
                while ( r->Fetch() ) {
                    r->GetItem(&dbname); // get the database name
                    r->GetItem(&crdate); // get the creation date
                    NcbiCout << dbname.Value() << ' '
                             << crdate.Value().AsString("M/D/Y h:m")
                             << NcbiEndl;
                }
                delete r; // we don't need this result anymore
            }
            delete lcmd; // delete the command
            delete con;  // delete the connection
        }
        catch (CDB_Exception& e) { // printing the error messages
            CDB_UserHandler_Stream myExHandler(&cerr);
            myExHandler.HandleIt(&e);
        }
    }

#### Error handling

Error handling is almost always a pain when you are working with an RDBMS because different systems implement different approaches. Depending on the system, you can get error messages through return codes, callbacks, handlers, and/or exceptions. These messages could have different formats. It could be just an integer (error code), a structure, or a set of callback's arguments. The NCBI DBAPI driver intercepts all those error messages in all different formats and converts them into various types of objects derived from [CDB\_Exception](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Exception.html).

[CDB\_Exception](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Exception.html) provides the following methods for all exceptions:

-   <span class="nctnt ncbi-func">GetDBErrCode()</span> - returns the integer code for this message (assigned by SQL server).

-   <span class="nctnt ncbi-func">SeverityString(void)</span> - returns the severity string of this message (assigned by SQL server).

-   <span class="nctnt ncbi-func">GetErrCodeString()</span> - returns the name for this error code (e.g. "eSQL").

-   <span class="nctnt ncbi-func">Type()</span> - returns the type value for this exception type (e.g. eSQL).

-   <span class="nctnt ncbi-func">TypeString()</span> - returns the type string for this exception type (e.g. "eSQL"). This is a pass-through to <span class="nctnt ncbi-func">CException::GetType()</span>.

-   <span class="nctnt ncbi-func">ErrCode()</span> - alias for <span class="nctnt ncbi-func">GetDBErrCode()</span>.

-   <span class="nctnt ncbi-func">Message()</span> - returns the error message itself. This is a pass-through to <span class="nctnt ncbi-func">CException::GetMsg()</span>.

-   <span class="nctnt ncbi-func">OriginatedFrom()</span> - returns the SQL server name. This is a pass-through to <span class="nctnt ncbi-func">CException::GetModule()</span>.

-   <span class="nctnt ncbi-func">SetServerName()</span> - sets the SQL server name.

-   <span class="nctnt ncbi-func">GetServerName()</span> - returns the SQL server name.

-   <span class="nctnt ncbi-func">SetUserName()</span> - sets the SQL user name.

-   <span class="nctnt ncbi-func">GetUserName()</span> - returns the SQL user name.

-   <span class="nctnt ncbi-func">SetExtraMsg()</span> - sets extra message text to be included in the message output.

-   <span class="nctnt ncbi-func">GetExtraMsg()</span> - gets the extra message text.

-   <span class="nctnt ncbi-func">SetSybaseSeverity()</span> - sets the severity value for a Sybase exception - <span class="nctnt highlight">N.B.</span> Sybase severity values can be provided for the Sybase/FreeTDS ctlib driver only.

-   <span class="nctnt ncbi-func">GetSybaseSeverity()</span> - gets the severity value for a Sybase exception - <span class="nctnt highlight">N.B.</span> Sybase severity values can be provided by the Sybase/FreeTDS ctlib driver only.

-   <span class="nctnt ncbi-func">ReportExtra()</span> - outputs any extra text to the supplied stream.

-   <span class="nctnt ncbi-func">Clone()</span> - creates a new exception based on this one.

<span class="nctnt highlight">N.B.</span> The following [CDB\_Exception](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Exception.html) methods are deprecated:

-   <span class="nctnt ncbi-func">Severity()</span> - returns the severity value of this message (assigned by SQL server).

-   <span class="nctnt ncbi-func">SeverityString(EDB\_Severity sev)</span> - returns the severity string of this message (assigned by SQL server).

The DBAPI driver may throw any of the following types derived from [CDB\_Exception](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Exception.html):

-   [CDB\_SQLEx](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__SQLEx.html) This type is used if an error message has come from a SQL server and indicates an error in a SQL query. It could be a wrong table or column name or a SQL syntax error. This type provides the additional methods:

    -   <span class="nctnt ncbi-func">BatchLine()</span> - returns the line number in the SQL batch that generated the error.

    -   <span class="nctnt ncbi-func">SqlState()</span> - returns a byte string describing an error (it's not useful most of the time).

-   [CDB\_RPCEx](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__RPCEx.html) An error message has come while executing an RPC or stored procedure. This type provides the additional methods:

    -   <span class="nctnt ncbi-func">ProcName()</span> - returns the procedure name where the exception originated.

    -   <span class="nctnt ncbi-func">ProcLine()</span> - returns the line number within the procedure where the exception originated.

-   [CDB\_DeadlockEx](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__DeadlockEx.html) An error message has come as a result of a deadlock.

-   [CDB\_DSEx](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__DSEx.html) An error has come from an RDBMS and is not related to a SQL query or RPC.

-   [CDB\_TimeoutEx](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__TimeoutEx.html) An error message has come due to a timeout.

-   [CDB\_ClientEx](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__ClientEx.html) An error message has come from the client side.

Drivers use two ways to deliver an error message object to an application. If it is possible to throw an exception, then the driver throws the error message object. If not, then the driver calls the [user's error handler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__UserHandler.html) with a pointer to the error message object as an argument. It's not always convenient to process all types of error messages in one error handler. Some users may want to use a special error message handler inside some function or block and a default error handler outside. To accommodate these cases the driver provides a [handler stack mechanism](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDBHandlerStack&d=C). The top handler in the stack gets the error message object first. If it knows how to deal with this message, then it processes the message and returns <span class="nctnt ncbi-monospace">true</span>. If handler wants to pass this message to the other handlers, then it returns <span class="nctnt ncbi-monospace">false</span>. So, the driver pushes the error message object through the stack until it gets <span class="nctnt ncbi-monospace">true</span> from the handler. The [default driver's error handler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__UserHandler__Stream.html), which just prints the error message to <span class="nctnt ncbi-var">stderr</span>, is always on the bottom of the stack.

Another tool which users may want to use for error handling is the [CDB\_MultiEx](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__MultiEx.html) object. This tool allows collecting multiple [CDB\_Exception](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__Exception.html) objects into one container and then throwing the container as one exception object.

#### Driver context and connections

Every program which is going to work with an NCBI DBAPI driver should create at least one Driver Context object first. The main purpose of this object is to be a Connection factory, but it's a good idea to customize this object prior to opening a connection. The first step is to setup two message handler stacks. The first one is for error messages which are not bound to some particular connection or could occur inside the <span class="nctnt ncbi-func">Connect()</span> method. Use <span class="nctnt ncbi-func">PushCntxMsgHandler()</span> to populate it. The other stack serves as an initial message handler stack for all connections which will be derived from this context. Use <span class="nctnt ncbi-func">PushDefConnMsgHandler()</span> method to populate this stack. The second step of customization is setting timeouts. The <span class="nctnt ncbi-func">SetLoginTimeout()</span> and <span class="nctnt ncbi-func">SetTimeout()</span> methods do the job. If you are going to work with text or image objects in your program, you need to call <span class="nctnt ncbi-func">SetMaxTextImageSize()</span> to define the maximum size for such objects. Objects which exceed this limit could be truncated.

    class CMyHandlerForConnectionBoundErrors : public CDB_UserHandler
    {
        virtual bool HandleIt(CDB_Exception* ex);
        ...
    };
    class CMyHandlerForOtherErrors : public CDB_UserHandler
    {
        virtual bool HandleIt(CDB_Exception* ex);
        ...
    };
    ...
    int main()
    {
        CMyHandlerForConnectionBoundErrors conn_handler;
        CMyHandlerForOtherErrors           other_handler;
        ...
        try { // to be sure that we are catching all driver related exceptions
            // We need to create a driver context first
            // In real program we have to replace CXXXContext with something real
            CXXXContext my_context;
            my_context.PushCntxMsgHandler(&other_handler);
            my_context.PushDefConnMsgHandler(&conn_handler);
            // set timeouts (in seconds) and size limits (in bytes):
            my_context.SetLoginTimeout(10); // for logins
            my_context.SetTimeout(15);      // for client/server communications
            my_context.SetMaxTextImageSize(0x7FFFFFFF); // text/image size limit
            ...
            CDB_Connection* my_con =
                my_context.Connect("MyServer", "my_user_name", "my_password",
                                   I_DriverContext::fBcpIn);
            ...
        }
        catch (CDB_Exception& e) {
            other_handler.HandleIt(&e);
        }
    }

The only way to get a connection to a server in an NCBI DBAPI driver is through a <span class="nctnt ncbi-func">Connect()</span> method in [driver context](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classI__DriverContext.html). The first three arguments: server name, user name and password are obvious. Values for <span class="nctnt ncbi-var">mode</span> are constructed by a bitwise-inclusive-OR of flags defined in EConnectionMode. If <span class="nctnt ncbi-var">reusable</span> is <span class="nctnt ncbi-monospace">false</span>, then driver creates a new connection which will be destroyed as soon as user delete the correspondent [CDB\_Connection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDB_Connection&d=C) (the <span class="nctnt ncbi-var">pool\_name</span> is ignored in this case).

Opening a connection to a server is an expensive operation. If program opens and closes connections to the same server multiple times it worth calling the <span class="nctnt ncbi-func">Connect()</span> method with <span class="nctnt ncbi-var">reusable</span> set to <span class="nctnt ncbi-monospace">true</span>. In this case driver does not close the connection when the correspondent CDB\_Connection is deleted, but keeps it around in a "recycle bin". Every time an application calls the <span class="nctnt ncbi-func">Connect()</span> method with <span class="nctnt ncbi-var">reusable</span> set to <span class="nctnt ncbi-monospace">true</span>, driver tries to satisfy the request from a "recycle bin" first and opens a new connection only if necessary.

The <span class="nctnt ncbi-var">pool\_name</span> argument is just an arbitrary string. An application could use this argument to assign a name to one or more connections (to create a connection pool) or to invoke a connection by name from this pool.

    ...
    // Create a pool of four connections (two to one server and two to another)
    // with the default database "DatabaseA"
    CDB_Connection* con[4];
    int i;
    for (i = 4;  i--; ) {
        con[i]= my_context.Connect((i%2 == 0) ? "MyServer1" : "MyServer2",
                                   "my_user_name", "my_password", 0, true,
                                   "ConnectionPoolA");
        CDB_LangCmd* lcmd= con[i]->LangCmd("use DatabaseA");
        lcmd->Send();
        while(lcmd->HasMoreResults()) {
            CDB_Result* r = lcmd->Result();
            delete r;
        }
        delete lcmd;
    }
    // return all connections to a "recycle bin"
    for(i= 0; i < 4; delete con_array[i++]);
    ...
    // in some other part of the program
    // we want to get a connection from "ConnectionPoolA"
    // but we don't want driver to open a new connection if pool is empty
    try {
        CDB_Connection* my_con= my_context.Connect("", "", "", 0, true,
                                                   "ConnectionPoolA");
        // Note that server name, user name and password are empty
        ...
    }
    catch (CDB_Exception& e) {
        // the pool is empty
        ...
    }

An application could combine in one pool the connections to the different servers. This mechanism could also be used to group together the connections with some particular settings (default database, transaction isolation level, etc.).

#### Driver Manager

It's not always known which NCBI DBAPI driver will be used by a particular program. Sometimes you want a driver to be a parameter in your program. Sometimes you need to use two different drivers in one binary but can not link them statically because of name collisions. Sometimes you just need the driver contexts factory. The [Driver Manager](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=C_DriverMgr&d=C) is intended to solve these problems.

Let's rewrite our [Sample program](#sample-program) using the <span class="nctnt ncbi-monospace">Driver Manager</span>. The original text was.

    #include <iostream>
    #include <dbapi/driver/public.hpp>
    #include <dbapi/driver/exception.hpp>
    /* Here, XXXlib has to be replaced with the real name, e.g. "ctlib" */
    #include <dbapi/driver/XXXlib/interfaces.hpp>
    USING_NCBI_SCOPE;
    int main()
    {
        try { // to be sure that we are catching all driver related exceptions
            // We need to create a driver context first
            // In real program we have to replace CXXXContext with something real
            CXXXContext my_context;
            // connecting to server "MyServer"
            // with user name "my_user_name" and password "my_password"
            CDB_Connection* con = my_context.Connect("MyServer", "my_user_name",
                                                     "my_password", 0);
            ...

If we use the <span class="nctnt ncbi-monospace">Driver Manager</span> we could allow the driver name to be a program argument.

    #include <iostream>
    #include <dbapi/driver/public.hpp>
    #include <dbapi/driver/exception.hpp>
    #include <dbapi/driver/driver_mgr.hpp> // this is a new header
    USING_NCBI_SCOPE;
    int main(int argc, const char* argv[])
    {
        try { // to be sure that we are catching all driver related exceptions
            C_DriverMgr drv_mgr;
            // We need to create a driver context first
            I_DriverContext* my_context= drv_mgr.GetDriverContext(
                                            (argc > 1)? argv[1] : "ctlib");
            // connecting to server "MyServer"
            // with user name "my_user_name" and password "my_password"
            CDB_Connection* con = my_context->Connect("MyServer", "my_user_name",
                                                     "my_password", 0);
            ...

This fragment creates an instance of the <span class="nctnt ncbi-monospace">Driver Manager</span>, dynamically loads the driver's library, implicitly registers this driver, creates the driver context and makes a connection to a server. If you don't want to load some drivers dynamically for any reason, but want to use the <span class="nctnt ncbi-monospace">Driver Manager</span> as a driver contexts factory, then you need to statically link your program with those libraries and explicitly register those using functions from <span class="nctnt ncbi-path">dbapi/driver/drivers.hpp</span> header.

#### Text and Image Data Handling

<span class="nctnt ncbi-type">text</span> and <span class="nctnt ncbi-type">image</span> are SQL datatypes and can hold up to 2Gb of data. Because they could be huge, the RDBMS keeps these values separately from the other data in the table. In most cases the table itself keeps just a special pointer to a text/image value and the actual value is stored separately. This creates some difficulties for text/image data handling.

When you retrieve a large text/image value, you often prefer to "stream" it into your program and process it chunk by chunk rather than get it as one piece. Some RDBMS clients allow to stream the text/image values only if a corresponding column is the only column in a select statement.

Let's suppose that you have a table T (i\_val int, t\_val text) and you need to select all i\_val, t\_val where i\_val \> 0. The simplest way is to use a query:

    select i_val, t_val from T where i_val > 0

But it could be expensive. Because two columns are selected, some clients will put the whole row in a buffer prior to giving access to it to the user. The better way to do this is to use two selects:

    select i_val from T where i_val > 0
    select t_val from T where i_val > 0

Looks ugly, but could save you a lot of memory.

Updating and inserting the text/image data is also not a straightforward process. For small texts and images it is possible to use just SQL <span class="nctnt ncbi-code">insert</span> and <span class="nctnt ncbi-code">update</span> statements, but it will be inefficient (if possible at all) for the large ones. The better way to insert and update text and image columns is to use the <span class="nctnt ncbi-func">SendData()</span> method of the [CDB\_Connection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDB_Connection&d=C) object or to use the [CDB\_SendDataCmd](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDB_SendDataCmd&d=C) object.

The recommended algorithm for inserting text/image data is:

-   Use a SQL <span class="nctnt ncbi-code">insert</span> statement to insert a new row into the table. Use a space value (<span class="nctnt ncbi-monospace">' '</span>) for each text column and a zero value (<span class="nctnt ncbi-monospace">0x0</span>) for each image column you are going to populate. Use <span class="nctnt ncbi-var">NULL</span> only if the value will remain <span class="nctnt ncbi-var">NULL</span>.

-   Use a SQL <span class="nctnt ncbi-code">select</span> statement to select all text/image columns from this row.

-   Fetch the row result and get an [I\_ITDescriptor](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classI__ITDescriptor.html) for each column.

-   Finish the results loop.

-   Use the <span class="nctnt ncbi-func">SendData()</span> method or [CDB\_SendDataCmd](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__SendDataCmd.html) object to populate the columns.

<span class="nctnt highlight">Example</span>

Let's suppose that we want to insert a new row into table T as described above.

    CDB_Connection* con;
    ...
    // preparing the query
    CDB_LangCmd* lcmd= con->LangCmd("insert T (i_val, t_val) values(100, ' ')\n");
    lcmd->More("select t_val from T where i_val = 100");
    // Sending this query to a server
    lcmd->Send();
    I_ITDescriptor* my_descr;
    // the result loop
    while(lcmd->HasMoreResults()) {
        CDB_Result* r= lcmd->Result();
        // skip all but row result
        if (r == 0  \|\|  r->ResultType() != eDB_RowResult) {
            delete r;
            continue;
        }
        // fetching the row
        while(r->Fetch()) {
            // read 0 bytes from the text (some clients need this trick)
            r->ReadItem(0, 0);
            my_deskr = r->GetImageOrTextDescriptor();
        }
        delete r; // we don't need this result anymore
    }
    delete lcmd; // delete the command
    CDB_Text my_text;
    my_text.Append("This is a text I want to insert");
    //sending the text
    con->SendData(my_descr, my_text);
    delete my_descr; // we don't need this descriptor anymore
    ...

The recommended algorithm for updating the text/image data is:

-   Use a SQL <span class="nctnt ncbi-code">update</span> statement to replace the current value with a space value (<span class="nctnt ncbi-monospace">' '</span>) for a text column and a zero value (<span class="nctnt ncbi-monospace">0x0</span>) for an image column.

-   Use a SQL <span class="nctnt ncbi-code">select</span> statement to select all text/image columns you want to update in this row.

-   Fetch the row result and get an [I\_ITDescriptor](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classI__ITDescriptor.html) for each column.

-   Finish the results loop.

-   Use the <span class="nctnt ncbi-func">SendData()</span> method or the [CDB\_SendDataCmd](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDB__SendDataCmd.html) object to populate the columns.

<span class="nctnt highlight">Example</span>

    CDB_Connection* con;
    ...
    // preparing the query
    CDB_LangCmd* lcmd= con->LangCmd("update T set t_val= ' ' where i_val =  100");
    lcmd->More("select t_val from T where i_val = 100");
    // Sending this query to a server
    lcmd->Send();
    I_ITDescriptor* my_descr;
    // the result loop
    while(lcmd->HasMoreResults()) {
        CDB_Result* r= lcmd->Result();
        // skip all but row result
        if (r == 0  \|\|  r->ResultType() != eDB_RowResult) {
            delete r;
            continue;
        }
        // fetching the row
        while(r->Fetch()) {
            // read 0 bytes from the text (some clients need this trick)
            r->ReadItem(0, 0);
            my_deskr = r->GetImageOrTextDescriptor();
        }
        delete r; // we don't need this result anymore
    }
    delete lcmd; // delete the command
    CDB_Text my_text;
    my_text.Append("This is a text I want to see as an update");
    //sending the text
    con->SendData(my_descr, my_text);
    delete my_descr; // we don't need this descriptor anymore
    ...

#### Results loop

Each connection in the NCBI DBAPI driver is always single threaded. Therefore, applications have to retrieve all the results from a current command prior to executing a new one. Not all results are meaningful (i.e. an RPC always returns a status result regardless of whether or not a procedure has a return statement), but all results need to be retrieved. The following loop is recommended for retrieving results from all types of commands:

    CDB_XXXCmd* cmd; // XXX could be Lang, RPC, etc.
    ...
    while (cmd->HasMoreResults()) {
            // HasMoreResults() method returns true        // if the Result() method needs to be called.
            // It doesn't guarantee that Result() will return not NULL result
       CDB_Result* res = cmd->Result();
       if (res == 0)
           continue; // a NULL res doesn't mean that there is no more results
       switch(res->ResultType()) {
          case eDB_RowResult: // row result
              while(res->Fetch()) {
                 ...
              }
              break;
          case eDB_ParamResult: // Output parameters
              while(res->Fetch()) {
                 ...
              }
              break;
          case eDB_ComputeResult: // Compute result
              while(res->Fetch()) {
                 ...
              }
              break;
          case eDB_StatusResult: // Status result
              while(res->Fetch()) {
                 ...
              }
              break;
          case eDB_CursorResult: // Cursor result
              while(res->Fetch()) {
                 ...
              }
              break;
       }
       delete res;
    }

If you don't want to process some particular type of result, just skip the <span class="nctnt ncbi-code">while (res-\>Fetch()){...}</span> in the corresponding <span class="nctnt ncbi-code">case</span>.

### Supported DBAPI drivers

The following sections cover the supported DBAPI drivers:

-   [FreeTDS (TDS ver. 7.0)](#freetds-tds-ver-70) -- this is the preferred driver

-   [Sybase CTLIB](#sybase-ctlib)

-   [ODBC](#odbc)

-   [MySQL Driver](#mysql-driver)

#### FreeTDS (TDS ver. 7.0)

<span class="nctnt highlight">Note:</span> This is the only driver supported by SDBAPI.

This driver is the most recommended, built-in, and portable.

-   Registration function (for the manual, static registration) [DBAPI\_RegisterDriver\_FTDS()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=DBAPI_RegisterDriver_FTDS&d=f)

-   Driver default name (for the run-time loading from a DLL) <span class="nctnt ncbi-lib">"ftds"</span>.

-   Driver library <span class="nctnt ncbi-lib">ncbi\_xdbapi\_ftds</span>

-   <span class="nctnt ncbi-monospace">FreeTDS</span> libraries and headers used by the driver [$(FTDS\_LIBS)](ch_config.html#ch_config.ch_configlocalizatio) [$(FTDS\_INCLUDE)](ch_config.html#ch_config.ch_configlocalizatio)

-   <span class="nctnt ncbi-monospace">FreeTDS</span>-specific driver context attributes "version", default = <span class="nctnt ncbi-monospace">\<DBVERSION\_UNKNOWN\></span> (also allowed: <span class="nctnt ncbi-monospace">"42"</span>, <span class="nctnt ncbi-monospace">"46"</span>, "70", "100")

-   FreeTDS works on UNIX and Windows platforms.

-   This driver supports Windows Domain Authentication using protocol NTLMv2, which is a default authentication protocol for Windows at NCBI.

-   This driver supports TDS protocol version auto-detection. TDS protocol version cannot be detected when connecting against Sybase Open Server.

-   Caveats:

    -   Default version of the TDS protocol (\<DBVERSION\_UNKNOWN\>) will work with both MS SQL Server and Sybase SQL Server.

    -   When using FTDS to connect to SQL Server, there are some limitations in updating LOB-fields which participate in replication.

    -   When using FTDS to connect to Sybase Open Server, you must explicitly set TDS version to 5.0, otherwise the connect operation will hang. This can be done either by using a driver parameter "version" equal to "50" or by setting an environment variable TDSVER to "50". Also, explicitly configuring the packet size setting to 3584 (7 \* 512) has historically been helpful.

    -   Although a slightly modified version of FreeTDS is now part of the C++ Toolkit, it retains its own license: the [GNU Library General Public License](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/dbapi/driver/ftds64/freetds/COPYING.LIB).

    -   TDS protocol version 4.2 should not be used with MS SQL server.

#### Sybase CTLIB

<span class="nctnt highlight">Note:</span> This driver is not supported by SDBAPI.

-   Registration function (for the manual, static registration) [DBAPI\_RegisterDriver\_CTLIB()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=DBAPI_RegisterDriver_CTLIB&d=f)

-   Driver default name (for the run-time loading from a DLL) <span class="nctnt ncbi-monospace">"ctlib"</span>

-   Driver library <span class="nctnt ncbi-lib">ncbi\_xdbapi\_ctlib</span>

-   <span class="nctnt ncbi-monospace">Sybase CTLIB</span> libraries and headers used by the driver (UNIX) [$(SYBASE\_LIBS)](ch_config.html#ch_config.ch_configlocalizatio) [$(SYBASE\_INCLUDE)](ch_config.html#ch_config.ch_configlocalizatio)

-   <span class="nctnt ncbi-monospace">Sybase CTLIB</span> libraries and headers used by the driver (MS Windows). You will need the Sybase OpenClient package installed on your PC. In MSVC++, set the "C/C++ / General / Additional Include Directories" and "Linker / General / Additional Library Directories" properties to the Sybase OpenClient headers and libraries (for example "C:\\Sybase\\include" and "C:\\Sybase\\lib" respectively). Also set the "Linker / Input / Additional Dependencies" property to include the needed Sybase OpenClient libraries: <span class="nctnt ncbi-lib">LIBCT.LIB LIBCS.LIB LIBBLK.LIB</span>. To run the application, you must set environment variable <span class="nctnt ncbi-monospace">%SYBASE%</span> to the Sybase OpenClient root directory (e.g. "<span class="nctnt ncbi-path">C:\\Sybase</span>"), and also to have your "interfaces" file there, in <span class="nctnt ncbi-path">INI/sql.ini</span>. In NCBI, we have the Sybase OpenClient libs installed in <span class="nctnt ncbi-path">\\\\snowman\\win-coremake\\Lib\\ThirdParty\\sybase</span>.

-   CTLIB-specific header (contains non-portable extensions) [dbapi/driver/ctlib/interfaces.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/dbapi/driver/ctlib/interfaces.hpp)

-   CTLIB-specific driver context attributes "reuse\_context" (default value is <span class="nctnt ncbi-monospace">"true"</span>), "version" (default value is <span class="nctnt ncbi-monospace">"125"</span>, also allowed <span class="nctnt ncbi-monospace">"100"</span> and <span class="nctnt ncbi-monospace">"110"</span>)

-   Caveats:

    -   Cannot communicate with MS SQL server using any TDS version.

#### ODBC

<span class="nctnt highlight">Note:</span> This driver is not supported by SDBAPI.

-   Registration function (for the manual, static registration) [DBAPI\_RegisterDriver\_ODBC()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=DBAPI_RegisterDriver_ODBC&d=f)

-   Driver default name (for the run-time loading from a DLL) <span class="nctnt ncbi-monospace">"odbc"</span>

-   Driver library <span class="nctnt ncbi-lib">dbapi\_driver\_odbc</span>

-   <span class="nctnt ncbi-monospace">ODBC</span> libraries and headers used by the driver (MS Windows) <span class="nctnt ncbi-lib">ODBC32.LIB ODBCCP32.LIB ODBCBCP.LIB</span>

-   <span class="nctnt ncbi-monospace">ODBC</span> libraries and headers used by the driver (UNIX) [$(ODBC\_LIBS)](ch_config.html#ch_config.ch_configlocalizatio)[$(ODBC\_INCLUDE)](ch_config.html#ch_config.ch_configlocalizatio)

-   <span class="nctnt ncbi-monospace">ODBC</span>-specific header (contains <span class="nctnt ncbi-monospace">non-portable</span> extensions) [dbapi/driver/odbc/interfaces.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/dbapi/driver/odbc/interfaces.hpp)

-   <span class="nctnt ncbi-monospace">ODBC</span>-specific driver context attributes "version" (default value is <span class="nctnt ncbi-monospace">"3"</span>, also allowed <span class="nctnt ncbi-monospace">"2"</span>), "use\_dsn" (default value is <span class="nctnt ncbi-monospace">false</span>, if you have set this attribute to <span class="nctnt ncbi-monospace">true</span>, you need to define your <span class="nctnt ncbi-monospace">data source</span> using <span class="nctnt ncbi-monospace">"Control Panel"/"Administrative Tools"/"Data Sources (ODBC)"</span>)

-   Caveats:

    -   The <span class="nctnt ncbi-func">CDB\_Result::GetImageOrTextDescriptor()</span> does not work for ODBC driver. You need to use [CDB\_ITDescriptor](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDB_ITDescriptor&d=C) instead. The other way to deal with <span class="nctnt ncbi-type">texts/images</span> in ODBC is through the [CDB\_CursorCmd](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CDB_CursorCmd&d=C) methods: <span class="nctnt ncbi-func">UpdateTextImage</span> and <span class="nctnt ncbi-func">SendDataCmd</span>.

    -   On most NCBI PCs, there is an old header <span class="nctnt ncbi-path">odbcss.h</span> (from 4/24/1998) installed. The symptom is that although everything compiles just fine, however in the linking stage there are dozens of unresolved symbol errors for ODBC functions. Ask "pc.systems" to fix this for your PC.

    -   On UNIX, it's only known to work with Merant's implementation of ODBC, and it has not been thoroughly tested or widely used, so surprises are possible.

#### MySQL Driver

<span class="nctnt highlight">Note:</span> This driver is not supported by SDBAPI.

There is a direct (without ODBC) MySQL driver in the NCBI C++ Toolkit DBAPI. However, the driver implements a very minimal functionality and does not support the following:

-   Working with images by chunks (images can be accessed as string fields though)

-   RPC

-   BCP

-   SendData functionality

-   Connection pools

-   Parameter binding

-   Canceling results

-   ReadItem

-   IsAlive

-   Refresh functions

-   Setting timeouts

The BDB Wrapper
---------------

NCBI created the "BDB" wrapper to simplify use of the open source [Berkeley DB](http://www.oracle.com/us/products/database/berkeley-db/overview/index.html) library. Berkeley DB provides tools for the development of specialized data storage in applications not having access to a centralized RDBMS.

-   **C++ wrapper on top of Berkeley DB.** The BDB wrapper takes care of many of the ultra low-level details for C programmers using the Berkeley DB. BDB implements B-Tree file access (both keyed and sequential), environments, cursors, and transactions.

-   **Error checking.** All error codes coming from the Berkeley DB are analyzed and processed in a manner common to all other components of the C++ Toolkit. When an error situation is detected, the BDB wrapper sends an exception that is reported by the diagnostic services and can be handled by the calling application, similar to any other Toolkit exception.

-   **Support for relational table structure and different data types.** The Berkeley DB itself is “type agnostic” and provides no means to manipulate data types. But for many cases, clear data type support can save a lot of work. The Toolkit implements all major scalar data types so it can be used like a regular database.

-   **Cross platform compatibility.** The BDB databases can be transferred across platforms without reconverting the data. The BDB tracks the fact that the database was created as big-endian or little-endian and does the conversion transparently when the database migrates.

-   **Easy BLOBs.** The BDB wrapper supports keyed BLOB storage. BLOBs can be streamed to and from the database. A set of additional interfaces has been written to simplify the BLOB access in comparison with the original Berkeley DB C library.

-   **Disk-based cache interface.** The BDB wrapper implements a cache disk cache service used by other Toolkit components to minimize client-server traffic and to store parts of the data locally. Different cache management and data expiration policies have been put in place.

-   **Database maps.** The BDB wrapper includes template classes similar to STL map and multimap but persistently stores the map content in the Berkeley DB files.

-   **Simple queries.** The BDB wrapper includes implementation of a simple query language to search records in flat files.

The SQLite Wrapper
------------------

The NCBI C++ Toolkit provides the "SQLite Wrapper" for the open source [SQLite](http://sqlite.org/) library. Like Berkeley DB, SQLite is implemented as a library linked with the application, rather than a separate DBMS.

Some of the SQLite wrapper features are:

-   A class, [CSQLITE\_Global](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCSQLITE__Global.html), with static functions for tuning SQLite as a whole as opposed to tuning connection-by-connection.

-   A convenience class, [CSQLITE\_Connection](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCSQLITE__Connection.html), for connecting to a database.

-   Convenience classes for working with prepared statements, blobs, and exceptions.

Please see the [sqlitewrapp.hpp](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c%2B%2B/include/db/sqlite/sqlitewrapp.hpp?view=log) header for API details.

For more information about SQLite, see the [online SQLite documentation](http://sqlite.org/docs.html).

Database Load-Balancing (DBLB)
------------------------------

Many server-based databases are load-balanced for efficient resource management. Accessing load-balanced databases is automatically done from C++ when using the [SDBAPI](#sdbapi) library, but requires some specific statements in the client code when using the [DBAPI](#dbapi) library or [scripts](#scripts). This section discusses setting up and using load-balanced databases.

The following sections cover specific aspects of Database Load-Balancing:

-   [Setting up Load-Balancing of Database Servers](#setting-up-load-balancing-of-database-servers)

-   [Using Database Load-Balancing from C++](#using-database-load-balancing-from-c++)

-   [Load-Balanced Database Access via Python and Perl](#load-balanced-database-access-via-python-and-perl)

-   [Advantages of using DBLB](#advantages-of-using-dblb)

-   [How it works (by default)](#how-it-works-by-default)

### Setting up Load-Balancing of Database Servers

For the following to be clear, it is important to distinguish between a database name, an underlying (actual) server name (e.g. MSSQL17), which hosts a variety of databases, a database server alias, and a service name. A server alias may be moved to a different underlying server. The server alias is often used with sqsh, and the GUI tools, such as SQL Management studio. The service name is used by the load-balancer to look up the underlying server to use, and is the name that should be used by an application. The server aliases and service names often share a common prefix and would look similar, and in fact for reasons presented below, there should be at least one server alias that is identical to the service name.

The following steps must be done prior to database load-balancing:

1  
Ask the DBAs to add your service name (e.g. YOURSERVICE) to the load-balancer configuration database. Typically, the names are clear, for example, there are server aliases YOURSERVICE1, and YOURSERVICE2 that already exist, and databases that have “YOURSERVICE” as an embedded string, but if not, the databases providing the service and the server aliases involved should be given. Note that if databases are moved to different underlying servers, both the server aliases, and the load-balancer configuration which points to those servers are both moved, synchronously.

2  
Tell the DBAs which of the server aliases point to the server that should be used, if the load-balancer is unavailable, as the DBAPI will look for a server alias with the same name as the service, in that case.

3  
The DBAs will also ask for a DNS name to match the service name as a backup connection method, should everything else fail.

### Using Database Load-Balancing from C++

For simplest access, see the section on [using SDBAPI](#using-sdbapi) above. SDBAPI uses the database load-balancing by default, so no further steps are necessary.

If DBAPI is being used (e.g. if a feature that is only available in DBAPI is required), and you want to activate database load-balancing:

1  
Before the very first DBAPI connection attempt, call:<br/><br/><span class="nctnt ncbi-code">\#include \<dbapi/driver/dbapi\_svc\_mapper.hpp\></span><br/><span class="nctnt ncbi-code">DBLB\_INSTALL\_DEFAULT();</span><br/>

2  
Link <span class="nctnt ncbi-lib">'$(XCONNEXT)'</span> and <span class="nctnt ncbi-lib">'xconnect'</span> libraries to your application.

If steps (1) and (2) above are done then the DBAPI connection methods (such as <span class="nctnt ncbi-func">Connect()</span> or <span class="nctnt ncbi-func">ConnectValidated()</span>) will attempt to resolve the passed server name as a load-balanced service name.

<span class="nctnt highlight">Note:</span> If steps (1) and (2) above are not done, or if DBLB library is not available (such as in the publicly distributed code base), or if the passed server name cannot be resolved as a load-balanced service name, then the regular database server name resolution will be used – i.e. the passed name will first be interpreted as a server alias (using the "interfaces" file), and if that fails, it will be interpreted as a DNS name. Note however that by default if the service name resolves (exists), then the regular database server name resolution will not be used as a fallback, even if DBAPI can't connect (for whatever reason) to the servers that the service resolves to.

Example:

    #include <dbapi/driver/dbapi_svc_mapper.hpp>

    DBLB_INSTALL_DEFAULT();
    IDataSource* ds = dm.CreateDs("ftds");
    IConnection* conn = ds->CreateConnection();

    // Note: It is possible to connect by calling conn->Connect(), but it is
    // recommended to use a validator here because, in conjunction with DBAPI's 
    // internal retry mechanism, a validator helps DBAPI select a good server from a
    // load-balanced set when the actual database is temporarily unavailable on one
    // (or more) of the servers.
    CTrivialConnValidator my_validator(my_databasename);
    conn->ConnectValidated(
        my_validator, my_username, my_password, my_servicename, my_ databasename);

### Load-Balanced Database Access via Python and Perl

There is a Unix command-line tool, <span class="nctnt ncbi-app">ncbi\_dblb\_cli</span>, that you can call from your script to perform service resolution and connection validation - i.e. to find a load-balanced server that is online and that supports a given service and database.

From Python:

    #!/bin/env python

    import subprocess, sys

    cmdline = [
        "/opt/machine/lbsm/bin/ncbi_dblb_cli",
        "lookup",
        "-service",
        "DBAPI_MS_TEST",
        "-database",
        "DBAPI_Sample",
    ]
    result = subprocess.Popen(cmdline, stdout=subprocess.PIPE).communicate()[0]
    # A successful result will match a line beginning with a name followed by a tab.
    if result:
        server = result.strip().split("\t")[0]
        # Do whatever is needed with the server name...

From Perl:

    #!/bin/env perl

    use strict;

    my $cmd = '/opt/machine/lbsm/bin/ncbi_dblb_cli';
    my $svc = 'DBAPI_MS_TEST';
    my $db  = 'DBAPI_Sample';
    my $cmdline = $cmd . ' lookup -service ' . $svc . ' -database ' . $db;
    # A successful result will match a line beginning with a name followed by a tab.
    if(`$cmdline` =~ m/^([^\t]+)/) {
        my $server = $1;
        # Do whatever is needed with the server name...
    }

If you don't need to ensure that the server is online or check for a specific database, you can just call <span class="nctnt ncbi-app">ncbi\_dblb</span>.

From Python:

    #!/bin/env python

    import subprocess, sys

    if len(sys.argv) > 1:
        # Use the -q option to fetch only the server name.
        cmd = ['/opt/machine/lbsm/bin/ncbi_dblb', '-q', sys.argv[1]]
        srv = subprocess.Popen(cmd, stdout=subprocess.PIPE).communicate()[0].strip()
        # Do whatever is needed with the server name...

From Perl:

    #!/bin/env perl

    use strict;

    if (@ARGV) {
        # Use the -q option to fetch only the server name.
        my $cmd = '/opt/machine/lbsm/bin/ncbi_dblb -q ' . $ARGV[0];
        my $srv = `$cmd`;
        chomp($srv);
        # Do whatever is needed with the server name...
    }

There is also a Python module that provides an interface to the load-balancing service:

-   code: <https://stash.ncbi.nlm.nih.gov/projects/LBSMD/repos/pylbsmd/browse>

-   documentation: <https://dsubmit.ncbi.nlm.nih.gov/docs/lbsmd/>

### Advantages of using DBLB

#### C++ Specific

-   A database-level verification mechanism.

-   Latch onto the same database server for the life of your process. It's often useful to avoid possible inter-server data discrepancy. The "latch-on" mechanism can be relaxed or turned off if needed.

-   Automatic connection retries. If a connection to the selected server cannot be established, the API will try again with other servers (unless it is against the chosen "latch-on" strategy).

-   The default connection strategy is \*configurable\*. You can change its parameters using a configuration file, environment variables, and/or programmatically. You can also configure locally for your application ad-hoc mappings to the database servers (this is usually not recommended but can come in handy in emergency cases or for debugging).

-   If needed, you can implement your own customized mapper. Components of the default connection strategy can be used separately, or in combination with each other and with the user-created strategies, if necessary.

#### General

-   Connecting to the database servers by server name and/or "interfaces" file based aliases still works the same as it used to.

-   Automatic avoidance of unresponsive database servers. This prevents your application from hanging for up to 30 seconds on the network timeout.

-   Independence from the database "interfaces" file. A centrally maintained service directory is used instead, which is accessible locally and/or via network. It also dynamically checks database servers' availability and excludes unresponsive servers.

### How it works (by default)

The following steps are performed each time a request is made to establish a load-balanced connection to a named database service:

1  
The requests will first go through the DBLB mechanism that tries to match the requested service name against the services known to the NCBI [Load Balancer](ch_app.html#ch_app.Load_Balancing) and/or those described in the application's configuration file.

2  
If the requested service name is unknown to the load balancer then this name will be used "as is".

3  
However, if this service name is known to the DBLB then the DBLB will try to establish a connection to the database server that it deems the most suitable. If the service is handled by the NCBI load-balancer, then the unresponsive servers will be weeded out, and a load on the machines that run the servers may be taken into account too.

4  
<span class="nctnt highlight">C++ only:</span> If the connection cannot be established, then DBLB will automatically retry the connection, now using another suitable database server.

5  
This procedure may be repeated several times, during which there will be only one attempt to connect to each database.

6  
<span class="nctnt highlight">C++ only:</span> Once a database connection is successfully established it will be "latched-on". This means that when you will try to connect to the same service or alias within the same application again then you will be connected to the same database server (this can be relaxed or turned off completely).

7  
For example, you can connect to the "PMC" service which is currently mapped to two servers. The server names are provided dynamically by the NCBI load-balancer, so you never have to change your configuration or recompile your application if either a service configuration or an "interfaces" file get changed.

8  
<span class="nctnt highlight">C++ only:</span> If <span class="nctnt ncbi-func">ConnectValidated()</span> is used to connect to a database, then requests to establish database connections will first go through the server-level load-balancing mechanism. On successful login to server, the database connection will be validated against the validator. If the validator does not "approve" the connection, then DBAPI will automatically close this connection and repeat this login/validate attempt with the next server, and so on, until a "good" (successful login + successful validation) connection is found. If you want to validate a connection against more than one validator/database, then you can combine validators. Class <span class="nctnt ncbi-class">CConnValidatorCoR</span> was developed to allow combining of other validators into a chain.


