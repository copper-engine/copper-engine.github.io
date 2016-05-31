---
layout: markdown
title: COPPER - Tutorial Part 2
tabname: docs
---

Tutorial part 2
===============



### Orchestration Example

a more realistic example with persistent engine

What you need to know:

- a local derby database is used to persist all workflow instances
- the adapter, engine and sender are started in different VMs
- Orchestration uses spring for Dependency Injection
- some basic spring framework knowledge

#### Step 1: run the orchestration example

Start the Engine "org.copperengine.examples.orchestration.OrchestrationEngine" in the IDE

Start the (external) Service "org.copperengine.examples.orchestration.simulators.servers.ServiceSimulatorMain" in the IDE

Send a test message to the engine with "org.copperengine.examples.orchestration.simulators.clients.OrchestrationServiceTestClient" and program arguments "http://localhost:9090/services/orchestration?wsdl 491716677889 sc00p"

(Problem with running parallel gradle tasks.)

#### Step 2: configuration of the orchestration example
OrchestrationEngineContext.xml contains the complete configuration for the persistent copper engine.

    	<bean id="persistent.engine" class="org.copperengine.core.persistent.PersistentScottyEngine" scope="singleton" init-method="startup" destroy-method="shutdown">
    		<property name="idFactory">
    			<bean class = "org.copperengine.core.common.JdkRandomUUIDFactory"></bean>
    		</property>
    		<property name="processorPoolManager" ref="persistent.PPManager"/>
    		<property name="dependencyInjector">
    			<bean class="org.copperengine.spring.SpringDependencyInjector"></bean>
    		</property>
    		<property name="dbStorage" ref="persistent.dbStorage"/>
    		<property name="wfRepository" ref="wfRepository"/>
        	<property name="statisticsCollector" ref="statisticsCollector"/>
    	</bean>

#### Step 3: Dependency Injection for the workflows
The spring configuration is also needed to inject external services into the workflow file:
Open org.copperengine.examples.orchestration.wf.ResetMailbox:

        @AutoWire
        public void setCustomerService(CustomerService customerService) {
            this.customerService = customerService;
        }

        @AutoWire
        public void setNetworkServiceAdapter(NetworkServiceAdapter networkServiceAdapter) {
            this.networkServiceAdapter = networkServiceAdapter;
        }


Because the workflow is created by the engine, we need a mechanism to inject external services.
Spring support is provided by "org.copperengine.spring.SpringDependencyInjector", is
but other [dependency injector implementations](https://github.com/copper-engine/copper-engine/blob/master/projects/copper-coreengine/src/main/java/org/copperengine/core/DependencyInjector.java) are also possible.


### Orchestration Example with Monitoring

In this variant of the previous example we enable monitoring and use the monitoring GUI to inspect the workflows.

#### Start the engine

Run `org.copperengine.examples.orchestration.enginemon.OrchestrationEngineWithMonitoring` in your IDE.  
If you use Eclipse, you can simply launch the `OrchestrationEngineWithMonitoring` run configuration.

Alternatively, you can start the engine by executing the `runEngineWithMonitoring` task in gradle.

Consult the [monitoringContext.xml](https://github.com/copper-engine/copper-starter/blob/master/src/main/resources/orchestrationWithMonitoring/monitoringContext.xml) file to see how the monitoring capabilities are configured.

#### Start the service

Run `org.copperengine.examples.orchestration.simulators.servers.ServiceSimulatorMain` in your IDE.  
If you use Eclipse, you can simply launch the `ServiceSimulatorMain` run configuration.

Alternatively, you can start the service by executing the `runService` task in gradle.

#### Start the monitoring GUI

Run `org.copperengine.monitoring.client.main.MonitorMain` in your IDE with the following arguments:
```
--monitorServerAddress=http://localhost:9090/monitoring
--monitorServerUser=user1
--monitorServerPassword=pass1
```

If you use Eclipse, you can simply launch the `ServiceSimulatorMain` run configuration.

#### Send test messages to the engine

You can choose between sending a single test message or a sequence of test messages to the engine.

For sending a single test message to the engine, run
 `org.copperengine.examples.orchestration.simulators.clients.OrchestrationServiceTestClient` in your IDE with the following arguments: `http://localhost:9090/services/orchestration?wsdl 491716677889 sc00p`.  
 If you use Eclipse, you can simply launch the `OrchestrationServiceTestClient` run configuration.

Alternatively, you can send a test message by executing the `sendMessage` task in gradle.

For continuously sending test messages to the engine, run
 `org.copperengine.examples.orchestration.simulators.clients.OrchestrationServiceLoadTestClient` in your IDE with the argument: `http://localhost:9090/services/orchestration?wsdl`.  
 If you use Eclipse, you can simply launch the `OrchestrationServiceLoadTestClient` run configuration.

 Alternatively, you can continuously send messages to the engine by executing the `fireMessages` task in gradle.
