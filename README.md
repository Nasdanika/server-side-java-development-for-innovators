# Introduction

There is a saying stating that "There is more than one way to skin a cat". There is definitely more than one way to develop
server-side software applications, including web applications and mobile applications' back-ends.

I work in the Innovation/Research & Development area and part of my job is to build prototypes for new business solutions.

Development of prototypes is different from a "traditional" software development in many ways. In particular:

* Requirements are in flux and may change as you demo your prototype.
* Information shall be collected from many different people on the business and technology side.
* The idea of the new system shall be communicated efficiently to both business and technology people.
* Getting access to back-end and third-party systems is often a challenge and most of the time such systems shall be stubbed, but the stubs shall resemble real back-end systems. For example:
  * The back-end support team may not have bandwidth to provide help to non-prioritized projects, which R&D/Innovation projects are,
  * The new interface is not yet available,
  * There is no connectivity to a third-party yet.  
* Prototypes are built by very small teams (often just one person) and such teams shall be familiar with all aspects of the prototype stack. Such teams often work on several efforts simultaneously.
* Prototypes may be put on hold and then revived.     

So, in a nutshell, the challenge of a prototype developer is to collect, retain, and disseminate knowledge.  

Some of the points above are applicable to "crowd-built" applications where there is no or a very small group of full-time developers, most people contribute on a part-time basis, and people join and leave the development team relatively often. In this situation knowledge shall be retained in the application, not in the team, and be readily available to new team members, as well as application users.   

In the course of building a good deal of prototypes I've assembled a stack of open source technologies (some of which I built myself) that helps me to address the aforementioned challenges.

The goal of this book is to show how to use the stack by building a simple online/mobile banking application. You may ask - "What is so innovative about an online banking application???". First of all this application was chosen because it provides a familiar context - most readers shall be familiar with functionality of web/mobile banking applications and it will help them to focus on how the app is built.

And if you work in a bank you may come up with an innovative idea while reading this book, take the application we build in the book, enhance it, show a working prototype instead of a bunch of PowerPoint slides to your colleagues and maybe it will help to get your idea to production!

The book is organized in several chapters. Each chapter, except the first, builds on the previous, takes some input information, and produces
output based on this information. The output includes source code and Docker image(s). 
The source code is published on [GitHub](https://github.com/Nasdanika/bank) and Docker images are published on [DockerHub](http://hub.docker.com).

Reasons for choosing a particular technology among the alternatives are explained along the way where applicable.

These are the key principles used in developing a new online banking product/prototype:
* __Functionality first__ - as one of my friends said once _"It is much easier to make a functional application fast than a fast application functional."_ You can replace ``functional`` with ``secure``, ``highly available`` or other quality attributes. This statement does not mean that the application developed in the course of this book will be insecure, faulty or slow. It only means that we will develop functionality first, then apply security, show how to scale the app vertically and horizontally and make it fault tolerant.
* __Focus__ - keep related things either close to each other or linked to minimize context switching and mental stitching.
* __Contextual execution__ - the framework takes care of creating an invocation context (persistence, services, ...) to minimize the amount of plumbing code in the application.
* __Modeling__
* __Declarative development__
* __Simplicity__ - when we need to do ``A`` we will use a tool or a framework which does just ``A`` and does it good instead of a tool which does the entire alphabet, even if the latter is super-hot. There are two reasons:
  * To minimize our learning and time-to-value.
  * To facilitate explaining how things work.
      