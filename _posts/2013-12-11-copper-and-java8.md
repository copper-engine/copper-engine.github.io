---
layout: blog
title: COPPER and Java 8
author: Dirk
tags: Java8
categories: development
---

[Java 8](https://jdk8.java.net/) is just around the corner and you might wonder if COPPER is ready for Java 8, especially the new language features.

**Alas, no!** The reason is that we are using the awesome [ASM bytecode engineering library](http://asm.ow2.org/) to load, modify and store workflow classes from and into the database. The current stable release is 4.2, which is not ready for Java 8 yet.

However, the ASM developers are already working on Java8 compatibility and [promise that it's ready with the next major release 5.0](http://asm.ow2.org/history.html). A beta version is already available, which we are testing right now.

Thus, in short: you cannot use Java8 with COPPER right now. If you *really* want to use COPPER with Java 8, you can do so, but be *very careful* to compile all your persitent workflow classes with `--source 1.7 --target 1.7`. (All your other classes can be compiled without these options.) But that's probably not what you want, because you cannot use the new language features in your workflows, especially closures.

We will release a new COPPER version with Java 8 support as soon as ASM 5.0-stable hits the scene, so stay tuned!
