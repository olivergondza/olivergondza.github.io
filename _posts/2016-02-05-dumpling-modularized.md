---
layout: post
title: What is new in Dumpling 2.0?
author: olivergondza
tags: dumpling
---

Dumpling 2.0 is considered feature complete. Let's see what's in there!

### Modularization of the maven project

One of the crucial use-cases is and always was bundling Dumpling into
the application as a maven dependency. Unfortunately, this was not always easy
due to a number of transitive dependencies that found its way into Dumpling, most
notably `com.google.guava:guava` and `org.codehaus.groovy:groovy*`.

Before 2.0, Dumpling was a singlemodule project distributed via maven repository
in two flavors: traditional jar to use as a maven dependency for other projects
and an uber jar to be used as a CLI client. The dependency creep was a problem for
the first use case as those dependencies was for the CLI client mostly. Splitting
it up was therefore a natural step.

#### Modules

##### `core`

The core module contains all the factories, data model and DSL as well
as all the queries. It will no longer depend on most of the problematic dependencies.
This module is expected to be used as a maven dependency.

##### `groovy-api`

Groovy interoperability module adds dependency on groovy and a couple of utility
classes to build custom groovy interpreters.

##### `cli`

The CLI client itself. Module is not supposed to be used as maven dependency and,
unlike the other modules, it is not part of Dumpling API.

See [Built-in Dumpling](https://olivergondza.github.io/dumpling/bundling.html) for more details.

#### How to migrate?

The odds are your application depends on classes that ware kept in the core module.
The core module will have the same artifactId as the Dumpling 1.0 so no change
is needed. Clients that depend on groovy interoperability will need to add `dumpling-groovy-api`
artifact. In case of dependency on classes that are now part of the CLI module, it still
may be possible to consume it as a dependency, though this use case is now unsupported.
