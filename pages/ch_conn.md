---
layout: default
title: C++ Toolkit test
nav: pages/ch_conn
---

<span class="label">9</span>Networking and IPC
==============================================

Last Update: October 26, 2014.

Connection Library [Library <span class="nctnt ncbi-path">xconnect</span>: [include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect)]
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

The connection library includes a generic socket interface (<span class="nctnt ncbi-type">SOCK</span>), connection object (<span class="nctnt ncbi-type">CONN</span>), and specialized <span class="nctnt ncbi-type">CONNECTOR</span> constructors (for sockets, files, HTTP, and services) to be used as engines for connections. It also provides access to the load-balancing daemon and NCBI named service dispatching facilities.

Although the core of the Connection Library is written in C and has an underlying C interface, the analogous C++ interfaces have been built to provide objects that work smoothly with the rest of the Toolkit.

<span class="nctnt highlight">Note</span>: Because of security issues, not all links in the public version of this file are accessible by outside NCBI users.

### Chapter Outline

The following is an outline of the topics presented in this chapter, with links to relevant source files:

-   [Overview](#overview)

-   [Connections and CONNECTORs](#connections-and-connectors)

    -   The notions of a connection and a <span class="nctnt ncbi-type">CONNECTOR</span>; different types of connections that the library provides; programming API.

    -   Open and Manage Connections to an Abstract I/O -- <span class="nctnt ncbi-path">ncbi\_connection</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_connection.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_connection.c)]

    -   Abstract I/O -- <span class="nctnt ncbi-path">ncbi\_connector</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_connector.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_connector.c)]

    -   [Socket Connector](#socket-connector) -- <span class="nctnt ncbi-path">ncbi\_socket\_connector</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket_connector.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_socket_connector.c)]

    -   [File Connector](#file-connector) -- <span class="nctnt ncbi-path">ncbi\_file\_connector</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_file_connector.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_file_connector.c)]

    -   In-memory Connector -- <span class="nctnt ncbi-path">ncbi\_memory\_connector</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_memory_connector.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_memory_connector.c)]

    -   Higher level connection protocols:

        -   [Common Parameters](#common-parameters) for higher level connectors

        -   [HTTP Connector](#http-connector) -- <span class="nctnt ncbi-path">ncbi\_http\_connector</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_http_connector.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_http_connector.c)]

        -   [Service Connector](#service-connector) -- <span class="nctnt ncbi-path">ncbi\_service\_connector</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_service_connector.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_service_connector.c)]

-   [Debugging Tools and Troubleshooting Documentation](#debugging-tools-and-troubleshooting-documentation)

-   [CONN-Based C++ Streams and Stream Buffers](#conn-based-c++-streams-and-stream-buffers) -- <span class="nctnt ncbi-path">ncbi\_conn\_stream</span>[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_conn_stream.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_conn_stream.cpp)], <span class="nctnt ncbi-path">ncbi\_conn\_streambuf</span>[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_conn_streambuf.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_conn_streambuf.cpp)]

    -   Built on top of connection objects.

-   [Servers and Service Mapping API](#servers-and-service-mapping-api)

    -   Description of service name resolution API.

    -   NCBI Server Meta-Address Info -- <span class="nctnt ncbi-path">ncbi\_server\_info</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_server_info.h) \\| [p.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_server_infop.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_server_info.c)]

    -   Resolve NCBI Service Name to the Server Meta-Address -- <span class="nctnt ncbi-path">ncbi\_service</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_service.h) \\| [p.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_servicep.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_service.c)]

    -   Resolve NCBI Service Name to the Server Meta-Address using NCBI Network Dispatcher ([DISPD](ch_app.html#ch_app.DISPD_Network_Dispat)) -- <span class="nctnt ncbi-path">ncbi\_dispd</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_dispd.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_dispd.c)]

    -   Resolve NCBI Service Name to the Server Meta-Address using NCBI [Load-Balancing Service Mapper (LBSM)](ch_app.html#ch_app.Load_Balancing_Servi) -- <span class="nctnt ncbi-path">ncbi\_lbsmd</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_lbsmd.h) \\| [.c](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_lbsmd.c) \\| [\_stub.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_lbsmd_stub.c)]

    -   [NCBI LBSM client-server data exchange API](#ncbi-lbsm-client-server-data-exchange-api) -- <span class="nctnt ncbi-path">ncbi\_lbsm</span>[[.h](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_lbsm.h) \\| [.c](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_lbsm.c)]

    -   Implementation of LBSM Using SYSV IPC (shared memory and semaphores) -- <span class="nctnt ncbi-path">ncbi\_lbsm\_ipc</span>[[.h](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_lbsm_ipc.h) \\| [.c](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_lbsm_ipc.c)]

-   [Threaded Server Support](#threaded-server-support)

For additional related information, see:

-   C++ Interfaces to the Library

    -   [Diagnostic Handler for E-Mailing Logs](ch_cgi.html#ch_cgi.cgi_diag.html) -- <span class="nctnt ncbi-path">email\_diag\_handler</span>[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/email_diag_handler.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/email_diag_handler.cpp)]

    -   [Using the CONNECT Library with the C++ Toolkit](ch_res.html#ch_res.c_cxx.html) -- <span class="nctnt ncbi-path">ncbi\_core\_cxx</span>[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core_cxx.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_core_cxx.cpp)]

    -   Multithreaded Network Server Framework -- <span class="nctnt ncbi-path">threaded\_server</span>[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/threaded_server.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/threaded_server.cpp)]

-   [Basic Types and Functionality (for Registry, Logging and MT Locks)](ch_res.html#ch_res.c_cxx.html_ref_RunTimeResources) -- <span class="nctnt ncbi-path">ncbi\_core</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_core.c)], <span class="nctnt ncbi-path">ncbi\_types</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_types.h)]

-   Portable TCP/IP Socket Interface -- <span class="nctnt ncbi-path">ncbi\_socket</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_socket.c)]

-   Memory Management

    -   Memory-Resident FIFO Storage Area -- <span class="nctnt ncbi-path">ncbi\_buffer</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_buffer.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_buffer.c)]

    -   Simple Heap Manager With Primitive Garbage Collection -- <span class="nctnt ncbi-path">ncbi\_heapmgr</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_heapmgr.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_heapmgr.c)]

-   Connection Library Utilities

    -   Connection Utilities -- <span class="nctnt ncbi-path">ncbi\_connutil</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_connutil.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_connutil.c)]

    -   Send Mail (in accordance with RFC821 [protocol] and RFC822 [headers]) -- <span class="nctnt ncbi-path">ncbi\_sendmail</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_sendmail.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_sendmail.c)]

    -   Auxiliary (optional) Code for <span class="nctnt ncbi-path">ncbi\_core</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_core.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_core.c)] -- <span class="nctnt ncbi-path">ncbi\_util</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_util.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_util.c)]

    -   Non-ANSI, Widely Used Functions -- <span class="nctnt ncbi-path">ncbi\_ansi\_ext</span>[[.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_ansi_ext.h) \\| [.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/ncbi_ansi_ext.c)]

-   daemons [[src/connect/daemons](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/daemons)]

    -   [LBSMD](ch_app.html#ch_app.Load_Balancing_Servi)

    -   [DISPD](ch_app.html#ch_app.DISPD_Network_Dispat)

    -   [Firewall Daemon](ch_app.html#ch_app.Firewall_Daemon_FWDa)

-   Test cases [[src/connect/test](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/test)]

Overview
--------

The NCBI C++ platform-independent connection library ([src/connect](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect) and [include/connect](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect)) consists of two parts:

-   Lower-level library written in C (also used as a replacement of the existing connection library in the NCBI C Toolkit)

-   Upper-level library written in C++ and using C++ streams

Functionality of the library includes:

-   <span class="nctnt ncbi-type">SOCK</span> interface (sockets), which works interchangeably on most UNIX varieties, MS Windows, and Mac

-   <span class="nctnt ncbi-type">SERV</span> interface, which provides mapping of symbolic service names into server addresses

-   <span class="nctnt ncbi-type">CONN</span> interface, which allows the creation of a <span class="nctnt ncbi-monospace">connection</span>, the special object capable to do read, write, etc. I/O operations

-   C++ streams built on top of the <span class="nctnt ncbi-type">CONN</span> interface

<span class="nctnt highlight">Note</span>: The lowest level (<span class="nctnt ncbi-type">SOCK</span>) interface is not covered in this document. A well-commented API can be found in [connect/ncbi\_socket.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket.h).

To use the CONNECT library, your makefile should include:

    LIB  = xconnect xncbi
    LIBS = $(NETWORK_LIBS) $(ORIG_LIBS)

Connections and Connectors
--------------------------

This section discusses the "C API" for connections and connectors. There is a corresponding "C++ API" (see the [C++ Connection Streams](#c++-connection-streams) section below) that is built on top of, is an approximate superset of, and should generally be preferred over, the "C API".

There are three simple types of connections: <span class="nctnt ncbi-monospace">socket</span>, <span class="nctnt ncbi-monospace">file</span> and <span class="nctnt ncbi-monospace">http</span>; and one hybrid type, <span class="nctnt ncbi-monospace">service</span> connection.

A connection is created with a call to [CONN\_Create()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Create), declared in [connect/ncbi\_connection.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_connection.h), and returned by a pointer to <span class="nctnt ncbi-type">CONN</span> passed as a second argument:

    CONN conn; /* connection handle */
    EIO_Status status = CONN_Create(connector, &conn);

The first argument of this function is a handle of a <span class="nctnt ncbi-monospace">connector</span>, a special object implementing the functionality of the connection being built. For each type of connection there is a special connector in the library. For each connector, one or more "constructors" are defined, e.g. <span class="nctnt ncbi-func">SOCK\_CreateConnector()</span>, each returning the connector's handle. Connectors' constructors are defined in individual header files, such as [connect/ncbi\_socket\_connector.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket_connector.h), [connect/ncbi\_http\_connector.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_http_connector.h), [connect/ncbi\_service\_connector.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_service_connector.h), etc. <span class="nctnt ncbi-func">CONN\_Create()</span> resets all timeouts to the default value [kDefaultTimeout](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=kDefaultTimeout).

<span class="nctnt highlight">Note:</span> Connectors, connections, and the objects represented by their metadata are all separate. Furthermore, connections are created in "closed" state, so invalid parameters given to the connector aren't detected when the connector and connections based on it are created. This means, for example, that a connection created by passing an invalid file name to <span class="nctnt ncbi-func">FILE\_CreateConnector()</span> will not fail for either the connector or connection, because both objects themselves are fine. A failure will only occur if the file name is invalid when an attempt is made to use the connection.

After successful creation with <span class="nctnt ncbi-func">CONN\_Create()</span>, the following calls from <span class="nctnt ncbi-type">CONN</span> API [connect/ncbi\_connection.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_connection.h) become available. All calls (except <span class="nctnt ncbi-func">CONN\_GetTimeout()</span> and <span class="nctnt ncbi-func">CONN\_GetType()</span> ) return an I/O completion status of type [EIO\_Status](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EIO_Status). Normal completion has code <span class="nctnt ncbi-var">eIO\_Success</span>.

<span class="nctnt highlight">Note</span>: There is no means to "open" a connection: it is done automatically when actually needed, and in most cases at the first I/O operation. But the forming of an actual link between source and destination can be postponed even longer. These details are hidden and made transparent to the connection's user. The connection is seen as a two-way communication channel, which is clear for use immediately after a call to <span class="nctnt ncbi-func">CONN\_Create()</span>.

<span class="nctnt highlight">Note</span>: If for some reason <span class="nctnt ncbi-func">CONN\_Create()</span> failed to create a connection (return code differs from <span class="nctnt ncbi-var">eIO\_Success</span>), then the connector passed to this function is left intact, that is, its handle can be used again. Otherwise, if the connection is created successfully, the passed connector handle becomes invalid and cannot be referenced anywhere else throughout the program (with one exception, however: it may be used as a replacing connector in a call to <span class="nctnt ncbi-func">CONN\_ReInit()</span> for the same connection).

<span class="nctnt highlight">Note</span>: There are no "destructors" on public connectors. A connector successfully put into connection is deleted automatically, along with that connection by <span class="nctnt ncbi-func">CONN\_Close()</span>, or explicitly with a call to <span class="nctnt ncbi-func">CONN\_ReInit()</span>, provided that the replacing connector is <span class="nctnt ncbi-macro">NULL</span> or different from the original.

|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Read](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Read)(CONN conn, void\* buf, size\_t size, size\_t\* n\_read, EIO\_ReadMethod how)</span> |

Read or peek data, depending on read method <span class="nctnt ncbi-var">how</span>, up to <span class="nctnt ncbi-var">size</span> bytes from connection to specified buffer <span class="nctnt ncbi-var">buf</span>, return (via pointer argument <span class="nctnt ncbi-var">n\_read</span>) the number of bytes actually read. The last argument <span class="nctnt ncbi-var">how</span> can be one of the following:

-   [eIO\_ReadPlain](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EIO_ReadMethod) - to read data in a regular way, that is, extracting data from the connection;

-   <span class="nctnt ncbi-var">eIO\_ReadPeek</span> - to peek data from the connection, i.e., the next read operation will see the data again;

-   <span class="nctnt ncbi-var">eIO\_ReadPersist</span> - to read exactly (not less than) <span class="nctnt ncbi-var">size</span> bytes or until an error condition occurs.

A return value other than <span class="nctnt ncbi-var">eIO\_Success</span> means trouble. Specifically, the return value <span class="nctnt ncbi-var">eIO\_Timeout</span> indicates that the operation could not be completed within the allotted amount of time; but some data may, however, be available in the buffer (e.g., in case of persistent reading, as with <span class="nctnt ncbi-var">eIO\_ReadPersist</span>), and this is actually the case for any return code.

|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_ReadLine](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_ReadLine)(CONN conn, char\* line, size\_t size, size\_t\* n\_read)</span> |

Read up to <span class="nctnt ncbi-var">size</span> bytes from connection into the string buffer pointed to by <span class="nctnt ncbi-var">line</span>. Stop reading if either '\\n' or an error is encountered. Replace '\\n' with '\\0'. Upon return <span class="nctnt ncbi-var">\*n\_read</span> contains the number of characters written to <span class="nctnt ncbi-var">line</span>, not including the terminating '\\0'. If not enough space provided in <span class="nctnt ncbi-var">line</span> to accomodate the '\\0'-terminated line, then all <span class="nctnt ncbi-var">size</span> bytes are used up and <span class="nctnt ncbi-var">\*n\_read</span> is equal to <span class="nctnt ncbi-var">size</span> upon return - this is the only case when <span class="nctnt ncbi-var">line</span> will not be '\\0'-terminated.

Return code advises the caller whether another read can be attempted:

-   <span class="nctnt ncbi-var">eIO\_Success</span> -- read completed successfully, keep reading;

-   other code -- an error occurred, and further attempt may fail.

This call utilizes <span class="nctnt ncbi-var">eIO\_Read</span> timeout as set by <span class="nctnt ncbi-func">CONN\_SetTimeout().</span>

|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Write](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Write)(CONN conn, const void\* buf, size\_t size, size\_t\* n\_written)</span> |

Write up to <span class="nctnt ncbi-var">size</span> bytes from the buffer <span class="nctnt ncbi-var">buf</span> to the connection. Return the number of actually written bytes in <span class="nctnt ncbi-var">n\_written</span>. It may not return <span class="nctnt ncbi-var">eIO\_Success</span> if no data at all can be written before the write timeout expired or an error occurred. Parameter <span class="nctnt ncbi-var">how</span> modifies the write behavior:

-   [eIO\_WritePlain](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EIO_WriteMethod) - return immediately after having written as little as 1 byte of data, or if an error has occurred;

-   <span class="nctnt ncbi-var">eIO\_WritePersist</span> - return only after having written all of the data from <span class="nctnt ncbi-var">buf</span> (<span class="nctnt ncbi-var">eIO\_Success</span>), or if an error has occurred (fewer bytes written, non-<span class="nctnt ncbi-var">eIO\_Success</span>).

<span class="nctnt highlight">Note:</span> See CONN\_SetTimeout() for how to set the write timeout.

|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_PushBack](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_PushBack)(CONN conn, const void\* buf, size\_t size)</span> |

Push back <span class="nctnt ncbi-var">size</span> bytes from the buffer <span class="nctnt ncbi-var">buf</span> into connection. Return <span class="nctnt ncbi-var">eIO\_Success</span> on success, other code on error.

<span class="nctnt highlight">Note 1:</span> The data pushed back may not necessarily be the same as previously obtained from the connection.

<span class="nctnt highlight">Note 2:</span> Upon a following read operation, the pushed back data are taken out first.

|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_GetPosition](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_GetPosition)(CONN conn, EIO\_Event event)</span> |

Get read (<span class="nctnt ncbi-var">event</span> == <span class="nctnt ncbi-var">eIO\_Read</span>) or write (<span class="nctnt ncbi-var">event</span> == <span class="nctnt ncbi-var">eIO\_Write</span>) position within the connection. Positions are advanced from 0 on, and only concerning I/O that has caused calling to the actual connector's "read" (i.e. pushbacks never considered, and peeks -- not always) and "write" methods. Special case: <span class="nctnt ncbi-var">eIO\_Open</span> as <span class="nctnt ncbi-var">event</span> causes to clear both positions with 0, and to return 0.

|---------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Flush](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Flush)(CONN conn)</span> |

Explicitly flush connection from any pending data written by <span class="nctnt ncbi-func">CONN\_Write()</span>.

<span class="nctnt highlight">Note 1: </span><span class="nctnt ncbi-func">CONN\_Flush()</span> effectively opens connection (if it wasn't open yet).

<span class="nctnt highlight">Note 2:</span> Connection considered open if underlying connector's "Open" method has successfully executed; an actual data link may not yet exist.

<span class="nctnt highlight">Note 3: </span><span class="nctnt ncbi-func">CONN\_Read()</span> always calls <span class="nctnt ncbi-func">CONN\_Flush()</span> before proceeding; so does <span class="nctnt ncbi-func">CONN\_Close()</span> but only if the connection is already open.

|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_SetTimeout](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_SetTimeout)(CONN conn, EIO\_Event action, const STimeout\* timeout)</span> |

Set the timeout on the specified I/O action, [eIO\_Read](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EIO_Event), <span class="nctnt ncbi-var">eIO\_Write</span>, <span class="nctnt ncbi-var">eIO\_ReadWrite</span>, <span class="nctnt ncbi-var">eIO\_Open</span>, and <span class="nctnt ncbi-var">eIO\_Close</span>. The latter two actions are used in a phase of opening and closing the link, respectively. If the connection cannot be read (written, established, closed) within the specified period, <span class="nctnt ncbi-var">eIO\_Timeout</span> would result from connection I/O calls. A timeout can be passed as the <span class="nctnt ncbi-macro">NULL</span>-pointer. This special case denotes an infinite value for that timeout. Also, a special value <span class="nctnt ncbi-var">kDefaultTimeout</span> may be used for any timeout. This value specifies the timeout set by default for the current connection type.

|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_GetTimeout](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_GetTimeout)(CONN conn, EIO\_Event action)</span> |

Obtain (via the return value of type <span class="nctnt ncbi-type">const </span>[STimeout](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=STimeout)<span class="nctnt ncbi-type">\*</span>) timeouts set by the <span class="nctnt ncbi-func">CONN\_SetTimeout()</span> routine, or active by default (i.e., special value <span class="nctnt ncbi-var">kDefaultTimeout</span>).

<span class="nctnt highlight">Caution</span>: The returned pointer is valid only for the time that the connection handle is valid, i.e., up to a call to <span class="nctnt ncbi-func">CONN\_Close().</span>

|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_ReInit](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_ReInit)(CONN conn, CONNECTOR replacement)</span> |

This function clears the current contents of a connection and places ("immerse") a new connector into it. The previous connector (if any) is closed first (if open), then gets destroyed, and thus must not be referenced again in the program. As a special case, the new connector can be the same connector, which is currently active within the connection. It this case, the connector is not destroyed; instead, it will be effectively re-opened. If the connector passed as <span class="nctnt ncbi-macro">NULL</span>, then the <span class="nctnt ncbi-var">conn</span> handle is kept existing but unusable (the old connector closed and destroyed) and can be <span class="nctnt ncbi-func">CONN\_ReInit()</span>'ed later. None of the timeouts are touched by this call.

|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Wait](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Wait)(CONN conn, EIO\_Event event, const STimeout\* timeout)</span> |

Suspend the program until the connection is ready to perform reading (<span class="nctnt ncbi-var">event</span> =<span class="nctnt ncbi-var">eIO\_Read</span>) or writing (<span class="nctnt ncbi-var">event</span> = <span class="nctnt ncbi-var">eIO\_Write</span>), or until the timeout (if non-<span class="nctnt ncbi-macro">NULL</span>) expires. If the timeout is passed as <span class="nctnt ncbi-macro">NULL</span>, then the wait time is indefinite.

|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Status](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Status)(CONN conn, EIO\_Event direction)</span> |

Provide the information about recent low-level data exchange in the link. The operation direction has to be specified as either <span class="nctnt ncbi-var">eIO\_Read</span> or <span class="nctnt ncbi-var">eIO\_Write</span>. The necessity of this call arises from the fact that sometimes the return value of a <span class="nctnt ncbi-type">CONN</span> API function does not really tell that the problem has been detected: suppose, the user peeks data into a 100-byte buffer and gets 10 bytes. The return status <span class="nctnt ncbi-var">eIO\_Success</span> signals that those 10 bytes were found in the connection okay. But how do you know whether the end-of-file condition occurred during the last operation? This is where <span class="nctnt ncbi-func">CONN\_Status()</span> comes in handy. When inquired about the read operation, return value <span class="nctnt ncbi-var">eIO\_Closed</span> denotes that <span class="nctnt ncbi-macro">EOF</span> was actually hit while making the peek, and those 10 bytes are in fact the only data left untaken, no more are expected to come.

|---------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Close](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Close)(CONN conn)</span> |

Close the connection by closing the link (if open), deleting underlying connector(s) (if any) and the connection itself. Regardless of the return status (which may indicate certain problems), the connection handle becomes invalid and cannot be reused.

|-----------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Cancel](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Cancel)(CONN conn)</span> |

Cancel the connection's I/O ability. This is **not** connection closure, but any data extraction or insertion (Read/Write) will be effectively rejected after this call (and <span class="nctnt ncbi-var">eIO\_Interrupt</span> will result, same for <span class="nctnt ncbi-func">CONN\_Status()</span>). <span class="nctnt ncbi-func">CONN\_Close()</span> is still required to release internal connection structures.

|-------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_GetType](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_GetType)(CONN conn)</span> |

Return character string (null-terminated), verbally representing the current connection type, such as <span class="nctnt ncbi-monospace">"HTTP"</span>, <span class="nctnt ncbi-monospace">"SOCKET"</span>, <span class="nctnt ncbi-monospace">"SERVICE/HTTP"</span>, etc. The unknown connection type gets returned as <span class="nctnt ncbi-macro">NULL</span>.

|---------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_Description](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Description)(CONN conn)</span> |

Return a human-readable description of the connection as a character '\\0'-terminated string. The string is not guaranteed to have any particular format and is intended solely for something like logging and debugging. Return <span class="nctnt ncbi-macro">NULL</span> if the connection cannot provide any description information (or if it is in a bad state). Application program must call <span class="nctnt ncbi-func">free()</span> to deallocate space occupied by the returned string when the description is no longer needed.

|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-code">[CONN\_SetCallback](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_SetCallback)(CONN conn, ECONN\_Callback type,</span><br/><span class="nctnt ncbi-code"> const SCONN\_Callback\* new\_cb, SCONN\_Callback\* old\_cb)</span> |

Set user callback function to be invoked upon an event specified by callback <span class="nctnt ncbi-var">type</span>. The old callback (if any) gets returned via the passed pointer <span class="nctnt ncbi-var">old\_cb</span> (if not <span class="nctnt ncbi-macro">NULL</span>). Callback structure [SCONN\_Callback](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SCONN_Callback) has the following fields: callback function <span class="nctnt ncbi-monospace">func</span> and <span class="nctnt ncbi-monospace">void\* data</span>. Callback function <span class="nctnt ncbi-var">func</span> should have the following prototype:

<span class="nctnt ncbi-code">typedef void (\*FCONN\_Callback)(CONN conn, TCONN\_Callback type, void\* data)</span>

When called, both <span class="nctnt ncbi-var">type</span> of callback and <span class="nctnt ncbi-var">data</span> pointer are supplied. The [callback types](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ECONN_Callback) defined at the time of this writing are:

-   <span class="nctnt ncbi-var">eCONN\_OnClose</span>

-   <span class="nctnt ncbi-var">eCONN\_OnRead</span>

-   <span class="nctnt ncbi-var">eCONN\_OnWrite</span>

-   <span class="nctnt ncbi-var">eCONN\_OnFlush</span>

-   <span class="nctnt ncbi-var">eCONN\_OnTimeout</span>

The callback function is always called prior to the event happening, e.g., a close callback is called when the connection is about to close.

<span class="nctnt highlight">Note:</span> When an <span class="nctnt ncbi-var">eCONN\_OnTimeout</span> callback occurs, the callback type <span class="nctnt ncbi-var">eCONN\_OnTimeout</span> gets OR'ed with the I/O direction, which timed out (<span class="nctnt ncbi-var">eIO\_Read</span>, <span class="nctnt ncbi-var">eIO\_Write</span>, or both when flushing), then passed as the type argument.

See the preamble to [ECONN\_Callback](http://www.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/ident?i=ECONN_Callback) for more details.

### Socket Connector

Constructors are defined in:

    #include <connect/ncbi_socket_connector.h>

A socket connection, based on the socket connector, brings almost direct access to the [SOCK](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket.h) API. It allows the user to create a peer-to-peer data channel between two programs, which can be located anywhere on the Internet.

To create a socket connection, user has to create a socket connector first, then pass it to <span class="nctnt ncbi-func">CONN\_Create()</span>, as in the following example:

    #include <connect/ncbi_socket_connector.h>
    #include <connect/ncbi_connection.h>

    #define MAX_TRY 3 /* Try to connect this many times before giving up */

    unsigned short port = 1234;
    CONNECTOR socket_connector = SOCK_CreateConnector("host.foo.com", port,
        MAX_TRY);
    if (!socket_connector)
        fprintf(stderr, "Cannot create SOCKET connector");
    else {
        CONN conn;
        if (CONN_Create(socket_connector, &conn) != eIO_Success)
            fprintf(stderr, "CONN_Create failed");
        else {
            /* Connection created ok, use CONN_... function */
            /* to access the network */
            ...
            CONN_Close(conn);
        }
    }

A variant form of this connector's constructor, [SOCK\_CreateConnectorEx()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SOCK_CreateConnectorEx), takes three more arguments: a pointer to data (of type <span class="nctnt ncbi-type">void\*</span>), data size (bytes) to specify the data to be sent as soon as the link has been established, and [flags](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=TSOCK_Flags).

The <span class="nctnt ncbi-path">CONN</span> library defines two more constructors, which build SOCKET connectors on top of existing [SOCK](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket.h) objects: [SOCK\_CreateConnectorOnTop()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SOCK_CreateConnectorOnTop) and [SOCK\_CreateConnectorOnTopEx()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SOCK_CreateConnectorOnTopEx), the description of which is intentionally omitted here because SOCK is not discussed either. Please refer to the [description](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket_connector.h) in the Toolkit code.

### File Connector

Constructors defined in:

    #include <connect/ncbi_file_connector.h>

    CONNECTOR file_connector = FILE_CreateConnector("InFile", "OutFile");

This connector could be used for both reading and writing files, when input goes from one file and output goes to another file. (This differs from normal file I/O, when a single handle is used to access only one file, but rather resembles data exchange via socket.)

Extended variant of this connector's constructor, [FILE\_CreateConnectorEx()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=FILE_CreateConnectorEx), takes an additional argument, a pointer to a structure of type [SFILE\_ConnAttr](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SFileConnAttr) describing file connector attributes, such as the initial read position to start from in the input file, an open mode for the output file (append [eFCM\_Append](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EFILE_ConnMode), truncate <span class="nctnt ncbi-var">eFCM\_Truncate</span>, or seek <span class="nctnt ncbi-var">eFCM\_Seek</span> to start writing at a specified file position), and the position in the output file, where to begin output. The attribute pointer passed as <span class="nctnt ncbi-macro">NULL</span> is equivalent to a call to <span class="nctnt ncbi-func">FILE\_CreateConnector()</span>, which reads from the very beginning of the input file and entirely overwrites the output file (if any), implicitly using <span class="nctnt ncbi-var">eFCM\_Truncate</span>.

### Connection-related parameters for higher-level connection protocols

The network information structure (from [connect/ncbi\_connutil.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_connutil.h)) defines parameters of the connection point, where a server is running. See the [Library Configuration](ch_libconfig.html#ch_libconfig.libconfig_connect) chapter for descriptions of the corresponding configuration parameters.

<span class="nctnt highlight">Note</span>: Not all parameters of the structure shown below apply to every network connector.

    /* Network connection related configurable info struct.
     * ATTENTION:  Do NOT fill out this structure (SConnNetInfo) "from scratch"!
     *             Instead, use ConnNetInfo_Create() described below to create
     *             it, and then fix (hard-code) some fields, if really necessary.
     * NOTE1:      Not every field may be fully utilized throughout the library.
     * NOTE2:      HTTP passwords can be either clear text or Base64 encoded values
     *             enclosed in square brackets [] (which are not Base-64 charset).
     *             For encoding / decoding, one can use command line open ssl:
     *             echo "password\|base64value" \| openssl enc {-e\|-d} -base64
     *             or an online tool (search the Web for "base64 online").
     */
    typedef struct {  /* NCBI_FAKE_WARNING: ICC */
        char            client_host[256]; /* effective client hostname ('\0'=def)*/
        EBURLScheme     scheme:3;         /* only pre-defined types (limited)    */
        TReqMethod      req_method:5;     /* method to use in the request (HTTP) */
        unsigned        version:1;        /* HTTP/1.v (or selected by req_method)*/
        EBFWMode        firewall:2;       /* to use firewall (relay otherwise)   */
        unsigned        stateless:1;      /* to connect in HTTP-like fashion only*/
        unsigned        lb_disable:1;     /* to disable local load-balancing     */
        EBDebugPrintout debug_printout:2; /* switch to printout some debug info  */
        unsigned        http_proxy_leak:1;/* non-zero when can fallback to direct*/
        char            user[64];         /* username (if specified)             */
        char            pass[64];         /* password (if any)                   */
        char            host[256];        /* host to connect to                  */
        unsigned short  port;             /* port to connect to, host byte order */
        char            path[1024];       /* path (e.g. to  a CGI script or page)*/
        char            args[1024];       /* args (e.g. for a CGI script)        */
        char            http_proxy_host[256]; /* hostname of HTTP proxy server   */
        unsigned short  http_proxy_port;      /* port #   of HTTP proxy server   */
        char            http_proxy_user[64];  /* http proxy username (if req'd)  */
        char            http_proxy_pass[64];  /* http proxy password             */
        char            proxy_host[256];  /* CERN-like (non-transp) f/w proxy srv*/
        unsigned short  max_try;          /* max. # of attempts to connect (>= 1)*/
        const STimeout* timeout;          /* ptr to I/O timeout(infinite if NULL)*/
        const char*     http_user_header; /* user header to add to HTTP request  */
        const char*     http_referer;     /* default referrer (when not spec'd)  */
        NCBI_CRED       credentials;      /* connection credentials (optional)   */

        /* the following field(s) are for the internal use only -- don't touch!  */
        STimeout        tmo;              /* default storage for finite timeout  */
        const char      svc[1];           /* service which this info created for */
    } SConnNetInfo;

<span class="nctnt highlight">Caution</span>: Unlike other "static fields" of this structure, [http\_user\_header](#httpuserheader) (if non-<span class="nctnt ncbi-macro">NULL</span>) is assumed to be dynamically allocated on the heap (via a call to <span class="nctnt ncbi-func">malloc</span>, <span class="nctnt ncbi-func">calloc</span>, or a related function, such as <span class="nctnt ncbi-func">strdup</span>).

#### ConnNetInfo convenience API

Although nothing prevents users from creating and populating the above <span class="nctnt ncbi-type">SConnNetInfo</span> structure field-by-field, this can lead to memory leaks, and there is a better, easier, much safer, and configurable way to deal with this structure. Unless there's a compelling reason not to, users should create this structure using <span class="nctnt ncbi-func">ConnNetInfo\_Create()</span> and destroy it with <span class="nctnt ncbi-func">ConnNetInfo\_Destroy()</span>. The creation step will populate the structure with configured values, if any, and the destruction step will properly free allocated memory, if any. See [connect/ncbi\_connutil.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_connutil.h) for the interface definition.

The convenience API consists of these functions:

-   [ConnNetInfo\_Create](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ConnNetInfo_Create)<span class="nctnt ncbi-code">(const char\* service)</span><br/><br/>Create and return a pointer to a new <span class="nctnt ncbi-type">SConnNetInfo</span> structure, filled either with parameters specific to a named <span class="nctnt ncbi-var">service</span> or with default values (if the <span class="nctnt ncbi-var">service</span> is specified as <span class="nctnt ncbi-macro">NULL</span>, which is typical in the case of ordinary HTTP connections). Parameters for the structure are taken from (in order of precedence):

    1  
    environment variables of the form <span class="nctnt ncbi-var">\<service\>\_CONN\_\<name\></span>, where <span class="nctnt ncbi-var">name</span> is the name of the field;

    2  
    a service-specific registry section (see below the note about the registry) named <span class="nctnt ncbi-var">[service]</span> using the key <span class="nctnt ncbi-var">CONN\_\<name\></span>;

    3  
    an environment variable of the form <span class="nctnt ncbi-var">CONN\_\<name\></span>; or

    4  
    a registry section named <span class="nctnt ncbi-var">[CONN]</span> using <span class="nctnt ncbi-var">name</span> as a key.<br/><br/>The default value is applied if none of the above resulted in a successful match.<br/><br/>The search for the keys in both environment and registry is not case-sensitive, but the values of the keys are case sensitive (except for enumerated types and boolean values, which can be of any, even mixed, case). Boolean fields accept <span class="nctnt ncbi-monospace">1</span>, <span class="nctnt ncbi-monospace">"ON"</span>, <span class="nctnt ncbi-monospace">"YES"</span>, and <span class="nctnt ncbi-monospace">"TRUE"</span> as <span class="nctnt ncbi-monospace">true</span> values; all other values are treated as <span class="nctnt ncbi-monospace">false</span>. In addition to a floating point number treated as a number of seconds, <span class="nctnt ncbi-var">timeout</span> can accept (case-insensitively) the keyword <span class="nctnt ncbi-monospace">"INFINITE"</span>.<br/><br/><span class="nctnt highlight">Note</span>: The first two steps in the above sequence are skipped if the <span class="nctnt ncbi-var">service</span> name is passed as <span class="nctnt ncbi-macro">NULL</span>.<br/><br/><span class="nctnt highlight">Caution</span>: The library can not provide reasonable default values for <span class="nctnt ncbi-var">path</span> and <span class="nctnt ncbi-var">args</span> when the structure is used for HTTP connectors.

<!-- -->

-   [ConnNetInfo\_Destroy](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ConnNetInfo_Destroy)<span class="nctnt ncbi-code">(SConnNetInfo\* info)</span><br/><br/>Delete and free the info structure via a passed pointer (note that the HTTP user header http\_user\_header is freed as well).

<!-- -->

-   [ConnNetInfo\_SetUserHeader](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ConnNetInfo_SetUserHeader)<span class="nctnt ncbi-code">(SConnNetInfo\* info, const char\* new\_user\_header)</span><br/><br/>Set the new HTTP user header (freeing the previous one, if any) by cloning the passed string argument and storing it in the http\_user\_header field. New\_user\_header passed as NULL resets the field.

<!-- -->

-   [ConnNetInfo\_Clone](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ConnNetInfo_Clone)<span class="nctnt ncbi-code">(SConnNetInfo\* info)</span><br/><br/>Create and return a pointer to a new <span class="nctnt ncbi-type">SConnNetInfo</span> structure, which is an exact copy of the passed structure. This function is recognizes the dynamic nature of the HTTP [user header field](#user-header-field).

<span class="nctnt highlight">Note about the registry:</span> The registry used by the <span class="nctnt ncbi-path">connect</span> library is separate from the [CNcbiRegistry](ch_core.html#ch_core.registry) class. To learn more about the difference and how to use both objects together in a single program, please see [Using NCBI C and C++ Toolkits Together](ch_res.html#ch_res.c_cxx.html).

### HTTP Connector

Constructors defined in:

    #include <connect/ncbi_http_connector.h>

The simplest form of this connector's constructor takes three parameters:

    CONNECTOR HTTP_CreateConnector(const SConnNetInfo* net_info,
                                   const char*         user_header,
                                   THTTP_Flags         flags);

a pointer to the network information structure (can be <span class="nctnt ncbi-macro">NULL</span>), a pointer to a custom HTTP tag-value(s) called a user-header, and a bitmask of various flags. The user-header has to be in the form <span class="nctnt ncbi-code">"HTTP-Tag: Tag-value\\r\\n"</span>, or even multiple tag-values delimited and terminated by <span class="nctnt ncbi-monospace">"\\r\\n"</span>. If specified, the <span class="nctnt ncbi-var">user\_header</span> parameter overrides the corresponding field in the passed <span class="nctnt ncbi-var">net\_info</span>.

The following fields of <span class="nctnt ncbi-type">SConnNetInfo</span> pertain to the HTTP connector: <span class="nctnt ncbi-var">client\_host</span>, <span class="nctnt ncbi-var">host</span>, <span class="nctnt ncbi-var">port</span>, <span class="nctnt ncbi-var">path</span>, <span class="nctnt ncbi-var">args</span>, <span class="nctnt ncbi-var">req\_method</span> (can be one of <span class="nctnt ncbi-monospace">"GET"</span>, <span class="nctnt ncbi-monospace">"POST"</span>, and <span class="nctnt ncbi-monospace">"ANY"</span>), <span class="nctnt ncbi-var">timeout</span>, <span class="nctnt ncbi-var">max\_try</span> (analog of maximal try parameter for the [socket connector](#socket-connector)), <span class="nctnt ncbi-var">http\_proxy\_host</span>, <span class="nctnt ncbi-var">http\_proxy\_port</span>, and <span class="nctnt ncbi-var">debug\_printout</span> (values are <span class="nctnt ncbi-monospace">"NONE"</span> to disable any trace printout of the connection data, <span class="nctnt ncbi-monospace">"SOME"</span> to enable printing of <span class="nctnt ncbi-type">SConnNetInfo</span> structure before each connection attempt, and <span class="nctnt ncbi-monospace">"DATA"</span> to print both headers and data of the HTTP packets in addition to dumps of <span class="nctnt ncbi-type">SConnNetInfo</span> structures). Values of other fields are ignored.

#### HTTP connector's flags

Argument [flags](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=TSOCK_Flags) in the HTTP connector's constructor is a bitwise OR of the following values:

-   <span class="nctnt ncbi-var">fHTTP\_AutoReconnect</span> - By default, only one request/reply is allowed and subsequent write attempts will result in <span class="nctnt ncbi-var">eIO\_Closed</span>. This flag allows multiple request/reply HTTP transactions, using the same URL and parameters for each request.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_Flushable</span> - By default, all data written to the connection are kept until a read begins (even though <span class="nctnt ncbi-func">CONN\_Flush()</span> might have been called in between the writes). With this flag set, <span class="nctnt ncbi-func">CONN\_Flush()</span> will result in the data actually being sent to the server side, so a subsequent write will form a new request and not get added to the previous one. Also, this flag ensures that the connector sends at least an HTTP header on "CLOSE" and re-"CONNECT", even if no data for an HTTP body have been written.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_KeepHeader</span> - By default, the HTTP connection parses the HTTP header and any HTTP errors. Thus, reading from the connection normally only returns data from the HTTP body. This flag disables that feature, and the HTTP header is not parsed but instead is passed "as is" to the application on a call to [CONN\_Read()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CONN_Create). The HTTP header (i.e. everything up to and including the first "<span class="nctnt ncbi-monospace">\\r\\n\\r\\n</span> ") from the CGI script's response (including any server error), is then made available for reading. <span class="nctnt highlight">Note:</span> This flag disables automatic authorization and redirection.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_UrlDecodeInput</span> - Perform URL decoding of the HTTP response body on read (interpreting it as single-part and URL-encoded), and deliver decoded data to the user.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_UrlEncodeOutput</span> - URL encode output data passed in the HTTP body to the HTTP server.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_UrlCodec</span> - Perform both decoding and encoding (<span class="nctnt ncbi-var">fHTTP\_UrlDecodeInput \\| fHTTP\_UrlEncodeOutput</span>).<br/>

-   <span class="nctnt ncbi-var">fHTTP\_NoUpread</span> - By default, writing always verifies that incoming data are available for reading, and those data are extracted and stored in an internal buffer. This default behavior avoids I/O deadlock, when writing creates a backward stream of data, which, if unread, blocks the connection entirely. This flag disables the default behavior, making connections ***not*** attempt to empty incoming data channel into a temporary intermediate buffer while sending data to the HTTP server.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_DropUnread</span> - By default, all data sent by the server get stored - even if not all of it had been requested prior to a "Write" following a "Read" (stream emulation). This flag disables that behavior, making the connection not collect incoming data in "Read" mode before switching into "Write" mode for preparing the next request. <br/>

-   <span class="nctnt ncbi-var">fHTTP\_NoAutoRetry</span> - Do not attempt any auto-retries when a connection fails (this flag effectively means having <span class="nctnt ncbi-var">SConnNetInfo::max\_try</span> set to 1).<br/>

-   <span class="nctnt ncbi-var">fHTTP\_NoAutomagicSID</span> - Do not add NCBI SID automagically.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_InsecureRedirect</span> - For security reasons the following redirects comprise security risk, and thus, are prohibited: switching from https to http, and/or re-POSTing data (regardless of the transport, either http or https). This flag allows such redirects (when encountered) to be honored.<br/>

-   <span class="nctnt ncbi-var">fHTTP\_AdjustOnRedirect</span> - Call adjust routine for redirects, too.

The HTTP connection will be established using the following URL: <span class="nctnt ncbi-monospace">http://host:port/path?args</span>

<span class="nctnt highlight">Note</span> that <span class="nctnt ncbi-var">path</span> has to have a leading slash "/" as the first character, that is, only "http://" and "?" are added by the connector; all other characters appear exactly as specified (but may be encoded with <span class="nctnt ncbi-var">fHTTP\_UrlEncodeArgs</span>). The question mark does not appear if the URL has no arguments.

A more elaborate form of the HTTP connector's constructor has the following prototype:

    typedef enum {
        eHTTP_HeaderError    = 0,  /**< Parse failed, treat as a server error */
        eHTTP_HeaderSuccess  = 1,  /**< Parse succeeded, retain server status */
        eHTTP_HeaderContinue = 2,  /**< Parse succeeded, continue with body   */
        eHTTP_HeaderComplete = 3   /**< Parse succeeded, no more processing   */
    } EHTTP_HeaderParse; 
    typedef EHTTP_HeaderParse (*FHTTP_ParseHeader)
    (const char*         http_header,   /* HTTP header to parse                  */
     void*               user_data,     /* supplemental user data                */
     int                 server_error   /* != 0 if HTTP error (NOT 2xx code)     */
     );

    typedef int/*bool*/ (*FHTTP_Adjust)
    (SConnNetInfo*       net_info,      /* net_info to adjust (in place)         */
     void*               user_data,     /* supplemental user data                */
     unsigned int        failure_count  /* low word: # of failures since open    */
     );

    typedef void        (*FHTTP_Cleanup)
    (void*               user_data      /* supplemental user data                */
     );

    CONNECTOR HTTP_CreateConnectorEx
    (const SConnNetInfo* net_info,
     THTTP_Flags         flags,
     FHTTP_ParseHeader   parse_header,  /* may be NULL, then no addtl. parsing   */
     void*               user_data,     /* user data for HTTP CBs (callbacks)    */
     FHTTP_Adjust        adjust,        /* may be NULL                           */
     FHTTP_Cleanup       cleanup        /* may be NULL                           */
     );

This form is assumed to be used rarely by the users directly, but it provides rich access to the internal management of HTTP connections.

The first two arguments are identical to their counterparts in the arguments number one and three of [HTTP\_CreateConnector()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=HTTP_CreateConnector). The HTTP user header field (if any) is taken directly from the <span class="nctnt ncbi-monospace">http\_user\_header</span> field of <span class="nctnt ncbi-type">SConnNetInfo</span>, a pointer to which is passed as <span class="nctnt ncbi-var">net\_info</span> (which in turn can be passed as <span class="nctnt ncbi-macro">NULL</span>, meaning to use the environment, registry, and defaults as described above).

The third parameter specifies a callback to be activated to parse the HTTP reply header (passed as a single string, with CR-LF (carriage return/line feed) characters incorporated to divide header lines). This callback also gets some custom data <span class="nctnt ncbi-var">user\_data</span> as supplied in the fourth argument of the connector's constructor and a boolean value of <span class="nctnt ncbi-monospace">true</span> if the parsed response code from the server was not okay. The callback can return <span class="nctnt ncbi-monospace">false</span> (zero), which is considered the same way as having an error from the HTTP server. However, the pre-parsed error condition (passed in <span class="nctnt ncbi-var">server\_error</span>) is retained, even if the return value of the callback is <span class="nctnt ncbi-monospace">true</span>, i.e. the callback is unable to "fix" the error code from the server. This callback is **not called** if [fHTTP\_KeepHeader](#fhttpkeepheader) is set in flags.

The fifth argument is a callback, which is in control when an attempt to connect to the HTTP server has failed. On entry, this callback has current <span class="nctnt ncbi-type">SConnNetInfo</span>, which is requested for an adjusted in an attempt that the connection to the HTTP server will finally succeed. That is, the callback can change anything in the info structure, and the modified structure will be kept for all further connection attempts, until changed by this callback again. The number (starting from 1) of successive failed attempts is given in the argument of the last callback. The callback return value <span class="nctnt ncbi-monospace">true</span> (non-zero) means a successful adjustment. The return value <span class="nctnt ncbi-monospace">false</span> (zero) stops connection attempts and returns an error to the application.

When the connector is being destroyed, the custom object <span class="nctnt ncbi-var">user\_data</span> can be destroyed in the callback, specified as the last argument of the extended constructor.

<span class="nctnt highlight">Note</span>: Any callback may be specified as <span class="nctnt ncbi-macro">NULL</span>, which means that no action is foreseen by the application, and default behavior occurs.

### Service Connector

Constructors defined in:

    #include <connect/ncbi_service_connector.h>

This is the most complex connector in the library. It can initiate data exchange between an application and a named NCBI service, and the data link can be either wrapped in HTTP or be just a byte-stream (similar to a socket). In fact, this connector sits on top of either [HTTP](#http) or [SOCKET](#socket) connectors.

The library provides two forms of the connector's constructor:

    SERVICE_CreateConnector(const char* service_name);
    SERVICE_CreateConnectorEx(
        const char*           service_name,
                                        /* The registered name of an NCBI service */
        TSERV_Type            types,    /* Accepted server types, bitmask */
        const SConnNetInfo*   net_info, /* Connection parameters */
        const SSERVICE_Extra* params);  /* Additional set of parameters, may be NULL */

The first form is equivalent to <span class="nctnt ncbi-code">SERVICE\_CreateConnectorEx(service\_name, fSERV\_Any, 0, 0)</span>. A named NCBI service is a CGI program or a stand-alone server (can be one of two supported types), which runs at the NCBI site and is accessible by the outside world. A special dispatcher (which runs on the [NCBI Web servers](http://www.ncbi.nlm.nih.gov)) performs automatic switching to the appropriate server without the client having to know the actual connection point. The client just uses the main entry gate of the NCBI Web (usually, [www.ncbi.nlm.nih.gov](http://www.ncbi.nlm.nih.gov)) with a request to have a service "<span class="nctnt ncbi-monospace">service\_name</span>". Then, depending on the service availability, the request will be declined, rejected, or honored by switching and routing the client to the machine actually running the server.

To the client, the entire process of dispatching is completely transparent (for example, try clicking several times on either of the latter two links and see that the error replies are actually sent from different hosts, and the successful processing of the first link is done by one of several hosts running the bouncing service).

<span class="nctnt highlight">Note:</span> Services can be [redirected](#redirected).

The [Dispatching Protocol](ch_app.html#ch_app.DISPD_Network_Dispat) per se is implemented on top of HTTP protocol and is parsed by a CGI program [dispd.cgi](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/daemons/dispd_cgi.c) (or another dispatching CGI), which is available on the NCBI Web. On every server running the named services, another program, called the [load-balancing daemon](ch_app.html#ch_app.Load_Balancing_Servi) ([lbsmd](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/daemons/lbsmd.c)), is executing. This daemon supports having the same service running on different machines and provides a choice of the one machine that is less loaded. When <span class="nctnt ncbi-app">dispd.cgi</span> receives a request for a named service, it first consults the load-balancing table, which is broadcasted by each load-balancing daemon and populated in a network-wide form on each server. When the corresponding server is found, the client request can be passed, or a dedicated connection to the server can be established. The dispatching is made in such a way that it can be also used directly from most Internet browsers.

The named service facility uses the following distinction of server types:

-   HTTP servers, which are usually CGI programs:

    -   <span class="nctnt ncbi-type">HTTP\_GET</span> servers are those accepting requests only using the HTTP <span class="nctnt ncbi-type">GET</span> method.

    -   <span class="nctnt ncbi-type">HTTP\_POST</span> servers are those accepting requests only using the HTTP <span class="nctnt ncbi-type">POST</span> method.

    -   <span class="nctnt ncbi-type">HTTP</span> servers are those accepting both of either <span class="nctnt ncbi-type">GET</span> or <span class="nctnt ncbi-type">POST</span> methods.

-   <span class="nctnt ncbi-type">NCBID</span> servers are those run by a special CGI engine, called [ncbid.cgi](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/daemons/dispd_cgi.c), a configurable program (now integrated within [ncbid.cgi](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/daemons/dispd_cgi.c) itself) that can convert byte-stream output from another program (server) started by the request from a dispatcher, to an HTTP-compliant reply (that is, a packet having both HTTP header and body, and suitable, for example, for Web browsers).

-   <span class="nctnt ncbi-type">STANDALONE</span> servers, similar to mailing daemons, are those listening to the network, on their own, for incoming connections.

-   <span class="nctnt ncbi-type">FIREWALL</span> servers are the special pseudo-servers, not existing in reality, but that are created and used internally by the dispatcher software to indicate that only a firewall connection mode can be used to access the requested service.

-   <span class="nctnt ncbi-type">DNS</span> servers are beyond the scope of this document because they are used to declare domain names, which are used internally at the NCBI site to help load-balancing based on DNS lookup (see [here](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/daemons/mghbn.c)).

A formal description of these types is given in [connect/ncbi\_server\_info.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_server_info.h):

    /* Server types
     */
    typedef enum {
        fSERV_Ncbid      = 0x01,
        fSERV_Standalone = 0x02,
        fSERV_HttpGet    = 0x04,
        fSERV_HttpPost   = 0x08,
        fSERV_Http       = fSERV_HttpGet \| fSERV_HttpPost,
        fSERV_Firewall   = 0x10,
        fSERV_Dns        = 0x20
    } ESERV_Type;

    #define fSERV_Any           0
    #define fSERV_StatelessOnly 0x80
    typedef unsigned TSERV_Type;  /* bit-wise OR of "ESERV_Type" flags */

The bitwise <span class="nctnt ncbi-monospace">OR</span> of the <span class="nctnt ncbi-type">ESERV\_Type</span> flags can be used to restrict the search for the servers, matching the requested service name. These flags passed as argument <span class="nctnt ncbi-var">types</span> are used by the dispatcher when figuring out which server is acceptable for the client. A special value <span class="nctnt ncbi-monospace">0</span> (or, better <span class="nctnt ncbi-var">fSERV\_Any</span>) states no such preference whatsoever. A special bit-value <span class="nctnt ncbi-var">fSERV\_StatelessOnly</span> set, together with other bits or just alone, specifies that the servers should be of stateless (HTTP-like) type only, and it is the client who is responsible for keeping track of the logical sequence of the transactions.

The following code fragment establishes a service connection to the named service <span class="nctnt ncbi-monospace">"io\_bounce"</span>, using only stateless servers:

    CONNECTOR c;
    CONN conn;
    if(!(c = SERVICE_CreateConnectorEx("io_bounce", fSERV_StatelessOnly, 0, 0))) {
        fprintf(stderr, "No such service available");
    } else if (CONN_Create(c, &conn) != eIO_Success) {
        fprintf(stderr, "Failed to create connection");
    } else {
        static const char buffer[] = "Data to pass to the server";
        size_t n_written;
        CONN_Write(conn, buffer, sizeof(buffer) - 1, &n_written);
        ...
    }

The real type of the data channel can be obtained via a call to <span class="nctnt ncbi-func">CONN\_GetType()</span>.

<span class="nctnt highlight">Note</span>: In the above example, the client has no assumption of how the data actually passed to the server. The server could be of any type in principle, even a stand-alone server, which was used in the request/reply mode of one-shot transactions. If necessary, such wrapping would have been made by the dispatching facility as well.

The next-to-last parameter of the extended constructor is the network info, described in the section devoted to the [HTTP connector](#http-connector). The service connector uses all fields of this structure, except for [http\_user\_header](#httpuserheader), and the following assumptions apply:

-   <span class="nctnt ncbi-var">path</span> specifies the dispatcher program (defaulted to <span class="nctnt ncbi-app">dispd.cgi</span>)

-   <span class="nctnt ncbi-var">args</span> specifies parameters for the requested service (this is service specific, no defaults)

-   <span class="nctnt ncbi-var">stateless</span> is used to set the <span class="nctnt ncbi-var">fSERV\_StatelessOnly</span> flag in the server type bitmask, if it was not set there already (which is convenient for modifying the dispatch by using environment and/or registry, if the flag is not set, yet allows hardcoding of the flag at compile time by setting it in the constructor's <span class="nctnt ncbi-var">types</span> argument explicitly)

-   <span class="nctnt ncbi-var">lb\_disable</span> set to <span class="nctnt ncbi-monospace">true</span> (non-zero) means to always use the remote dispatcher (via network connection), even if locally running load-balancing daemon is available (by default, the local load-balancing deamon is consulted first to resolve the name of the service)

-   <span class="nctnt ncbi-var">firewall</span> set to true (non-zero) disables the direct connection to the service; instead,

    -   a connection to a proxy [firewall daemon](ch_app.html#ch_app.Firewall_Daemon_FWDa) ([fwdaemon](http://intranet.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/lxr/source/src/connect/daemons/fwdaemon.c)), running at the NCBI site, is initiated to pass the data in stream mode;

    -   or data get relayed via the dispatcher, if the stateless server is used

-   <span class="nctnt ncbi-var">http\_user\_header</span> merged not to conflict with special dispatcher parameter.

As with the HTTP connector, if the network information structure is specified as <span class="nctnt ncbi-macro">NULL</span>, the default values are obtained as described above, as with the call to [ConnNetInfo\_Create(service\_name)](#connnetinfocreateservicename).

Normally, the last parameter of <span class="nctnt ncbi-func">SERVICE\_CreateConnectorEx()</span> is left <span class="nctnt ncbi-macro">NULL</span>, which sets all additional parameters to their default values. Among others, this includes the default procedure of choosing an appropriate server when the connector is looking for a mapping of the service name into a server address. To see how this parameter can be used to change the mapping procedure, please see the [service mapping API](#service-mapping-api) section. The library provides an additional interface to the service mapper, which can be found in [connect/ncbi\_service.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_service.h).

<span class="nctnt highlight">Note</span>: Requesting <span class="nctnt ncbi-var">fSERV\_Firewall</span> in the <span class="nctnt ncbi-var">types</span> parameter effectively selects the firewall mode regardless of the network parameters, loaded via the <span class="nctnt ncbi-type">SConnNetInfo</span> structure.

#### Service Redirection

Services can be redirected without changing any code - for example, to test production code with a test service, or for debugging. Services are redirected using the <span class="nctnt ncbi-monospace">\<service\>\_CONN\_SERVICE\_NAME</span> environment variable or the <span class="nctnt ncbi-monospace">[\<service\>] CONN\_SERVICE\_NAME</span> registry entry (see the connection library [configuration section](ch_libconfig.html#ch_libconfig.libconfig_connect)). The client application will use the original service name, but the connection will actually be made to the redirected-to service.

Debugging Tools and Troubleshooting
-----------------------------------

Each connector (except for the <span class="nctnt ncbi-type">FILE</span> connector) provides a means to view data flow in the connection. In case of the [SOCKET](#socket) connector, debugging information can be turned on by the last argument in [SOCK\_CreateConnectorEx()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SOCK_CreateConnectorEx) or by using the global routine [SOCK\_SetDataLoggingAPI()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SOCK_SetDataLoggingAPI) (declared in [connect/ncbi\_socket.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_socket.h))

<span class="nctnt highlight">Note</span>: In the latter case, every socket (including sockets implicitly used by other connectors such as <span class="nctnt ncbi-type">HTTP</span> or <span class="nctnt ncbi-type">SERVICE</span>) will generate debug printouts.

In case of [HTTP](#http) or [SERVICE](#service) connectors, which use [SConnNetInfo](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SConnNetInfo), debugging can be activated directly from the environment by setting <span class="nctnt ncbi-var">CONN\_DEBUG\_PRINTOUT</span> to <span class="nctnt ncbi-monospace">TRUE</span> or <span class="nctnt ncbi-monospace">SOME</span>. Similarly, a registry key <span class="nctnt ncbi-var">DEBUG\_PRINTOUT</span> with a value of either <span class="nctnt ncbi-monospace">TRUE</span> or <span class="nctnt ncbi-var">SOME</span> found in the section <span class="nctnt ncbi-var">[CONN]</span> will have the same effect: it turns on logging of the connection parameters each time the link is established. When set to <span class="nctnt ncbi-monospace">ALL</span>, this variable (or key) also turns on debugging output on all underlying sockets ever created during the life of the connection. The value <span class="nctnt ncbi-monospace">FALSE</span> (default) turns debugging printouts off. Moreover, for the <span class="nctnt ncbi-type">SERVICE</span> connector, the debugging output option can be set on a per-service basis using <span class="nctnt ncbi-var">\<service\>\_CONN\_DEBUG\_PRINTOUT</span> environment variables or individual registry sections <span class="nctnt ncbi-var">[\<service\>]</span> and the key <span class="nctnt ncbi-var">CONN\_DEBUG\_PRINTOUT</span> in them.

<span class="nctnt highlight">Note</span>: Debugging printouts can only be controlled in a described way via environment or registry if and only if <span class="nctnt ncbi-type">SConnNetInfo</span> is always created with the use of [convenience routines](#convenience-routines).

Debugging output is always sent to the same destination, the CORE log file, which is a C object shared between both C and C++ Toolkits. As said previously, the logger is an abstract object, i.e. it is empty and cannot produce any output if not populated accordingly. The library defines a few calls gathered in [connect/ncbi\_util.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_util.h), which allow the logger to output via the <span class="nctnt ncbi-type">FILE</span> file pointer, such as <span class="nctnt ncbi-var">stderr</span>: [CORE\_SetLOGFILE()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CORE_SetLOGFILE) for example, as shown in [test\_ncbi\_service\_connector.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/test/test_ncbi_service_connector.c), or to be a regular file on disk. Moreover, both Toolkits define [interfaces](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/ctools) to deal with registries, loggers, and locks that use native objects of each toolkit and use them as replacements for the objects of the corresponding abstract layers.

There is a common problem that has been reported several times and actually concerns network configuration rather than representing a malfunction in the library. If a test program, which connects to a named NCBI service, is not getting anything back from the NCBI site, one first has to check whether there is a transparent proxying/caching between the host and NCBI. Because the service dispatching is implemented on top of the ordinary HTTP protocol, the transparent proxying may latch unsuccessful service searches (which can happen and may not indicate a real problem) as error responses from the NCBI server. Afterwards, instead of actually connecting to NCBI, the proxy returns those cached errors (or sometimes just an empty document), which breaks the service dispatcher code. In most cases, there are configurable ways to exclude certain URLs from proxying and caching, and they are subject for discussion with a local network administrator.

Here is another tip: Make sure that all custom HTTP header tags (as passed, for example, in the [SConnNetInfo::user\_header](#sconnnetinfouserheader) field) have "\\r\\n" as tag separators (including the last tag). Many proxy servers (including transparent proxies, of which the user may not even be aware) are known to be sensitive to whether each and every HTTP tag is closed by "\\r\\n" (and not by a single "\\n" character). Otherwise, the HTTP packet may be treated as a defective one and can be discarded.

Additional discussion on parameters of the service dispatcher as well as the trouble shooting tips can be found [here](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/NETWORK/dispatcher.html).

C++ Connection Streams
----------------------

This section discusses the "C++ API" for [connection streams](http://www.ncbi.nlm.nih.gov/ieb/ToolBox/CPP_DOC/doxyhtml/group__ConnStreams.html), which should generally be preferred over the corresponding "C API" for [connections and connectors](#connections-and-connectors).

Using connections and connectors (via the entirely procedural approach) in C++ programs would fail to take advantage of the power of the language. Therefore, the C++ API for connection streams was created to provide C++ users with an improved connections API that takes advantage of the power of C++. Because the C++ API is built on top of the corresponding C API connections and connectors, the C++ API contains essentially a superset of the C API functionality. Also, these classes all derive from <span class="nctnt ncbi-class">CNcbiIostream</span> (which is typedef'd as <span class="nctnt ncbi-class">std::iostream</span>) so they can be used with all the common stream I/O operators, manipulators, etc. The hierarchy for the C++ API connection stream classes is shown in [Figure 1](#figure-1):

![Figure 1. C++ API connection stream classes](/book-test/static/img/ch_conn_CConn_IOStream.png)

Figure 1. C++ API connection stream classes

The declarations of the connection stream constructors can be found in [connect/ncbi\_conn\_stream.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_conn_stream.hpp). We tried to keep the number and order of the constructors' parameters in the C++ API the same as their counterparts in the C API. For example:

    /* C API */
    extern NCBI_XCONNECT_EXPORT CONNECTOR SOCK_CreateConnector
    (const char*    host,      /* host to connect to                             */
     unsigned short port,      /* port to connect to                             */
     unsigned short max_try    /* max.number of attempts to establish connection */
     );

    // C++ API
    class NCBI_XCONNECT_EXPORT CConn_SocketStream : public CConn_IOStream
    ...
    CConn_SocketStream
    (const string&   host,                        ///< host to connect to
     unsigned short  port,                        ///< ... and port number
     unsigned short  max_try,                     ///< number of attempts
     const STimeout* timeout  = kDefaultTimeout,
     size_t          buf_size = kConn_DefaultBufSize);

The code below is a C++ style example for a service connection:

    #include <connect/ncbi_conn_stream.hpp>
    try {
        CConn_ServiceStream ios("io_bounce", fSERV_StatelessOnly, 0);
        ios << "Data to be passed to the server";
    } STD_CATCH_ALL ("Connection problem");

<span class="nctnt highlight">Note</span>: The stream constructor may throw an exception if, for instance, the requested service is not found, or some other kind of problem arose. To see the actual reason, we used a standard toolkit macro [STD\_CATCH\_ALL()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=STD_CATCH_ALL), which prints the message and problem description into the log file (<span class="nctnt ncbi-var">cerr</span>, by default).

The next example establishes a TCP socket connection with a specified host/port, allowing direct access to the stream for subsequent operations:

    // Create a SOCKET-type connection stream.
    CConn_SocketStream ss(m_Host, m_Port);

    // Send a request.
    ss << m_Request << NcbiEndl;

    // Read the response.
    ss >> m_Buf;

The next example is a complete program that fetches the response from a URL and displays the HTTP status code and response body (regardless of whether the status is successful or not):

    #include <corelib/ncbiapp.hpp>
    #include <connect/ncbi_conn_stream.hpp>

    USING_NCBI_SCOPE;

    // This is a callback to parse the header and return a code indicating
    // how to handle errors.
    EHTTP_HeaderParse ParseHeader
    (const char*         http_header,   /**< HTTP header to parse                */
     void*               user_data,     /**< supplemental user data              */
     int                 server_error   /**< != 0 if HTTP error (NOT 2xx code)   */
     )
    {
        return (server_error >= 200 && server_error <= 299) 
               ? eHTTP_HeaderSuccess
               : eHTTP_HeaderContinue;
    }

    int main(int argc, const char* argv[])
    {
        // Get a URL.
        string url;
        if (argc > 1) {
            url = argv[1];
        } else {
            url = "http://www.ncbi.nlm.nih.gov/";
        }

        // Send an HTTP request to the URL.
        const string header = "Content-Type: application/x-www-form-urlencoded";
        CConn_HttpStream httpstr(url, nullptr, header, ParseHeader);

        // Copy the HTTP response into a string.
        CConn_MemoryStream mem_str;
        NcbiStreamCopy(mem_str, httpstr);
        string response;
        mem_str.ToString(&response);

        // Output the results.
        NcbiCout << "Status Code: "  << httpstr.GetStatusCode() << NcbiEndl;
        NcbiCout << "Response:" << NcbiEndl;
        NcbiCout << response;

        return 0;
    }

<span class="nctnt highlight">Note:</span> HTTP error responses are generally not well-formed, and they generally don't originate from the accessed resource (but instead often originate from the server on behalf of the resource). Therefore, by default, HTTP connections and streams in the connection library prevent access to data in the response in such cases. However, the above example shows how to tell the stream that it's okay to pass the message data through to the user level. Specifically, it installs the <span class="nctnt ncbi-func">ParseHeader()</span> callback (of type [FHTTP\_ParseHeader](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/group__Connectors.html#ga397d6d094a398603a9d45abbf2727314)) to parse the HTTP header and return a code indicating how the stream should handle header errors. In this case, it simply returns values that will release the HTTP response body as stream / connection data. A more sophisticated callback could involve an actual header analysis and return different codes depending on the outcome.

However, there is a bug (scheduled to be fixed in the near future) with <span class="nctnt ncbi-var">eHTTP\_HeaderContinue</span> processing that can cause a retry of the request. To work around the issue, try one of the following:

-   set <span class="nctnt ncbi-var">CONN\_MAX\_TRY</span> to 1 (search for CONN\_MAX\_TRY in the Connection library [configuration parameters](http://www.ncbi.nlm.nih.gov/toolkit/doc/book/ch_libconfig/#ch_libconfig.T7)); or

-   pass <span class="nctnt ncbi-var">fHTTP\_NoAutoRetry</span> in the stream flags; or

-   return <span class="nctnt ncbi-var">eHTTP\_HeaderComplete</span> from the header callback (if no redirects and/or additional authentication is expected in the stream).

Additional examples can be found in the test files:

-   [test\_ncbi\_conn\_stream.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/test/test_ncbi_conn_stream.cpp)

-   [test\_ncbi\_ftp\_download.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/test/test_ncbi_ftp_download.cpp)

Service mapping API
-------------------

The API defined in [connect/ncbi\_service.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_service.h) is designed to map the required service name into the server address. Internally, the mapping is done either directly or indirectly by means of the [load-balancing daemon](ch_app.html#ch_app.Load_Balancing_Servi), running at the NCBI site. For the client, the mapping is seen as reading from an iterator created by a call to <span class="nctnt ncbi-func">SERV\_Open()</span>, similar to the following fragment (for more examples, please refer to the test program [test\_ncbi\_disp.c](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/connect/test/test_ncbi_disp.c)):

    #include <connect/ncbi_service.h>
    SERV_ITER iter = SERV_Open("my_service", fSERV_Any, SERV_ANYHOST, 0);
    int n = 0;
    if (iter != 0) {
        const SSERV_Info * info;
        while ((info = SERV_GetNextInfo(iter)) != 0) {
            char* str = SERV_WriteInfo(info);
            printf("Server = `%s'\n", str);
            free(str);
            n++;
        }
        SERV_Close(iter);
    }
    if (!iter \|\| !n)
        printf("Service not found\n");

<span class="nctnt highlight">Note:</span> Services can be [redirected](#redirected).

<span class="nctnt highlight">Note</span>: A non-<span class="nctnt ncbi-macro">NULL</span> iterator returned from <span class="nctnt ncbi-func">SERV\_Open()</span> **does not** yet guarantee that the service is available, whereas the <span class="nctnt ncbi-macro">NULL</span> iterator definitely means that the service does not exist.

As shown in the above example, a loop over reading from the iterator results in the sequence of successive structures (none of which is to be freed by the program!) that along with the conversion functions [SERV\_ReadInfo()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SERV_ReadInfo), [SERV\_WriteInfo()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SERV_WriteInfo) and others are defined in [connect/ncbi\_server\_info.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_server_info.h). Structure [SSERV\_Info](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SSERV_Info) describes a server that implements the requested service. <span class="nctnt ncbi-macro">NULL</span> gets returned when no more servers (if any) could be found. The iterator returns servers in the order that the load-balancing algorithm arranges them. Each server has a rating, and the larger the rating the better the chance for the server to be coming out first (but not necessarily in the order of the rates).

<span class="nctnt highlight">Note</span>: Servers returned from the iterator are all of the requested type, with only one exception: they can include servers of type <span class="nctnt ncbi-var">fSERV\_Firewall</span>, even if this type has not been explicitly requested. Therefore, the application must sort these servers out. But if <span class="nctnt ncbi-var">fSERV\_Firewall</span> is set in the types, then the search is done for whichever else types are requested, and with the assumption that the client has chosen a firewall connection mode, regardless of the network parameters supplied in <span class="nctnt ncbi-type">SConnNetInfo</span> or read out from either the registry or environment.

<span class="nctnt highlight">Note</span>: A search for servers of type <span class="nctnt ncbi-var">fSERV\_Dns</span> is not inclusive with <span class="nctnt ncbi-var">fSERV\_Any</span> specified as a server type. That is, servers of type DNS are only returned if specifically requested in the server mask at the time the iterator was opened.

There is a simplified version of <span class="nctnt ncbi-func">SERV\_Open()</span>, called [SERV\_OpenSimple()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SERV_OpenSimple), as well as an advanced version, called [SERV\_OpenEx()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SERV_OpenEx). The former takes only one argument, the service name. The latter takes two more arguments, which describe the set of servers **not** to be returned from the iterator (server descriptors that to be excluded).

There is also an advanced version of <span class="nctnt ncbi-func">SERV\_GetNextInfo()</span>, called [SERV\_GetNextInfoEx()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SERV_GetNextInfoEx), that, via its second argument, provides the ability to get many host parameters, among which is so-called host environment; a <span class="nctnt ncbi-monospace">"\\0"</span>-terminated string, consisting of a set of lines separated by <span class="nctnt ncbi-monospace">"\\n"</span> characters and specified in the configuration file of the [load-balancing daemon](ch_app.html#ch_app.Load_Balancing_Servi) of the host, where the returned server has been found. The typical line within the set has a form <span class="nctnt ncbi-monospace">"name=value"</span> and resembles very much the shell environment, where its name comes after. The host environment could be very handy for passing additional information to applications if the host has some limitations or requires special handling, should the server be selected and used on this host. The example below shall give an idea. At the time of writing, getting the host information is only implemented when the service is obtained via direct access to the load-balancing daemon. Unlike returned server descriptors, the returned host information handle is not a constant object and must be explicitly freed by the application when no longer needed. All operations (getter methods) that are defined on the host information handle are declared in [connect/ncbi\_host\_info.h](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/connect/ncbi_host_info.h). If the server descriptor was obtained using dispatching CGI (indirect dispatching, see below), then the host information handle is always returned as <span class="nctnt ncbi-macro">NULL</span> (no host information available).

The back end of the service mapping API is split into two independent parts: *direct* access to [LBSMD](ch_app.html#ch_app.Load_Balancing_Servi), if the one is both available on the current host and is not disabled by parameter [lb\_disable](#lbdisable) at the iterator opening. If <span class="nctnt ncbi-app">LBSMD</span> is either unavailable or disabled, the second (*indirect*) part of the back-end API is used, which involves a connection to the [dispatching CGI](ch_app.html#ch_app.DISPD_Network_Dispat), which in turn connects to <span class="nctnt ncbi-app">LBSMD</span> to carry out the request. An attempt to use the CGI is done only if the [net\_info](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SERV_OpenEx) argument is provided as non-<span class="nctnt ncbi-macro">NULL</span> in the calls to <span class="nctnt ncbi-func">SERV\_Open()</span> or <span class="nctnt ncbi-func">SERV\_OpenEx()</span>.

<span class="nctnt highlight">Note</span>: In a call to <span class="nctnt ncbi-func">SERV\_OpenSimple()</span>, <span class="nctnt ncbi-var">net\_info</span> gets created internally before an upcall to <span class="nctnt ncbi-func">SERV\_Open()</span> and thus CGI dispatching is likely to happen, unless either <span class="nctnt ncbi-var">net\_info</span> could not be constructed from the environment, or the environment variable <span class="nctnt ncbi-var">CONN\_LB\_DISABLE</span> (or a service-specific one, or either of the corresponding registry keys) is set to <span class="nctnt ncbi-monospace">TRUE</span>.

<span class="nctnt highlight">Note</span>: In the above conditions, the network service name resolution is also undertaken if the service name could not be resolved (because the service could not be found or because of some other error) with the use of locally running <span class="nctnt ncbi-app">LBSMD</span>.

The following code example uses both a [service connector](#service-connector) and the service mapping API to access certain services using an alternate way (other than the connector's default) of choosing appropriate servers. By default, the service connector opens an internal service iterator and then tries to connect to the next server, which <span class="nctnt ncbi-func">SERV\_GetNextInfo()</span> returns when given the iterator. That is, the server with a higher rate is tried first. If user provides a pointer to structure <span class="nctnt ncbi-type">SSERVICE\_Extra</span> as the last parameter of the connector's constructor, then the user-supplied routine (if any) can be called instead to obtain the next server. The routine is also given a supplemental custom argument <span class="nctnt ncbi-var">data</span> taken from <span class="nctnt ncbi-type">SSERVICE\_Extra</span>. The (intentionally simplified) example below tries to create a connector to an imaginary service <span class="nctnt ncbi-monospace">"my\_service"</span> with the restriction that the server has to additionally have a certain (user-specified) database present. The database name is taken from the LBSMD host environment keyed by <span class="nctnt ncbi-monospace">"my\_service\_env"</span>, the first word of which is assumed to be the required database name.

    #include <connect/ncbi_service_connector.h>
    #include <ctype.h>
    #include <stdlib.h>
    #include <string.h>

    #define ENV_DB_KEY "my_service_env="

    /* This routine gets called when connector is about to be destructed. */
    static void s_CleanupData(void* data)
    {
        free(data); /* we kept database name there */
    }

    /* This routine gets called on each internal close of the connector
     * (which may be followed by a subsequent internal open).
     */
    static void s_Reset(void* data)
    {
        /* just see that reset happens by printing DB name */
        printf("Connection reset, DB: %s\n", (char*) data);
    }

    /* 'Data' is what we supplied among extra-parameters in connector's
     * constructor.
     * 'Iter' is an internal service iterator used by service connector;
     * it must remain open.
     */
    static const SSERV_Info* s_GetNextInfo(void* data, SERV_ITER iter)
    {
        const char* db_name = (const char*) data;
        size_t len = strlen(db_name);
        SSERV_Info* info;
        HOST_INFO hinfo;
        int reset = 0;
        for (;;) {
            while ((info = SERV_GetNextInfoEx(iter, &hinfo)) != 0) {
                const char* env = HINFO_Environment(hinfo);
                const char* c;
                for (c = env; c; c = strchr(c, '\n')) {
                    if (strncmp(c == env ? c : ++c, ENV_DB_KEY,
                        sizeof(ENV_DB_KEY)-1) == 0) {
                        /* Our keyword has been detected in environment */
                        /* for this host */
                        c += sizeof(ENV_DB_KEY) - 1;
                        while (*c && isspace(*c))
                            c++;
                        if (strncmp(c, db_name, len) == 0 && !isalnum(c + len)) {
                            /* Database name match */
                            free(hinfo); /* must be freed explicitly */
                            return info;
                        }
                    }
                }
                if (hinfo)
                free(hinfo); /* must be freed explicitly */
            }
            if (reset)
                break; /* coming to reset 2 times in a row means no server fit */
            SERV_Reset(iter);
            reset = 1;
        }
        return 0; /* no match found */
    }

    int main(int argc, char* argv[])
    {
        char* db_name = strdup(argv[1]);
        SSERVICE_Extra params;
        CONNECTOR c;
        CONN conn;
        memset(&params, 0, sizeof(params));
        params.data = db_name; /* custom data, anything */
        params.reset = s_Reset; /* reset routine, may be NULL */
        params.cleanup = s_CleanupData; /* cleanup routine, may be NULL*/
        params.get_next_info = s_GetNextInfo; /* custom iterator routine */
        if (!(c = SERVICE_CreateConnectorEx("my_service",
            fSERV_Any, NULL, &params))) {
            fprintf(stderr, "Cannot create connector");
            exit(1);
        }
        if (CONN_Create(c, &conn) != eIO_Success) {
            fprintf(stderr, "Cannot create connection");
            exit(1);
        }
        /* Now we can use CONN_Read(),CONN_Write() etc to deal with
         * connection, and we are assured that the connection is made
         * only to the server on such a host which has "db_name"
         * specified in configuration file of LBSMD.
         */
        ...
        CONN_Close(conn);
        /* this also calls cleanup of user data provided in params */
        return 0;
    }

<span class="nctnt highlight">Note</span>: No network (indirect) mapping occurs in the above example because <span class="nctnt ncbi-var">net\_info</span> is passed as <span class="nctnt ncbi-macro">NULL</span> to the connector's constructor.

### Local specification of the LBSM table

The LBSM table can also be specified locally, in config file and/or environment variables.

Service lookup process now involves looking up through the following sources, in this order:

-   Local environment/registry settings;

-   LBSM table (only in-house; this step does not exist in the outside builds);

-   Network dispatcher.

Only one source containing the information about the service is used; the next source is only tried if the previous one did not yield in any servers (for the service).

Step 1 is disabled by default, to enable it set <span class="nctnt ncbi-var">CONN\_LOCAL\_ENABLE</span> environment variable to "1" (or "ON, or "YES", or "TRUE") or add <span class="nctnt ncbi-var">LOCAL\_ENABLE</span>=1 to [<span class="nctnt ncbi-monospace">CONN</span>] section in <span class="nctnt ncbi-path">.ini</span> file. Steps 2 and 3 are enabled by default. To disable them use <span class="nctnt ncbi-var">CONN\_LBSMD\_DISABLE</span> and/or <span class="nctnt ncbi-var">CONN\_DISPD\_DISABLE</span> set to "1" in the environment or <span class="nctnt ncbi-var">LBSMD\_DISABLE</span>=1 and/or <span class="nctnt ncbi-var">DISPD\_DISABLE</span>=1 under the section "[<span class="nctnt ncbi-monospace">CONN</span>]" in the registry, respectively.

<span class="nctnt highlight">Note:</span> Alternatively, and for the sake of backward compatibility with older application, the use of local LBSM table can be controlled by <span class="nctnt ncbi-var">CONN\_LB\_DISABLE</span>={0,1} in the environment or <span class="nctnt ncbi-var">LB\_DISABLE</span>={0,1} in the "[<span class="nctnt ncbi-monospace">CONN</span>]" section of the registry, or individually on a per service basis:

For a service "ANAME", <span class="nctnt ncbi-var">ANAME\_CONN\_LB\_DISABLE</span>={0,1} in the environment, or <span class="nctnt ncbi-var">CONN\_LB\_DISABLE</span>={0,1} in the section "[<span class="nctnt ncbi-monospace">ANAME</span>]" in the registry (to affect setting of this particular service, and no others).

The local environment / registry settings for service "ANAME" are screened in the following order:

-   A value of environment variable "<span class="nctnt ncbi-var">ANAME\_CONN\_LOCAL\_SERVER\_n</span>";

-   A value of registry key "<span class="nctnt ncbi-var">CONN\_LOCAL\_SERVER\_n</span>" in the registry section "[<span class="nctnt ncbi-monospace">ANAME</span>]"

Note that service names are not case sensitive, yet the environment variables are looked up all capitalized.

An entry found in the environment takes precedence over an entry (for the same "n") found in the registry. "n" counts from 0 to 100, and need not to be sequential.

All service entries can be (optionally) grouped together in a list as a value of either:

-   Environment variable "<span class="nctnt ncbi-var">CONN\_LOCAL\_SERVICES</span>", or

-   Registry key "<span class="nctnt ncbi-var">LOCAL\_SERVICES</span>" under the registry section "[<span class="nctnt ncbi-monospace">CONN</span>]".

The list of local services is only used in cases of wildcard searches, or in cases of reverse lookups, and is never consulted in regular cases of forward searches by a complete service name.

Examples:

1. In <span class="nctnt ncbi-path">.ini</span> file

    [CONN]
    LOCAL_ENABLE=yes
    LOCAL_SERVICES="MSSQL10 MSSQL14 MSSQL15 MSSQL16 MSSQL17"

    [MSSQL10]
    CONN_LOCAL_SERVER_6="DNS mssql10:1433 L=yes"

    [MSSQL15]
    CONN_LOCAL_SERVER_9="DNS mssql15:1433 L=yes"

Note that entries for MSSQL14, 16, and 17 are not shown, and they are not required (inexistent definitions for declared services are simply ignored).

2. In environment set the following variables (equivalent to the <span class="nctnt ncbi-path">.ini</span> fragment above but having a higher precedence):

    CONN_LOCAL_ENABLE=yes
    CONN_LOCAL_SERVICES="MSSQL10 MSSQL14 MSSQL15 MSSQL16 MSSQL17"
    MSSQL10_CONN_LOCAL_SERVER_6="DNS mssql10:1433 L=yes"
    MSSQL15_CONN_LOCAL_SERVER_9="DNS mssql15:1433 L=yes"

You can also look at the [detailed description of LBSMD](ch_app.html#ch_app.Load_Balancing_Servi) and a sample configuration file.

Threaded Server Support
-----------------------

This library also provides [CServer](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CServer&d=), an abstract base class for multithreaded network servers. [Here](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/src/connect/test/test_server.cpp) is a demonstration of its use. For more information, see the [Implementing a Server with CServer](ch_grid.html#ch_grid.CServer_Multithreade) section.


