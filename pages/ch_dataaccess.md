---
layout: default
title: C++ Toolkit test
nav: pages/ch_dataaccess
---

<span class="label">17</span>Access to NCBI data
================================================

Created: January 26, 2009.

Overview
--------

The overview for this chapter consists of the following topics:

-   Introduction

-   Chapter Outline

### Introduction

This chapter describes access to the NCBI data using the NCBI C++ Toolkit.

### Chapter Outline

-   [Object Manager: Generic API for retrieving and manipulating biological sequence data](#object-manager-generic-api-for-retrieving-and-manipulating-biological-sequence-data)

-   [E-Utils: Access to Entrez Data](#e-utils-access-to-entrez-data)

Object Manager: Generic API for retrieving and manipulating biological sequence data
------------------------------------------------------------------------------------

The information about Object Manager library is [here](ch_objmgr.html).

E-Utils: Access to Entrez Data
------------------------------

### EUtils requests

The base class for all requests is [CEUtils\_Request](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CEUtils_Request). Derived request classes provide *Get/Set* methods to specify arguments for each request. The returned data can be read in several ways:

-   <span class="nctnt ncbi-func">***Read()***</span> - reads the data returned by the server into a string.

-   <span class="nctnt ncbi-func">***GetStream()***</span> - allows to read plain data returned by the server.

-   <span class="nctnt ncbi-func">***GetObjectIStream()***</span> - returns serial stream for reading data (in most cases it's an XML stream).

### Connection context

[CEUtils\_ConnContext](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/ident?i=CEUtils_ConnContext) allows transferring EUtils context from one request to another. It includes user-provided information (tool, email) and history data (WebEnv, query\_key). If no context is provided for a request (the *ctx* argument is *NULL*), a temporary context will be created while executing the request.

### EUtils objects

Most requests return specific data types described in EUtils DTDs. The C++ classes generated from the DTDs can be found in [include/objtools/eutils/](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/include/objtools/eutils)<span class="nctnt ncbi-path">*\<util-name\>*</span>.

### Sample application

An example of using EUtils API can be found in [sample/app/eutils/eutils\_sample.cpp](http://www.ncbi.nlm.nih.gov/IEB/ToolBox/CPP_DOC/lxr/source/src/sample/app/eutils/eutils_sample.cpp).


