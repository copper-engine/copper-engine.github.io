---
layout: markdown
title: COPPER - Getting Started
tabname: docs
---

Getting started
=================

A quickstarter example project for cooper-engine.

### Overview

copper provides a performant solution for the following business workflow:

- the application needs to handle many workflows in a performant way.
- Each workflow sends requests to an external asynchronous adapter. 
- the adapter provides synchronous a correlationID for later references.
- After some time the adapter sends back the response data together with the correlationID
- The workflow continues execution and handles the response data.

![Overview](/images/gs-copper-overview.png)

The following features of copper engine will be demonstrated in this starter project:

- copper is a workflow engine that provides unlimited "threads" 
- all threads will be serialized transient or persistent
- workflows are coded in java with wait()-continuation breaks
- There are limitations for the supported java syntax
- workflows have typesafe RequestData and Response object.
- workflow code is parsed, compiled and instrumented at runtime
- workflows can be updates at runtime, and provide versioning support
- workflows have name and version attributes to support old long running jobs
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

### Tutorial

##### Structure of copper-starter project

copper-starter contains two examples:

- a very simple HelloWorld Workflow:
	- send 5000 asyncronous "Hello World" request to a mocked "external" Adapter.
	- receive 5000 answer call backs
	- uses the transient copper-engine: all threads are stored in memory.

- a more realistic orchestration example:
    - engine, sender and adapter are separated java programs
    - persistent copper engine stores all threads in derby database

### Hello World Example

![HelloWorldApplication Overview](/images/gs-copper1.png)

What you need to know:

- HelloWorldTestApplication.java initializes the transient copper engine and requests 5000 workflows. 
- Copper Enigne is configured and initialized by TransientEngineFactory.java
- src/main/workflow/ contains source code for the workflow Java Files and is monitored for changes during execution.

- each workflow sends request data to a mocked "external" adapter and gets a ticket correlationID.
- the workflow execution stops and the java object is serialized and stored in memory.
- The adapter responses by calling HelloWorldService with the correlationId and the response
- HelloWorldService informs the copper engine.
- the corresponding workflow is deserialized and the workflow continues.




##### Feature 1: copper provides "unlimited" threads

Copper serializes each workflow and the thread is freed for other tasks. Copper can handle "unlimited" workflow threads.

Increase the number of workflow thread to 100000. (HelloWorldTestApplication.java (line: 28)

```Java
	private static int NUMBER_OF_WORKFLOWS = 100000;
```

Run the application.

	gradle --info runHello


##### Feature 2: workflow code can be changed during execution

Open HelloWorldWorkFlow.java in Editor.
Run the application with 100.000 workflows.
	
	gradle --info runHello

During execution uncomment HelloWorldWorkFlow.java line: 38
```Java
	logger.info(correlationId+ ": workflow   part one: Hello World!");
```

After maximum 5 sec you will see the additional log entry in the logout.

##### Feature 3: workflow uses wait() to suspend execution until correlationId is reactivated by the copper engine.

```Java
 @Override
    public void main() throws Interrupt {
        final String correlationId = HelloWorldAdapter.get().asyncSendHelloWorld(getData());
        logger.info(correlationId+ ": workflow    break: Hello World!");
        logger.info(correlationId+ ": workflow   part one: Hello World!");

        wait(WaitMode.ALL, 5 * 60 * 60 * 1000, correlationId);
        (...)
```

wait() can wait for one or mutliple corellationIds and continue, if one or all IDs are handled.
A timeout (in ms) can be specified for the maximum waiting time.

```Java
        wait(WaitMode.ONE, 5 * 60 * 60 * 1000, correlationId, correlationId2, correlationId3);
```

If  `HelloWorldService.sendResponse(String correlationId, boolean success, String answer)` is called by the external system, it informs the copper engine with the response.

```Java
    public void sendResponse(String correlationId, boolean success, String answer) {
        HelloWorldResponse payload = new HelloWorldResponse(success, answer);
        Response<HelloWorldResponse> response = new Response<HelloWorldResponse>(correlationId, payload, null);
        Acknowledge.DefaultAcknowledge ack = new Acknowledge.DefaultAcknowledge();

        engine.notify(response, ack);
        ack.waitForAcknowledge();
    }
```

The workflow continues and can retrieve the response and handle it.
```Java
        final Response<HelloWorldResponse> response = getAndRemoveResponse(correlationId);

        logger.info(correlationId + ": workflow continue: " + response.getResponse().getAnswer());
```
After completing the workflow the engine gets an acknowledge for the successful completion.

##### Feature 4: Requirements for a new workflow

```Java
@WorkflowDescription(alias = "HelloWorldWorkFlow", majorVersion = 1, minorVersion = 0, patchLevelVersion = 0)
public class HelloWorldWorkFlow extends Workflow<HelloWorldRequest> {


    @Override
    public void main() throws Interrupt {
    	...
    }

}
```

New workflows have the following requirements:
	* the source code of the workflow needs to be found by the copper engine either in the classpath or in a specified source directory
	* only parts of the Java 7 syntax is supported.
	* included Java fields needs to be serializable.
	* needs to extend org.copperengine.core.Workflow or org.copperengine.core.persistent.PersistentWorkflow
	* include workflow logic in `public void main() throws Interrupt {}`
	* wait() stops the execution with correlationId and timeout
	* copper starts continuation with correlationID and workflow can get response data with getAndRemoveResponse(correlationId)
	* each Workflow has a name and version annotation for reference in the engine.

The workflow needs to be created by the engine with a name reference (and optional version)

```Java
        engine.run("HelloWorldWorkFlow", getNewData());
```



