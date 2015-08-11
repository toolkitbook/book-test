---
layout: default
title: C++ Toolkit test
nav: pages/ch_grid
---

<span class="label">23</span>Distributed Computing
==================================================

Created: May 14, 2007; Last Update: November 13, 2013.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter describes the NCBI GRID framework. This framework allows creating, running and maintaining a scalable, load-balanced and fault-tolerant pool of network servers ([Worker Nodes](#worker-nodes)).

Note: Users within NCBI may find additional information on the [internal Wiki page](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/GRID).

### Chapter Outline

The following is an outline of the topics presented in this chapter:

-   [Getting Help](#getting-help)

-   [GRID Overview](#grid-overview)

    -   [Purpose](#purpose)

    -   [Components](#components)

    -   [Architecture and Data Flow](#architecture-and-data-flow)

    -   [The GRID Farm](#the-grid-farm)

-   [Worker Nodes](#worker-nodes)

    -   [Create a GRID Worker Node from scratch](#create-a-grid-worker-node-from-scratch)

    -   [Converting an existing CGI application into a GRID Node](#converting-an-existing-cgi-application-into-a-grid-node)

    -   [Wrapping an existing CGI application into a GRID Node](#wrapping-an-existing-cgi-application-into-a-grid-node)

    -   [Wrapping an existing command-line application into a GRID Node](#wrapping-an-existing-command-line-application-into-a-grid-node)

    -   [Worker Node Cleanup Procedure](#worker-node-cleanup-procedure)

-   [Job Submitters](#job-submitters)

-   [Implementing a Network Server](#implementing-a-network-server)

    -   [Typical Client-Server Interactions](#typical-client-server-interactions)

    -   [The CServer Framework Classes](#the-cserver-framework-classes)

    -   [State, Events, and Flow of Control](#state-events-and-flow-of-control)

    -   [Socket Closure and Lifetime](#socket-closure-and-lifetime)

    -   [Diagnostics](#diagnostics)

    -   [Handling Exceptions](#handling-exceptions)

    -   [Server Configuration](#server-configuration)

    -   [Other Resources](#other-resources)

-   [GRID Utilities](#grid-utilities)

    -   [netschedule\_control](#netschedulecontrol)

    -   [ns\_remote\_job\_control](#nsremotejobcontrol)

    -   [Alternate list input and output](#alternate-list-input-and-output)

Getting Help
------------

Users at NCBI have the following sources for help:

-   [JIRA](http://jira/secure/CreateIssue!default.jspa) for submitting a request or bug report. Select project [C++ Toolkit](http://jira.be-md.ncbi.nlm.nih.gov/browse/CXX) and component [GRID](http://jira.be-md.ncbi.nlm.nih.gov/browse/CXX/component/10197).

-   Mailing lists:

    -   The [grid](http://www.ncbi.nlm.nih.gov/mailman/listinfo/grid) mailing list (<span class="oem_mark">grid@ncbi.nlm.nih.gov</span>) for general GRID-related discussion and announcements.

    -   The [grid-core](http://www.ncbi.nlm.nih.gov/mailman/listinfo/grid-core) mailing list (<span class="oem_mark">grid-core@ncbi.nlm.nih.gov</span>) for getting help using or trouble-shooting a GRID service.

-   The GRID developers:

    -   [Dmitry Kazimirov](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Dmitry_Kazimirov) for questions about Client-side APIs, [Worker Nodes](#worker-nodes), [NetCache](ch_app.html#ch_app.ncbi_netcache_service) and [NetSchedule](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/NetSchedule) deployment, auxiliary tools and utilities, administration - setup, installation, and upgrades.

    -   [Andrei Gourianov](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Andrei_Gourianov) for [NetCache](ch_app.html#ch_app.ncbi_netcache_service) server questions.

    -   [Sergey Satskiy](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Sergey_Satskiy) for [NetSchedule](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/NetSchedule) server questions.

    -   [David McElhany](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/David_McElhany) for questions about related documentation in the C++ Toolkit book.

    -   [Denis Vakatov](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/Denis_Vakatov) for supervision questions.

GRID Overview
-------------

The following sections provide an overview of the GRID system:

-   [Purpose](#purpose)

-   [Components](#components)

-   [Architecture and Data Flow](#architecture-and-data-flow)

-   [The GRID Farm](#the-grid-farm)

### Purpose

The NCBI GRID is a framework to create, run and maintain a scalable, load-balanced and fault-tolerant pool of network servers ([Worker Nodes](#worker-nodes)).

It includes independent components that implement distributed data storage and job queueing. It also provides APIs and frameworks to implement worker nodes and job submitters.

Worker nodes can be written from scratch, but there are also convenience APIs and frameworks to easily create worker nodes out of existing C++ CGI code, or even from CGI or command-line scripts and executables.

There is also a GRID farm where developers can jump-start their distributed computation projects.

Two PowerPoint presentations have additional information about the NCBI GRID:

-   <ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/DOC/PPT/GRID-Dec14-2006/GRID_Dec14_2006.pps>

-   <ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/DOC/PPT/NCBI-Grid.ppt>

### Components

The NCBI GRID framework is built of the following components:

1  
Network job queue ([NetSchedule](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/NetSchedule))

2  
Network data storage ([NetCache](ch_app.html#ch_app.ncbi_netcache_service))

3  
Server-side APIs and tools to develop [Worker Nodes](#worker-nodes):

a  
[Out of an existing command-line executable](#out-of-an-existing-command-line-executable)

b  
[Out of an existing CGI executable](#out-of-an-existing-cgi-executable)

c  
[Out of an existing CGI code](#out-of-an-existing-cgi-code) (if it's written using the [NCBI C++ CGI framework](ch_cgi.html))

d  
[Create a GRID Worker Node from scratch](#create-a-grid-worker-node-from-scratch)

4  
Client-side API

5  
Remote CGI -- enables moving the actual CGI execution to the grid.

6  
[GRID Utilities](#grid-utilities) for remote administration, monitoring, retrieval and submission (<span class="nctnt ncbi-app">netschedule\_control</span>, <span class="nctnt ncbi-app">netcache\_control</span>, <span class="nctnt ncbi-app">ns\_remote\_job\_control</span>, <span class="nctnt ncbi-app">ns\_submit\_remote\_job</span>, etc.)

All these components are fully portable, in the sense that they can be built and then run and communicate with each other across all platforms that are supported by the NCBI C++ Toolkit (Unix, MS-Windows, MacOSX).

The NetCache and NetSchedule components can be used independently of each other and the rest of the grid framework - they have their respective client APIs. Worker Nodes get their tasks from NetSchedule, and may also use NetCache to get the data related to the tasks and to store the results of computation. Remote-CGI allows one to easily convert an existing CGI into a back-end worker node -- by a minor, 1 line of source code, modification. It can solve the infamous "30-sec CGI timeout" problem.

All these components can be load-balanced and are highly scalable. For example, one can just setup 10 NetCache servers or 20 Worker Nodes on new machines, and the storage/computation throughput would increase linearly. Also, NetCache and NetSchedule are lightning-fast.

To provide more flexibility, load balancing, and fault-tolerance, it is highly advisable to pool NetSchedule and NetCache servers using [NCBI Load Balancer and Service Mapper](ch_app.html#ch_app.Load_Balancing_Servi) (LBSM).

### Architecture and Data Flow

[NetSchedule](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/NetSchedule) and [NetCache](ch_app.html#ch_app.ncbi_netcache_service) servers create a media which Submitters and [Worker Nodes](#worker-nodes) use to pass and control jobs and related data:

1  
Submitter prepares input data and stores it in the pool of NetCache servers, recording keys to the data in the job's description.

2  
Submitter submits the job to the appropriate queue in the pool of NetSchedule servers.

3  
Worker Node polls "its" queue on the NetSchedule servers for jobs, and takes the submitted job for processing.

4  
Worker Node retrieves the job's input data from the NetCache server(s) and processes the job.

5  
Worker Node stores the job's results in NetCache and changes the job's status to "<span class="nctnt ncbi-monospace">done</span>" in NetSchedule.

6  
Submitter sees that the job is done and reads its result from NetCache.

The following diagram illustrates this flow of control and data:

<span>[![Image grid-collab.png](/book-test/static/img/grid-collab.png)](/book-test/static/img/grid-collab.png "Click to see the full-resolution image")</span>

### The GRID Farm

To help developers jump-start their distributed computation projects, there is a small farm of machines for general use, running:

-   Several flavors of job queues

-   Several flavors of network data storage

-   A framework to run and maintain users' [Worker Nodes](#worker-nodes)

<span class="nctnt highlight">NOTE:</span> Most of the GRID components can be deployed or used outside of the GRID framework (applications can communicate with the components directly via the components' own client APIs). However, in many cases it is beneficial to use the whole GRID framework from the start.

NCBI users can find more information on the [GRID farm Wiki page](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/GRID_Farm).

Worker Nodes
------------

The following sections describe how to create, configure and run worker nodes:

-   [Create a GRID Worker Node from scratch](#create-a-grid-worker-node-from-scratch)

-   [Converting an existing CGI application into a GRID Node](#converting-an-existing-cgi-application-into-a-grid-node)

-   [Wrapping an existing CGI application into a GRID Node](#wrapping-an-existing-cgi-application-into-a-grid-node)

-   [Wrapping an existing command-line application into a GRID Node](#wrapping-an-existing-command-line-application-into-a-grid-node)

-   [Worker Node Cleanup Procedure](#worker-node-cleanup-procedure)

### Create a GRID Worker Node from scratch

The following sections describe how to Create a GRID Worker Node from scratch:

-   [Purpose](#purpose)

-   [Diagram](#diagram)

#### Purpose

Framework to create a multithreaded server that can run on a number of machines and serve the requests using [NetSchedule](http://intranet.ncbi.nlm.nih.gov:6224/wiki-private/CxxToolkit/index.cgi/NetSchedule) and [NetCache](ch_app.html#ch_app.ncbi_netcache_service) services to exchange the job info and data.

#### Diagram

<ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/DOC/PPT/IMAGES/GRID_Dec14_2006/Slide3.PNG>

### Converting an existing CGI application into a GRID Node

The following sections describe how to convert an existing CGI application into a GRID node:

-   [Purpose](#purpose)

-   [Converting a CGI into a Remote-CGI server](#converting-a-cgi-into-a-remote-cgi-server)

-   [Diagram](#diagram)

-   [Features and benefits](#features-and-benefits)

#### Purpose

With a rather simple and formal conversion, a CGI's real workload can be moved from the Web servers to any other machines. It also helps to work around the infamous "30-sec Web timeout problem".

#### Converting a CGI into a Remote-CGI server

1  
Modify the code of your original CGI to make it a standalone Remote-CGI server ([Worker Node](#worker-node)). The code conversion is very easy and formal:

a  
Change application's base class from <span class="nctnt ncbi-class">CCgiApplication</span> to <span class="nctnt ncbi-class">CRemoteCgiApp</span>

b  
Link the application with the library <span class="nctnt ncbi-lib">xgridcgi</span> rather than with <span class="nctnt ncbi-lib">xcgi</span>

2  
Replace your original CGIs by a one-line shell scripts that calls "remote CGI gateway" (<span class="nctnt ncbi-app">cgi2rcgi.cgi</span>) application.

3  
Match "remote CGI gateways" against Remote-CGI servers:

a  
Ask us to register your remote CGI in the GRID framework

b  
Define some extra parameters in the configuration files of "remote CGI gateway" and Remote-CGI servers to connect them via the GRID framework

4  
Install and run your Remote-CGI servers on as many machines as you need. They don't require Web server, and can be installed even on PCs and Macs.

#### Diagram

<ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/DOC/PPT/IMAGES/GRID_Dec14_2006/Slide1.PNG>

#### Features and benefits

-   Solves 30-sec Web server timeout problem.

-   Provides software infrastructure for back-end computation farm for CGIs. Cross-platform, Unix-Windows compatible, minimal administration.

-   Existing CGIs can be easily converted into back-end worker nodes.

-   While the request is being executed by the Remote-CGI server, the user can be interactively provided with a standard or customized progress report.

-   Can be used for parallel network programming.

-   High availability infrastructure. All central components can have 2-3 times reservation to accommodate request peak hours and possible hardware failures.

-   Remote-CGI servers are extremely mobile.

-   Remote-CGI servers can be administered (gentle shutdown, request statistics, etc.) using special tool.

-   Easy to debug, as the Remote-CGI server can be run under debugger or any memory checker on any machine (Unix or MS-Windows)

### Wrapping an existing CGI application into a GRID Node

The following sections describe how to wrap an existing CGI application into a GRID Node:

-   [Running existing CGI executable through Grid Framework](#running-existing-cgi-executable-through-grid-framework)

-   [Diagram](#diagram)

#### Running existing CGI executable through Grid Framework

In this case a real CGI does not need to be modified at all and <span class="nctnt ncbi-app">remote\_cgi</span> utility serves as an intermediate between NetSchedule service and a real CGI. The real CGI and <span class="nctnt ncbi-app">remote\_cgi</span> utility go to the server side. The <span class="nctnt ncbi-app">remote\_cgi</span> gets a job from NetSchedule service, deserializes the CGI request and <span class="nctnt ncbi-var">stdin</span> stream and runs the real CGI with this data. When the CGI finishes the <span class="nctnt ncbi-app">remote\_cgi</span> utility serializes its <span class="nctnt ncbi-var">stdout</span> stream and sends it back to the client.

On the client side (front-end) <span class="nctnt ncbi-app">cgi2rcgi</span> sees that the job’s status is changed to “done” gets the data sent by the server side (back-end), deserializes it and writes it on its <span class="nctnt ncbi-var">stdout</span>.

<span class="nctnt ncbi-app">cgi2rcgi</span> utility has two html template files to define its look. The first file is [cgi2rcgi.html](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/app/grid/cgi2rcgi/cgi2rcgi.html) (can be redefined in [cgi2rcgi.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/app/grid/cgi2rcgi/cgi2rcgi.ini) file) which is the main html template file and it contains all common html tags for the particular application. It also has to have two required tags.

<span class="nctnt ncbi-monospace">\<@REDIRECT@\></span> should be inside <span class="nctnt ncbi-monospace">\<head\></span> tag and is used to inject a page reloading code.

<span class="nctnt ncbi-monospace">\<@VIEW@\></span> should be inside <span class="nctnt ncbi-monospace">\<body\></span> tag and is to render information about a particular job’s status.

The second file is cgi2rcgi.inc.html (can be redefined in cgi2.rcgi.ini) which defines tags for particular job’s states. The tag for the particular job’s state replaces <span class="nctnt ncbi-monospace">\<@VIEW@\></span> tag in the main html template file.

#### Diagram

<ftp://ftp.ncbi.nlm.nih.gov/toolbox/ncbi_tools++/DOC/PPT/IMAGES/GRID_Dec14_2006/Slide1.PNG>

### Wrapping an existing command-line application into a GRID Node

The following sections describe how to wrap an existing CGI application into a GRID Node:

-   [Running arbitrary applications through Grid Framework](#running-arbitrary-applications-through-grid-framework)

-   [Diagram](#diagram)

#### Running arbitrary applications through Grid Framework

The client side collects a command line, a <span class="nctnt ncbi-var">stdin</span> stream and some other parameters, serialize them and through Grid Framework to the server side. On the server side <span class="nctnt ncbi-app">remote\_app</span> utility picks up submitted job, deserializes the command line, the <span class="nctnt ncbi-var">stdin</span> and other parameters, and starts a new process with the application and the input data. Then <span class="nctnt ncbi-app">remote\_app</span> waits for the process to finish collecting its <span class="nctnt ncbi-var">stdout</span>, <span class="nctnt ncbi-var">stdin</span> and <span class="nctnt ncbi-var">errcode</span>. After that it serializes collected data and sends it back to the client side. The application for run is set in [remote\_app.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/app/grid/remote_app/remote_app.ini) configuration file.

**Source code:** [src/app/grid/remote\_app/remote\_app\_wn.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/app/grid/remote_app/remote_app_wn.cpp)

**Config file:** [remote\_app.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/app/grid/remote_app/remote_app.ini)

Classes that should be used to prepare an input data a remote application and get its results are [CRemoteAppRequest](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CRemoteAppRequest&d=) and [CRemoteAppResult](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CRemoteAppResult&d=). See also [CGridClient](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCGridClient.html), [CGridClientApp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCGridClientApp.html).

**Client example:** [src/sample/app/netschedule/remote\_app\_client\_sample.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/netschedule/remote_app_client_sample.cpp)

**Config file:** [src/sample/app/netschedule/remote\_app\_client\_sample.ini](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/netschedule/remote_app_client_sample.ini)

<span class="nctnt ncbi-app">ns\_submit\_remote\_job</span> utility allows submitting a job for a remote application from its command line or a jobs file. See <span class="nctnt ncbi-app">ns\_submit\_remote\_job –help</span>.

**Jobs file format:**

Each line in the file represents one job (lines starting with ‘<span class="nctnt ncbi-monospace">\#</span>’ are ignored). Each job consists of several parameters. Each parameter has in the form: <span class="nctnt ncbi-monospace">name="value"</span>. The parameter’s value must be wrapped in double quotes. All of these parameters are optional. Supported parameters:

-   <span class="nctnt ncbi-monospace">args</span> – command line arguments.

-   <span class="nctnt ncbi-monospace">aff</span> – affinity token.

-   <span class="nctnt ncbi-monospace">tfiles</span> – a list of semicolon-separated file names which will be transferred to the server side.

-   <span class="nctnt ncbi-monospace">jout</span> – a file name where the application’s output to <span class="nctnt ncbi-var">stdout</span> will be stored.

-   <span class="nctnt ncbi-monospace">jerr</span> – a file name where the application’s output to <span class="nctnt ncbi-var">stderr</span> will be stored.

-   <span class="nctnt ncbi-monospace">runtime</span> – a time in seconds of the remote application’s running time. If the application is running longer then this time it is assumed to be failed and its execution is terminated.

-   <span class="nctnt ncbi-monospace">exclusive</span> – instructs the <span class="nctnt ncbi-app">remote\_app</span> to not get any other jobs from the NetSchedule service while this job is being executed.

#### Diagram

<ftp://ftp.ncbi.nih.gov/toolbox/ncbi_tools++/DOC/PPT/IMAGES/GRID_Dec14_2006/Slide2.PNG>

### Worker Node Cleanup Procedure

The following sections describe the procedure for cleaning up Worker Nodes:

-   [Purpose](#purpose)

-   [Job Cleanup](#job-cleanup)

-   [Worker Node Cleanup](#worker-node-cleanup)

#### Purpose

It is necessary to provide a framework to support worker node and job cleanup. For example, a job may create temporary files that need to be deleted, or a worker node may need to clean up resources shared by multiple jobs.

To receive cleanup events, the worker node must implement interface <span class="nctnt ncbi-class">IWorkerNodeCleanupEventListener</span>. The interface has a single abstract method:

<span class="nctnt ncbi-code">void HandleEvent(EWorkerNodeCleanupEvent cleanup\_event)</span>

At the time of the call, <span class="nctnt ncbi-var">cleanup\_event</span> will be set to either <span class="nctnt ncbi-var">eRegularCleanup</span> (for normal cleanup) or <span class="nctnt ncbi-var">eOnHardExit</span> (for an emergency shutdown).

There are two types of listeners: those called after each job is done and those called when the worker node is shutting down.

#### Job Cleanup

Listeners of the first type (per-job cleanup) are installed in the <span class="nctnt ncbi-code">Do()</span> method via a call to <span class="nctnt ncbi-code">CWorkerNodeJobContext::GetCleanupEventSource()-\>AddListener()</span>:

    class CMyWorkerNodeJob : public IWorkerNodeJob
    /* ... */
    virtual int Do(CWorkerNodeJobContext& context)
    {
        context.GetCleanupEventSource()->AddListener( new CMyWorkerNodeJobCleanupListener(resources_to_free));
    }

#### Worker Node Cleanup

Listeners of the second type (worker node cleanup) are installed in the constructor of the <span class="nctnt ncbi-class">IWorkerNodeJob</span>-derived class via a call to <span class="nctnt ncbi-code">IWorkerNodeInitContext::GetCleanupEventSource()-\>AddListener()</span>:

    class CMyWorkerNodeJob : public IWorkerNodeJob
    /* ... */
    CMyWorkerNodeJob(const IWorkerNodeInitContext& context)
    {
        context.GetCleanupEventSource()->AddListener( new CMyWorkerNodeCleanupListener(resources_to_free));
    }

Note that depending on the current value of the <span class="nctnt ncbi-monospace">[server]/reuse\_job\_object</span> configuration parameter, this constructor of <span class="nctnt ncbi-class">CMyWorkerNodeJob</span> can be called multiple times - either once per job or once per worker thread, so additional guarding may be required.

The approach of doing worker node cleanup described above is a newer approach, but there is an older approach which may also be used:

The <span class="nctnt ncbi-class">IGridWorkerNodeApp\_Listener</span> interface has two methods, <span class="nctnt ncbi-code">OnGridWorkerStart()</span> and <span class="nctnt ncbi-code">OnGridWorkerStop()</span> which are called during worker node initialization and shutdown respectively. A handler implementing this interface can be installed using the <span class="nctnt ncbi-code">SetListener()</span> method of <span class="nctnt ncbi-class">CGridWorkerApp</span>. The code that calls the <span class="nctnt ncbi-code">OnGridWorkerStop()</span> method will run in the context of the dedicated cleanup thread and also respect the <span class="nctnt ncbi-var">force\_close</span> parameter.

The older method does not require the guarding that the new method requires.

Job Submitters
--------------

An API is available to submit tasks to [Worker Nodes](#worker-nodes), and to monitor and control the submitted tasks.

Implementing a Network Server
-----------------------------

The [CServer](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCServer.html), [IServer\_ConnectionFactory](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIServer__ConnectionFactory.html), and [IServer\_ConnectionHandler](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classIServer__ConnectionHandler.html) classes provide a framework for creating multithreaded network servers with one-thread-per-request scheduling. The server creates a pool of connection handlers for maintaining the socket connections, and a pool of threads for handling the socket events. With each socket event, <span class="nctnt ncbi-class">CServer</span> allocates a thread from the thread pool to handle the event, thereby making it possible to serve a large number of concurrent requests efficiently.

The following topics discuss the various aspects of implementing a network server:

-   [Typical Client-Server Interactions](#typical-client-server-interactions)

    -   [Protocols](#protocols)

    -   [Request Format](#request-format)

    -   [Response Handling](#response-handling)

-   [The CServer Framework Classes](#the-cserver-framework-classes)

    -   [CServer](#cserver)

    -   [IServer\_ConnectionFactory](#iserverconnectionfactory)

    -   [IServer\_ConnectionHandler](#iserverconnectionhandler)

-   [State, Events, and Flow of Control](#state-events-and-flow-of-control)

-   [Socket Closure and Lifetime](#socket-closure-and-lifetime)

-   [Diagnostics](#diagnostics)

-   [Handling Exceptions](#handling-exceptions)

-   [Server Configuration](#server-configuration)

-   [Other Resources](#other-resources)

### Typical Client-Server Interactions

The <span class="nctnt ncbi-class">CServer</span> framework is based on sockets and imposes few constraints on client-server interactions. Servers can support many concurrent connections, and the client and server can follow any protocol, provided that they handle errors. If the protocol includes a server response, then the client and server should alternate between requests and responses on a given connection.

Typical client-server interactions differ in the following categories:

-   [Protocols](#protocols)

-   [Request Format](#request-format)

-   [Response Handling](#response-handling)

#### Protocols

The simplest protocol is probably a consistent pattern of a client request followed by a server response. The [Track Manager](http://mini.ncbi.nlm.nih.gov/1k2qd) server uses this protocol.

The [NetScheduler](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/src/app/netschedule/) server follows a modified request / response protocol. It expects three "messages" - two information lines followed by a command line - then it returns a response.

The [Genome Pipeline](http://mini.ncbi.nlm.nih.gov/1k2qn) server protocol adds a client acknowledgment to the interaction. A missing or corrupt acknowledgment triggers a rollback of the transaction.

Your server can follow whatever pattern of requests and responses is appropriate for the service. Note that a given server is not limited to a fixed communication pattern. As long as the client and server follow the same rules, the protocol can be adapted to whatever is appropriate at the moment.

#### Request Format

At a low level, the server simply receives bytes through a socket, so it must parse the input data into separate requests.

Perhaps the easiest request format to parse simply delimits requests with newlines - this is the request format used by the [NetScheduler](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/src/app/netschedule/) server.

A more robust way to define the request and response formats is with an ASN.1 specification. NCBI servers that use an ASN.1-defined request format include:

-   [Ideogram](http://mini.ncbi.nlm.nih.gov/1k2qe)

-   [OS Gateway](http://mini.ncbi.nlm.nih.gov/1k2qo)

-   [Track Manager](http://mini.ncbi.nlm.nih.gov/1k2qd)

-   [Genome Pipeline](http://mini.ncbi.nlm.nih.gov/1k2qn)

#### Response Handling

Servers may be implemented to respond immediately (i.e. in the same thread execution where the request is read), or to delay their responses until the socket indicates that the client is ready to receive. Responding immediately can make the code simpler, but may not be optimal for resource scheduling.

NCBI Servers that use respond immediately include:

-   [NetScheduler](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/src/app/netschedule/)

-   [Ideogram](http://mini.ncbi.nlm.nih.gov/1k2qe)

NCBI servers that delay their response include:

-   [OS Gateway](http://mini.ncbi.nlm.nih.gov/1k2qo)

-   [Track Manager](http://mini.ncbi.nlm.nih.gov/1k2qd)

-   [Genome Pipeline](http://mini.ncbi.nlm.nih.gov/1k2qn)

### The CServer Framework Classes

The main classes in the <span class="nctnt ncbi-class">CServer</span> framework are:

-   [CServer](#cserver)

-   [IServer\_ConnectionFactory](#iserverconnectionfactory)

-   [IServer\_ConnectionHandler](#iserverconnectionhandler)

#### CServer

The <span class="nctnt ncbi-class">CServer</span> class manages connections, socket event handling for reading and writing, timer and timeout events, and error conditions. <span class="nctnt ncbi-class">CServer</span> creates a connection pool and a thread pool. When a client request arrives, a socket is established and assigned to one of the connection handler objects. For each socket event (e.g. connection opened, data arrival, client ready for data, etc.), a thread is allocated from the pool to serve that particular event and is returned to the pool when the handler finishes. You can use <span class="nctnt ncbi-class">CServer</span> directly, but typically it is subclassed.

If you want to provide a gentle shutdown ability, then create a <span class="nctnt ncbi-class">CServer</span> subclass and override <span class="nctnt ncbi-func">ShutdownRequested()</span>. It should return true when the application-specific logic determines that the server is no longer needed - for example, if a shutdown command has been received; if a timeout has expired with no client communication; if a watchfile has been updated; etc. A typical subclass could also include a <span class="nctnt ncbi-func">RequestShutdown()</span> method that sets a flag that is in turn checked by <span class="nctnt ncbi-func">ShutdownRequested()</span>. This approach makes it easy to trigger a shutdown from a client.

If you want to process data in the main thread on timeout, then create a <span class="nctnt ncbi-class">CServer</span> subclass, override <span class="nctnt ncbi-func">ProcessTimeout()</span>, and use <span class="nctnt ncbi-func">GetParameters()</span> / <span class="nctnt ncbi-func">SetParameters()</span> to replace the <span class="nctnt ncbi-var">accept\_timeout</span> parameter with the proper value for your application.

If you don't want to provide a gentle shutdown ability and you don't want to process data in the main thread on timeout, then you can use <span class="nctnt ncbi-class">CServer</span> directly.

Your server application will probably define, configure, start listening, and run a <span class="nctnt ncbi-class">CServer</span> object in its <span class="nctnt ncbi-func">Run()</span> method - something like:

    CMyServer server;
    server.SetParameters(params);
    server.AddListener(new CMyConnFactory(&server), params.port);
    server.Run();

#### IServer\_ConnectionFactory

The connection factory simply creates connection handler objects. It is registered with the server and is called when building the connection pool.

It is possible to create a server application without defining your own connection factory (the <span class="nctnt ncbi-class">CServer</span> framework has a default factory). However you must create a connection factory if you want to pass server-wide parameters to your connection handler objects - for example to implement a gentle shutdown.

The connection factory class can be as simple as:

    class CMyConnFactory : public IServer_ConnectionFactory
    {
    public:
        CMyConnFactory(CMyServer * server)
        : m_Server(server) {}
        ~CMyConnFactory(void) {}
        virtual IServer_ConnectionHandler * Create(void)
        {
            return new CMyConnHandler(m_Server);
        }
    private:
        CMyServer * m_Server;
    };

#### IServer\_ConnectionHandler

Classes derived from <span class="nctnt ncbi-class">IServer\_ConnectionHandler</span> do the actual work of handling requests. The primary methods are:

-   <span class="nctnt ncbi-func">GetEventsToPollFor()</span> - This method identifies the socket events that should be handled by this connection, and can establish a timer.

-   <span class="nctnt ncbi-func">OnOpen()</span> - Indicates that a client has opened a connection. The socket can be configured here.

-   <span class="nctnt ncbi-func">OnClose()</span> - Indicates that a connection was closed. Note that connections can be closed by either the server or the client (the closer is indicated by a parameter).

-   <span class="nctnt ncbi-func">OnRead()</span> - Indicates that a client has sent data. This is where you should parse the data coming from the socket.

-   <span class="nctnt ncbi-func">OnWrite()</span> - Indicates that a client is ready to receive data. This is where you should write the response to the socket.

-   <span class="nctnt ncbi-func">OnTimeout()</span> - Indicates that a client has been idle for too long. The connection will be closed synchronously after this method is called.

-   <span class="nctnt ncbi-func">OnTimer()</span> - Called when the timer established by <span class="nctnt ncbi-func">GetEventsToPollFor()</span> has expired.

-   <span class="nctnt ncbi-func">OnOverflow()</span> - Called when there's a problem with the connection - for example, the connection pool cannot accommodate another connection. <span class="nctnt highlight">Note:</span> The connection is destroyed after this call.

The <span class="nctnt ncbi-func">OnOpen()</span>, <span class="nctnt ncbi-func">OnRead()</span>, and <span class="nctnt ncbi-func">OnWrite()</span> methods are pure virtual and must be implemented by your server.

<span class="nctnt highlight">Note:</span> If your client-server protocol is line-oriented, you can use <span class="nctnt ncbi-class">IServer\_LineMessageHandler</span> instead of <span class="nctnt ncbi-class">IServer\_ConnectionHandler</span>. In this case you would implement the <span class="nctnt ncbi-func">OnMessage()</span> method instead of <span class="nctnt ncbi-func">OnRead()</span>.

### State, Events, and Flow of Control

Generally, your connection handler class should follow a state model and implement the <span class="nctnt ncbi-func">GetEventsToPollFor()</span> method, which will use the state to select the events that will be handled. This is typically how the connection state is propagated and how socket events result in the flow of control being passed to the events handlers.

<span class="nctnt highlight">Note:</span> You don't need to implement a state model or the <span class="nctnt ncbi-func">GetEventsToPollFor()</span> method if you immediately write any reponses in the same handler that performs the reading. For line-oriented protocols, your connection handler can inherit from <span class="nctnt ncbi-class">IServer\_LineMessageHandler</span> instead of from <span class="nctnt ncbi-class">IServer\_ConnectionHandler</span>. <span class="nctnt ncbi-class">IServer\_LineMessageHandler</span> implements <span class="nctnt ncbi-func">OnRead()</span> and parses the input into lines, calling <span class="nctnt ncbi-func">OnMessage()</span> for each line. In this case you would both read from and write to the client in the <span class="nctnt ncbi-func">OnMessage()</span> method (and implement a dummy <span class="nctnt ncbi-func">OnWrite()</span> method).

For servers that implement a state model and follow a simple request / response protocol, the state variable should be initialized to "reading"; set to "writing" after the request is read in the <span class="nctnt ncbi-func">OnRead()</span> method; and set to "reading" after the response is sent in the <span class="nctnt ncbi-func">OnWrite()</span> method. This results in an orderly alternation between reading and writing. The <span class="nctnt ncbi-func">GetEventsToPollFor()</span> method uses the current connection state (the current state corresponds to the next expected event) to select the appropriate event to respond to. For example:

    EIO_Event CMyConnHandler::GetEventsToPollFor(const CTime** alarm_time)
    {
        return (m_State == eWriting) ? eIO_Write : eIO_Read;
    }

Your state model should reflect the communication protocol and can be more complex than a simple read / write alternation. It could include acknowledgements, queuing, timed responses, etc. Of course it should include error handling.

<span class="nctnt ncbi-func">GetEventsToPollFor()</span> is guaranteed to not be called at the same time as the event handling functions (<span class="nctnt ncbi-func">OnOpen()</span>, <span class="nctnt ncbi-func">OnRead()</span>, etc.), so you shouldn't guard the variables they use with mutexes.

<span class="nctnt ncbi-func">GetEventsToPollFor()</span> is called from the main thread while the other socket event handling methods are called from various threads allocated from the thread pool.

### Socket Closure and Lifetime

Nominally, sockets are owned by (and therefore closed by) the <span class="nctnt ncbi-class">CServer</span> framework. However, there may be cases where your derived class will need to manually close or take ownership of the socket.

-   Well-behaved clients will close a connection when they have no more outstanding requests and have completed reading the responses to all requests made on the connection. <span class="nctnt ncbi-class">CServer</span>-based applications are intended to operate in this paradigm. In this case you don't need to close or take ownership of the socket.<br/><br/><span class="nctnt highlight">Note:</span> If connections are not closed by the client after reading the response, then you may run out of file descriptors and/or available port numbers. If you have a high connection volume and try to mitigate slow connection closings by closing connections in your code, you run the risk of terminating the connection before the client has read all the data. Well-behaved clients are therefore necessary for optimum server performance.

-   <span class="nctnt ncbi-class">CServer</span> will automatically close a connection after an inactivity timeout or if an exception occurs in an event handler. You don't need to manage sockets in these cases.

-   If you encounter problems such as a broken protocol or an invalid command then you should close the connection from your code.

-   If you need to close a connection from your code, you should do so by calling <span class="nctnt ncbi-func">CServer::CloseConnection()</span> - not by explicitly closing the socket object. The <span class="nctnt ncbi-class">CServer</span> framework generally owns the socket and therefore needs to manage it.

-   <span class="nctnt highlight">Note:</span> There is one case when the <span class="nctnt ncbi-class">CServer</span> framework shouldn't own the socket. If you create a <span class="nctnt ncbi-class">CConn\_SocketStream</span> on an existing socket, then you must take ownership as shown here:<br/><br/><span class="nctnt ncbi-code">SOCK sk = GetSocket().GetSOCK();</span><br/><span class="nctnt ncbi-code">GetSocket().SetOwnership(eNoOwnership);</span><br/><span class="nctnt ncbi-code">GetSocket().Reset(0, eTakeOwnership, eCopyTimeoutsToSOCK);</span><br/><span class="nctnt ncbi-code">AutoPtr\<CConn\_SocketStream\> stream = new CConn\_SocketStream(sk);</span><br/>

### Diagnostics

To facilitate logfile analysis, the more detailed "new" log posting format is recommended. To enable the new format, call <span class="nctnt ncbi-func">SetOldPostFormat()</span> before calling <span class="nctnt ncbi-func">AppMain()</span>:

    int main(int argc, const char* argv[])
    {
        GetDiagContext().SetOldPostFormat(false);
        return CMyServerApp().AppMain(argc, argv);
    }

Grouping diagnostics into request-specific blocks is very helpful for post-processing. To facilitate this, [CDiagContext](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCDiagContext.html) provides the <span class="nctnt ncbi-func">PrintRequestStart()</span>, <span class="nctnt ncbi-func">PrintRequestStop()</span>, <span class="nctnt ncbi-func">Extra()</span>, and various <span class="nctnt ncbi-func">Print()</span>, methods.

The <span class="nctnt ncbi-func">CDiagContext::SetRequestContext()</span> method enables you to use a [CRequestContext](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCRequestContext.html) object to pass certain request-specific information - such as request ID, client IP, bytes sent, request status, etc. - to the diagnostics context. The request context information can be invaluable when analyzing logs.

[CRequestContext](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCRequestContext.html) objects are merely convenient packages for passing information - they can be preserved across multiple events or re-created as needed. However, as <span class="nctnt ncbi-class">CObject</span>-derived objects, they should be wrapped by <span class="nctnt ncbi-class">CRef</span> to avoid inadvertent deletion by code accepting a <span class="nctnt ncbi-class">CRef</span> parameter.

The following code fragments show examples of API calls for creating request-specific blocks in the logfile. Your code will be slightly different and may make these calls in different event handlers (for example, you might call <span class="nctnt ncbi-func">PrintRequestStart()</span> in <span class="nctnt ncbi-func">OnRead()</span> and <span class="nctnt ncbi-func">PrintRequestStop()</span> in <span class="nctnt ncbi-func">OnWrite()</span>).

    // Set up the request context:
    CRef<CRequestContext> rqst_ctx(new CRequestContext());
    rqst_ctx->SetRequestID();
    rqst_ctx->SetClientIP(socket.GetPeerAddress(eSAF_IP));

    // Access the diagnostics context:
    CDiagContext & diag_ctx(GetDiagContext());
    diag_ctx.SetRequestContext(rqst_ctx.GetPointer());

    // Start the request block in the log:
    diag_ctx.PrintRequestStart()
            .Print("peer", "1.2.3.4")
            .Print("port", 5555);

    // Other relevant info...
    CDiagContext_Extra extra(diag_ctx.Extra());
    extra.Print("name1", "value1")
         .Print("name2", "value2");

    // Terminate the request block in the log.
    rqst_ctx->SetBytesRd(socket.GetCount(eIO_Read));
    rqst_ctx->SetBytesWr(socket.GetCount(eIO_Write));
    rqst_ctx->SetRequestStatus(eStatus_OK);
    diag_ctx.PrintRequestStop();

Code like the above will result in [AppLog](http://mini.ncbi.nlm.nih.gov/1k2vj) entries that look similar to:

<span>[![Image ch\_grid\_cserver\_applog.png](/book-test/static/img/ch_grid_cserver_applog.png)](/book-test/static/img/ch_grid_cserver_applog.png "Click to see the full-resolution image")</span>

Each thread has its own diagnostics context. Therefore, simultaneous calls to <span class="nctnt ncbi-func">GetDiagContext().SetRequestContext()</span> in multiple event handlers will not interfere with each other.

The connection handler should ensure that each request-start has a corresponding request-stop - for example by writing the request-stop in a destructor if it wasn't already written.

### Handling Exceptions

There are server application-wide configuration parameters to control whether or not otherwise-unhandled exceptions will be caught by the server. See the [Server Configuration](#server-configuration) section for details.

<span class="nctnt highlight">Note:</span> If your event handler catches an exception, it does **not** need to close the connection because <span class="nctnt ncbi-class">CServer</span> automatically closes connections in this case.

See the [Socket Closure and Lifetime](#socket-closure-and-lifetime) section for related information.

### Server Configuration

The following configuration parameters can be used to fine-tune <span class="nctnt ncbi-class">CServer</span>-derived server behavior:

| Parameter                                                                                      | Brief Description                                                                                        | Default |
|------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------|---------|
| <span class="nctnt ncbi-var">CSERVER\_CATCH\_UNHANDLED\_EXCEPTIONS</span>                      | Controls whether <span class="nctnt ncbi-class">CServer</span> should catch exceptions.                  | true    |
| <span class="nctnt ncbi-var">NCBI\_CONFIG\_\_THREADPOOL\_\_CATCH\_UNHANDLED\_EXCEPTIONS</span> | Controls whether <span class="nctnt ncbi-class">CThreadInPool\_ForServer</span> should catch exceptions. | true    |

See the [connection library configuration reference](ch_libconfig.html#ch_libconfig.libconfig_connect) for more information on configuration parameters.

### Other Resources

Here are some places to look for reference and to see how to <span class="nctnt ncbi-class">CServer</span> is used in practice:

-   [CServer Class Reference](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCServer.html)

-   [CServer test application](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/src/connect/test/test_server.cpp)

-   [NetScheduler](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/src/app/netschedule/)

-   [Ideogram](http://mini.ncbi.nlm.nih.gov/1k2qe) (NCBI only)

-   [OS Gateway](http://mini.ncbi.nlm.nih.gov/1k2qo) (NCBI only)

-   [Track Manager](http://mini.ncbi.nlm.nih.gov/1k2qd) (NCBI only)

-   [Genome Pipeline](http://mini.ncbi.nlm.nih.gov/1k2qn) (NCBI only)

GRID Utilities
--------------

The following sections describe the GRID Utilities:

-   [netschedule\_control](#netschedulecontrol)

-   [ns\_remote\_job\_control](#nsremotejobcontrol)

-   [Alternate list input and output](#alternate-list-input-and-output)

### netschedule\_control

**DESCRIPTION:**

NCBI NetSchedule control utility. This program can be used to operate NetSchedule servers and server groups from the command line.

**OPTIONS:**

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">-h</td>
<td align="left">Print brief usage and description; ignore other arguments.</td>
</tr>
<tr class="even">
<td align="left">-help</td>
<td align="left">Print long usage and description; ignore other arguments.</td>
</tr>
<tr class="odd">
<td align="left">-xmlhelp</td>
<td align="left">Print long usage and description in XML format; ignore other arguments.</td>
</tr>
<tr class="even">
<td align="left">-version-full</td>
<td align="left">Print extended version data; ignore other arguments.</td>
</tr>
<tr class="odd">
<td align="left">-service &lt;SERVICE_NAME&gt;</td>
<td align="left">Specifies a NetSchedule service name to use. It can be either an LBSMD service name or a server name / port number pair separated by a colon, such as: <span class="nctnt ncbi-monospace">host:1234</span></td>
</tr>
<tr class="even">
<td align="left">-queue &lt;QUEUE_NAME&gt;</td>
<td align="left">The queue name to operate with.</td>
</tr>
<tr class="odd">
<td align="left">-jid &lt;JOB_ID&gt;</td>
<td align="left">This option specifies a job ID for those operations that need it.</td>
</tr>
<tr class="even">
<td align="left">-shutdown</td>
<td align="left">This command tells the specified server to shut down. The server address is defined by the <span class="nctnt ncbi-monospace">-service</span> option. An LBSMD service name cannot be used with <span class="nctnt ncbi-monospace">-shutdown</span>.</td>
</tr>
<tr class="odd">
<td align="left">-shutdown_now</td>
<td align="left">The same as <span class="nctnt ncbi-monospace">-shutdown</span> but does not wait for job termination.</td>
</tr>
<tr class="even">
<td align="left">-log &lt;ON_OFF&gt;</td>
<td align="left">Switch server side logging on and off.</td>
</tr>
<tr class="odd">
<td align="left">-monitor</td>
<td align="left">Starts monitoring of the specified queue. Events associated with that queue will be dumped to the standard output of <span class="nctnt ncbi-monospace">netschedule_control</span> until it's terminated with <span class="nctnt ncbi-monospace">Ctrl-C</span>.</td>
</tr>
<tr class="even">
<td align="left">-ver</td>
<td align="left">Prints server version(s) of the server or the group of servers specified by the <span class="nctnt ncbi-monospace">-service</span> option.</td>
</tr>
<tr class="odd">
<td align="left">-reconf</td>
<td align="left">Send a request to reload server configuration.</td>
</tr>
<tr class="even">
<td align="left">-qlist</td>
<td align="left">List available queues.</td>
</tr>
<tr class="odd">
<td align="left">-qcreate</td>
<td align="left">Create queue (qclass should be present, and comment is an optional parameter).</td>
</tr>
<tr class="even">
<td align="left">-qclass &lt;QUEUE_CLASS&gt;</td>
<td align="left">Class for queue creation.</td>
</tr>
<tr class="odd">
<td align="left">-comment &lt;COMMENT&gt;</td>
<td align="left">Optional parameter for the <span class="nctnt ncbi-monospace">-qcreate</span> command</td>
</tr>
<tr class="even">
<td align="left">-qdelete</td>
<td align="left">Delete the specified queue.</td>
</tr>
<tr class="odd">
<td align="left">-drop</td>
<td align="left">Unconditionally drop ALL jobs in the specified queue.</td>
</tr>
<tr class="even">
<td align="left">-stat &lt;STAT_TYPE&gt;</td>
<td align="left">Print queue statistics. Available values for <span class="nctnt ncbi-monospace">STAT_TYPE</span>: <span class="nctnt ncbi-monospace">all</span>, <span class="nctnt ncbi-monospace">brief</span>.</td>
</tr>
<tr class="odd">
<td align="left">-affstat &lt;AFFINITY_NAME&gt;</td>
<td align="left">Print queue statistics summary based on affinity.</td>
</tr>
<tr class="even">
<td align="left">-dump</td>
<td align="left">Print queue dump or job dump if <span class="nctnt ncbi-monospace">-jid</span> parameter is specified.</td>
</tr>
<tr class="odd">
<td align="left">-reschedule &lt;JOB_ID&gt;</td>
<td align="left">Reschedule the job specified by the <span class="nctnt ncbi-monospace">JOB_ID</span> parameter.</td>
</tr>
<tr class="even">
<td align="left">-cancel &lt;JOB_ID&gt;</td>
<td align="left">Cancel the specified job.</td>
</tr>
<tr class="odd">
<td align="left">-qprint &lt;JOB_STATUS&gt;</td>
<td align="left">Print queue content for the specified job status.</td>
</tr>
<tr class="even">
<td align="left">-count &lt;QUERY_STRING&gt;</td>
<td align="left">Count all jobs within the specified queue with tags set by query string.</td>
</tr>
<tr class="odd">
<td align="left">-count_active</td>
<td align="left">Count active jobs in all queues.</td>
</tr>
<tr class="even">
<td align="left">-show_jobs_id &lt;QUERY_STRING&gt;</td>
<td align="left">Show all job IDs by query string.</td>
</tr>
<tr class="odd">
<td align="left">-query &lt;QUERY_STRING&gt;</td>
<td align="left">Perform a query on the jobs withing the specified queue.</td>
</tr>
<tr class="even">
<td align="left">-fields &lt;FIELD_LIST&gt;</td>
<td align="left">Fields (separated by ','), which should be returned by one of the above query commands.</td>
</tr>
<tr class="odd">
<td align="left">-select &lt;QUERY_STRING&gt;</td>
<td align="left">Perform a select query on the jobs withing the specified queue.</td>
</tr>
<tr class="even">
<td align="left">-showparams</td>
<td align="left">Show service parameters.</td>
</tr>
<tr class="odd">
<td align="left">-read &lt;BATCH_ID_OUTPUT,JOB_IDS_OUTPUT,LIMIT,TIMEOUT&gt;</td>
<td align="left">Retrieve IDs of the completed jobs and change their state to Reading.<br/><br/>For the first two parameters, the <a href="#alternate-list-output">Alternate list output</a> format can be used.<br/><br/><strong>Parameter descriptions:</strong><br/><span class="nctnt ncbi-var">BATCH_ID_OUTPUT</span>
<ul>
<li><p>Defines where to send the ID of the retrieved jobs. Can be either a file name or '-'.</p></li>
</ul>
<span class="nctnt ncbi-var">JOB_IDS</span>
<ul>
<li><p>Defines where to send the list of jobs that were switched to the state <span class="nctnt ncbi-monospace">Reading</span>. Can be either a file name or '-'.</p></li>
</ul>
<span class="nctnt ncbi-var">LIMIT</span>
<ul>
<li><p>Maximum number of jobs retrieved.</p></li>
</ul>
<span class="nctnt ncbi-var">TIMEOUT</span>
<ul>
<li><p>Timeout before jobs will be switched back to the state <span class="nctnt ncbi-monospace">Done</span> so that they can be returned again in response to another <span class="nctnt ncbi-monospace">-read</span>.</p></li>
</ul>
<br/><strong>Examples:</strong><br/>
<pre><code>    netschedule_control -service NS_Test -queue test \
        -read batch_id.txt,job_ids.lst,100,300
    netschedule_control -service NS_Test -queue test \
        -read -,job_ids.lst,100,300
    netschedule_control -service NS_Test -queue test \
        -read batch_id.txt,-,100,300</code></pre></td>
</tr>
<tr class="even">
<td align="left">-read_confirm &lt;JOB_LIST&gt;</td>
<td align="left">Mark jobs in <span class="nctnt ncbi-monospace">JOB_LIST</span> as successfully retrieved. The <a href="#alternate-list-input">Alternate list input</a> format can be used to specify <span class="nctnt ncbi-monospace">JOB_LIST</span>. If this operation succeeds, the specified jobs will change their state to <span class="nctnt ncbi-monospace">Confirmed</span>.<br/><br/>Examples:<br/>
<pre><code>    netschedule_control -service NS_Test -queue test \
        -read_confirm @job_ids.lst
    netschedule_control -service NS_Test -queue test \
        -read_confirm - &lt; job_ids.lst
    netschedule_control -service NS_Test -queue test \
        -read_confirm JSID_01_4_130.14.24.10_9100,JSID_01_5_130.14.24.10_9100</code></pre></td>
</tr>
<tr class="odd">
<td align="left">-read_rollback &lt;JOB_LIST&gt;</td>
<td align="left">Undo the <span class="nctnt ncbi-monospace">-read</span> operation for the specified jobs thus making them available for the subsequent <span class="nctnt ncbi-monospace">-read</span> operations. See the description of <span class="nctnt ncbi-monospace">-read_confirm</span> for information on the <span class="nctnt ncbi-monospace">JOB_LIST</span> argument and usage examples.</td>
</tr>
<tr class="even">
<td align="left">-read_fail &lt;JOB_LIST&gt;</td>
<td align="left">Undo the <span class="nctnt ncbi-monospace">-read</span> operation for the specified jobs thus making them available for the subsequent <span class="nctnt ncbi-monospace">-read</span> operations. This command is similar to <span class="nctnt ncbi-monospace">-read_rollback</span> with the exception that it also increases the counter of the job result reading failures for the specified jobs. See the description of <span class="nctnt ncbi-monospace">-read_confirm</span> for information on the <span class="nctnt ncbi-monospace">JOB_LIST</span> argument and usage examples.</td>
</tr>
<tr class="odd">
<td align="left">-logfile &lt;LOG_FILE&gt;</td>
<td align="left">File to which the program log should be redirected.</td>
</tr>
<tr class="even">
<td align="left">-conffile &lt;INI_FILE&gt;</td>
<td align="left">Override configuration file name (by default, <span class="nctnt ncbi-path">netschedule_control.ini</span>).</td>
</tr>
<tr class="odd">
<td align="left">-version</td>
<td align="left">Print version number; ignore other arguments.</td>
</tr>
<tr class="even">
<td align="left">-dryrun</td>
<td align="left">Do nothing, only test all preconditions.</td>
</tr>
</tbody>
</table>

### ns\_remote\_job\_control

DESCRIPTION:

This utility acts as a submitter for the remote\_app daemon. It initiates job execution on remote\_app, and then checks the status and the results of the job.

OPTIONS:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">-h</td>
<td align="left">Print brief usage and description; ignore other arguments.</td>
</tr>
<tr class="even">
<td align="left">-help</td>
<td align="left">Print long usage and description; ignore other arguments.</td>
</tr>
<tr class="odd">
<td align="left">-xmlhelp</td>
<td align="left">Print long usage and description in XML format; ignore other arguments.</td>
</tr>
<tr class="even">
<td align="left">-q &lt;QUEUE&gt;</td>
<td align="left">NetSchedule queue name.</td>
</tr>
<tr class="odd">
<td align="left">-ns &lt;SERVICE&gt;</td>
<td align="left">NetSchedule service address (service_name or host:port).</td>
</tr>
<tr class="even">
<td align="left">-nc &lt;SERVICE&gt;</td>
<td align="left">NetCache service address (service_name or host:port).</td>
</tr>
<tr class="odd">
<td align="left">-jlist &lt;STATUS&gt;</td>
<td align="left">Show jobs by status. STATUS can be one of the following:
<ul>
<li><p><span class="nctnt ncbi-monospace">all</span></p></li>
<li><p><span class="nctnt ncbi-monospace">canceled</span></p></li>
<li><p><span class="nctnt ncbi-monospace">done</span></p></li>
<li><p><span class="nctnt ncbi-monospace">failed</span></p></li>
<li><p><span class="nctnt ncbi-monospace">pending</span></p></li>
<li><p><span class="nctnt ncbi-monospace">returned</span></p></li>
<li><p><span class="nctnt ncbi-monospace">running</span></p></li>
</ul>
<br/></td>
</tr>
<tr class="even">
<td align="left">-qlist</td>
<td align="left">Print the list of queues available on the specified NetSchedule server or a group of servers identified by the service name.</td>
</tr>
<tr class="odd">
<td align="left">-wnlist</td>
<td align="left">Show registered worker nodes.</td>
</tr>
<tr class="even">
<td align="left">-jid &lt;JOB_ID&gt;</td>
<td align="left">Show information on the specified job.</td>
</tr>
<tr class="odd">
<td align="left">-bid &lt;BLOB_ID&gt;</td>
<td align="left">Show NetCache blob contents.</td>
</tr>
<tr class="even">
<td align="left">-attr &lt;ATTRIBUTE&gt;</td>
<td align="left">Show one of the following job attributes:
<ul>
<li><p><span class="nctnt ncbi-monospace">cmdline</span></p></li>
<li><p><span class="nctnt ncbi-monospace">progress</span></p></li>
<li><p><span class="nctnt ncbi-monospace">raw_input</span></p></li>
<li><p><span class="nctnt ncbi-monospace">raw_output</span></p></li>
<li><p><span class="nctnt ncbi-monospace">retcode</span></p></li>
<li><p><span class="nctnt ncbi-monospace">status</span></p></li>
<li><p><span class="nctnt ncbi-monospace">stdin</span></p></li>
<li><p><span class="nctnt ncbi-monospace">stdout</span></p></li>
<li><p><span class="nctnt ncbi-monospace">stderr</span></p></li>
</ul>
<br/><br/>Alternatively, the ATTRIBUTE parameter can be specified as one of the following attribute sets:
<ul>
<li><p><span class="nctnt ncbi-monospace">standard</span></p></li>
<li><p><span class="nctnt ncbi-monospace">full</span></p></li>
<li><p><span class="nctnt ncbi-monospace">minimal</span></p></li>
</ul>
<br/></td>
</tr>
<tr class="odd">
<td align="left">-stdout &lt;JOB_IDS&gt;</td>
<td align="left">Dump concatenated standard output streams of the specified jobs. The <span class="nctnt ncbi-monospace">JOB_IDS</span> argument can be specified in the <a href="#alternate-list-input">Alternate list input</a> format.<br/>Examples:<br/>
<pre><code>    ns_remote_job_control -ns NS_Test -q test \
        -stdout JSID_01_4_130.14.24.10_9100,JSID_01_5_130.14.24.10_9100
    ns_remote_job_control -ns NS_Test -q test -stdout @job_ids.lst
    ns_remote_job_control -ns NS_Test -q test -stdout - &lt; job_ids.lst</code></pre></td>
</tr>
<tr class="even">
<td align="left">-stderr &lt;JOB_IDS&gt;</td>
<td align="left">Dump concatenated standard error streams of the specified jobs. The <span class="nctnt ncbi-monospace">JOB_IDS</span> argument can be specified in the <a href="#alternate-list-input">Alternate list input</a> format. See the description of the <span class="nctnt ncbi-monospace">-stdout</span> command for examples.</td>
</tr>
<tr class="odd">
<td align="left">-cancel &lt;JOB_ID&gt;</td>
<td align="left">Cancel the specified job.</td>
</tr>
<tr class="even">
<td align="left">-cmd &lt;COMMAND&gt;</td>
<td align="left">Apply one of the following commands to the queue specified by the <span class="nctnt ncbi-monospace">-q</span> option:
<ul>
<li><p><span class="nctnt ncbi-monospace">drop_jobs</span></p></li>
<li><p><span class="nctnt ncbi-monospace">kill_nodes</span></p></li>
<li><p><span class="nctnt ncbi-monospace">shutdown_nodes</span></p></li>
</ul>
<br/></td>
</tr>
<tr class="odd">
<td align="left">-render &lt;OUTPUT_FORMAT&gt;</td>
<td align="left">Set the output format of the informational commands like <span class="nctnt ncbi-monospace">-qlist</span>. The format can be either of the following: <span class="nctnt ncbi-monospace">text</span>, <span class="nctnt ncbi-monospace">xml</span>.</td>
</tr>
<tr class="even">
<td align="left">-of &lt;OUTPUT_FILE&gt;</td>
<td align="left">Output file for operations that actually produce output.</td>
</tr>
<tr class="odd">
<td align="left">-logfile &lt;LOG_FILE&gt;</td>
<td align="left">File to which the program log should be redirected.</td>
</tr>
<tr class="even">
<td align="left">-conffile &lt;INI_FILE&gt;</td>
<td align="left">Override configuration file name (by default, <span class="nctnt ncbi-monospace">ns_remote_job_control.ini</span>).</td>
</tr>
<tr class="odd">
<td align="left">-version</td>
<td align="left">Print version number; ignore other arguments.</td>
</tr>
<tr class="even">
<td align="left">-version-full</td>
<td align="left">Print extended version data; ignore other arguments.</td>
</tr>
<tr class="odd">
<td align="left">-dryrun</td>
<td align="left">Do nothing, only test all preconditions.</td>
</tr>
</tbody>
</table>

### Alternate list input and output

This section describes two alternative methods of printing the results of operations that generate lists (e.g. lists of job IDs) and three methods of inputting such lists as command line arguments.

#### Alternate list output

The <span class="nctnt ncbi-monospace">-read</span> command of <span class="nctnt ncbi-monospace">netschedule\_control</span> produces a list of job IDs as its output. This list can be sent either to a file (if a file name is specified) or to <span class="nctnt ncbi-var">stdout</span> (if a dash ('-') is specified in place of the file name).

**Example:**

        # Read job results: send batch ID to STDOUT,
        # and the list of jobs to job_ids.lst
        netschedule_control -service NS_Test -queue test \
            -read -,job_ids.lst,10,300

#### Alternate list input

There are three ways one can specify a list of arguments in a command line option that accepts the Alternate list input format (like the -stdout and stderr options of ns\_remote\_job\_conrol):

1  
Via a comma-separated (or a space-separated) list.

2  
By using a text file (one argument per line). The name of the file must be prefixed with '@' to distinguish from the explicit enumeration of the previous case.

3  
Via <span class="nctnt ncbi-var">stdin</span> (denoted by '-'). This variant does not differ from using a text file except that list items are red from the standard input - one item per line.

**Examples:**

        # Concatenate and print stdout
        ns_remote_job_control -ns NS_Test -q rmcgi_sample \
            -stdout JSID_01_4_130.14.24.10_9100,JSID_01_5_130.14.24.10_9100

        # Confirm job result reading for batch #6
        netschedule_control -service NS_Test -queue test \
            -read_confirm 6,@job_ids.lst

        # The same using STDIN
        netschedule_control -service NS_Test -queue test \
            -read_confirm 6,- < job_ids.lst


