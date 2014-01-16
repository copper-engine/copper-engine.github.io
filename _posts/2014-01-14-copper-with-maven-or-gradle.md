---
layout: blog
title: Using COPPER with Maven or Gradle
author: Dirk
categories: development
tags: [ "Maven", "Gradle" ]
---

Since [release 3.0]({% post_url 2014-01-14-copper-3.0-released %}) COPPER is available on the [Maven Central Repository](http://search.maven.org).

This means that now you can pull in the COPPER core engine and all its assorted libraries as a Maven dependency. Simply add the following dependency to your project's POM:

    <dependency>
        <groupId>org.copper-engine</groupId>
        <artifactId>copper-coreengine</artifactId>
        <version>3.0</version>
     </dependency>

This automatically pulls in the dependent libraries `copper-jmx-interface` and the third-party libraries `asm`, `slf4j-api`, `log4j`, `commons-codec`, `c3p0` and `aopalliance`. (Note: we are currently working on a refactoring to make the list of third-party dependencies even smaller -- in the end only `asm` and `slf4j-api` will be needed!)

If you are using **Gradle**, then add the following line to your dependencies:

    compile 'org.copper-engine:copper-coreengine:3.0'

If you're using the **Spring Framework** then you will also need `copper-spring`, because since 3.0 the Spring support classes have been moved from the core to its own project:

    <dependency>
        <groupId>org.copper-engine</groupId>
        <artifactId>copper-spring</artifactId>
        <version>3.0</version>
    </dependency>

Likewise for Gradle:

    compile 'org.copper-engine:copper-spring:3.0'

There are other COPPER artifacts on Maven Central, namely `copper-monitoring.*` which contains our upcoming monitoring server and UI and `orch-engine`/`orch-interfaces`/`orch-simulators` which contains an exhaustive real-world COPPER example application. Both will be explained in future posts, so stay tuned!
