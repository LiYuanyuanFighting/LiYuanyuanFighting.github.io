---
layout: post
title: Distributed Programming2(xml)---Materials
categories: [blog ]
tags: [study,dp2,xml,ant,marshalling,java ]
description: 
---  

### [ANT](http://www.cnblogs.com/wufengxyz/archive/2011/11/24/2261797.html "ANT")

To **marshall** an object is to convert it into a form suitable for serialised storage or
transmission; that is, to convert it from its native form within the JVM's memory, into
a form that could be sent down a wire, inserted into a file/database, etc. The specifics
will vary depending on the form of marshalling involved; Java's default serialisation
mechanism is one way, but converting the object into an XML or JSON representation are equally valid.

**Unmarshalling** is just the reverse/other side of this process; taking a representation of
the object created by marshalling, and using it to reconstitute an object instance within the JVM.