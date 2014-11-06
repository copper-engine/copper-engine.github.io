---
layout: markdown
title: COPPER - Getting Started
tabname: docs
---

Getting started
=================

copper-starter ist a quick starter project for the cooper-engine. (https://github.com/copper-engine/copper-starter)
We use this project to demonstrate the main features of copper.

### Overview

copper provides a performant solution for the following business workflow:

0. an application needs to handle many workflows in a performant way and integrates copper-engine
1. the copper engine provides the workflow instance
2. Each workflow sends requests to an external asynchronous adapter. 
3. the adapter provides synchronous a correlationID for later references.
4. After some time the adapter sends back the response data together with the correlationID
5. the copper-engine continues the execution of the corresponding workflow
6. the workflow handles the response data.

![Overview](/images/gs-copper-overview.png)

### Features 

The following features of copper engine will be demonstrated in the copper-starter project:

- copper is a workflow engine that provides unlimited "threads" 
	- all threads will be serialized transient or persistent
- workflow code is parsed, compiled and instrumented at runtime
	- workflows can have multiple versions at the same time.
	- workflows are coded in java with wait()-continuation breaks
	- workflows have typesafe RequestData and Response objects.
- There are limitations for the supported java syntax
- IDE Breakpoints in Workflow-files works fine for debugging
- How to inject Services into workflow
- How to configure the processing engine

### Setup

##### Prerequisites

- Java JDK 6,7 or 8 
- gradle 2.0+
- git
- console, eclipse or Intellij Idea

```sh
git clone https://github.com/copper-engine/copper-starter.git
cd copper-starter
gradle assemble
```

##### test run in eclipse

	gradle eclipse

- import in eclipse as Java Project
- run HelloWorldTestApplication.java

##### test run in Intellij IDEA

- import in IDEA as Java Project
- run HelloWorldTestApplication.java

##### test run in console
	 gradle --info runHello

If the test run succeeds, your are prepared for the tutorial.

### Getting Started Tutorial

##### Structure of the copper-starter project

copper-starter contains two examples:

- a very simple HelloWorld Workflow:
	- send 5000 asyncronous "Hello World" request to a mocked "external" Adapter.
	- receive 5000 answer call backs
	- uses the transient copper-engine: all threads are stored in memory.

- a more realistic orchestration example:
    - engine, sender and adapter are separated java programs
    - persistent copper engine stores all threads in derby database


<h4><a href="tutorial1.html">Start with Tutorial part 1 HelloWorld Example</a></h4>
<h4><a href="tutorial1.html">Start with Tutorial part 2 Orchestration Example</a></h4>