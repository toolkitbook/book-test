---
layout: default
title: C++ Toolkit test
nav: pages/ch_cgi
---

<span class="label">11</span>CGI and Fast-CGI
=============================================

Created: January 1, 2005; Last Update: February 2, 2015.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

**CGI and Fast-CGI** [Libraries <span class="nctnt ncbi-path">xcgi</span> and <span class="nctnt ncbi-path">xfcgi</span>: [include](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/cgi) \\| [src](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi)]

These library classes represent an [integrated framework](#integrated-framework-) with which to write CGI applications and are designed to help retrieve and parse an HTTP request and then to compose and deliver an HTTP response. (See also this additional [class reference documentation](#class-reference-documentation-)). <span class="nctnt ncbi-path">xfcgi</span> is a FastCGI version of <span class="nctnt ncbi-path">xcgi</span>.

<span class="nctnt highlight">Hint:</span> Requires the target executable to be linked with a third-party FastCGI library, as in:

[LIBS](ch_proj.html#ch_proj.make_proj_app)<span class="nctnt ncbi-code"> = $(FASTCGI\_LIBS) $(ORIG\_LIBS)</span>.

<span class="nctnt highlight">Hint:</span> On non-FastCGI capable platforms (or if run as a plain CGI on a FastCGI-capable platform), it works the same as a plain CGI.

CGI Interface

-   [Basic CGI Application Class](#basic-cgi-application-class-) (includes [CGI Diagnostic Handling](#cgi-diagnostic-handling-)) cgiapp[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/cgi/cgiapp.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/cgiapp.cpp)]

-   [CGI Application Context Classes](#cgi-application-context-classes-) cgictx[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/cgi/cgictx.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/cgictx.cpp)]

-   [HTTP Request Parser](#http-request-parser-) ncbicgi[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/cgi/ncbicgi.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/ncbicgi.cpp)]

-   [HTTP Cookies](#http-cookies-) ncbicgi[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/cgi/ncbicgi.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/ncbicgi.cpp)]

-   [HTTP Response Generator](#http-response-generator-) ncbicgir[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/cgi/ncbicgir.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/ncbicgir.cpp)]

-   [Basic CGI Resource Class](#basic-cgi-resource-class-) ncbires[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/cgi/ncbires.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/ncbires.cpp)]

<span class="nctnt ncbi-type">FastCGI</span> CGI Interface

-   Adapter Between C++ and FastCGI Streams fcgibuf[[.hpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/fcgibuf.hpp) \\| [.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/fcgibuf.cpp)]

-   Fast-CGI Loop Function fcgi\_run[[.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/fcgi_run.cpp)]

-   Plain CGI Stub for the Fast-CGI Loop Function cgi\_run[[.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/cgi_run.cpp)]

**Demo Cases** [[src/cgi/demo](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/demo) \\| [C++/src/sample/app/cgi/](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/cgi/)]

**Test Cases** [[src/cgi/test](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/test)]

.

### Chapter Outline

The following is an outline of the topics presented in this chapter:

[Developing CGI applications](#developing-cgi-applications-)

-   [Overview of the CGI classes](#overview-of-the-cgi-classes--)

-   [The CCgiApplication class](#the-ccgiapplication-class--)

-   [The CNcbiResource and CNcbiCommand classes](#the-cncbiresource-and-cncbicommand-classes--)

-   [The CCgiRequest class](#the-ccgirequest-class--)

-   [The CCgiResponse class](#the-ccgiresponse-class--)

-   [The CCgiCookie class](#the-ccgicookie-class--)

-   [The CCgiCookies class](#the-ccgicookies-class--)

-   [The CCgiContext class](#the-ccgicontext-class--)

-   [The CCgiUserAgent class](#the-ccgiuseragent-class-)

-   [Example code using the CGI classes](#example-code-using-the-cgi-classes--)

-   [CGI Registry configuration](#cgi-registry-configuration)

-   [Supplementary Information](#supplementary-information)

[CGI Diagnostic Handling](#cgi-diagnostic-handling-)

-   [diag-destination](#diag-destination-)

-   [diag-threshold](#diag-threshold-)

-   [diag-format](#diag-format-)

[NCBI C++ CGI Classes](#ncbi-c++-cgi-classes-)

-   [CCgiRequest](#ccgirequest-)

-   [CCgiResponse](#ccgiresponse-)

-   [CCgiCookie](#ccgicookie-)

-   [CCgiCookies](#ccgicookies-)

[An example web-based CGI application](#an-example-web-based-cgi-application-)

-   [Introduction](#introduction-)

-   [Program description](#program-description-)

-   [Program design: Distributing the work](#program-design-distributing-the-work-)

[CGI Status Codes](#cgi-status-codes)

[FCGI Redirection and Debugging C++ Toolkit CGI Programs](#fcgi-redirection-and-debugging-c++-toolkit-cgi-programs)

Developing CGI applications
---------------------------

-   [Overview of the CGI classes](#overview-of-the-cgi-classes--)

-   [The CCgiApplication class](#the-ccgiapplication-class--)

-   [The CNcbiResource and CNcbiCommand classes](#the-cncbiresource-and-cncbicommand-classes--)

-   [The CCgiRequest class](#the-ccgirequest-class--)

-   [The CCgiResponse class](#the-ccgiresponse-class--)

-   [The CCgiCookie class](#the-ccgicookie-class--)

-   [The CCgiCookies class](#the-ccgicookies-class--)

-   [The CCgiContext class](#the-ccgicontext-class--)

-   [The CCgiUserAgent class](#the-ccgiuseragent-class-)

-   [Example code using the CGI classes](#example-code-using-the-cgi-classes--)

-   [CGI Registry configuration](#cgi-registry-configuration)

-   [Supplementary Information](#supplementary-information)

Although CGI programs are generally run as web applications with HTML interfaces, this section of the Programming Manual places emphasis on the CGI side of things, omitting HTML details of the implementation where possible. Similarly, the section on [Generating web pages](ch_html.html#ch_html.webpgs.html) focuses largely on the usage of HTML components independent of CGI details. The two branches of the NCBI C++ Toolkit hierarchy are all but independent of one another - with but one explicit hook between them: the constructors for HTML [page](ch_html.html#ch_html.page_classes) components accept a <span class="nctnt ncbi-class">CCgiApplication</span> as an optional argument. This <span class="nctnt ncbi-class">CCgiApplication</span> argument provides the HTML page component with access to all of the CGI objects used in the application.

Further discussion of combining a CGI application with the HTML classes can be found in the section on [An example web-based CGI application](#an-example-web-based-cgi-application-). The focus in this chapter is on the CGI classes only. For additional information about the CGI classes, the reader is also referred to the discussion of [NCBI C++ CGI Classes](#ncbi-c++-cgi-classes-) in the Reference Manual.

### The CGI classes

[Figure 1](#figure-1-) illustrates the layered design of the CGI classes.

[![Figure 1. Layered design of the CGI classes](/book-test/static/img/cgi.gif)](/book-test/static/img/cgi.gif "Click to see the full-resolution image")

Figure 1. Layered design of the CGI classes

This design is best described by starting with a consideration of the capabilities one might need to implement a CGI program, including:

-   A way to retrieve and store the current values of environment variables

-   A means of retrieving and interpreting the client's query request string

-   Mechanisms to service and respond to the requested query

-   Methods and data structures to obtain, store, modify, and send cookies

-   A way to set/reset the context of the application (for Fast-CGI)

The <span class="nctnt ncbi-class">CCgiContext</span> class unifies these diverse capabilities under one aggregate structure. As their names suggest, the <span class="nctnt ncbi-class">CCgiRequest</span> class receives and parses the request, and the <span class="nctnt ncbi-class">CCgiResponse</span> class outputs the response on an output stream. All incoming <span class="nctnt ncbi-class">CCgiCookie</span>s are also parsed and stored by the <span class="nctnt ncbi-class">CCgiRequest</span> object, and the outgoing cookies are sent along with the response by the <span class="nctnt ncbi-class">CCgiResponse</span> object. The request is actually processed by the application's <span class="nctnt ncbi-class">CNcbiResource</span>. The list of <span class="nctnt ncbi-class">CNcbiCommand</span>s stored with that resource object are scanned to find a matching command, which is then executed.

The <span class="nctnt ncbi-class">CCgiContext</span> object, which is a <span class="nctnt ncbi-code">friend</span> to the <span class="nctnt ncbi-class">CCgiApplication</span> class, orchestrates this sequence of events in coordination with the application object. The same application may be run in many different contexts, but the <span class="nctnt ncbi-monospace">resource</span> and defined set of <span class="nctnt ncbi-monospace">commands</span> are invariant. What changes with each context is the request and its associated response.

The <span class="nctnt ncbi-class">CCgiApplication</span> class is a specialization of <span class="nctnt ncbi-class">CNcbiApplication</span>. [Figure 2](#figure-2-) illustrates the adaptation of the <span class="nctnt ncbi-func">Init()</span> and <span class="nctnt ncbi-func">Run()</span> member functions inherited from the <span class="nctnt ncbi-class">CNcbiApplication</span> class to the requirements of CGI programming. Although the application is <span class="nctnt ncbi-monospace">contained</span> in the context, it is the application which creates and initializes each context in which it participates. The program arguments and environmental variables are passed along to the context, where they will be stored, thus freeing the application to be restarted in a new context, as in Fast-CGI.

[![Figure 2. Adapting the init() and run() methods inherited from CNcbiApplication](/book-test/static/img/cgirun.gif)](/book-test/static/img/cgirun.gif "Click to see the full-resolution image")

Figure 2. Adapting the init() and run() methods inherited from CNcbiApplication

The application's <span class="nctnt ncbi-func">ProcessRequest</span> member function is an abstract function that must be implemented for each application project. In most cases, this function will access the query and the environment variables via the <span class="nctnt ncbi-class">CCgiContext</span>, using <span class="nctnt ncbi-func">ctx.GetRequest()</span> and <span class="nctnt ncbi-func">ctx.GetConfig()</span>. The application may then service the request using its resource's <span class="nctnt ncbi-func">HandleRequest()</span> method. The context's response object can then be used to send an appropriate response.

These classes are described in more detail below, along with abbreviated synopses of the class definitions. These are included here to provide a conceptual framework and are not intended as reference materials. For example, constructor and destructor declarations that operate on void arguments, and <span class="nctnt ncbi-code">const</span> methods that duplicate non-const declarations are generally not included here. Certain virtual functions and data members that have no meaning outside of a web application are also omitted. For complete definitions, refer to the header files via the source browsers.

### The CCgiApplication Class ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiApplication.html))

As mentioned, the <span class="nctnt ncbi-class">CCgiApplication</span> class implements its own version of [Init()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiApplication.html#a0a910deea4387498e472b209967569f0), where it instantiates a [CNcbiResource](#cncbiresource-) object using <span class="nctnt ncbi-func">LoadResource()</span>. [Run()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiApplication.html#a9c4be90774829c6a66320a2391e7fcbb) is no longer a pure virtual function in this subclass, and its implementation now calls <span class="nctnt ncbi-func">CreateContext(), ProcessRequest()</span>, and <span class="nctnt ncbi-func">CCgiContext::GetResponse()</span>. The <span class="nctnt ncbi-class">CCgiApplication</span> class does **not** have a <span class="nctnt ncbi-class">CCgiContext</span> data member, because the application object can participate in multiple <span class="nctnt ncbi-class">CCgiContext</span>s. Instead, a local variable in each <span class="nctnt ncbi-func">Run()</span> invocation stores a pointer to the context created there. The <span class="nctnt ncbi-func">LoadServerContext()</span> member function is used in Web applications, where it is necessary to store more complex run-time data with the context object. The <span class="nctnt ncbi-class">CCgiServerContext</span> object returned by this function is stored as a data member of a <span class="nctnt ncbi-class">CCgiContext</span> and is application specific.

    class CCgiApplication : public CNcbiApplication 
    { 
        friend class CCgiContext; 

    public: 
        void Init(void); 
        void Exit(void); 
        int Run(void); 

        CNcbiResource& GetResource(void); 
        virtual int ProcessRequest(CCgiContext&) = 0; 
        CNcbiResource* LoadResource(void); 
        virtual CCgiServerContext* LoadServerContext(CCgiContext& context); 

        bool IsFastCGI(void) const; 

    protected: 
        CCgiContext* CreateContext(CNcbiArguments*, CNcbiEnvironment*, 
                                   CNcbiIstream*, CNcbiOstream*); 

    private: auto_ptr<CNcbiResource> m_resource; 
    };

If the program was **not** compiled as a FastCGI application (or the environment does not support FastCGI), then [IsFastCGI()](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=IsFastCGI) will return <span class="nctnt ncbi-monospace">false</span>. Otherwise, a "FastCGI loop" will be iterated over <span class="nctnt ncbi-var">def\_iter</span> times, with the initialization methods and <span class="nctnt ncbi-func">ProcessRequest()</span> function being executed on each iteration. The value returned by <span class="nctnt ncbi-func">IsFastCGI()</span> in this case is <span class="nctnt ncbi-monospace">true</span>. <span class="nctnt ncbi-func">Run()</span> first calls <span class="nctnt ncbi-func">IsFastCGI()</span>, and if that returns <span class="nctnt ncbi-monospace">false</span>, the application is run as a plain CGI program.

### The CNcbiResource ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiResource.html)) and CNcbiCommand ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiCommand.html)) Classes

The resource class is at the heart of the application, and it is here that the program's functionality is defined. The single argument to the resource class's constructor is a [CNcbiRegistry](ch_core.html#ch_core.CNcbiRegistry) object, which defines data paths, resources, and possibly environmental variables for the application. This information is stored in the resource class's data member, <span class="nctnt ncbi-var">m\_config</span>. The only other data member is a [TCmdList](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=TCmdList) (a list of <span class="nctnt ncbi-class">CNcbiCommand</span>s) called <span class="nctnt ncbi-var">m\_cmd</span>.

    class CNcbiResource 
    { 
    public: 

        CNcbiResource(CNcbiRegistry& config); 

        CNcbiRegistry&  GetConfig(void); 
        const TCmdList& GetCmdList(void) const; 
        virtual CNcbiCommand* GetDefaultCommand(void) const = 0; 
        virtual const CNcbiResPresentation* GetPresentation(void) const; 

        void AddCommand(CNcbiCommand* command); 
        virtual void HandleRequest(CCgiContext& ctx);

    protected: 

        CNcbiRegistry& m_config; 
        TCmdList m_cmd; 
    };

The <span class="nctnt ncbi-func">AddCommand()</span> method is used when a resource is being initialized, to add commands to the command list. Given a <span class="nctnt ncbi-class">CCgiRequest</span> object defined in a particular context <span class="nctnt ncbi-var">ctx</span>, [HandleRequest(ctx)](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=HandleRequest) compares entries in the context's request to commands in <span class="nctnt ncbi-var">m\_cmd</span>. The first command in <span class="nctnt ncbi-var">m\_cmd</span> that matches an entry in the request is then executed (see below), and the request is considered "handled". If desired, a default command can be installed that will execute when no matching command is found. The default command is defined by implementing the pure virtual function <span class="nctnt ncbi-func">GetDefaultCommand()</span>. The [CNcbiResPresentation](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCNcbiResPresentation.html) class is an abstract base class, and the member function, <span class="nctnt ncbi-func">GetPresentation()</span>, returns 0. It is provided as a hook for implementing interfaces between information resources (e.g., databases) and CGI applications.

    class CNcbiCommand 
    { 
    public: 
        CNcbiCommand(CNcbiResource& resource); 

        virtual CNcbiCommand* Clone(void) const = 0; 
        virtual string GetName() const = 0; 
        virtual void Execute(CCgiContext& ctx) = 0; 
        virtual bool IsRequested(const CCgiContext& ctx) const; 

    protected: 
        virtual string GetEntry() const = 0; 
        CNcbiResource& GetResource() const { return m_resource; } 

    private: 
        CNcbiResource& m_resource; 
    };

<span class="nctnt ncbi-class">CNcbiCommand</span> is an abstract base class; its only data member is a reference to the resource it belongs to, and most of its methods - with the exception of <span class="nctnt ncbi-func">GetResource()</span> and <span class="nctnt ncbi-func">IsRequested()</span> - are pure virtual functions. <span class="nctnt ncbi-func">IsRequested()</span> examines the <span class="nctnt ncbi-monospace">key=value</span> entries stored with the context's request object. When an entry is found where <span class="nctnt ncbi-code">key==GetEntry()</span> and <span class="nctnt ncbi-code">value==GetName()</span>, <span class="nctnt ncbi-func">IsRequested()</span> returns <span class="nctnt ncbi-monospace">true</span>.

The resource's <span class="nctnt ncbi-func">HandleRequest()</span> method iterates over its command list, calling <span class="nctnt ncbi-func">CNcbiCommand::IsRequested()</span> until the first match between a command and a request entry is found. When <span class="nctnt ncbi-func">IsRequested()</span> returns <span class="nctnt ncbi-monospace">true</span>, the command is <span class="nctnt ncbi-monospace">cloned</span>, and the cloned command is then <span class="nctnt ncbi-monospace">executed</span>. Both the <span class="nctnt ncbi-func">Execute()</span> and <span class="nctnt ncbi-func">Clone()</span> methods are pure virtual functions that must be implemented by the user.

### The CCgiRequest Class ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiRequest.html))

The <span class="nctnt ncbi-class">CCgiRequest</span> class serves as an interface between the user's query and the CGI program. Arguments to the constructor include a <span class="nctnt ncbi-class">CNcbiArguments</span> object, a <span class="nctnt ncbi-class">CNcbiEnvironment</span> object, and a <span class="nctnt ncbi-class">CNcbiIstream</span> object. The class constructors do little other than invoke <span class="nctnt ncbi-func">CCgiRequest::x\_Init()</span>, where the actual initialization takes place.

<span class="nctnt ncbi-func">x\_Init()</span> begins by examining the environment argument, and if it is <span class="nctnt ncbi-monospace">NULL</span>, <span class="nctnt ncbi-var">m\_OwnEnv</span> (an auto\_ptr) is reset to a dummy environment. Otherwise, <span class="nctnt ncbi-var">m\_OwnEnv</span> is reset to the passed environment, making the request object the effective owner of that environment. The environment is then used to cache network information as "gettable" properties. Cached properties include:

-   server properties, such as the server name, gateway interface, and server port

-   client properties (the remote host and remote address)

-   client data properties (content type and content length of the request)

-   request properties, including the request method, query string, and path information

-   authentication information, such as the remote user and remote identity

-   standard HTTP properties (from the HTTP header)

These properties are keyed to an enumeration named [ECgiProp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/group__CGIReqRes.html#a8) and can be retrieved using the request object's <span class="nctnt ncbi-func">GetProperty()</span> member function. For example, <span class="nctnt ncbi-code">GetProperty(eCgi\_HttpCookie)</span> is used to access cookies from the HTTP Header, and <span class="nctnt ncbi-code">GetProperty(eCgi\_RequestMethod)</span> is used to determine from where the query string should be read.

<span class="nctnt highlight">NOTE:</span> Setting <span class="nctnt ncbi-var">$QUERY\_STRING</span> without also setting <span class="nctnt ncbi-var">$REQUEST\_METHOD</span> will result in a failure by <span class="nctnt ncbi-func">x\_init()</span> to read the input query. <span class="nctnt ncbi-func">x\_init()</span> first looks for the definition of <span class="nctnt ncbi-var">$REQUEST\_METHOD</span>, and depending on if it is <span class="nctnt ncbi-type">GET</span> or <span class="nctnt ncbi-type">POST</span>, reads the query from the environment or the input stream, respectively. If the environment does not define <span class="nctnt ncbi-var">$REQUEST\_METHOD</span>, then <span class="nctnt ncbi-func">x\_Init()</span> will try to read the query string from the command line only.

    class CCgiRequest { 
    public: 
        CCgiRequest(const CNcbiArguments*, const CNcbiEnvironment*, 
            CNcbiIstream*, TFlags); 

        static const string& GetPropertyName(ECgiProp prop); 
        const string& GetProperty(ECgiProp prop) const; 
        size_t GetContentLength(void) const; 
        const CCgiCookies& GetCookies(void) const; 
        const TCgiEntries& GetEntries(void) const; 
        static SIZE_TYPE ParseEntries(const string& str, TCgiEntries& entries); 
    private: 
        void x_Init(const CNcbiArguments*, const CNcbiEnvironment*, 
            CNcbiIstream*, TFlags); 

        const CNcbiEnvironment* m_Env; 
        auto_ptr<CNcbiEnvironment> m_OwnEnv; 
        TCgiEntries m_Entries; 
        CCgiCookies m_Cookies; 
    };

This abbreviated definition of the <span class="nctnt ncbi-class">CCgiRequest</span> class highlights its primary functions:

To parse and store the <span class="nctnt ncbi-monospace">\<key=value\></span> pairs contained in the query string (stored in <span class="nctnt ncbi-var">m\_Entries</span>).

To parse and store the cookies contained in the HTTP header (stored in <span class="nctnt ncbi-var">m\_Cookies</span>).

As implied by the "T" prefix, [TCgiEntries](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=TCgiEntries) is a type definition, and defines <span class="nctnt ncbi-var">m\_Entries</span> to be an STL multimap of <span class="nctnt ncbi-monospace">\<string,string\></span> pairs. The <span class="nctnt ncbi-class">CCgiCookies</span> class (described [below](#below-)) contains an STL set of [CCgiCookie](#ccgicookie-) and implements an interface to this set.

### The CCgiResponse Class ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiResponse.html))

The <span class="nctnt ncbi-class">CCgiResponse</span> class provides an interface to the program's output stream (usually <span class="nctnt ncbi-var">cout</span>), which is the sole argument to the constructor for <span class="nctnt ncbi-class">CCgiResponse</span>. The output stream can be accessed by the program using <span class="nctnt ncbi-func">CCgiResponse::GetOutput()</span>, which returns a pointer to the output stream, or, by using <span class="nctnt ncbi-func">CCgiResponse::out()</span>, which returns a reference to that stream.

In addition to implementing controlled access to the output stream, the primary function of the response class is to generate appropriate HTML headers that will precede the rest of the response. For example, a typical sequence in the implementation of a particular command's execute function might be:

    MyCommand::Execute(CCgiContext& ctx) 
    { 
        // ... generate the output and store it in MyOutput 

        ctx.GetResponse().WriteHeader(); 
        ctx.GetResponse().out() << MyOutput; 
        ctx.GetResponse.out() << "</body></html>" << endl; 
        ctx.GetResponse.Flush(); 
    }

Any cookies that are to be sent with the response are included in the headers generated by the response object.

Two member functions are provided for outputting HTML headers: <span class="nctnt ncbi-func">WriteHeader()</span> and <span class="nctnt ncbi-func">WriteHeader(CNcbiOstream&)</span>. The second of these is for writing to a specified stream other than the default stream stored with the response object. Thus, <span class="nctnt ncbi-code">WriteHeader(out())</span> is equivalent to <span class="nctnt ncbi-code">WriteHeader()</span>.

The <span class="nctnt ncbi-func">WriteHeader()</span> function begins by invoking <span class="nctnt ncbi-func">IsRawCgi()</span> to see whether the application is a [non-parsed header](http://tools.ietf.org/html/rfc3875#section-5) program. If so, then the first header put on the output stream is an HTTP status line, taken from the private static data member, <span class="nctnt ncbi-var">sm\_HTTPStatusDefault</span>. Next, unless the content type has been set by the user (using <span class="nctnt ncbi-func">SetContentType()</span>), a default content line is written, using <span class="nctnt ncbi-var">sm\_ContentTypeDefault</span>. Any cookies stored in <span class="nctnt ncbi-var">m\_Cookies</span> are then written, followed by any additional headers stored with the request in <span class="nctnt ncbi-var">m\_HeaderValues</span>. Finally, a new line is written to separate the body from the headers.

    class CCgiResponse { 
    public: 
        CCgiResponse(CNcbiOstream* out = 0); 

        void SetRawCgi(bool raw); 
        bool IsRawCgi(void) const; 
        void SetHeaderValue(const string& name, const string& value); 
        void SetHeaderValue(const string& name, const tm& value); 
        void RemoveHeaderValue(const string& name); 
        void SetContentType(const string &type); 
        string GetHeaderValue(const string& name) const; 
        bool HaveHeaderValue(const string& name) const; 
        string GetContentType(void) const; 

        CCgiCookies& Cookies(void);                 // Get cookies set 
        CNcbiOstream* SetOutput(CNcbiOstream* out); // Set default output stream 
        CNcbiOstream* GetOutput(void) const;        // Query output stream 
        CNcbiOstream& out(void) const;              // Conversion to ostream 
                                                    // to enable << 

        void Flush() const; 

        CNcbiOstream& WriteHeader(void) const;  // Write HTTP response header 
        CNcbiOstream& WriteHeader(CNcbiOstream& out) const; 
    protected: 
        typedef map<string, string> TMap; 
        static const string sm_ContentTypeName; 
        static const string sm_ContentTypeDefault; 
        static const string sm_HTTPStatusDefault; 
        bool m_RawCgi; 
        CCgiCookies m_Cookies; 
        TMap m_HeaderValues;    // Additional header lines in alphabetical order 
        CNcbiOstream* m_Output; // Default output stream };

### The CCgiCookie Class ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiCookie.html))

The traditional means of maintaining state information when servicing a multi-step request has been to include <span class="nctnt ncbi-type">hidden</span> input elements in the query strings passed to subsequent URLs. The newer, preferred method uses HTTP cookies, which provide the server access to client-side state information stored with the client. The cookie is a text string consisting of four key=value pairs:

-   name (required)

-   expires (optional)

-   domain (optional)

-   path (optional)

The <span class="nctnt ncbi-class">CCgiCookie</span> class provides a means of creating, modifying, and sending cookies. The constructor requires at least two arguments, specifying the <span class="nctnt ncbi-monospace">name</span> and <span class="nctnt ncbi-monospace">value</span> of the cookie, along with the optional <span class="nctnt ncbi-var">domain</span> and <span class="nctnt ncbi-var">path</span> arguments. Format errors in the arguments to the constructor (see [Supplementary Information](#supplementary-information-)) will cause the invalid argument to be thrown. The <span class="nctnt ncbi-func">CCgiCookie::Write(CNcbiOstream&)</span> member function creates a <span class="nctnt ncbi-monospace">Set-Cookie</span> directive using its private data members and places the resulting string on the specified output stream:

    Set-Cookie: 
    m_Name=
    m_Value; expires=
    m_Expires; path=
    m_Path; 
    domain=
    m_Domain; 
    m_Secure

As with the constructor, and in compliance with the proposed standard ([RFC 6265](http://tools.ietf.org/html/rfc6265)), only the name and value are mandatory in the directive.

    class CCgiCookie {
    public:
        CCgiCookie(const string& name, const string& value,
                   const string& domain = NcbiEmptyString,
                   const string& path   = NcbiEmptyString);
        const string& GetName(void) const;
        CNcbiOstream& Write(CNcbiOstream& os) const;
        void Reset(void);
        void CopyAttributes(const CCgiCookie& cookie);
        void SetValue  (const string& str);
        void SetDomain (const string& str);
        void SetPath   (const string& str);
        void SetExpDate(const tm& exp_date);
        void SetSecure (bool secure);
        const string& GetValue  (void) const;
        const string& GetDomain (void) const;
        const string& GetPath   (void) const;
        string        GetExpDate(void) const;
        bool GetExpDate(tm* exp_date) const;
        bool GetSecure(void)          const;
        bool operator<(const CCgiCookie& cookie) const;
        typedef const CCgiCookie* TCPtr;
        struct PLessCPtr {
            bool operator() (const TCPtr& c1, const TCPtr& c2) const {
                return (*c1 < *c2);
            }
        };
    private:
        string m_Name;
        string m_Value;
        string m_Domain;
        string m_Path;
        tm     m_Expires;
        bool   m_Secure;
    };

With the exception of <span class="nctnt ncbi-var">m\_Name</span>, all of the cookie's data members can be reset using the <span class="nctnt ncbi-func">SetXxx(), Reset()</span>, and <span class="nctnt ncbi-func">CopyAttributes()</span> member functions; <span class="nctnt ncbi-var">m\_Name</span> is non-mutable. As with the constructor, format errors in the arguments to these functions will cause the invalid argument to be thrown. By default, <span class="nctnt ncbi-var">m\_Secure</span> is <span class="nctnt ncbi-monospace">false</span>. The <span class="nctnt ncbi-func">GetXxx()</span> methods return the stored value for that attribute or, if no value has been set, a reference to <span class="nctnt ncbi-class">NcbiEmptyString</span>. <span class="nctnt ncbi-code">GetExpDate(tm\*)</span> returns <span class="nctnt ncbi-monospace">false</span> if no expiration date was previously set. Otherwise, <span class="nctnt ncbi-var">tm</span> is reset to <span class="nctnt ncbi-var">m\_Expire</span>, and <span class="nctnt ncbi-monospace">true</span> is returned.

### The CCgiCookies Class ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiCookies.html))

The <span class="nctnt ncbi-class">CCgiCookies</span> class provides an interface to an STL set of <span class="nctnt ncbi-class">CCgiCookie</span>s (<span class="nctnt ncbi-var">m\_Cookies</span>). Each cookie in the set is uniquely identified by its name, domain, and path values and is stored in ascending order using the <span class="nctnt ncbi-type">CCgiCookie::PLessCPtr</span> construct. Two constructors are provided, allowing the user to initialize <span class="nctnt ncbi-var">m\_Cookies</span> to either an empty set or to a set of <span class="nctnt ncbi-var">N</span> new cookies created from the string "name1=value1; name2=value2; ...; nameN=valuenN". Many of the operations on a <span class="nctnt ncbi-class">CCgiCookies</span> object involve iterating over the set, and the class's type definitions support these activities by providing built-in iterators and a typedef for the set, <span class="nctnt ncbi-type">TSet</span>.

The <span class="nctnt ncbi-func">Add()</span> methods provide a variety of options for creating and adding new cookies to the set. As with the constructor, a single string of name-value pairs may be used to create and add <span class="nctnt ncbi-var">N</span> cookies to the set at once. Previously created cookies can also be added to the set individually or as sets. Similarly, the <span class="nctnt ncbi-func">Remove()</span> methods allow individual cookies or sets of cookies (in the specified range) to be removed. All of the remove functions destroy the removed cookies when <span class="nctnt ncbi-code">destroy=true</span>. <span class="nctnt ncbi-func">CCgiCookies::Write(CNcbiOstream&)</span> iteratively invokes the <span class="nctnt ncbi-func">CCgiCookie::Write()</span> on each element.

    class CCgiCookies {
    public:
        typedef set<CCgiCookie*, CCgiCookie::PLessCPtr>  TSet;
        typedef TSet::iterator         TIter;
        typedef TSet::const_iterator   TCIter;
        typedef pair<TIter,  TIter>    TRange;
        typedef pair<TCIter, TCIter>   TCRange;
        CCgiCookies(void);           // create empty set of cookies
        CCgiCookies(const string& str);
        // str = "name1=value1; name2=value2; ..."
        bool Empty(void) const;
        CCgiCookie* Add(const string& name, const string& value,
                        const string& domain = NcbiEmptyString,
                        const string& path   = NcbiEmptyString);
        CCgiCookie* Add(const CCgiCookie& cookie);
        void Add(const CCgiCookies& cookies);
        void Add(const string& str);
        // "name1=value1; name2=value2; ..."
       CCgiCookie* Find(const string& name, const string& domain,
                        const string& path);
        CCgiCookie* Find(const string& name, TRange*  range=0);
        bool Remove(CCgiCookie* cookie, bool destroy=true);
        size_t Remove(TRange& range, bool destroy=true);
        size_t Remove(const string& name, bool destroy=true);
        void Clear(void);
        CNcbiOstream& Write(CNcbiOstream& os) const;
    private:
        TSet m_Cookies;
    };

### The CCgiContext Class ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiContext.html))

As depicted in [Figure 1](#figure-1-), a <span class="nctnt ncbi-class">CCgiContext</span> object contains an application object, a request object, and a response object, corresponding to its data members <span class="nctnt ncbi-var">m\_app, m\_request</span>, and <span class="nctnt ncbi-var">m\_response</span>. Additional data members include a string encoding the URL for the context (<span class="nctnt ncbi-var">m\_selfURL</span>), a message buffer (<span class="nctnt ncbi-var">m\_lmsg</span>), and a <span class="nctnt ncbi-class">CCgiServerContext</span>. These last three data members are used only in complex Web applications, where it is necessary to store more complex run-time data with the context object. The message buffer is essentially an STL list of string objects the class definition of which ([CCtxMsgString](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CCtxMsgString)) includes a <span class="nctnt ncbi-func">Write()</span> output function. <span class="nctnt ncbi-func">GetServCtx()</span> returns <span class="nctnt ncbi-var">m\_srvCtx</span> if it has been defined and, otherwise, calls the application's <span class="nctnt ncbi-func">CCgiApplication::LoadServerContext()</span> to obtain it.

    class CCgiContext
    {
    public:
        CCgiContext(CCgiApplication&        app,
                    const CNcbiArguments*   args = 0,
                    const CNcbiEnvironment* env  = 0,
                    CNcbiIstream*           inp  = 0,
                    CNcbiOstream*           out  = 0);
        const CCgiApplication& GetApp(void) const;
        CNcbiRegistry&     GetConfig(void);
        CCgiRequest&       GetRequest(void);
        CCgiResponse&      GetResponse(void);
        const string&      GetSelfURL(void) const;
        CNcbiResource&     GetResource(void);
        CCgiServerContext&     GetServCtx(void);
        // output all msgs in m_lmsg to os
        CNcbiOstream& PrintMsg(CNcbiOstream& os);   
        void PutMsg(const string& msg);     // add message to m_lmsg
        void PutMsg(CCtxMsg* msg);          // add message to m_lmsg
        bool EmptyMsg(void);            // true iff m_lmsg is empty
        void ClearMsg(void);            // delete all messages in m_lmsg
        string GetRequestValue(const string& name) const;
        void AddRequestValue(const string& name, const string& value);
        void RemoveRequestValues(const string& name);
        void ReplaceRequestValue(const string& name, const string& value);
    private:
        CCgiApplication&      m_app;
        auto_ptr<CCgiRequest> m_request;
        CCgiResponse      m_response;
        mutable string    m_selfURL;
        list<CCtxMsg*>        m_lmsg;        // message buffer
        auto_ptr<CCgiServerContext> m_srvCtx;
        // defined by CCgiApplication::LoadServerContext()
        friend class CCgiApplication;
    };

### The CCgiUserAgent class ([\*](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCCgiUserAgent.html))

The <span class="nctnt ncbi-class">CCgiUserAgent</span> class is used to gather information about the client's user agent - i.e. browser type, browser name, browser version, browser engine type, browser engine version, Mozilla version (if applicable), platform, and robot information. The default constructor looks for the user agent string first in the <span class="nctnt ncbi-class">CCgiApplication</span> context using the <span class="nctnt ncbi-var">eCgi\_HttpUserAgent</span> request property, then in the <span class="nctnt ncbi-class">CNcbiApplication</span> instance <span class="nctnt ncbi-var">HTTP\_USER\_AGENT</span> environment variable, and finally in the operating system <span class="nctnt ncbi-var">HTTP\_USER\_AGENT</span> environment variable.

    class CCgiUserAgent
    {
    public:
        CCgiUserAgent(void);
        CCgiUserAgent(const string& user_agent);

        void Reset(const string& user_agent);

        string           GetUserAgentStr(void) const;
        EBrowser         GetBrowser(void) const;
        const string&    GetBrowserName(void) const;
        EBrowserEngine   GetEngine(void) const;
        EBrowserPlatform GetPlatform(void) const;

        const TUserAgentVersion& GetBrowserVersion(void) const;
        const TUserAgentVersion& GetEngineVersion(void) const;
        const TUserAgentVersion& GetMozillaVersion(void) const;

        typedef unsigned int TBotFlags;
        bool IsBot(TBotFlags flags = fBotAll, const string& patterns = kEmptyStr) const;

    protected:
        void x_Init(void);
        void x_Parse(const string& user_agent);
        bool x_ParseToken(const string& token, int where);

    protected:
        string            m_UserAgent;
        EBrowser          m_Browser;
        string            m_BrowserName;
        TUserAgentVersion m_BrowserVersion;
        EBrowserEngine    m_Engine;
        TUserAgentVersion m_EngineVersion;
        TUserAgentVersion m_MozillaVersion;
        EBrowserPlatform  m_Platform;
    };

### Example Code Using the CGI Classes

The [sample CGI program](#sample-cgi-program) demonstrates a simple application that combines the NCBI C++ Toolkit's CGI and HTML classes. [justcgi.cpp](#justcgicpp) is an adaptation of that program, stripped of all HTML references and with additional request-processing added (see [Box 1](#box-1) and [Box 2](#box-2)).

#### Box 1

    // File name: justcgi.cpp
    // Description: Demonstrate the basic CGI classes and functions
    #include "justcgi.hpp"
    #include <cgi/cgictx.hpp>
    #include <corelib/ncbistd.hpp>
    #include <corelib/ncbireg.hpp>
    #include <memory>
    USING_NCBI_SCOPE;
    /////////////////////////////////////////////////////////////////////////
    // Implement the application's LoadResource() and ProcessRequest() methods
    CNcbiResource* CCgiApp::LoadResource(void)
    {
        auto_ptr<CCgiResource> resource(new CCgiResource(GetConfig()));
        resource->AddCommand(new CCgiBasicCommand(*resource));
        resource->AddCommand(new CCgiReplyCommand(*resource));
        return resource.release();
    }
    // forward declarations 
    void ShowCommands (const TCmdList& cmds, CCgiContext& ctx);
    void ShowEntries (const TCgiEntries& entries);
    int CCgiApp::ProcessRequest(CCgiContext& ctx)
    {
        ShowCommands (GetResource().GetCmdList(), ctx);
        ShowEntries (const_cast<TCgiEntries&>(ctx.GetRequest().GetEntries()));
        GetResource().HandleRequest(ctx);
        return 0;
    }
    /////////////////////////////////////////////////////////////
    // Define the resource's default command if none match queryCNcbiCommand* CCgiResource::GetDefaultCommand(void) const
    {
        cerr << "    executing CCgiResource::GetDefaultCommand()" << endl;
        return new CCgiBasicCommand(const_cast<CCgiResource&>(*this));
    }
    ///////////////////////////////////////////////////////////////
    // Define the Execute() and Clone() methods for the commands 
    void CCgiCommand::Execute(CCgiContext& ctx)
    {
        cerr << "    executing CCgiCommand::Execute " << endl;
        const CNcbiRegistry& reg = ctx.GetConfig();
        ctx.GetResponse().WriteHeader();
    }
    CNcbiCommand* CCgiBasicCommand::Clone(void) const
    {
        cerr << "    executing CCgiBasicCommand::Clone()" << endl;
        return new CCgiBasicCommand(GetCgiResource());
    }
    CNcbiCommand* CCgiReplyCommand::Clone(void) const
    {
        cerr << "    executing CCgiReplyCommand::Clone" << endl;
        return new CCgiReplyCommand(GetCgiResource());
    }
    // Show what commands have been installed
    void ShowCommands (const TCmdList& cmds, CCgiContext& ctx)
    {
        cerr << "Commands defined for this application are: \n";
        ITERATE(TCmdList, it, cmds) {
             cerr << (*it)->GetName();
             if ((*it)->IsRequested(ctx)) {
                cerr << " (requested)" << endl;
             } else {
                cerr << " (not requested)" << endl;
             }
        }
    }
    // Show the <key=value> pairs in the request string 
    void ShowEntries (const TCgiEntries& entries)
    {
        cerr << "The entries in the request string were: \n";
        ITERATE(TCgiEntries, it, entries) {
             if (! (it->first.empty() && it->second.empty()))
                cerr << it->first << "=" << it->second << endl;
        }
    }
    static CCgiApp theCgiApp;
    int main(int argc, const char* argv[])
    {
        SetDiagStream(&cerr);
        return theCgiApp.AppMain(argc, argv);
    }

#### Box 2

    // File name: justcgi.hpp
    // Description: Demonstrate the basic CGI classes and functions
    #ifndef CGI_HPP
    #define CGI_HPP
    #include <cgi/cgiapp.hpp>
    #include <cgi/ncbires.hpp>
    USING_NCBI_SCOPE;
    class CCgiApp : public CCgiApplication
    {
    public:
        virtual CNcbiResource* LoadResource(void);
        virtual int ProcessRequest(CCgiContext& context);
    };
    class CCgiResource : public CNcbiResource
    {
    public:
        CCgiResource(CNcbiRegistry& config)
          : CNcbiResource(config) {}
        virtual ~CCgiResource() {};
        // defines the command to be executed when no other command matches
        virtual CNcbiCommand* GetDefaultCommand( void ) const;
    };
    class CCgiCommand : public CNcbiCommand
    {
    public:
        CCgiCommand( CNcbiResource& resource ) : CNcbiCommand(resource) {};
        virtual ~CCgiCommand( void ) {};
        virtual void Execute( CCgiContext& ctx );
        virtual string GetLink(CCgiContext&) const { return NcbiEmptyString; }
    protected:
        CCgiResource& GetCgiResource() const
            {
      return dynamic_cast<CCgiResource&>( GetResource() );
    }
        virtual string GetEntry() const { return string("cmd"); }
    };
    class CCgiBasicCommand : public CCgiCommand
    {
    public:
        CCgiBasicCommand(CNcbiResource& resource) : CCgiCommand(resource) {};
        virtual ~CCgiBasicCommand(void) {};
        virtual CNcbiCommand* Clone( void ) const;
        virtual string GetName( void ) const { return string("init"); };
    protected:
        virtual string GetEntry() const { return string("cmd1"); }
    };
    class CCgiReplyCommand : public CCgiBasicCommand
    {
    public:
        CCgiReplyCommand( CNcbiResource& resource) : CCgiBasicCommand(resource) {};
        virtual ~CCgiReplyCommand(void) {};
        virtual CNcbiCommand* Clone( void ) const;
        virtual string GetName( void ) const { return string("reply"); };
    protected:
        virtual string GetEntry() const { return string("cmd2"); }
    };
    #endif /* CGI_HPP */

Executing

    ./cgi 'cmd1=init&cmd2=reply'

results in execution of only <span class="nctnt ncbi-var">cmd1</span>, as does executing

    ./cgi 'cmd2=reply&cmd1=init'

The commands are matched in the order that they are registered with the resource, not according to the order in which they occur in the request. The assumption is that only the first entry (if any) in the query actually specifies a command, and that the remaining entries provide optional arguments to that command. The Makefile (see [Box 3](#box-3-)) for this example links to both the <span class="nctnt ncbi-path">xncbi</span> and <span class="nctnt ncbi-path">xcgi</span> libraries. Additional examples using the CGI classes can be found in [src/cgi/test](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/cgi/test). (For Makefile.fastcgi.app, see [Box 4](#box-4-).)

#### Box 3

    # Author: Diane Zimmerman 
    # Build CGI application "CGI" 
    # NOTE: see  to build Fast-CGI 
    ################################# 
    APP = cgi 
    OBJ = cgiapp 
    LIB = xcgi xncbi

#### Box 4

    # Author: Diane Zimmerman 
    # Build test Fast-CGI application "FASTCGI" 
    # NOTES:  - it will be automagically built as a plain CGI application if 
    #           Fast-CGI libraries are missing on your machine. 
    #         - also, it auto-detects if it is run as a FastCGI or a plain 
    #           CGI, and behave appropriately. 
    ################################# 
    APP = fastcgi 
    OBJ = cgiapp 
    LIB = xfcgi xncbi 
    LIBS = $(FASTCGI_LIBS) $(ORIG_LIBS)

### CGI Registry Configuration

The application registry defines CGI-related configuration settings in the <span class="nctnt ncbi-var">[CGI]</span> section (see [this table](ch_libconfig.html#ch_libconfig.CGI)).

FastCGI settings. <span class="nctnt ncbi-var">[FastCGI]</span> section (see [this table](ch_libconfig.html#ch_libconfig.FCGI)).

CGI load balancing settings. <span class="nctnt ncbi-var">[CGI-LB]</span> section (see [this table](ch_libconfig.html#ch_libconfig.CGI_Load_balancing_configur)).

### Supplementary Information

Restrictions on arguments to the <span class="nctnt ncbi-class">CCgiCookie</span> constructor.

See [Table 1](#table-1).

Table 1. Restrictions on arguments to the CCgiCookie constructor

| Field             | Restrictions                                                |
|-------------------|-------------------------------------------------------------|
| name (required)   | No spaces; must be printable ASCII; cannot contain = , or ; |
| value (required)  | No spaces; must be printable ASCII; cannot contain , or ;   |
| domain (optional) | No spaces; must be printable ASCII; cannot contain , or ;   |
| path (optional)   | Case sensitive                                              |

CGI Diagnostic Handling
-----------------------

By default, CGI applications support three query parameters affecting [diagnostic output](ch_core.html#ch_core.diag): [diag-destination](#diag-destination-), [diag-threshold](#diag-threshold-), and [diag-format](#diag-format-). It is possible to modify this behavior by overriding the virtual function [CCgiApplication::ConfigureDiagnostics](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=ConfigureDiagnostics). (In particular, production applications may wish to disable these parameters by defining <span class="nctnt ncbi-func">ConfigureDiagnostics</span> to be a no-op.)

### diag-destination

The parameter <span class="nctnt ncbi-var">diag-destination</span> controls where diagnostics appear. By default, there are two possible values (see [Table 2](#table-2-)).

Table 2. Effect of setting the diag-destination parameter

| value  | effects                                                          |
|--------|------------------------------------------------------------------|
| stderr | Send diagnostics to the standard error stream (default behavior) |
| asbody | Send diagnostics to the client in place of normal output         |

However, an application can make other options available by calling [RegisterDiagFactory](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=RegisterDiagFactory) from its <span class="nctnt ncbi-func">Init</span> routine. In particular, calling

    #include <connect/email_diag_handler.hpp> 
    ... 
    RegisterDiagFactory("email", new CEmailDiagFactory);

and linking against <span class="nctnt ncbi-lib">xconnect</span> and <span class="nctnt ncbi-lib">connect</span> enables destinations of the form <span class="nctnt ncbi-monospace">email:user@host</span>, which will cause the application to e-mail diagnostics to the specified address when done.

Similarly, calling

    #include <html/commentdiag.hpp> 
    ... 
    RegisterDiagFactory("comments", new CCommentDiagFactory);

and linking against <span class="nctnt ncbi-lib">xhtml</span> will enable the destination <span class="nctnt ncbi-monospace">comments</span>. With this destination, diagnostics will take the form of comments in the generated HTML, provided that the application has also used [SetDiagNode](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagNode) to indicate where they should go. (Applications may call that function repeatedly; each invocation will affect all diagnostics until the next invocation. Also, <span class="nctnt ncbi-func">SetDiagNode</span> is effectively a no-op for destinations other than <span class="nctnt ncbi-monospace">comments</span>, so applications may call it unconditionally.)

Those destinations are not available by default because they introduce additional dependencies; however, either may become a standard possibility in future versions of the toolkit.

### diag-threshold

The parameter <span class="nctnt ncbi-var">diag-threshold</span> sets the minimum [severity level](ch_core.html#ch_core.diag_severity) of displayed diagnostics; its value can be either <span class="nctnt ncbi-monospace">fatal</span>, <span class="nctnt ncbi-monospace">critical</span>, <span class="nctnt ncbi-monospace">error</span>, <span class="nctnt ncbi-monospace">warning</span>, <span class="nctnt ncbi-monospace">info</span>, or <span class="nctnt ncbi-monospace">trace</span>. For the most part, setting this parameter is simply akin to calling [SetDiagPostLevel](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagPostLevel). However, setting <span class="nctnt ncbi-var">diag-threshold</span> to <span class="nctnt ncbi-monospace">trace</span> is **not** equivalent to calling <span class="nctnt ncbi-code">SetDiagPostLevel(eDiag\_Trace)</span>; the former reports all diagnostics, whereas the latter reports only traces.

### diag-format

Finally, the parameter <span class="nctnt ncbi-var">diag-format</span> controls diagnostics' default appearance; setting it is akin to calling [{Set,Unset}DiagPostFlag](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=SetDiagPostFlag). Its value is a list of flags, delimited by spaces (which appear as "+" signs in URLs); possible flags are <span class="nctnt ncbi-monospace">file</span>, <span class="nctnt ncbi-monospace">path</span>, <span class="nctnt ncbi-monospace">line</span>, <span class="nctnt ncbi-monospace">prefix</span>, <span class="nctnt ncbi-monospace">severity</span>, <span class="nctnt ncbi-monospace">code</span>, <span class="nctnt ncbi-monospace">subcode</span>, <span class="nctnt ncbi-monospace">time</span>, <span class="nctnt ncbi-monospace">omitinfosev</span>, <span class="nctnt ncbi-monospace">all</span>, <span class="nctnt ncbi-monospace">trace</span>, <span class="nctnt ncbi-monospace">log</span>, and <span class="nctnt ncbi-monospace">default</span>. Every flag but <span class="nctnt ncbi-monospace">default</span> corresponds to a value in [EDiagPostFlag](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=EDiagPostFlag), and can be turned off by preceding its name with an exclamation point ("!"). <span class="nctnt ncbi-monospace">default</span> corresponds to the four flags which are on by default: <span class="nctnt ncbi-monospace">line</span>, <span class="nctnt ncbi-monospace">prefix</span>, <span class="nctnt ncbi-monospace">code</span>, and <span class="nctnt ncbi-monospace">subcode</span>, and may not be subtracted.

NCBI C++ CGI Classes
--------------------

The Common Gateway Interface (CGI) is a method used by web servers to pass information from forms displayed in a web browser to a program running on the server and then allow the program to pass a web page back. The NCBI C++ CGI Classes are used by the program running on the server to decode the CGI input from the server and to send a response. The library also supports cookies, which is a method for storing information on the user's machine. The library supports the http methods GET and POST via application/x-www-form-urlencoded, and supports the POST via multipart/form-data (often used for file upload). In the POST via multipart/form-data, the data gets read into a <span class="nctnt ncbi-type">TCgiEntries</span>; you also can get the filename out of it (the name of the entry is as specified by "name=" of the data-part header). For more information on CGI, see the book **HTML Sourcebook** by Ian Graham or http://hoohoo.ncsa.uiuc.edu/cgi/

There are 5 main classes:

<span class="nctnt ncbi-class">CCgiRequest</span>–what the CGI program is getting from the client.

<span class="nctnt ncbi-class">CCgiResponse</span>–what the CGI program is sending to the client.

<span class="nctnt ncbi-class">CCgiEntry</span>–a single field value, optionally accompanied by a filename.

<span class="nctnt ncbi-class">CCookie</span>–a single cookie

<span class="nctnt ncbi-class">CCookies</span>–a cookie container

Note: In the following libraries you will see references to the following typedefs: <span class="nctnt ncbi-class">CNcbiOstream</span> and <span class="nctnt ncbi-class">CNcbiIstream</span>. On Solaris and NT, these are identical to the standard library output stream (ostream) and input stream (istream) classes. These typedefs are used on older computers to switch between the old stream library and the new standard library stream classes. Further details can be found in an accompanying document (to be written).

A demo program, cgidemo.cpp, can be found in internal/c++/src/corelib/demo.

### CCgiRequest

<span class="nctnt ncbi-class">CCgiRequest</span> is the class that reads in the input from the web server and makes it accessible to the CGI program.

<span class="nctnt ncbi-class">CCgiRequest</span> uses the following typedefs to simplify the code:

    typedef map<string, string>         TCgiProperties
    typedef multimap<string, CCgiEntry> TCgiEntries
    typedef TCgiEntries::iterator       TCgiEntriesI
    typedef list<string>                TCgiIndexes

All of the basic types come from the C++ Standard library (<span class="nctnt ncbi-monospace">http://www.sgi.com/Technology/STL/</span>)

**CCgiRequest(int argc, char\* argv[], CNcbiIstream\* istr=0, bool indexes\_as\_entries=true)**

A CGI program can receive its input from three sources: the command line, environment variables, and an input stream. Some of this input is given to the <span class="nctnt ncbi-class">CCgiRequest</span> class by the following arguments to the constructor:

<span class="nctnt ncbi-code">int argc, char\* argv[]</span> : standard command line arguments.

<span class="nctnt ncbi-code">CNcbiIstream\* istr=0</span> : the input stream to read from. If 0, reads from stdin, which is what most web servers use.

<span class="nctnt ncbi-code">bool indexes\_as\_entries=true</span> : if query has any ISINDEX like terms (i.e. no "=" sign), treat it as a form query (i.e. as if it had an "=" sign).

Example:

    CCgiRequest * MyRequest = new CCgiRequest(argc, argv);

**const TCgiEntries& GetEntries(void) const**

Get a set of decoded form entries received from the web browser. So if you sent a cgi query of the form ?name=value, the multimap referenced by TCgiEntries& includes "name" as a .first member and \<"value", ""\> as a .second member.

TCgiEntries& also includes "indexes" if "indexes\_as\_entries" in the constructor was "true".

**const TCgiIndexes& GetIndexes(void) const**

This performs a similar task as <span class="nctnt ncbi-func">GetEntries()</span>, but gets a set of decoded entries received from the web browser that are ISINDEX like terms (i.e. no "=" sign),. It will always be empty if "indexes\_as\_entries" in the constructor was "true"(default).

**const string& GetProperty(ECgiProp prop) const**

Get the value of a standard property (empty string if not specified). See the "Standard properties" list below.

**static const string& GetPropertyName(ECgiProp prop)**

The web server sends the CGI program properties of the web server and the http headers received from the web browser (headers are simply additional lines of information sent in a http request and response). This API gets the name(not value!) of standard properties. See the "Standard properties" list below.

**Standard properties:**

    eCgi_ServerSoftware ,
    eCgi_ServerName,
    eCgi_GatewayInterface,
    eCgi_ServerProtocol,
    eCgi_ServerPort,        // see also "GetServerPort()"
    // client properties
    eCgi_RemoteHost,
    eCgi_RemoteAddr,        // see also "GetRemoteAddr()"
    // client data properties
    eCgi_ContentType,
    eCgi_ContentLength,     // see also "GetContentLength()"
    // request properties
    eCgi_RequestMethod,
    eCgi_PathInfo,
    eCgi_PathTranslated,
    eCgi_ScriptName,
    eCgi_QueryString,
    // authentication info
    eCgi_AuthType,
    eCgi_RemoteUser,
    eCgi_RemoteIdent,
    // semi-standard properties(from HTTP header)
    eCgi_HttpAccept,
    eCgi_HttpCookie,
    eCgi_HttpIfModifiedSince,
    eCgi_HttpReferer,
    eCgi_HttpUserAgent

**const string& GetRandomProperty(const string& key) const**

Gets value of any http header that is passed to the CGI program using environment variables of the form <span class="nctnt ncbi-var">"$HTTP\_\<key\>"</span>. In general, these are special purpose http headers not included in the list above.

**Uint2 GetServerPort(void) const**

Gets the server port used by web browser to access the server.

**size\_t GetContentLength(void) const**

Returns the length of the http request.

**const CCgiCookies& GetCookies(void) const**

Retrieve the cookies that were sent with the request. Cookies are text buffers that are stored in the user's web browsers and can be set and read via http headers. See the CCookie and CCookies classes defined below.

**static SIZE\_TYPE ParseEntries(const string& str, TCgiEntries& entries)**

This is a helper function that isn't normally used by CGI programs. It allows you to decode the URL-encoded string "str" into a set of entries \<"name", "value"\> and add them to the "entries" multimap. The new entries are added without overriding the original ones, even if they have the same names. If the "str" is in ISINDEX format then the entry "value" will be empty. On success, return zero; otherwise return location(1-base) of error.

**static SIZE\_TYPE ParseIndexes(const string& str, TCgiIndexes& indexes)**

This is also a helper function not usually used by CGI programs. This function decodes the URL-encoded string "str" into a set of ISINDEX-like entries (i.e. no "=" signs in the query) and adds them to the "indexes" set. On success, return zero, otherwise return location(1-base) of error.

### CCgiResponse

<span class="nctnt ncbi-class">CCgiResponse</span> is the object that takes output from the CGI program and sends it to the web browser via the web server.

**CNcbiOstream& WriteHeader() const**

**CNcbiOstream& WriteHeader(CNcbiOstream& out) const**

This writes the MIME header necessary for all documents sent back to the web browser. By default, this function assumes that the "Content-type" is "text/html". Use the second form of the function if you want to use a stream other that the default.

**void SetContentType(const string &type)**

Sets the content type. By default this is "text/html". For example, if you were to send plaintext back to the client, you would set type to "text/plain".

**string GetContentType(void) const**

Retrieves the content type.

**CNcbiOstream& out(void) const**

This returns a reference to the output stream being used by the <span class="nctnt ncbi-class">CCgiResponse</span> object. Example:

    CCgiResponse Response; 
    Response.WriteHeader(); 
    Response.out() << "hello, world" << flush;

**CNcbiOstream\* SetOutput(CNcbiOstream\* out)**

Sets the default output stream. By default this is stdout, which is what most web servers use.

**CNcbiOstream\* GetOutput(void) const**

Get the default output stream.

**void Flush() const**

Flushes the output stream.

**void SetRawCgi(bool raw)**

Turns on [non-parsed](http://tools.ietf.org/html/rfc3875#section-5) cgi mode. When this is turned on AND the name of the cgi program begins with "nph-", then the web server does no processing of the data sent back to the client. In this situation, the client must provide all appropriate http headers. This boolean switch causes some of these headers to be sent.

**bool IsRawCgi(void) const**

Check to see if [non-parsed](http://tools.ietf.org/html/rfc3875#section-5) cgi mode is on.

**void SetHeaderValue(const string& name, const string& value)**

Sets an http header with given name and value. For example, SetHeaderValue("Mime-Version", "1.0"); will create the header "Mime-Version: 1.0".

**void SetHeaderValue(const string& name, const tm& value)**

Similar to the above, but sets a header value using a date. See time.h for the definition of tm.

**void RemoveHeaderValue(const string& name)**

Remove the header with name name.

**string GetHeaderValue(const string& name) const**

Get the value of the header with name name.

**bool HaveHeaderValue(const string& name) const**

Check to see if the header with name name exists.

**void AddCookie(const string& name, const string& value) void AddCookie(const CCgiCookie& cookie)**

Add a cookie to the response. This can either be a name, value pair or use the CCookie class described below.

**void AddCookies(const CCgiCookies& cookies)**

Add a set of cookies to the response. See the CCookies class described below.

**const CCgiCookies& Cookies(void) const CCgiCookies& Cookies(void)**

Return the set of cookies to be sent in the response.

**void RemoveCookie(const string& name)**

Remove the cookie with the name name.

**void RemoveAllCookies(void)**

Remove all cookies.

**bool HaveCookies(void) const**

Are there cookies?

**bool HaveCookie(const string& name) const**

Is there a cookie with the given name?

**CCgiCookie\* FindCookie(const string& name) const**

Return a cookie with the given name.

### CCgiCookie

A cookie is a name, value string pair that can be stored on the user's web browser. Cookies are allocated per site and have restrictions on size and number. Cookies have attributes, such as the domain they originated from. <span class="nctnt ncbi-class">CCgiCookie</span> is used by the <span class="nctnt ncbi-class">CCgiRequest</span> and <span class="nctnt ncbi-class">CCgiResponse</span> classes.

**CCgiCookie(const string& name, const string& value)**

Creates a cookie with the given name and value. Throw the "invalid\_argument" if "name" or "value" have invalid format:

-   the "name" must not be empty; it must not contain '='

-   both "name" and "value" must not contain: ";, "

**const string& GetName (void) const**

Get the cookie name. The cookie name cannot be changed.

**CNcbiOstream& Write(CNcbiOstream& os) const**

Write the cookie out to ostream os. Normally this is handled by <span class="nctnt ncbi-class">CCgiResponse</span>.

**void Reset(void)**

Reset everything but the name to the default state

**void CopyAttributes(const CCgiCookie& cookie)**

Set all attribute values(but name!) to those from "cookie"

**void SetValue (const string& str) void SetDomain (const string& str) void SetValidPath (const string& str) void SetExpDate (const tm& exp\_date) void SetSecure (bool secure) // "false" by default**

These function set the various properties of a cookie. These functions will throw "invalid\_argument" if "str" has invalid format. For the definition of tm, see time.h.

**bool GetValue (string\* str) const bool GetDomain (string\* str) const bool GetValidPath (string\* str) const bool GetExpDate (string\* str) const bool GetExpDate (tm\* exp\_date) const bool GetSecure (void) const**

These functions return true if the property is set. They also return value of the property in the argument. If the property is not set, str is emptied. These functions throw the "invalid\_argument" exception if the argument is a zero pointer.

The string version of <span class="nctnt ncbi-func">GetExpDate</span> will return a string of the form "Wed Aug 9 07:49:37 1994"

### CCgiCookies

<span class="nctnt ncbi-class">CCgiCookies</span> aggregates a collection of <span class="nctnt ncbi-class">CCgiCookie</span>

**CCgiCookies(void) CCgiCookies(const string& str)**

Creates a <span class="nctnt ncbi-class">CCgiCookies</span> container. To initialize it with a cookie string, use the format: "name1=value1; name2=value2; ..."

**CCgiCookie\* Add(const string& name, const string& value)**

Add a cookie with the given name, value pair. Note the above requirements on the string format. Overrides any previous cookie with same name.

**CCgiCookie\* Add(const CCgiCookie& cookie)**

Add a <span class="nctnt ncbi-class">CCgiCookie</span>.

**void Add(const CCgiCookies& cookies)**

Adds a <span class="nctnt ncbi-class">CCgiCookie</span> of cookies.

**void Add(const string& str)**

Adds cookies using a string of the format "name1=value1; name2=value2; ..." Overrides any previous cookies with same names.

**CCgiCookie\* Find(const string& name) const**

Looks for a cookie with the given name. Returns zero not found.

**bool Empty(void) const**

"true" if contains no cookies.

**bool Remove(const string& name)**

Find and remove a cookie with the given name. Returns "false" if one is not found.

**void Clear(void)**

Remove all stored cookies

**CNcbiOstream& Write(CNcbiOstream& os) const**

Prints all cookies into the stream "os" (see also <span class="nctnt ncbi-class">CCgiCookie::</span><span class="nctnt ncbi-func">Write()</span>). Normally this is handled by <span class="nctnt ncbi-class">CCgiResponse</span>.

An example web-based CGI application
------------------------------------

-   [Introduction](#introduction--)

-   [Program description](#program-description--)

-   [Program design: Distributing the work](#program-design-distributing-the-work--)

### Introduction

The previous two chapters described the NCBI C++ Toolkit's [CGI](#cgi-) and [HTML](ch_html.html#ch_html.webpgs.html) classes, with an emphasis on their independence from one another. In practice however, a real application must employ both types of objects, with a good deal of inter-dependency.

As described in the description of the CGI classes, the [CNcbiResource](#cncbiresource-) class can be used to implement an application whose functionality varies with the query string. Specifically, the resource class contains a list of <span class="nctnt ncbi-class">CNcbiCommand</span> objects, each of which has a defined <span class="nctnt ncbi-func">GetName()</span> and <span class="nctnt ncbi-func">GetEntry()</span>method. The only command selected for execution on a given query is the one whose <span class="nctnt ncbi-func">GetName()</span> and <span class="nctnt ncbi-func">GetEntry()</span> values match the leading <span class="nctnt ncbi-monospace">key=value</span> pair in the query string.

The <span class="nctnt ncbi-class">CHelloResource</span> class has different commands which will be executed depending on whether the query string invoked an <span class="nctnt ncbi-monospace">init</span> or a <span class="nctnt ncbi-monospace">reply</span> command. For many applications however, this selection mechanism adds unnecessary complexity to the interface, as the application always performs the same function, albeit on different input. In these cases, there is no need to use a <span class="nctnt ncbi-class">CNcbiResource</span> object, or <span class="nctnt ncbi-class">CNcbiCommand</span> objects, as the necessary functionality can be encoded directly in the application's <span class="nctnt ncbi-func">ProcessRequest()</span> method. The example program described in this section uses this simpler approach.

### Program description

The <span class="nctnt ncbi-app">car.cgi</span> program presents an HTML form for ordering a custom color car with selected features. The form includes a group of checkboxes (listing individual features) and a set of radio buttons listing possible colors. Initially, no features are selected, and the default color is black. Following the form, a summary stating the currently selected features and color, along with a price quote, is displayed. When the <span class="nctnt ncbi-monospace">submit</span> button is clicked, the form generates a new query string (which includes the selected features and color), and the program is restarted.

The program uses a [CHTMLPage](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCHTMLPage.html) object with a template file ([car.html](ch_demo.html#ch_demo.carhtml)) to create the display. The template file contains three \<@tag@\> locations, which the program uses to map <span class="nctnt ncbi-class">CNCBINode</span>s to the page, using the <span class="nctnt ncbi-func">AddTagMap()</span> method. Here is an outline of the execution sequence:

Create an instance of class <span class="nctnt ncbi-class">CCar</span> named <span class="nctnt ncbi-var">car</span>.

Load <span class="nctnt ncbi-var">car</span> with the color and features specified in the query string.

Create a <span class="nctnt ncbi-class">CHTMLPage</span> named <span class="nctnt ncbi-var">page</span>.

Generate a <span class="nctnt ncbi-class">CHTML\_form</span> object using the features and color currently selected for <span class="nctnt ncbi-var">car</span>, and map that HTML form to the \<@FORM@\> tag in <span class="nctnt ncbi-var">page</span>.

Generate the summary statement and save it in a <span class="nctnt ncbi-class">CHTMLText</span> node mapped to the \<@SUMMARY@\> tag.

Generate a price quote and save it in a <span class="nctnt ncbi-class">CHTMLText</span> node mapped to the \<@PRICE@\> tag.

Output <span class="nctnt ncbi-var">page</span> and exit.

The <span class="nctnt ncbi-class">CCar</span> created in step 1 initially has the default color (black) and no features. Any features or colors specified in the query string with which the program was invoked are added to <span class="nctnt ncbi-var">car</span> in step 2, prior to generating the HTML display elements. In step 4, the form element is created using the set of possible features and the set of possible colors. These sets of attributes are stored as static data members in an external utility class, <span class="nctnt ncbi-class">CCarAttr</span>. Each feature corresponds to a <span class="nctnt ncbi-class">CHTML\_checkbox</span> element in the form, and each color corresponds to a <span class="nctnt ncbi-class">CHTML\_radio</span> button. The selected color, along with all currently selected features, will be displayed as selected in the form.

The summary statement uses a <span class="nctnt ncbi-class">CHTML\_ol</span> list element to itemize the selected features in <span class="nctnt ncbi-var">car</span>. The price is calculated as <span class="nctnt ncbi-var">CCar::m\_BasePrice</span> plus an additional $1000 per feature. The <span class="nctnt ncbi-monospace">submit</span> button generates a fresh page with the new query string, as the <span class="nctnt ncbi-code">action</span> attribute of the form is the URL of <span class="nctnt ncbi-app">car.cgi</span>.

### Program design: Distributing the work

The program uses three classes: <span class="nctnt ncbi-class">CCar</span>, <span class="nctnt ncbi-class">CCarAttr</span>, and <span class="nctnt ncbi-class">CCarCgi</span>. The <span class="nctnt ncbi-class">CCar</span> class knows nothing about HTML nodes or CGI objects - its only functions are to store the currently selected color and features, and compute the resulting price:

    class CCar
    {
    public:
        CCar(unsigned base_price = 12000) { m_BasePrice = base_price; }
        // Mutating member functions
        void AddFeature(const string& feature_name);
        void SetColor(const string& color_name);
        // Access member functions
        bool HasFeature(const string& feature_name) const;
        string GetColor(void) const;
        string GetPrice(void) const;
        const set<string>& GetFeatures() const;
    private:
        set<string> m_Features;
        string      m_Color;
        unsigned    m_BasePrice;
    };

Instead, the <span class="nctnt ncbi-class">CCar</span> class provides an interface to all of its data members, thus allowing the application to get/set features of the car as needed. The static utility class, <span class="nctnt ncbi-class">CCarAttr</span>, simply provides the sets of possible features and colors, which will be used by the application in generating the HTML form for submission:

    class CCarAttr {
    public:
        CCarAttr(void);
        static const set<string>& GetFeatures(void) { return sm_Features; }
        static const set<string>& GetColors  (void) { return sm_Colors; }
    private:
        static set<string> sm_Features;
        static set<string> sm_Colors;
    };

Both of these classes are defined in a header file which is <span class="nctnt ncbi-code">\#include</span>'d in the <span class="nctnt ncbi-path">\*.cpp</span> files. Finally, the application class does most of the actual work, and this class must know about <span class="nctnt ncbi-class">CCar</span>, <span class="nctnt ncbi-class">CCarAttr</span>, <span class="nctnt ncbi-lib">HTML</span>, and <span class="nctnt ncbi-lib">CGI</span> objects. The <span class="nctnt ncbi-class">CCarCgi</span> class has the following interface:

    class CCarCgi : public CCgiApplication
    {
    public:
        virtual int ProcessRequest(CCgiContext& ctx);
    private:
        CCar* CreateCarByRequest(const CCgiContext& ctx);
        void PopulatePage(CHTMLPage& page, const CCar& car);
        static CNCBINode* ComposeSummary(const CCar& car);
        static CNCBINode* ComposeForm   (const CCar& car);
        static CNCBINode* ComposePrice  (const CCar& car);
        static const char sm_ColorTag[];
        static const char sm_FeatureTag[];
    };

The source code is distributed over three files:

car.hpp

car.cpp

car\_cgi.cpp

The <span class="nctnt ncbi-class">CCar</span> and <span class="nctnt ncbi-class">CCarAttr</span> classes are defined in <span class="nctnt ncbi-path">car.hpp</span>, and implemented in <span class="nctnt ncbi-path">car.cpp</span>. Both the class definition and implementation for the CGI application class are in <span class="nctnt ncbi-path">car\_cgi.cpp</span>. With this design, only the application class will be affected by changes made to either the HTML or CGI class objects. The additional files needed to compile and run the program are:

[car.html](ch_demo.html#ch_demo.carhtml)

Makefile.car\_app

CGI Status Codes
----------------

For compliance with the HTTP standard ([RFC7321](https://tools.ietf.org/html/rfc7231)), CGI applications must issue an HTTP header line indicating the response status, for example:

    Status: 200 OK

By default, the <span class="nctnt ncbi-class">CCgiApplication</span> framework will issue a status line with an appropriate status code (e.g. 200 for success; 400 for a malformed HTTP request, etc.). To set a non-default status code use <span class="nctnt ncbi-func">CCgiResponse::SetStatus()</span> or <span class="nctnt ncbi-func">CCgiApplication::SetHTTPStatus()</span>. You can also override <span class="nctnt ncbi-func">CCgiApplication::OnException()</span> for custom handling of exceptions.

The official list of HTTP status codes along with hyperlinks to their definitive meanings can be found at: <http://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml> However, status codes issued by NCBI CGI applications should be selected from [Table 7](#table-7), which is a subset of the official list plus two special non-standard additions (299 and 499). There is also a class, [CRequestStatus](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/doxyhtml/classCRequestStatus.html), that holds an enumeration of the valid NCBI C++ Toolkit status codes. (Although the class is named <span class="nctnt ncbi-class">CRequestStatus</span>, it relates to HTTP response status codes.)

Table 7. HTTP Status Codes

| Status Code | Description                                                         |
|-------------|---------------------------------------------------------------------|
| 200         | OK                                                                  |
| 201         | Created                                                             |
| 202         | Accepted                                                            |
| 203         | Non-Authoritative Information                                       |
| 204         | No Content                                                          |
| 205         | Reset Content                                                       |
| 206         | Partial Content                                                     |
| 299         | Partial Content Broken Connection (non-standard - C++ Toolkit only) |
| 300         | Multiple Choices                                                    |
| 301         | Moved Permanently                                                   |
| 302         | Found                                                               |
| 303         | See Other                                                           |
| 304         | Not Modified                                                        |
| 305         | Use Proxy                                                           |
| 307         | Temporary Redirect                                                  |
| 400         | Bad Request                                                         |
| 401         | Unauthorized                                                        |
| 402         | Payment Required                                                    |
| 403         | Forbidden                                                           |
| 404         | Not Found                                                           |
| 405         | Method Not Allowed                                                  |
| 406         | Not Acceptable                                                      |
| 407         | Proxy Authentication Required                                       |
| 408         | Request Timeout                                                     |
| 409         | Conflict                                                            |
| 410         | Gone                                                                |
| 411         | Length Required                                                     |
| 412         | Precondition Failed                                                 |
| 413         | Payload Too Large                                                   |
| 414         | URI Too Long                                                        |
| 415         | Unsupported Media Type                                              |
| 416         | Requested Range Not Satisfiable                                     |
| 417         | Expectation Failed                                                  |
| 499         | Broken Connection (non-standard - C++ Toolkit only)                 |
| 500         | Internal Server Error                                               |
| 501         | Not Implemented                                                     |
| 502         | Bad Gateway                                                         |
| 503         | Service Unavailable                                                 |
| 504         | Gateway Timeout                                                     |
| 505         | HTTP Version Not Supported                                          |

<span class="nctnt highlight">Note:</span> Status code 404 should be reserved for situations when the requested resource does not exist. It should not be used as a "catch-all" such as when the client simply uses invalid parameters.

FCGI Redirection and Debugging C++ Toolkit CGI Programs
-------------------------------------------------------

Development, testing, and debugging of CGI applications can be greatly facilitated by making them [FastCGI](http://www.fastcgi.com/)-capable and using a simple proxy script. The basic idea is that FastCGI-enabled applications can be started once and process many requests without exiting, thereby improving performance.

Applications that were written to use the C++ Toolkit CGI framework (see [example above](#example-above)) can easily be made to run under your account, on your development machine, and in a number of ways (e.g. standalone, with special configuration, under a debugger, using a memory checker, using <span class="nctnt ncbi-app">strace</span>, etc.). This is accomplished by "tunneling" through a simple FCGI proxy script that forwards HTTP requests to your application and returns the HTTP responses.

The process is described in the following sections:

-   [Platform specifics](#platform-specifics)

-   [Creating a FastCGI-enabled application from scratch](#creating-a-fastcgi-enabled-application-from-scratch)

-   [Connecting the FCGI proxy to the FCGI application](#connecting-the-fcgi-proxy-to-the-fcgi-application)

-   [Debugging an existing CGI or FCGI application](#debugging-an-existing-cgi-or-fcgi-application)

### Platform specifics

The subsequent sections are based on a Linux platform. If you are using Windows, be aware of the following differences:

-   Running <span class="nctnt ncbi-cmd">new\_project myapp app/cgi</span> creates file names with "cgi\_sample" in them rather than "myapp".

-   The proxy script, [fcgi\_sample.cgi](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c%2B%2B/src/sample/app/cgi/fcgi_sample.cgi?view=log), is a Bourne shell script and therefore won't run on Windows. Please copy it to a Linux web server.

### Creating a FastCGI-enabled application from scratch

If you are starting from scratch, use the [new\_project](ch_proj.html#ch_proj.new_project_Starting) script to create an application that is FastCGI-enabled:

    new_project myapp app/cgi
    rm -rf lib              # not needed
    cd myapp
    rm -rf cgires           # not needed
    rm *cgi_session_sample* # not needed
    vim Makefile.[io]*      # remove cgi_session_sample and SUB_PROJ

This results in the following files:

| File                                            | Purpose                                                                                                                                       |
|-------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-path">fmyapp.cgi</span> | This is the proxy script - it's a regular CGI that the client will call via HTTP. It is the only file that needs to reside on the web server. |
| <span class="nctnt ncbi-path">fmyapp.ini</span> | This is the INI file for the FCGI application, not for the proxy (the proxy does not use an INI file).                                        |
| <span class="nctnt ncbi-path">myapp.cpp</span>  | This is the sample application source code and should be adapted or replaced by your application.                                             |
| <span class="nctnt ncbi-path">myapp.html</span> | This is an HTML template that goes with the sample code - it can be deleted or adapted to your application.                                   |

Building creates both a FastCGI-enabled application and a regular CGI application:

| File                                             | Purpose                                                                                                           |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| <span class="nctnt ncbi-path">fmyapp.fcgi</span> | This is the FastCGI version of your application (hence the <span class="nctnt ncbi-path">.fcgi</span> extension). |
| <span class="nctnt ncbi-path">myapp.cgi</span>   | This is the regular CGI version of your application.                                                              |

### Connecting the FCGI proxy to the FCGI application

The round-trip sequence of events for a FastCGI request/response is:

1  
The FCGI application is started (independent of any requests).

2  
An HTTP request is sent to the URL for the proxy script.

3  
The web server invokes the proxy script using the regular CGI protocol (<span class="nctnt ncbi-var">STDIN</span> and environment variables).

4  
The proxy script forwards the request to your FCGI application through a socket.

5  
The FCGI application processes the request and sends the response and exit status back to the proxy script via the socket.

6  
The proxy sends the response back to the web server via the CGI protocol (<span class="nctnt ncbi-var">STDOUT</span> and exit status).

7  
The web server sends the response and status code to the client via HTTP.

<span>[![Image ch\_cgi\_fcgi\_events.png](/book-test/static/img/ch_cgi_fcgi_events.png)](/book-test/static/img/ch_cgi_fcgi_events.png "Click to see the full-resolution image")</span>

<span class="nctnt highlight">Note:</span> The primary purpose of the FastCGI protocol is to eliminate repeated long application startup times (and possibly shutdown times). This means that, unlike normal CGI applications, FCGI applications will not be started by the web server - they're expected to be already running and listening for a socket connection request from the proxy script. Thus, the proxy script will just attempt to connect to an already running FCGI application via the configured socket. If you send a request to the proxy without having first started your FCGI application, the request will appear to hang until it times out or until the FCGI application is started.

To connect the proxy script to the FCGI application, first find a port that's available on the machine that will run the application. For example, to find out if port 5000 is available for listening:

    myhost$  netstat -l --numeric-ports \| grep 5000
    tcp        0      0 *:5000                      *:*                         LISTEN

If the port is available there won't be any output; if it's in use by another process the output will be similar to the above.

Next, edit the proxy script (on the web server) and set the <span class="nctnt ncbi-monospace">-connect</span> option using the host that will be running the FCGI application, and the port from the previous step:

    # fmyapp.cgi
    /netopt/ncbi_tools/fcgi-current/bin/cgi-fcgi -bind -connect myhost:5000

Then, edit the INI file for the FCGI application and set the listening port using the <span class="nctnt ncbi-monospace">StandaloneServer</span> entry (be sure to prepend a colon to the port):

    # fmyapp.ini
    [FastCGI]
    StandaloneServer = :5000
    Iterations = 10

The <span class="nctnt ncbi-monospace">Iterations</span> entry specifies the number of requests that the FCGI application will process before exiting. You could set this to 1 for single-shot testing, but it's better to use a higher number to simulate typical FastCGI operation.

### Debugging an existing CGI or FCGI application

To debug a "plain" CGI, first create a FastCGI-capable version of it, then debug that. To create a FastCGI-capable version of a "plain" CGI:

1  
Change the makefile to build <span class="nctnt ncbi-path">fmyapp.fcgi</span> instead of <span class="nctnt ncbi-path">myapp.cgi</span> and to link with <span class="nctnt ncbi-lib">xfcgi.lib</span> instead of <span class="nctnt ncbi-lib">xcgi.lib</span>. <span class="nctnt highlight">Note:</span> the application must use the C++ Toolkit's CGI framework (as in the above [example](#example)).

2  
Rebuild.

3  
Install the [proxy script](http://www.ncbi.nlm.nih.gov/viewvc/v1/trunk/c++/src/sample/app/cgi/fcgi_sample.cgi?view=log) on the web server in place of, or in addition to, the existing CGI.

4  
[Configure the connection](#configure-the-connection) between the proxy and the application.

To debug a FastCGI-capable application:

1  
If desired, move the application to a development host for more flexibility in debugging:

a  
Copy the FCGI application and the files it uses (e.g. <span class="nctnt ncbi-path">fmyapp.fcgi</span>, <span class="nctnt ncbi-path">fmyapp.ini,</span> and <span class="nctnt ncbi-path">myapp.html</span>) to the desired host.

b  
[Configure the connection](#configure-the-connection) between the proxy and the application.

2  
Start <span class="nctnt ncbi-path">fmyapp.fcgi</span> under the debugger (or a memory checker or other tool), set a breakpoint on <span class="nctnt ncbi-func">ProcessRequest()</span>, and issue a "run" command. The program will remain in the running state while listening for a request from the proxy script.

3  
From your web browser (or using <span class="nctnt ncbi-cmd">GET</span>/<span class="nctnt ncbi-cmd">POST</span> command-line utilities), submit a web request to the proxy, <span class="nctnt ncbi-path">fmyapp.cgi</span>. The request will be tunneled to <span class="nctnt ncbi-path">fmyapp.fcgi</span> and the debugger will stop at the breakpoint.

4  
Debug the processing of the request.

5  
Issue another "run" command to the debugger to complete processing of the current request, send the response back to the proxy, and begin waiting for the next request (or exit if the configured number of iterations has been reached).


