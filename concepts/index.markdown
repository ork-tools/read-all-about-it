---
layout: page
title: Concept Glossary
---

**Bounded Context** [see](http://martinfowler.com/bliki/BoundedContext.html)

: A self-contained and complete unit of a system model.

**Core**

: the central processes in the system.

**Kernel**

: a unit of domain code that can be trusted and shared among bounded contexts

**Processor**

: provides orchestrations in the system [[core]].

  * They generally form the command API for the system core.
  * They define the [[business transaction]] boundaries. 
  * They often coordinate operations between multiple [[aggregates]].
  * 
